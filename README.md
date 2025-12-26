# 🐢turtlebot3
Turtlebot3 실행 기초 guide (standard : Ubuntu 22.04 LTS / ROS2 Humble)

Basically follow the link : https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/

✅**기본 생활 습관화 code** (build 이후, 뭔가가 안되면 이거 안한거)

    source ~/.bashrc
    source {WS_NAME}/install/setup.bash #ex) source turtlebot_ws/install/setup.bash

이 두가지를 모르면 ros2 기초 영상 다시 보고 오기

## 📷Camera 
D435i guide : https://emanual.robotis.com/docs/en/platform/openmanipulator_x/ros_perceptions/

🚨**주의 : realsense는 무조건 USB3.0 포트에서 사용해야함.**🚨

librealsense 와 realsense-ros 다운 (realsense를 연결해 놓은 HW에서 진행)

    mkdir ~/camera_ws  #desktop에 카메라 workspace를 만들어줌
    cd ~/camera_ws
    git clone https://github.com/realsenseai/realsense-ros.git
    git clone https://github.com/realsenseai/librealsense.git
    colcon build 

build 완료가 되면

    ros2 launch realsense2_camera rs_launch.py

정상적으로 켜지게 되면 다른 터미널 창 연다음에

    rqt #image /color/raw와 /depth/raw가 잘 나오는지 확인
    
## 🦾Manipulator

turtlebot3_openmanipulator guide : https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#turtlebot3-with-openmanipulator

7.2의 sw setup을 진행 (Remote PC : 조종하는 개인 노트북, Turtlebot SBC : ssh로 연결한 라즈베리파이)

7.4의 firmware setup도 진행 (이전의 펌웨어는 turtlebot만 움직이기 위한것. turtlebot+manipulator 모두 움직이기 위한 펌웨어 업데이트라 생각)

    ros2 launch turtlebot3_manipulation_bringup hardward.launch.py

### ⛔️IF doesn't work it
1. dynamixel 기본 세팅에 문제가 있는 것.
2. openCR을 라즈베리파이와 연결을 해제, PC와 연결.(manipulator와 OpenCR은 결선)
3. 7.4.1을 진행했으면 OpenCR에 Dynamixel firmware를 업로드.
4. dynamixel Wizard 2.0 설치(모터 세팅값을 바꿔준다 생각하면 됨) <https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_wizard2/>
5. 이후 검색을 진행하여 ID 11~15까지 검색이 되는지 확인.
6. 모든 모터의 delay time 을 250ms --> 0ms 설정. **저장 눌러요**
7. 그리고 다시 OpenCR과 라즈베리파이를 연결하고 firmware부터 다시 진행. 
