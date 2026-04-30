# tf

| info.           | Description |
| --------------- | ----------- |
| stock component | Yes         |
| contains speculation | Yes   |
| Location.       |       ??   |
| related artical | [ros wiki tf](https://wiki.ros.org/tf) [understanding ros transforms foxglove](https://foxglove.dev/blog/understanding-ros-transforms) |

## Description

the ari robot system uses the stock ros1 tf system for publishing and solving of all moving parts including its physical location relative to the world and the location of it arms and legs
tf uses a one way tree node graph to track the positions and update the positions of components
by "one way tree node graph" we mean each component has a parent child relation where a child node will never have a parent node higher up the tree be its child

this tree is encapsulated entirely  with in the /tf ros topic so don’t confuse any ros topics with the same name as being the topic witch publishes its location

the components/nodes in the tf tree are called "frames" they represent each components of the robot and other objects in its environment(even non-moving part like a fixed camera ).
frames are connected by transformations (position and rotation) relative to another frame as long as there are a chain of transformations between any 2 frames tf will be able to give you back the overall transformation between those 2 frames e.g

if there is a transformation from the world to the base and a transformations from the base to a hand then tf can give you the transformations from the world to the hand
this is like a reverse version of how the unity game engine’s game object tree works

!speculation: the top frame in our case is the "/map" or "/world" frame representing its actual real world and ground, it is published by some node relating to the pal_navigation_sm and pal_map_manager ros pagkges

the frame representing the robots relative to the world is either "base_link" or "base_footprint"


the way that way that the transformations between frames are updated are 

## Details

!speculation: - Uses /opt/pal/gallium/share/ari_2dnav publish and or reads /tf 
