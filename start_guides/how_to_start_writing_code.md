## how to start writing code

related article:
  https://wiki.ros.org/catkin/Tutorials/create_a_workspace
  https://wiki.ros.org/catkin/Tutorials/CreatingPackage

to start making code that works with ros you have to create what's called a catkin workspace. catkin the build/compilation tool used by ros1 and a workspace is a folder that has project setup environment like a visual studio project file.

start I recommend using a pal development VM witch as all the tools we will be using with the right versions already set-up for us
for you can work directly off the robot but will obviously have to have accesses to the robot to develop and test in that case

I also recommend that you read through this whole explanation before actually trying to follow it

to create a new catkin workspace it is easiest to simply create a folder with a src folder in it

```bash
mkdir my_cool_ws
mkdir my_cool_ws/src
```
in this example the folder my_cool_ws is what catkin calls the "root of the workspace" basically the top dir that catkin likes to run from
to compilation and setup the workspace.
the src folder is mostly here for later but is required to let catkin know that its a valid workspace.
you can now run catkin_make while in the "root of the workspace" to set it up

```bash
catkin_make
```

then to make a "package" basically a single propose program navigate into the src directory from the root of the workspace then you can use "catkin_make_pkg"
witch has the following prams
catkin_make_pkg [new package name] [library name 1] [library name 2] [library name 3] ...

e.g

```bash 
catkin_make_pkg my_new_pkg rospy roscpp std_msgs
```

this will make a package folder that contains the code for your program(or programs) that can be ether be a compiled executable made in c++ or a script in python or a combination of the 2
note that you need to add roscpp to the catkin_make_pkg to use c++ and rospy for python

std_msgs is the collection of standard message types for robotics e.g strings, images, positions and rotations , point clouds

then is will create a folder of the same name in your my_cool_ws/src folder being my_cool_ws/src/my_new_pkg. my_cool_ws/src/my_new_pkg it self will have a src folder. so the over all file tree should looks like this.
```
my_cool_ws
├────src
│		└──my_new_pkg
│				└─src
```
or if you have run catkin

```
my_cool_ws
├────build
│		└─...
├────devek
│		└─...
├────src
│		├──my_new_pkg
│				└─src
│		└──CMakeList.txt
```

the project setup we use in the code repo already has a workspace set up so I recommend making new packages in the workspace of the central repo

