# Technical Documentation Hub

Developer-focused documentation for advanced features, APIs, and system internals.

## Overview

This technical track contains complete implementation details, API references, configuration guides, and architectural information for all ARI custom features and integrations.

**Audience**: Developers, system administrators, integration engineers, researchers

---

## Feature Documentation

### 🎥 ARI Selfie-Cam System

Dual-camera setup with admin phone control and auto-upload to remote displays.

| Document | Purpose |
|----------|---------|
| [Setup Guide](../../ARI-Selfie-Cam-Setup.md) | Installation, hardware requirements, configuration |
| [User Guide](../../ARI-Selfie-Cam-User-Guide.md) | Operator instructions, workflows |
| [Admin Control](../../ARI-Selfie-Cam-Admin-Control.md) | Phone app interface, control options |
| [API Reference](../../ARI-Selfie-Cam-API.md) | HTTP endpoints, request/response formats |
| [Configuration](../../ARI-Selfie-Cam-Configuration.md) | Settings, tuning parameters, customization |
| [Architecture](../../ARI-Selfie-Cam-Architecture.md) | System design, data flow, camera integration |
| [Troubleshooting](../../ARI-Selfie-Cam-Troubleshooting.md) | Problem solving, diagnostic procedures |
| [Index](../../ARI-Selfie-Cam-Index.md) | Navigation hub, quick-start paths |

**Tech Stack**: Python, Flask, CSI ribbon, UVC USB, multipart HTTP  
**Key Features**: Dual cameras, admin control, auto-upload, photo capture  
**Integration**: Works with Remote Slideshow for auto-display

---

### 📸 Remote Slideshow

Flask server on separate Pi receiving uploads and displaying slideshows.

| Document | Purpose |
|----------|---------|
| [Setup Guide](../../Remote-Slideshow-Setup.md) | Hardware, installation, network configuration |
| [User Guide](../../Remote-Slideshow-User-Guide.md) | Operating instructions, manual uploads |
| [Integration Guide](../../Remote-Slideshow-Selfie-Cam-Integration.md) | Auto-upload setup, workflow documentation |
| [API Reference](../../Remote-Slideshow-API.md) | Upload endpoints, control commands, media access |
| [Configuration](../../Remote-Slideshow-Configuration.md) | Server settings, display options, storage management |
| [Architecture](../../Remote-Slideshow-Architecture.md) | System design, request flows, cleanup mechanisms |
| [Troubleshooting](../../Remote-Slideshow-Troubleshooting.md) | Network issues, display problems, storage errors |
| [Index](../../Remote-Slideshow-Index.md) | Navigation hub, setup checklists |

**Tech Stack**: Python, Flask, Raspberry Pi, JavaScript, HTTP multipart  
**Key Features**: Auto-upload, slideshow display, real-time updates, cleanup  
**Integration**: Primary display for Selfie-Cam captures

---

### 🎮 Gandalf LLM Security Game

ROS-integrated interactive security game calling Lakera API.

| Document | Purpose |
|----------|---------|
| [Getting Started](../../Gandalf-LLM-Game-Getting-Started.md) | Overview, game mechanics, learning outcomes |
| [Operator Guide](../../Gandalf-LLM-Game-Operator-Guide.md) | Running sessions, facilitating gameplay |
| [Setup Guide](../../Gandalf-LLM-Game-Setup.md) | Installation, dependencies, ROS configuration |
| [API Reference](../../Gandalf-LLM-Game-API.md) | Game endpoints, difficulty levels, state management |
| [Configuration](../../Gandalf-LLM-Game-Configuration.md) | Difficulty settings, scoring, Lakera API keys |
| [Architecture](../../Gandalf-LLM-Game-Architecture.md) | ROS integration, AI flow, frontend architecture |
| [Troubleshooting](../../Gandalf-LLM-Game-Troubleshooting.md) | API errors, ROS issues, gameplay problems |
| [Index](../../Gandalf-LLM-Game-Index.md) | Navigation hub, configuration checklists |

**Tech Stack**: Python, ROS, Lakera API, Web Frontend, WebSocket  
**Key Features**: 12 difficulty levels, prompt injection teaching, real-time scoring  
**Integration**: Runs on ARI via web interface or ROS topics

---

## Missing Documentation (TODO)

### ⚠️ Slideshow JSON Format

