- 표준단위
    - SI 유닛 사용
- 좌표 표현 방식
    - x : forward
    - y : left
    - z : up
- 프로그래밍 규칙
    - ros.org 에 c++ style guide 참고

- catkin 작업공간 만들기
```
mkdir -p ~/catkin_ws/src
```
- catkin 작업공간에 tutorial 패키지 생성
```
cd ~/catkin_ws/src

catkin_create_pkg ros_tutorials_topic message_generation std_msgs roscpp
cd ros_tutorials_topic
```

- catkin_create_pkg(패키지생성) ros_tutorials_topic(이름) message_generation std_msgs roscpp(ros c++)등의 의존성 설정 3개 

- package.xml :ros 필수 설정파일, 패키지 이름 저작자 라이센서 의존성 패키지등 기술

```
gedit package.xml
안 내용을 변경(강의 내용)
<?xml version="1.0"?>
<package format="2">
<name>ros_tutorials_topic</name>
<version>0.1.0</version>
<description>ROS turtorial package to learn the topic</description>
<license>Apache 2.0</license>
<author email="pyo@robotis.com">Yoonseok Pyo</author>
<maintainer email="pyo@robotis.com">Yoonseok Pyo</maintainer>
<url type="website">http://www.robotis.com</url>
<url type="repository">https://github.com/ROBOTIS-GIT/ros_tutorials.git</url>
<url type="bugtracker">https://github.com/ROBOTIS-GIT/ros_tutorials/issues</url ><buildtool_depend>catkin</buildtool_depend>
<depend>roscpp</depend>
<depend>std_msgs</depend>
<depend>message_generation</depend>
<export></export >
</package> 
```
- 유튜브 "ROBOTIS opensourceTeam"님의 ros 한글코스 강의영상 강의자료 깃허브 https://github.com/robotpilot/ros-seminar

