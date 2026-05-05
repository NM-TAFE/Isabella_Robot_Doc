# Isabella Robot Documentation

Welcome to the comprehensive documentation for the **Isabella Robot** project. This wiki provides detailed information about hardware, software architecture, setup guides, and API reference.

## Quick Links

- **[Getting Started](#getting-started)** - New to Isabella? Start here
- **[Hardware](#hardware)** - Device specifications and setup
- **[Software](#software)** - System architecture and components
- **[Networking](#networking)** - Ports and Docker configuration
- **[Upgrades & Migration](#upgrades--migration)** - ARI 23.12 → 25.01 upgrade guide

## Getting Started

Get up and running with Isabella Robot:

- [Quick Start Guide](getting-started/overview.md)
- [Testing](getting-started/test.md)

## Hardware

Learn about Isabella's hardware components:

- [Depth Camera](hardware/Depth-Camera.md)
- [LiDAR Sensor](hardware/Lidar.md)
- [Docking Station](hardware/Docking-Station.md)
- [Respeaker Microphone Array](hardware/Respeaker.md)

## Software

Understand the software architecture and components:

- [ROS Nodes & Services](software/ROS-Nodes-and-Services.md)
- [LLM Server Integration](software/LLM-Server.md)
- [Localizer](software/Localizer.md)
- [Social Perception](software/Social-Perception.md)
- [Transform (TF) System](software/tf.md)

## Architecture

Deep dive into system design:

- [Intents System](architecture/Intents.md)
- [Filesystem & Persistence](architecture/Filesystem-and-Persistence.md)

## Networking

Network configuration and deployment:

- [Ports & Networking](networking/Ports-and-Networking.md)
- [Docker Setup](networking/docker.md)

## Upgrades & Migration

Prepare for ARI OS updates:

- [Overview](upgrades/overview.md) - What's changing from 23.12 to 25.01
- [Pre-Upgrade Checklist](upgrades/pre-upgrade-checklist.md) - Backup and verification steps
- [Upgrade Guide](upgrades/upgrade-guide.md) - Step-by-step installation instructions
- [Component Updates](upgrades/component-updates.md) - Isabella-specific impacts
- [Post-Upgrade Validation](upgrades/post-upgrade-validation.md) - Testing and verification

## Related Repositories

- **Isabella Robot Doc** (Public) - This wiki: [NM-TAFE/Isabella_Robot_Doc](https://github.com/NM-TAFE/Isabella_Robot_Doc)
- **ARI Selfie-Cam** (Public) - Phone admin control, selfie & interview capture: [prgrobots/ARI-selfie-cam](https://github.com/prgrobots/ARI-selfie-cam)
- **Remote Slideshow** (Public) - Pi-based remote photo display: [prgrobots/remote-slideshow](https://github.com/prgrobots/remote-slideshow)
- **Gandalf CLI ARI** (Public) - LLM security game: [prgrobots/gandalf_cli_ari](https://github.com/prgrobots/gandalf_cli_ari)
- **Rpi-ollama-LLM** (Private) - Live Mode implementation: [prgrobots/Rpi-ollama-LLM](https://github.com/prgrobots/Rpi-ollama-LLM)

## Support

For issues, questions, or contributions, visit the [GitHub repository](https://github.com/NM-TAFE/Isabella_Robot_Doc).
