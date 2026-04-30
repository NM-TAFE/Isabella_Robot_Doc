# Isabella Robot Overview

Isabella Robot is an advanced mobile platform designed for social interaction, perception, and autonomous navigation.

## Key Features

- **Mobile Base**: Durable navigation and movement capabilities
- **Perception**: Depth camera, LiDAR, and social perception systems
- **Voice Interaction**: Respeaker microphone array for audio processing
- **Intelligent Core**: LLM-based decision making and intent recognition
- **ROS Integration**: Full Robot Operating System support for extensibility

## Architecture Highlights

- Modular software design using ROS nodes and services
- Persistent filesystem for data and learning
- Network communication via Docker containerization
- Real-time transform (TF) system for coordinate management

## Quick Start

To get started with Isabella:

1. Review the [hardware setup](../hardware/depth-camera.md)
2. Install required software dependencies
3. Configure networking and Docker
4. Run the test suite
5. Explore the ROS nodes and services

## Project Structure

```
Isabella_Robot_Doc/
├── docs/
│   ├── hardware/
│   ├── software/
│   ├── architecture/
│   └── networking/
├── mkdocs.yml
└── zensical.toml
```

See the sidebar navigation for detailed documentation on each system.
