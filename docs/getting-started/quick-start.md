# Quick Start Guide

Get Isabella Robot up and running in minutes.

## Prerequisites

- Ubuntu 20.04 or later
- Python 3.8+
- ROS Noetic or newer
- Docker and Docker Compose
- Git

## Installation Steps

### 1. Clone the Repository

```bash
git clone https://github.com/NM-TAFE/Isabella_Robot_Doc.git
cd Isabella_Robot_Doc
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Build Documentation Locally

```bash
mkdocs serve
```

Visit `http://localhost:8000` to view the documentation.

### 4. Build Static Site

```bash
mkdocs build
```

The static site will be generated in the `site/` directory.

## Running Tests

```bash
# Run the test suite
python -m pytest tests/
```

## Next Steps

- [Explore Hardware Documentation](../hardware/depth-camera.md)
- [Review Software Architecture](../software/ros-nodes-services.md)
- [Configure Networking](../networking/ports-networking.md)

## Getting Help

Check the [documentation index](../index.md) or visit the [GitHub repository](https://github.com/NM-TAFE/Isabella_Robot_Doc).
