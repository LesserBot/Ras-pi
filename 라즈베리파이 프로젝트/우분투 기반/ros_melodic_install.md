# 터미널로 접속하여 복사 붙여넣기
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

sudo apt update

sudo apt install ros-melodic-desktop-full



sudo apt-get install python-pip

sudo pip install -U rosdep                                        or                              sudo apt install python-rosdep

sudo rosdep init

rosdep update

echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc

source /opt/ros/melodic/setup.bash

sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential
```
- 설치 후 roscore 입려으로 접속 
- 컨트롤 +c 로 종료

우분투 종료코드
```
sudo shutdown -h 0

```


