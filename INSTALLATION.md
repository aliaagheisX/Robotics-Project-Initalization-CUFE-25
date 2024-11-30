# Installation Guide
help you setup enviornment
1. install `wsl`
1. install `Ubuntu-20.04` using `wsl` and setup `Xlaunch`
1. install `ROS-noetic` with all needed packages 
    - `rviz` - `gazebo` - `rqt-graph`
1. setup initial project structure using `catkin_make`
1. download and load `turtlebot3` on enviornment


> you can skip **1, 2** if you will install it dual-boot or VM or directly as main machine

## 1. Install WSL

here's FreeCodeCamp blog explain with details  
[How to Install WSL2 (Windows Subsystem for Linux 2) on Windows 10](
https://www.freecodecamp.org/news/how-to-install-wsl2-windows-subsystem-for-linux-2-on-windows-10/)

## 2. Install `Ubuntu-20.04`  
### 2.1 - Download it on  `wsl`
you can use this command to download it
```bash
wsl --install -d Ubuntu-20.04
```

to run it you have two options (set it default version and just type `wsl` or specify when launching wsl)

to set as default and run it
```bash
wsl --set-default ubuntu-20.04

wsl
```
for myself I have other default version so I will run it using version everytime 
```bash
wsl  -d Ubuntu-20.04
```
### 2.2 - Setup XLaunch
you'll needed  to open GUI application using wsl

download it from [here](https://sourceforge.net/projects/vcxsrv/)

make sure you setup it like this
![image](https://janbernloehr.de/assets/images/RosOnWsl2/vcxsrv.PNG)

## 3. Install `ROS-noetic`
> if this document isn't recent at the time you read it \
> you can follow this link which will open the official document using google translated to english (IDK why but this will open link faster) [here](https://wiki-ros-org.translate.goog/noetic/Installation/Ubuntu?_x_tr_sl=ar&_x_tr_tl=en&_x_tr_hl=en&_x_tr_pto=wapp)

### 3.1 Setup your sources.list
```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

### 3.2 Set up your keys
if you haven't already installed curl
```bash
sudo apt install curl
```
```bash
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

### 3.3 Installation
```bash
sudo apt update
```
**Desktop-Full Install**: (Recommended) : 
Everything we need (ros1 desktop + rqt_graph + rviz + gazebo)

```bash
sudo apt install ros-noetic-desktop-full
```

### 3.4 Environment setup
you  need to source ros script in bash terminal everytime you open new terminal and want to ros on it

t to automatically source this script every time a new shell is launched. These commands will do that for you.

```bash
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
```
```bash
source ~/.bashrc
```

*bashrc* is a file called everytime you open new terminal 
we will use it alot to automatically load things for us 

## 4. Initalize Project
we will use `catkin` to initalize our workspace
### 4.1 Create Our Workspace 
this will contain our packages and nodes (i.e our code)

``` bash
mkdir project_workspace

cd project_workspace

mkdir src

catkin_make
```

**recommened** to run catkin_make everytime you add package or library etc..

since we will use ros with python not c++ we **won't needed to run it** everytime we change a thing


### 4.2 Create package
you can skip it if you won't write code now

```bash
cd src

catkin_create_pkg my_turtle_controller rospy std_msgs
```

> you can add other libraries in your package despite `rospy` and `std_msgs` by adding it in  
> src/my_turtle_controller/package.xml

### 4.3 Source WorkSpace

`catkin` create `devel/setup.bash` you should also source it as this how ros work
```bash
source devel/setup.bash 
```

you can automate step in bashrc (optional) (as clean programmer you shouldn't) (as smart programmer you should)

```bash
echo "source $(pwd)/devel/setup.bash" >> ~/.bashrc

source ~/.bashrc
```


## 5. Install turtlebot3
section  4 was written sine I don't know if we should download turtlebot3 inside our project or outside and source it automatically

I will assume it's inside our project workspace and repo

### 5.1 Download turtlebot3 noetic

```bash
cd src

git clone -b  noetic-devel https://github.com/ROBOTIS-GIT/turtlebot3

git clone -b  noetic-devel https://github.com/ROBOTIS-GIT/turtlebot3_msgs

git clone -b  noetic-devel https://github.com/ROBOTIS-GIT/turtlebot3_simulations

cd ..
catkin_make
```

### 5.2 Play with it
note once you source workspace you can run this commands where-ever you want
```bash
# play with it using gazebo
roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

to move it using keyboard on terminal *open another terminal*

```bash
roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```
