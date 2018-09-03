# deeplearning_tutorial

## Syllabus
1. Machine Learning Basic
2. Neural Network & Training Techniques
3. Convlutional Neural Network
3. Transfer Learnig (CNN)
4. Recurrent Neural Network
5. Reinforcement Learning (DQN)


## Docker Setup
### Docker on Ubuntu with CPU
```
docker run --name pytorch --rm \
    -v "/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    -v "$HOME/.Xauthority:/root/.Xauthority:rw" \
    -e QT_X11_NO_MITSHM=1 \
    -e DISPLAY=$DISPLAY \
    -p 8888:8888 \
    --net=host \
    --mount 'type=bind,src=/home/ghryou/Workspace/deeplearning_tutorial,dst=/app' \
    -it ghryou/pytorch:cpu bash

jupyter notebook --allow-root
```

### Docker on Ubuntu with GPU
```
docker run --name pytorch --rm \
   --runtime=nvidia \
   -v "/tmp/.X11-unix:/tmp/.X11-unix:rw" \
   -v "$HOME/.Xauthority:/root/.Xauthority:rw" \
   -e QT_X11_NO_MITSHM=1 \
   -e DISPLAY=$DISPLAY \
   --net=host \
   --mount 'type=bind,src=/home/naverlabs/Workspace/deeplearning_tutorial,dst=/app' \
   -it ghryou/pytorch:gpu bash​
```

### Docker on Windows
1. setup shared memory at docker settings
Right click docker icon -> Settings -> Shared Drives -> Check C Drive -> Apply -> Type password -> Restart docker

![docker_windows_1](res/docker_windows_1.png)
![docker_windows_2](res/docker_windows_2.png)
![docker_windows_3](res/docker_windows_3.png)
2. Install [Xming X Server](https://sourceforge.net/projects/xming/)
3. Follow [this link](https://blogs.msdn.microsoft.com/jamiedalton/2018/05/17/windows-10-docker-gui/) to setup Xming X Server

![docker_windows_4](res/docker_windows_4.png)
![docker_windows_5](res/docker_windows_5.png)
![docker_windows_6](res/docker_windows_6.png)
![docker_windows_7](res/docker_windows_7.png)
4. Run Xming
5. Open windows 10 powershell & Run this code
```
docker run --name pytorch --rm `
-p 8888:8888 `
-p 6006:6006 `
-e DISPLAY:<IP_Address>:0 `
--mount type=bind,src=C:\USERS\naverlabs\Desktop\deeplearning_tutorial,dst=/app `
-it ghryou/pytorch:cpu bash​
```

### Docker on Mac
1. Install [homebrew](https://brew.sh/index_ko)
2. brew install xquartx
3. Follow [this link](https://sourabhbajaj.com/blog/2017/02/07/gui-applications-docker-mac/) for X11 security Setup
4. Run this code
```
open -a XQuartz
./docker/run.mac.sh ghryou/pytorch:cpu bash
```

### Ubuntu Commands
```
sudo chown <User name> -R <directory path>
```

### Docker Cheat Sheet
```
docker build -t <tag name> -f <Dockerfile path> <Dockerfile directory>
docker exec -it pytorch bash
docker images
docker ps -al
docker stop <image id>
docker rmi <image id>
docker system prune
```

[Push images to Docker Cloud](https://docs.docker.com/docker-cloud/builds/push-images/)

### Jetson Setup
1. Download JetPack from [Nvidia website](https://developer.nvidia.com/embedded/jetpack)
2. Makes run files executable with **sudo chmod +x JetPack-L4T-3.3-linux-x64_b39.run**
3. Run **./JetPack-L4T-3.3-linux-x64_b39.run**
4. Follow the Commands of JetPack Installer (Install every package)
![jetson_0](res/jetson_0.png)
![jetson_1](res/jetson_1.png)
5. Select **Device accesses Internet via Router/Switch**
![jetson_2](res/jetson_2.png)
6. Connect jetson with host PC and follow the instructions
![jetson_3](res/jetson_3.png)
7. Connect display to jetson and setup internet connection on jetson
8. Check IP address of jetson with **ifconfig**
9. Run **./JetPack-L4T-3.3-linux-x64_b39.run** again
10. Disable **Flash OS Image to Target**
![jetson_4](res/jetson_4.png)
11. Select **Device accesses Internet via Router/Switch**
![jetson_2](res/jetson_2.png)
12. Enter jetson's IP address and follow the instructions

### Jetson Pytorch Setup

0. Operate fan
```
sudo /home/nvidia/^Ctson_clocks.sh
```
1. Insstall pytorch(will take 2hour)
```
 git clone --recursive https://github.com/pytorch/pytorch
 cd pytorch
 python3 setup.py install
```
    never mind denied error, it will work on next step

2. build pytorch

```
python3 setup.py build_deps
sudo python3 setup.py develop
```
3. Verify CUDA and pytorch(from python3 interactive terminal)
```
python3
import torch
print(torch.__version__)
print(torch.cuda.is_available())
```
    result :  True

    Reference: https://gist.github.com/dusty-nv/ef2b372301c00c0a9d3203e42fd83426

4. Install opencv(it will take 1.5hour)
```
git clone https://github.com/jetsonhacks/buildOpenCVTX2
cd buildOpenCVTX2/
 ./buildOpenCV.sh -s
 ```
 5. Verify opencv
```
python3
import cv2
cv2.__version__
```
    result :'3.4.1-dev'
 
 
 6. Install ROS
 
    follow webpage : http://wiki.ros.org/kinetic/Installation/Ubuntu
 ```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116 
sudo apt-get update
sudo apt-get install ros-kinetic-desktop
sudo rosdep init
rosdep update
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
```
 7. Verify ROS
```
roscore
```
