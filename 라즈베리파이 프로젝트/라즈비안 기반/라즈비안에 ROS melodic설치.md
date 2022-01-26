# 라즈비안 버스터 기준
 - 1. SSH허용 후 재부팅
 - 2. ifconfig 로 아이피확인
 
 - vscode 확장-> remote ssh 설치 후 원격 추가 
 - 터미널 열면 라즈베리파이랑 연동 터미널
 
 1024 검색해서 스왑변경

 # Ros 설치
 ```
 $ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
$ sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

$ sudo apt-get update
$ sudo apt-get upgrade

$ sudo apt install -y python-rosdep python-rosinstall-generator python-wstool python-rosinstall build-essential cmake

$ sudo rosdep init
$ rosdep update
```