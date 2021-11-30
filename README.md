# F1TENTH Gym Environment
해당 저장소는 F1TENTH Gym Envrionment를 위한 것입니다.

해당 시뮬레이션 환경의 출처는 [F1TENTH_GYM](https://github.com/f1tenth/f1tenth_gym)입니다.

해당 환경에 대한 설명은 [Documentation](https://f1tenth-gym.readthedocs.io/en/latest)에서 확인할 수 있습니다.

# F1TENTH Gym
## Introduction
F1tenth Gym 환경은 자율주행 고속경주 상황을 실험하기 위한 시뮬레이션으로써 Open AI - Gym의 환경으로 생성되어있습니다.  

## Installation
본 환경의 설치 과정은 다음과 같습니다.
```bash
$ git clone https://github.com/CSLab-GNU/f1tenth_gym.git 
$ cd f1tenth_gym
$ pip3 install --user -e gym/
```
간단한 예제 실행은 다음과 같습니다.
```bash
$ cd examples <br>
$ python3 waypoint_follow.py
```

Docker 환경 에서도 실행가능한데 실행 방법은 다음과 같습니다. (nvidia GPU가 필요합니다.)
``` bash
docker build -t f1tenth_gym_container -f Dockerfile .
docker run --gpus all -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix f1tenth_gym_container
```

예제실행은 위와 동일합니다.

# F1TENTH Gym ROS
위의 F1TENTH Gym 환경을 차량에 이식하기 위해 ROS 환경으로 구현한 것입니다.

## Installation
### System Requirements
* Ubuntu-18.04 or Ubuntu-20.04
* ROS (Melodic or Noetic)
* Docker

1. catkin_ws/src 폴더에 해당 repository를 Clone 합니다
```bash
$ mkdir catkin_ws/src
$ cd catkin_ws/src
$ git clone https://github.com/CSLab-GNU/f1tenth_Gym_ros
```
2. catkin_ws 폴더에서 catkin_make를 실행합니다.
```bash
$ cd ..
$ catkin_make
```
3. docker image를 빌드합니다.
```bash
$ cd f1tenth_gym_ros
$ sudo ./build_docker.sh
```

4. 환경을 실행시키기 위해, 다음 Shell Script를 실행합니다.
```bash
$ sudo ./docker.sh
```

5. Host 시스템의 다른 Terminal 창에서 주행 관련 launch 파일을 실행합니다.
```bash
$ roslaunch <패키지명> <주행알고리즘>.launch
```

## Change Map
환경에 설정된 맵을 변경하는 방법은 다음과 같습니다.  
1. catkin_ws/src/f1tenth_gym_ros/params.yaml 파일을 수정합니다.
```yaml
...
map_path: '/f1tenth/maps/<바꿀 맵명>.yaml'
...
```
2. 이후 catkin_ws경로에서 catkin_make를 실행합니다.
```bash
$ cd ~/catkin_ws
$ catkin_make
```
3. 이후 docker image를 다시 build 합니다.
```bash
$ sudo ./build_docker.sh
```

맵 관련 파일은 <지도 이름>.png 와 <지도 이름>.yaml 파일 두가지가 존재하여야합니다.  
이전 대회에서 사용되었던 맵은 [Maps](https://github.com/f1tenth/f1tenth_racetracks)에서 확인할 수 있습니다.

