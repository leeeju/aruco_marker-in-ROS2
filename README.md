# ros2_aruco

OpenCV기반의 Aruco 마커 트레킹 ROS2 버전

이 패키지는 최신 버전의 OpenCV python 을 사용합니다.

```
pip install opencv-contrib-python   # or pip3   <- Open_CV 설치
```

## ROS2 API for the ros2_aruco Node

해당 노드는 이미지에서 Aruco AR 마커를 찾고 해당 ID와 poses정보를 표시합니다.

Subscriptions:
* `/camera/image_raw` (`sensor_msgs.msg.Image`)
* `/camera/camera_info` (`sensor_msgs.msg.CameraInfo`)

Published Topics:
* `/aruco_poses` (`geometry_msgs.msg.PoseArray`) - 감지된 모든 마커의 포즈 (rviz 사용가능)
* `/aruco_markers` (`ros2_aruco_interfaces.msg.ArucoMarkers`) 
 - 해당 마커 ID와 함께 모든 poses의 배열을 제공합니다.

Parameters:
* `marker_size` - 미터 단위의 마커 크기  ( [EX = (default .0625)] 6.25cm )
* `aruco_dictionary_id` - 마커를 생성하는 데 사용된  DICT 값 (default `DICT_5X5_250`)
* `image_topic` - 표시되는 이미지  (default `/camera/image_raw`)
* `camera_info_topic` - Camera info topic (default `/camera/camera_info`)
* `camera_frame` - 사용할 카메라 광학 프레임 (카메라 정보 메시지에서 제공한 프레임 ID로 기본 설정),(작성자는 USB 카메라 사용함)

## Generating Marker Images
- 마커 생성 예시

```
ros2 run ros2_aruco aruco_generate_marker --id 1 --size 200 --dictionary DICT_4X4_100
```
--> 마커의 ID는 1번, 크기는 200, 찾을 마커정보 DICT_4X4_100

--> 뒤에 따로 인자값을 입력하지 않으면 전부 초기값인  --id 1 --size 200 --dictionary DICT_5X5_250 의 마커가 생성됩니다.

Pass the `-h` flag for usage information: 

```
usage: aruco_generate_marker [-h] [--id ID] [--size SIZE] [--dictionary]

Generate a .png image of a specified maker.

optional arguments:
  -h, --help     show this help message and exit
  --id ID        Marker id to generate (default: 1)
  --size SIZE    Side length in pixels (default: 200)
  --dictionary   Dictionary to use. Valid options include: DICT_4X4_100,
                 DICT_4X4_1000, DICT_4X4_250, DICT_4X4_50, DICT_5X5_100,
                 DICT_5X5_1000, DICT_5X5_250, DICT_5X5_50, DICT_6X6_100,
                 DICT_6X6_1000, DICT_6X6_250, DICT_6X6_50, DICT_7X7_100,
                 DICT_7X7_1000, DICT_7X7_250, DICT_7X7_50, DICT_ARUCO_ORIGINAL
                 (default: DICT_5X5_250)
```

- 마커 탐색 예시

```
ros2 topic echo /aruco_markers
```

```
ros2 run ros2_aruco aruco_node
```

