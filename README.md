# KSC21 Winter F1TENTH SESSION
해당 시뮬레이션 환경의 출처는 [f1tenth/f1tenth_gym](https://github.com/f1tenth/f1tenth_gym), [f1tenth/f1tenth_gym_ros](https://github.com/f1tenth/f1tenth_gym_ros) 이며, 학습 목적으로 folk 되었습니다.

해당 환경에 대한 설명은 [Documentation](https://f1tenth-gym.readthedocs.io/en/latest) 에서 확인할 수 있습니다.

# F1TENTH Gym
## Introduction
f1tenth_gym 환경은 자율주행 고속경주 상황을 실험하기 위한 시뮬레이션으로써 Open AI - Gym의 환경으로 생성되어있습니다.  

## Quickstart
### System Requirements
* Python 3.8 or later
* llvmlite를 사용할 수 있는 환경 

### Test Environment
* Ubuntu 18.04 LTS & macOS Monterey 12.0.1
* Python 3.8+ (Anaconda)

### Installation
본 환경의 설치 과정은 다음과 같습니다.
```bash
$ git clone https://github.com/CSLab-GNU/f1tenth_gym.git 
$ cd f1tenth_gym
$ pip3 install --user -e gym/
```

### Run
간단한 예제 실행은 다음과 같습니다.
```bash
$ cd examples
$ python3 sim.py
```

#### Selecting Algorithm Planner
`examples/sim.py`에서 `planner` 변수를 수정함으로써 실험하고자 하는 알고리즘을 선택할 수 있습니다.
```python
from planner import waypoint_follow as wf
from planner import fgm_convolution as fc
...

    wfp = wf.PurePursuitPlanner(conf, 0.17145 + 0.15875)
    fgp = fc.FgPlanner(conf, 0.17145 + 0.15875)

    planner = fgp # or wfp
```

#### Parameters
`examples/params.yaml`에서 각각의 변수를 수정함으로써 실험하고자 하는 파라미터를 설정할 수 있습니다.

****

# F1TENTH Gym ROS
위의 F1TENTH Gym 환경을 차량에 이식하기 용이하기 하기 위하여 ROS 환경으로 구현한 것입니다.

## Quickstart
### System Requirements
* Ubuntu 18.04 LTS or Ubuntu 20.04 LTS
* ROS Melodic or ROS Noetic
* Docker 20.10.11 or later
* Python 2.7
* Python 3.6 or later

### Test Environment
* Ubuntu 18.04
* ROS Melodic
* Python 2.7

**ROS Melodic (or later)와 Docker가 설치 되어 있어야 합니다. 설치 방법은 [Docker 설치방법](https://docs.docker.com/engine/install/ubuntu/), [ROS 설치방법]() 을 따라 설치하여 주시길 바랍니다.**

### Installation
1. catkin_ws/src 폴더에 해당 Repository 를 Clone 합니다
```bash
$ mkdir ~/catkin_ws/src
$ cd ~/catkin_ws/src
$ git clone https://github.com/CSLab-GNU/f1tenth_gym_ros
```
2. docker image를 빌드합니다.
```bash
$ cd f1tenth_gym_ros
$ sudo ./build_docker.sh
```

3. (Optional) catkin_ws/src/Sample-Code 폴더에 예제 코드를 Clone 합니다.
```bash
$ cd ~/catkin_ws/src
$ git clone https://github.com/CSLab-GNU/Sample-Code
$ cd ..
$ catkin_make
$ source devel/setup.bash
```

### Run

1. 환경을 실행시키기 위해, 다음 Shell Script를 실행합니다.
```bash
$ sudo ./docker.sh
```

2. Host 시스템의 다른 Terminal 창에서 주행 관련 launch 파일을 실행합니다.
```bash
$ roslaunch <패키지명> <주행알고리즘>.launch
```


#### Change Map
환경에 설정된 맵을 변경하는 방법은 다음과 같습니다.  
1. params.yaml 파일을 수정합니다.
```yaml
...
map_path: '/f1tenth/maps/<맵파일>.yaml'
...
```
2. gym_bridge.launch 파일을 수정합니다.
```xml
...
<arg name="map" default="$(find f1tenth_gym_ros)/maps/<맵파일>.yaml"/>
...
```
3. 이후 docker image를 다시 build 합니다.
```bash
$ sudo ./build_docker.sh
```

맵 관련 파일은 <지도 이름>.png 와 <지도 이름>.yaml 파일 두가지가 존재하여야합니다.  
이전 대회에서 사용되었던 맵은 [f1tenth/f1tenth_racetracks](https://github.com/f1tenth/f1tenth_racetracks) 에서 확인할 수 있습니다.

