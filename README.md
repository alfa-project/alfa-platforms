# ALFA-Platforms
---

ALFA supports the following platforms: 
- Desktop Linux:
  - [x] Ubuntu 22.04.1 LTS with ROS 2 Humble Hawksbill ([Click Here](#alfa-installation-and-setup-desktop))
- Embedded Platforms:
  - [x] Xilinx Zynq UltraScale+ MPSoC ZCU104 (Petaliunx and Vivado) ([Click Here](#xilinx-zynq-ultrascale-mpsoc-zcu104))
  - [x] More platforms - TBA


<!-- ## Table of Contents

- [Architecture](#architecture)
  - [Software](#software)
  - [Hardware](#hardware)
- [Supported ALFA Configurations and Setups](#supported-alfa-configurations-and-setups)
  - [Desktop](#desktop)
  - [Embedded Software](#embedded-software)
  - [Embedded Software-Hardware](#embedded-software-hardware)
- [Repositories Overview](#repositories-overview)
- [Installation](#installation)
- [Publications](#publications)
-->

# Desktop Ubuntu 22.04.1 LTS with ROS 2 Humble Hawksbill 
---

ALFA is built on top of the [Robot Operating System (ROS)](https://www.ros.org/) architecture to read sensor's data, communicate with other modules, and to perform point cloud processing. Such processing often resorts to [Point Cloud Library (PCL)](https://pointclouds.org/) software modules. 

#### 1. ROS2 Installation [Humble Hawksbill](https://docs.ros.org/en/humble/Installation.html) 

Make sure you have a locale which supports UTF-8:

```sh
locale
```

Enable the Ubuntu Universe Repository:

```sh
sudo apt install software-properties-common
```

```sh
sudo add-apt-repository universe
```

Add the ROS 2 GPG key with apt:

```sh
sudo apt update && sudo apt install curl
```

```sh
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

Add the repository to your sources list:

```sh
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

Update your apt repository caches after setting up the repositories. It is important that systemd and udev-related packages are updated before installing ROS 2:

```sh
sudo apt update
```

```sh
sudo apt upgrade
```

(Full) Desktop Install (ROS, RViz, Dev tools, and Libraries):

```sh
sudo apt install ros-humble-desktop
```

Set up the environment by sourcing the following file:

```sh
source /opt/ros/humble/setup.bash
```

You can permanently do this by adding the following into the ~/.bashrc file:

```sh
# Enable ROS2
if [ -f /opt/ros/humble/setup.bash ]; then
 source /opt/ros/humble/setup.bash
fi
```

Install colcon. You will need it for building ROS2 packages:

```sh
sudo apt install python3-colcon-common-extensions
```

<!---```sh
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```
Add the ROS 2 apt repository to your system:
```sh
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```
```sh
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```
```sh
sudo apt update
```
```sh
sudo apt install ros-melodic-desktop-full
```
```sh
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
```
```sh
source ~/.bashrc
```
-->

#### 2. Point Cloud Library (PCL)

Installing the ROS2 full desktop version, also installs the PCL. However, if another ROS version present in you distribution, PCL may have to be installed separately. Make sure the PCL is installed:

```sh
sudo apt install libpcl-dev
```

#### 3. ALFA framework

We recommend you to have a folder named 'ALFA' anywhere you like and keep inside all related software and tools. Clone all repositories inside the local 'ALFA' repository. You need the git tool installed.

```sh
sudo apt install git
```
```sh
git clone -b main https://github.com/alfa-project/alfa-framework.git --recurse-submodules ALFA && cd ALFA
```

#### 4. Create a ROS Workspace
To use the Desktop version of ALFA, only a few more steps are required. First, create a workspace for working with ALFA packages without interfering with the existing default ROS2 workspace. To know more about creating a workspace check [ROS2 documentation](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html). This will include all ROS2-related software provided by ALFA.

```sh
mkdir -p ros2_ws/src && cd ros2_ws/src
```

Then, link the provided ALFA ROS2 packages to the new ROS2 workspace:

- **alfa_node** from [alfa-node](https://github.com/alfa-project/alfa-node)

```sh
ln -s ../../ALFA/alfa-framework/alfa-node alfa_node
```

- **alfa_msg** from [alfa-messages](https://github.com/alfa-project/alfa-messages)

```sh
ln -s ../../ALFA/alfa-framework/alfa-messages alfa_msg
```

- **software alfa-extensions** from [alfa-extensions](https://github.com/alfa-project/alfa-extensions/)

```sh
ln -s ../../ALFA/alfa-framework/alfa-extensions alfa_ext
```

- **alfa-monitor** from [alfa-monitor](https://github.com/alfa-project/alfa-monitor/)

```sh
ln -s ../../ALFA/alfa-framework/alfa-monitor/ alfa_monitor
```


## Compile ALFA extensions

ALFA software extensions are pieces of software written in C/C++ with ROS and ALFA-node dependencies. Therefore, compile them for desktop usage requires *colcon* within the ros2 workspace. Make sure that the packages alfa_node, alfa_msg and at least one extension (in this case we included the dummy extension) are inside your src folder and then build the workspace with the following commands:

```sh
cd ALFA/ros2_ws 
```

```sh
colcon build 
```

The following output should appear (if other packages are present they will also build and the output will include them):

```sh
Starting >>> alfa_msg
Finished <<< alfa_msg [0.61s]                     
Starting >>> alfa_node
Finished <<< alfa_node [0.18s]                
Starting >>> alfa_monitor
Starting >>> ext_distance_filter
Starting >>> ext_dummy
Finished <<< ext_distance_filter [0.15s]                                                
Finished <<< ext_dummy [0.15s]
Finished <<< alfa_monitor [0.32s]                    

Summary: 5 packages finished [1.36s]
```

Source the workspace environment to run the extensions:

```sh
source ./install/setup.bash 
```

- ##### To run an Extension, check the [ALFA extensions](https://github.com/alfa-project/alfa-extensions) repository.
- ##### To run the ALFA Monitor, check the [ALFA Monitor](https://github.com/alfa-project/alfa-monitor) repository.



# Xilinx Zynq UltraScale+ MPSoC ZCU104
---

<b>Available Soon!</b>