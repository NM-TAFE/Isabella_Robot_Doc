# Social Perception

![Social Perception](../media/ARI-General-Social-Perception-image1.png)

| info.           | Description |
| --------------- | ----------- |
| stock component | Yes         |
| contains speculation | No   |
| Location.       | See ARI documentation |
| related artical | [PAL Social Perception Docs](https://docs.pal-robotics.com/sdk/24.09/social-perception.html) |

## Description

Social perception enables the ARI robot to detect, track, and interact with humans using vision and audio cues. It is a key part of human-robot interaction (HRI).

## Details

- Detects faces, bodies, and voices.
- Tracks multiple people in real time.
- Integrates with ARI's navigation and interaction systems.

related ros nodes inclued but may not be limeted to:
- /hri_face_detect
- /hri_face_identification
- [/hri_fullbody](./ros_nodes/hri_fullbody_node)
- /hri_overlay_throttle
- /hri_person_manager
- /hri_visualization

## troubleshooting

### Social perception not working
- Check camera and microphone functionality.
- Ensure relevant ROS nodes are running. 
