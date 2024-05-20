# Lidar_camera_sensorfusion
카메라와 라이다를 센서 퓨전하여 utm좌표로 객체의 위치를 나타내는 코드입니다.
## description

![autopg_rqt](https://github.com/ERP1-Team/LIdar_camera_sensorfusion/assets/140485388/8f3a9114-54d2-4612-8c58-449ec05884c3)
node들간의 구조는 위와 같습니다. 

먼저, autopg_lidar packge에서 클러스터링 된 객체의 데이터를 /Lidar_object 토픽으로 발행한다. 메시지 타입은 아래와 같다.

<summary>
/Lidar_object 메시지
</summary>
string classes

uint16 idx

float32 x

float32 y

float32 z

float32 xMin

float32 yMin

float32 zMin

float32 xMax

float32 yMax

float32 zMax
</details>
두번째로, yolo_v8_msgs에서 raw_image를 subscribe하여 /yolov8/img(yolo 이미지)와 /yolov8/bboxes(바운딩박스 정보)를 pub한다. 

<summary>
/yolov8/bboxes의 메시지
  
</summary>

string class_name

float32 xMin

float32 yMin

float32 xMax

float32 yMax

float32 confidence

</details>
</details>
세번째로, 카메라 라이다 센서퓨전을 "sensor_fu" package에서 하여 /project_img와 /matched_objects 토픽을 publish한다. /project_img에서 센서퓨전되는 이미지를 볼 수 있으며 /matched_objects 토픽의 메시지는 다음과 같다. 카메라의 class 정보를 라이다 데이터에서도 확인할 수 있게 되었으며 라이다 좌표계에서의 x,y,z center 값들을 얻을 수 있다. 


<summary>
  
/matched_objects의 메시지
  
</summary>

string class_name

float32 lidar_x

float32 lidar_y

float32 lidar_z

float32 distance


</details>

</details>
마지막으로, /matched_objects, /utm_coordinates 토픽을 subscribe하여 라이다에서 탐지한 객체의 위치를 utm좌표로 받을 수 있게 하였다. lidar to gps sensor의 T 또한 고려하였다. 그리고 계산된 utm 좌표는 
/utm_objects 의 토픽으로 발행되며 

<summary>
  
/transformed_objects의 메시지
  
</summary>

string class_name

float32 lidar_x

float32 lidar_y

float32 lidar_z

float32 distance
roslaunch sensor_fu 

</details>
      
## Installation
1. cd ~/ros1_ws/src
2. git clone https://github.com/ERP1-Team/LIdar_camera_sensorfusion.git
3. 압축해제 후
4. cd ..
5. catkin_make

## Usage
roslaunch sensor_fu lidar_yolo.launch 

위의 명령어를 통해 lidar, yolo, lidar_gps 를 실행한다. 