- CMakeLists.txt 도 수정
```
강의내용
cmake_minimum_required(VERSION 2.8.3)
project(ros_tutorials_topic)
## 캐킨 빌드를 할 때 요구되는 구성요소 패키지이다.
## 의존성 패키지로 message_generation, std_msgs, roscpp이며 이 패키지들이 존재하지 않으면 빌드 도중에 에러가 난다.
find_package(catkin REQUIRED COMPONENTS message_generation std_msgs roscpp)
## 메시지 선언: MsgTutorial.msg
add_message_files(FILES MsgTutorial.msg)
## 의존하는 메시지를 설정하는 옵션이다.
## std_msgs가 설치되어 있지 않다면 빌드 도중에 에러가 난다.
generate_messages(DEPENDENCIES std_msgs)
## 캐킨 패키지 옵션으로 라이브러리, 캐킨 빌드 의존성, 시스템 의존 패키지를 기술한다.
catkin_package(
LIBRARIES ros_tutorials_topic
CATKIN_DEPENDS std_msgs roscpp
)## 인클루드 디렉터리를 설정한다.
include_directories(${catkin_INCLUDE_DIRS})
## topic_publisher 노드에 대한 빌드 옵션이다.
## 실행 파일, 타깃 링크 라이브러리, 추가 의존성 등을 설정한다.
add_executable(topic_publisher src/topic_publisher.cpp)
add_dependencies(topic_publisher ${${PROJECT_NAME}_EXPORTED_TARGETS}
${catkin_EXPORTED_TARGETS})
target_link_libraries(topic_publisher ${catkin_LIBRARIES})
## topic_subscriber 노드에 대한 빌드 옵션이다.
add_executable(topic_subscriber src/topic_subscriber.cpp)
add_dependencies(topic_subscriber ${${PROJECT_NAME}_EXPORTED_TARGETS}
${catkin_EXPORTED_TARGETS})
target_link_libraries(topic_subscriber ${catkin_LIBRARIES})
Topic / Publisher / Sub
```
- 메세지 파일 작성
```
$ mkdir msg → ros_tutorials_topic 패키지에 msg라는 메시지 폴더를 신규 작성
$ cd msg → 작성한 msg 폴더로 이동
$ gedit MsgTutorial.msg → MsgTutorial.msg 파일 신규 작성 및 내용 수정
$ cd .. → ros_tutorials_topic 패키지 폴더로 이동
```
```
time stamp
int32 data
```
- 퍼블리셔 노드 작성
```
$ cd src → ros_tutorials_topic 패키지의 소스 폴더인 src 폴더로 이동
$ gedit topic_publisher.cpp → 소스 파일 신규 작성 및 내용 수정
```
```
#include "ros/ros.h" // ROS 기본 헤더파일
#include "ros_tutorials_topic/MsgTutorial.h"// MsgTutorial 메시지 파일 헤더(빌드 후 자동 생성됨)
int main(int argc, char **argv) // 노드 메인 함수
{
ros::init(argc, argv, "topic_publisher"); // 노드명 초기화
ros::NodeHandle nh; // ROS 시스템과 통신을 위한 노드 핸들 선언
// 퍼블리셔 선언, ros_tutorials_topic 패키지의 MsgTutorial 메시지 파일을 이용한
// 퍼블리셔 ros_tutorial_pub 를 작성한다. 토픽명은 "ros_tutorial_msg" 이며,
// 퍼블리셔 큐(queue) 사이즈를 100개로 설정한다는 것이다
ros::Publisher ros_tutorial_pub = nh.advertise<ros_tutorials_topic::MsgTutorial>("ros_tutorial_msg", 100);
// 루프 주기를 설정한다. "10" 이라는 것은 10Hz를 말하는 것으로 0.1초 간격으로 반복된다
ros::Rate loop_rate(10);
// MsgTutorial 메시지 파일 형식으로 msg 라는 메시지를 선언
ros_tutorials_topic::MsgTutorial msg;
// 메시지에 사용될 변수 선언
int count = 0;
while (ros::ok())
{
msg.stamp = ros::Time::now(); // 현재 시간을 msg의 하위 stamp 메시지에 담는다
msg.data = count; // count라는 변수 값을 msg의 하위 data 메시지에 담는다
ROS_INFO("send msg = %d", msg.stamp.sec); // stamp.sec 메시지를 표시한다
ROS_INFO("send msg = %d", msg.stamp.nsec); // stamp.nsec 메시지를 표시한다
ROS_INFO("send msg = %d", msg.data); // data 메시지를 표시한다
ros_tutorial_pub.publish(msg); // 메시지를 발행한다
loop_rate.sleep(); // 위에서 정한 루프 주기에 따라 슬립에 들어간다
++count; // count 변수 1씩 증가
}
return 0;
}
```
- 서브 스크라이버 노드 작성
```
$ roscd ros_tutorials_topic/src → 패키지의 소스 폴더인 src 폴더로 이동
$ gedit topic_subscriber.cpp → 소스 파일 신규 작성 및 내용 수정
```
```
#include "ros/ros.h" // ROS 기본 헤더파일
#include "ros_tutorials_topic/MsgTutorial.h" // MsgTutorial 메시지 파일 헤더 (빌드 후 자동 생성됨)
// 메시지 콜백 함수로써, 밑에서 설정한 ros_tutorial_msg라는 이름의 토픽
// 메시지를 수신하였을 때 동작하는 함수이다
// 입력 메시지로는 ros_tutorials_topic 패키지의 MsgTutorial 메시지를 받도록 되어있다
void msgCallback(const ros_tutorials_topic::MsgTutorial::ConstPtr& msg)
{
ROS_INFO("recieve msg = %d", msg->stamp.sec); // stamp.sec 메시지를 표시한다
ROS_INFO("recieve msg = %d", msg->stamp.nsec); // stamp.nsec 메시지를 표시한다
ROS_INFO("recieve msg = %d", msg->data); // data 메시지를 표시한다
}
int main(int argc, char **argv) // 노드 메인 함수
{
ros::init(argc, argv, "topic_subscriber"); // 노드명 초기화
ros::NodeHandle nh; // ROS 시스템과 통신을 위한 노드 핸들 선언
// 서브스크라이버 선언, ros_tutorials_topic 패키지의 MsgTutorial 메시지 파일을 이용한
// 서브스크라이버 ros_tutorial_sub 를 작성한다. 토픽명은 "ros_tutorial_msg" 이며,
// 서브스크라이버 큐(queue) 사이즈를 100개로 설정한다는 것이다
ros::Subscriber ros_tutorial_sub = nh.subscribe("ros_tutorial_msg", 100, msgCallback);
// 콜백함수 호출을 위한 함수로써, 메시지가 수신되기를 대기,
// 수신되었을 경우 콜백함수를 실행한다
ros::spin();
return 0;
}
```
- Ros 노드 빌드
```
$ cd ~/catkin_ws → catkin 폴더로 이동
$ catkin_make → catkin 빌드 실행
```

- 퍼블리셔 실행
```
$ rosrun ros_tutorials_topic topic_publisher
```
- 여기서 패키지를 찾을 수 없는 경우
```
source ~/catkin_ws/devel/setup.bash
```
실행후 재 실행