**Status**: NOT DOCUMENTED  
**Location**: On-board ARI robot feature (not Pi-based)  
**Purpose**: Custom JSON schema for defining slideshows with transitions, timings, layouts  
**Needed By**: Users creating custom presentation formats  
**TODO**: Create documentation reverse-engineering from codebase

### ⚠️ Modified Network Stack

**Status**: NOT DOCUMENTED  
**Location**: Custom modifications to ARI OS networking layer  
**Purpose**: Custom networking behavior (specific modifications unknown)  
**Needed By**: Developers integrating with ARI network  
**TODO**: Document modifications and rationale

---

## System Integration Diagrams

### Complete System Flow

```
┌─────────────────────────────────────────────────────────────┐
│                    User Level                               │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  [Event Organizer]  [Educator]  [Researcher]  [Operator]   │
│         │                │             │            │       │
└─────────────────────────────────────────────────────────────┘
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
   ┌────▼────┐      ┌────▼────┐      ┌────▼────┐
   │Selfie   │      │Slideshow │      │ Gandalf │
   │ Cam     │      │ System   │      │ Game    │
   │ Admin   │      │ (Pi)     │      │ (ROS)   │
   └────┬────┘      └────┬────┘      └────┬────┘
        │                │                │
        └────────────────┼────────────────┘
                         │
        ┌────────────────▼────────────────┐
        │      ARI Robot Platform         │
        ├─────────────────────────────────┤
        │ • Movement (ROS Nav Stack)      │
        │ • Audio (TTS/ASR)               │
        │ • Display (Touchscreen)         │
        │ • Network (Modified Stack)      │
        │ • Slideshow JSON (Custom)       │
        └─────────────────────────────────┘
```

### Data Flow: Photo Capture to Display

```
[Selfie-Cam App] ──(capture photo)──> [ARI Robot]
                                           │
                                      [Take photo]
                                           │
                    ┌──────────────────────▼──────────────────────┐
                    │   Auto-Upload via HTTP multipart/form-data  │
                    └──────────────────────┬──────────────────────┘
                                           │
                                    [Slideshow Server on Pi]
                                           │
                    ┌──────────────────────▼──────────────────────┐
                    │  Store photo in slideshow_images/            │
                    │  Update display immediately                  │
                    └──────────────────────┬──────────────────────┘
                                           │
                                    [Remote Display]
                                           │
                                    [Photo appears]
                                      on big screen
```

### API Architecture

```
[Client] ──(REST/HTTP)──> [ARI/Pi Servers]
                              │
                              ├─ /api/v1/upload (POST photos)
                              ├─ /api/v1/control (slideshow commands)
                              ├─ /api/v1/images (list photos)
                              ├─ /api/v1/game (Gandalf game state)
                              └─ /api/v1/status (system status)
```

---

## Configuration Quick Reference

### Selfie-Cam Config
- **Host/Port**: Default localhost:5000
- **Camera 1 (CSI)**: `/dev/video0` (ribbon camera)
- **Camera 2 (USB)**: `/dev/video1` (USB camera)
- **Upload URL**: Set in config for slideshow server
- **Photo Quality**: JPEG, 1920x1080 default

### Slideshow Server Config
- **Host/Port**: Default 0.0.0.0:5001
- **Storage**: `./slideshow_images/` directory
- **MAX_IMAGES**: 100 (configurable)
- **Display Duration**: 3-5 seconds (configurable)
- **Cleanup**: Auto-delete oldest when storage full

### Gandalf Game Config
- **ROS Topic**: `/gandalf/game_state` (subscribe)
- **Lakera API**: https://gandalf.lakera.ai/ (external)
- **Difficulty Levels**: 12 (hardcoded)
- **Frontend**: Web browser on ARI touchscreen
- **Scoring**: Points per successful evasion

---

## Development Setup

### Prerequisites
- Python 3.8+
- ROS (for Gandalf integration)
- Raspberry Pi OS or Ubuntu
- Flask 2.0+
- Requests library

### Quick Start
```bash
# Clone repositories
git clone <selfie-cam-repo>
git clone <slideshow-repo>
git clone <gandalf-repo>

# Install dependencies
pip install -r requirements.txt

# Configure servers
cp config.example.json config.json
# Edit config.json with your settings

# Start services
python selfie_cam/app.py &
python slideshow/app.py &
ros launch gandalf gandalf.launch &
```

---

## API Reference Summary

### Selfie-Cam Endpoints
- `POST /api/capture` - Trigger photo capture
- `GET /api/photo/<id>` - Retrieve photo by ID
- `POST /api/speak` - Make ARI speak text
- `GET /api/camera_feed` - Get live camera stream

