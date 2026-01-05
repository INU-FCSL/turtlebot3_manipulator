# 🐢Turtlebot3_Manipulator

- 기본 환경설정 : Ubuntu 22.04 / Ros2 Humble
- turtelbot3 guide : <https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/> 
- 작성자 : 박정우
- 작성일 : 2026.01.05

---

## ⭐️ ROS2 기본 명령어
build 진행 후 **꼭** 해줘야하는 명령
```
source ~/.bashrc
source ~/{WS_name}/install/setup.bash
```
**위 두 가지 명령어는 평소에도 몸에 익히도록.**

정 손에 안익으면 bashrc에 저 두 명령어를 넣고 alise로 bashrc불러오는 것을 추천

## 📷 Camera(Realsense D435i)

**realsense는 usb 3.0 port에 연결**

realsense를 사용하기 위한 SDK와 launch 패키지 설치
```
cd && mkdir camera_ws
cd camera_ws && mkdir src
git clone https://github.com/realsenseai/realsense-ros 
git clone https://github.com/realsenseai/librealsense
cd .. && colcon build --symlink-install
```

이후 카메라를 실행 시키고 싶으면
```
ros2 launch rs_camera rs_launch.py #카메라 실행
```

이후 다른 터미널을 열고 송출이 잘 되는지 확인
- color 및 depth의 /raw 토픽을 확인

```
rqt #이미지 송출잘되는지 확인
```


## 🦾Manipulator

### ⛔️Bringup이 안될 때
기본 Dynamixel 모터의 세팅값에 문제가 있음 (이는 로보티즈 포럼에도 언급된 사례)

1. Dynamixel과 OpenCR을 연결.
2. OpenCR과 라즈베리파이 연결을 해제. 그리고 PC(개인 노트북)에 연결
3. Dynamixel Wizaerd 2.0 SW 다운로드 <https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_wizard2/>
4. 검색을 진행하고 ID 11~15까지 모터 검색이 되는지 확인(안되면 OpenCR 펌웨어를 변경)
5. 정상적으로 진행 됐다면 Return Delay time 250ms -> 0ms로 변경
6. 설정 저장 후 연결 해제 및 다시 라즈베리파이와 연결 후 bringup 재시도

### ✅Bringup이 진행 됐을 경우
[Turtlebot]
```
ros2 launch turtlebot3_manipulation_bringup hardware.launch.py
```

follow the 7.10.4.1

[remote PC]
```
cd ~/turtlebot3_ws/src
git clone -b humble https://github.com/ROBOTIS-GIT/turtlebot3_home_service_challenge.git
cd .. && colcon build --symlink-install
ros2 topic pub -1 /manipulator_control std_msgs/msg/String "{data: 'pick_target'}"
ros2 topic pub -1 /manipulator_control std_msgs/msg/String "{data: 'place_target'}"
```
pick & place를 사용하기 위해서 home service challenge 패키지 설치

각각 pick 하거나 place 하거나 선택해서 사용


## 🚍Aruco Tracker

10.9 의 Aruco Tracker가 있음.

기본적으로 ROBOTIS에서 제공해주는 패키지를 최대한 활용해서 메커니즘 파악 후 manipulator에도 적용하는 방안을 생각

**다만 이건 V4l2 카메라 사용하는 것이니 참고(Not Realsense)**

realsense에도 이 패키지를 사용할 수 있다면 좋지 않을까...? 한번 알아보면 좋을 듯

참고 링크 : <https://github.com/Ammah2-git/Final-Year-Project>
