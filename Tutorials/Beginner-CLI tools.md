## Configuring environment

The core ROS 2 workspace is called the underlay.

Subsequent local workspaces are called overlays.

结合工作空间可以在不同版本ros上进行开发

使用前要先 source setup files

### 1. source the setup files

```shell
source /opt/ros/humble/setup.bash
```

### 2. Add sourcing to your shell startup script

```shell
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
```

### 3. Check environment variables

```shell
printenv | grep -i ROS
```

#### 3.1. The `ROS_DOMAI_ID` variable

DDS, ROS 2用于通信的默认中间件

让不同逻辑网络共享一个物理网络的主要机制称为域ID

默认情况下，所有ROS2节点都使用域ID 0

**短版域ID**

0-101之间

**长版域ID**

0-232

```shell
export ROS_DOMAIN_ID=<your_domain_id>
```



#### 3.2. The `ROS_LOCALHOST_ONLY` variable

是否限制只在本地

```shell
export ROS_LOCALHOST_ONLY=1
```

## Using `turtlesim` and `rqt`

`turtlesim`是一个轻量的模拟器

`rqt`是ROS 2的GUI

主要用于理解 `nodes`, `topics`, and 'services'的分离

### 1. Install `turtlesim`

```shell
sudo apt update

sudo apt install ros-humble-turtlesim
```

**Check that the package installed**

```shell
ros2 pkg executables turtlesim
```

返回下面的，说明安装ok

```shell
turtlesim draw_square
turtlesim mimic
turtlesim turtle_teleop_key
turtlesim turtlesim_node
```

### 2. Start `turtlesim`

启动命令

```shell
ros2 run turtlesim turtlesim_node
```

启动正常会有界面

### 3. Use `turtlesim`

打开一个新的终端，`source`一下

使用一个新的节点去控制第一个节点

```shell
ros2 run turtlesim turtle_teleop_key
```

使用下面的命令可以查看节点，以及与之相关的服务、主题和动作

```shell
ros2 node list
ros2 topic list
ros2 service list
ros2 action list
```

### 4. Install `rqt`

```shell
sudo apt update
sudo apt install ~nros-humble-rqt*
```



run

```shell
rqt
```



### 5. Use rqt

如果窗口是空的

`Plugins -> Services -> Service Caller`

#### 5.1. Try the spawn service

left-refresh

select `/spawn`

修改信息，然后

call

这个过程有点类似于实例化类

#### 5.2. Try the set_pen service

类似于修改属性

### 6. Remapping

修改节点控制的节点

### 7. Close `turtlesim`



## Understanding nodes

### The ROS 2 graph

每个节点负责一个用途，节点之间可以互相沟通

### Tasks

#### 1. ros2 run

```shell
ros2 run <package_name> <executable_name>
```

#### 2. ros2 node list

```shell
ros2 node list
```

##### 2.1. Remapping

打开一个新的模拟窗口，并且改变这个节点的名字

```shell
ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle
```

#### 3. ros2 node info

```shell
ros2 node info /my_turtle
```

## Understanding topics

### Background

ros2 把复杂的节点分成了许多模块化的节点。

一个节点可以发布任意数量的主题，同时订阅任意数量的主题

主题是两个接待你或者系统之间 数据传输的最重要的方式

### Tasks

#### 1. Setup

```shell
ros2 run turtlesim turtlesim_node
```

另一个终端

```shell
ros2 run turtlesim turtle_teleop_key
```

#### 2. `rqt_graph`

run

```
rqt_graph
```

or

`rqt`->`Plugins`->`Introspection`->`Node Graph`



#### 3. ros2 topic list

run

```shell
ros2 topic list
```

and run

```shell
ros2 topic list -t
```

#### 4. ros2 topic echo

use

```shell
ros2 topic echo <topic_name>
```

这个时候，会有一个新的节点订阅话题

#### 5. ros2 topic info

run

```shell
ros2 topic info /turtle1/cmd_vel
```

#### 6. ros2 interface show

发布和订阅必须发送和接收相同类型的通信信息

先通过`ros topic list -t`获得各主题的通信信息

然后可以查看`interface`

`ros2 interface show geometry_msgs/msg/Twist`

#### 7. ros2 topic hub

直接从命令行通过主题发布数据

格式

`ros2 topic pub <topic_name> <msg_type> '<args>'`

要用`YAML`格式

#### 8. ros2 topic hz

```shell
ros2 topic hz /turtle1/pose
```

#### 9. Clean up

`Ctrl+C`



































