### Slideshow Endpoints
- `POST /upload` - Upload photo to slideshow
- `GET /control?action=<action>` - Control slideshow playback
- `GET /images` - List available images
- `DELETE /image/<filename>` - Remove image

### Gandalf Endpoints
- `GET /game/state` - Current game state
- `POST /game/attempt` - Submit guess/attack
- `POST /game/level/<level>` - Start specific difficulty
- `GET /game/score` - Player score

---

## Architecture Decisions

### Why Separate Pi for Slideshow?
- ✅ Decouples display from robot operation
- ✅ Allows multi-location event setups
- ✅ Better scalability (can run on any Pi)
- ✅ Simpler network topology

### Why HTTP for Upload?
- ✅ Simple, standard protocol
- ✅ Works over WiFi reliably
- ✅ No custom drivers needed
- ✅ Easy to debug and monitor

### Why Gandalf Integrates with ROS?
- ✅ Leverages existing ARI ROS stack
- ✅ Standard robotics architecture
- ✅ Clean topic-based communication
- ✅ Extensible for future features

---

## Performance Considerations

### Network
- **Bandwidth**: ~2-5 Mbps for photo uploads
- **Latency**: <200ms typical for display update
- **WiFi**: 5GHz band recommended for reliability

### Storage
- **Photo Size**: ~500 KB JPEG average
- **100 Photos**: ~50 MB total
- **Auto-cleanup**: Remove oldest when threshold hit

### CPU/Memory
- **Slideshow Server**: ~50-100 MB RAM typical
- **Selfie-Cam App**: ~150-200 MB RAM
- **Gandalf Game**: ~200-300 MB RAM (with model)

---

## Security Considerations

### WiFi
- ✅ Use WPA2/WPA3 encryption
- ✅ Strong password recommended
- ✅ Isolate network if sensitive data

### API
- ⚠️ No authentication by default (assumes trusted network)
- ⚠️ Consider API key for production deployments
- ⚠️ Validate all inputs

### Data
- ✅ Photos auto-deleted per retention policy
- ⚠️ Consider encryption for stored media
- ⚠️ GDPR considerations for captured photos

---

## Troubleshooting Commands

### Check Service Status
```bash
# Selfie-Cam
curl http://localhost:5000/api/status

# Slideshow
curl http://localhost:5001/status

# Gandalf
rostopic list | grep gandalf
```

### View Logs
```bash
# Check Flask logs
journalctl -u selfie-cam -f

# Check slideshow server
tail -f slideshow/logs/app.log
```

### Network Diagnostics
```bash
# Check connectivity
ping <slideshow_server_ip>

# Check port availability
netstat -an | grep 5001

# Verify WiFi signal
iwconfig wlan0
```

---

## Common Integration Patterns

### Pattern 1: Full Event Setup
```
Selfie-Cam (on ARI) ──> Slideshow Server ──> Big Screen
     ↓
  Admin controls via phone app
```

### Pattern 2: Education Setup
```
Gandalf Game (on ARI touchscreen)
     ↓
Students interact via touchscreen or web browser
```

### Pattern 3: Multi-Location Event
```
ARI moves through venue ──> Multiple slideshow displays
                              in different rooms
```

---

## Next Steps for Developers

1. **Start with Setup Guides** for each feature
2. **Review API Documentation** for integration
3. **Check Architecture Diagrams** for design overview
4. **Study Configuration** for customization options
5. **Run Troubleshooting Checklist** before deployment

---

## Additional Resources

- **PAL ARI Official Docs**: https://docs.pal-robotics.com/ari/legacy/
- **Lakera Gandalf Game**: https://gandalf.lakera.ai/
- **ROS Documentation**: https://www.ros.org/
- **Flask Documentation**: https://flask.palletsprojects.com/
- **Raspberry Pi Docs**: https://www.raspberrypi.org/documentation/

---

## Support & Reporting

### Bug Reports
- Provide detailed logs
- Include reproduction steps
- Specify hardware/software versions

### Feature Requests
- Describe use case
- Propose solution
- Link to related features

### Questions
- Check FAQ and troubleshooting first
- Review similar issues in documentation
- Search code comments for implementation details

---

**Level**: Advanced  
**Audience**: Developers, DevOps, System Administrators, Integration Engineers  
**Last Updated**: 2025  
**For Non-Technical Users**: See [Getting Started Documentation](../getting-started/)
