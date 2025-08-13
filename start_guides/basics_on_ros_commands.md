# basics on ros

for a better understanding on how commands actually work (this will clear up some of the weird-ness and headacks) see [how Linux's commands work](./linux_underview.md)

## ros core
```bash
roscore
```
roscore is a the program that runs on the primary commander computer it is responsible for organizing, deciding and informing all nodes about what other nodes are publishing on what topics so that they can connect to each other

all nodes (including all ros commands) use an environment variable to know where the core is. The name of this variable is ROS_MASTER_URI. It can be set by running the following command

```bash
export ROS_MASTER_URI=http://ari-20c:11311
```

this will set it appropriately for working on the robot.

You can see what it is set to by running
```
echo ${ROS_MASTER_URI}
```

## rostopic 

rostopic will is used for getting information about all topics that have been subscribed or published to

```bash 
rostopic list
```
will give you a list of the names of all subscribed/published topics 
you can then get some information about that topic by runing
```bash
rostopic info /rosout
```
replace “/rosout” with the name of the topic you wish to use

first is “Type:” this is the name of the ros message format that this topic uses

second is “Publishers:” which is a list of the names of the nodes that are publishing too this topic

last is “Subscribers:” which is a list of the names of the nodes that are subscribe (listening to).

```bash
rostopic echo /rosout
```
will out put a mostly human readable (if possibly for that msg type) version of any new updates on that topic

## rosmsg
rosmsg can be used to get info on the format of a specific ros message type


```bash
rosmsg list
```
gives a list of all ros meagse types that it knows about

```bash
rosmsg show sensor_msgs/Image
```
will give the ros message definition for a given massge type (in this case “sensor_msgs/Image”)

## rosnode
will give you info about runing ros nodes 
```bash 
rosnode list
```
will give you a list of all running nodes 

```bash 
rosnode info /jetbot_camera
```
will give you info on a running rosnode such as what topics it is publishing/subscribing too.

# tips 

use grep on the different ros list commands to narrow down the output e.g
```bash
rostopic list | grep camra
```
replace camra with the text you are looking for.

