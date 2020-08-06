# 20200805_dnn_detect_noetic_support

Testing conducted for https://github.com/UbiquityRobotics/dnn_detect/issues/8

Confims that the current version (6f80c298) can build and run in ROS noetic.

 - note the current version is failing to build tests

## Build 
```bash
docker build . -t dnn_detect:noetic
```

## Run
```bash
# Launch dnn_detect in the docker image:
docker run --network=host dnn_detect:noetic roslaunch dnn_detect dnn_detect.launch camera:=cv_camera image:=image_raw
```
```bash
# in a new terminal, run cv_camera
rosrun cv_camera cv_camera_node
```
```bash
# in a third terminal, run rqt_image_view. Confirm objects are being detected
rqt_image_view /dnn_images
```
