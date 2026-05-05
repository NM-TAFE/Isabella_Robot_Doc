# ARI Selfie-Cam Documentation Index

## Complete Documentation Set

Welcome to the comprehensive documentation for the ARI Selfie-Cam system. This index provides quick access to all documentation resources.

---

## 📚 Core Documentation

### [1. Setup & Installation Guide](ARI-Selfie-Cam-Setup.md)
**For**: System administrators, technicians, first-time installers

**Covers**:
- Hardware requirements (cameras, microphone, Pi)
- Software prerequisites and installation steps
- Camera and audio device configuration
- Auto-start configuration with systemd
- Verification checklist
- Installation troubleshooting

**Start here if**: You're setting up the system for the first time

---

### [2. User Guide](ARI-Selfie-Cam-User-Guide.md)
**For**: End users, event organizers, system operators

**Covers**:
- Accessing the application from different devices
- Taking selfies (step-by-step)
- Recording interviews with audio
- Viewing and managing gallery
- Admin panel overview
- Data privacy and file backup
- Tips for best results

**Start here if**: You're using the system at an event or workshop

---

### [3. Admin Control Features Guide](ARI-Selfie-Cam-Admin-Control.md)
**For**: Administrators, system operators, event coordinators

**Covers**:
- Admin panel layout and sections
- System status indicators
- Camera preview and controls
- Quick action buttons
- ARI robot integration (TTS and motion)
- Real-time feedback and status monitoring
- Workflow examples
- Mobile optimization tips

**Start here if**: You're managing the system and controlling the ARI robot

---

### [4. API Reference](ARI-Selfie-Cam-API.md)
**For**: Developers, system integrators, advanced users

**Covers**:
- All available endpoints (routes and API)
- Request/response formats
- HTTP status codes
- Page routes (HTML returns)
- Camera control API
- Admin API endpoints
- Gallery management API
- CORS headers
- Error handling
- Integration examples (cURL, JavaScript)

**Start here if**: You're integrating with other systems or building custom clients

---

### [5. Configuration Reference](ARI-Selfie-Cam-Configuration.md)
**For**: Administrators, system customizers

**Covers**:
- Flask server settings
- Storage configuration (gallery path)
- Camera configuration (resolution, indices)
- Audio device setup
- ARI robot URL and settings
- Performance tuning options
- Security considerations
- Environment variables
- Configuration validation

**Start here if**: You need to customize system behavior or optimize performance

---

### [6. Architecture & Design Overview](ARI-Selfie-Cam-Architecture.md)
**For**: Developers, system architects, technical reviewers

**Covers**:
- System architecture diagram
- Component overview (Flask, CameraManager, ARI integration)
- Data flow diagrams (selfie, interview, TTS, preview)
- Threading model
- Technology stack
- Request/response cycles
- Error handling strategy
- Performance characteristics
- Dependencies and startup sequence
- Future improvement ideas

**Start here if**: You need to understand how the system works internally

---

### [7. Troubleshooting Guide](ARI-Selfie-Cam-Troubleshooting.md)
**For**: System operators, troubleshooters, support staff

**Covers**:
- Quick troubleshooting matrix
- Camera issues (CSI, UVC, detection)
- Audio recording problems
- Network and connectivity issues
- Storage and performance issues
- ARI robot integration problems
- File permissions and crashes
- Performance optimization
- Debug information collection
- Escalation path

**Start here if**: Something isn't working and you need to fix it

---

## 🎯 Quick Start Paths

### Path 1: "I'm Installing This for the First Time"
1. Read: [Setup & Installation Guide](ARI-Selfie-Cam-Setup.md)
2. Reference: [Configuration Reference](ARI-Selfie-Cam-Configuration.md)
3. Test: [User Guide - Getting Started](ARI-Selfie-Cam-User-Guide.md#getting-started)
4. Troubleshoot: [Troubleshooting Guide](ARI-Selfie-Cam-Troubleshooting.md) if needed

### Path 2: "I'm Running an Event Today"
1. Quick review: [Admin Control Features](ARI-Selfie-Cam-Admin-Control.md#accessing-the-admin-panel)
2. Reference: [User Guide - Admin Guide](ARI-Selfie-Cam-User-Guide.md#administrator-guide)
3. Workflow: [Admin Control - Workflow Examples](ARI-Selfie-Cam-Admin-Control.md#admin-control-workflow-examples)
4. Troubleshoot: [Troubleshooting Guide](ARI-Selfie-Cam-Troubleshooting.md) if issues arise

### Path 3: "I'm Building an Integration"
1. Reference: [API Reference](ARI-Selfie-Cam-API.md)
2. Understand: [Architecture & Design](ARI-Selfie-Cam-Architecture.md)
3. Configuration: [Configuration Reference](ARI-Selfie-Cam-Configuration.md)
4. Debug: Troubleshooting Guide section on integration

### Path 4: "Something's Broken"
1. Start: [Troubleshooting Guide](ARI-Selfie-Cam-Troubleshooting.md#quick-troubleshooting-matrix)
2. Detail: Find specific issue section
3. Reference: Link to relevant configuration or setup doc
4. Escalate: Follow escalation path if needed

---

## 📖 Documentation by Use Case

### System Administrator
- [Setup Guide](ARI-Selfie-Cam-Setup.md) - Installation
- [Configuration Reference](ARI-Selfie-Cam-Configuration.md) - System configuration
- [Troubleshooting Guide](ARI-Selfie-Cam-Troubleshooting.md) - Problem resolution

### End User / Event Participant
- [User Guide - End User Section](ARI-Selfie-Cam-User-Guide.md#end-user-guide) - Taking selfies, interviews
- [User Guide - Getting Started](ARI-Selfie-Cam-User-Guide.md#getting-started) - Accessing application

### Event Coordinator / Operator
- [User Guide - Admin Guide](ARI-Selfie-Cam-User-Guide.md#administrator-guide) - System management
- [Admin Control Features](ARI-Selfie-Cam-Admin-Control.md) - Detailed controls
- [Admin Control - Workflows](ARI-Selfie-Cam-Admin-Control.md#admin-control-workflow-examples) - Real-world scenarios

### Developer / Integrator
- [API Reference](ARI-Selfie-Cam-API.md) - All endpoints
- [Architecture & Design](ARI-Selfie-Cam-Architecture.md) - Internal workings
- [Configuration Reference](ARI-Selfie-Cam-Configuration.md) - System customization

### Support / Troubleshooter
- [Troubleshooting Guide](ARI-Selfie-Cam-Troubleshooting.md) - All solutions
- All other guides for context and reference

---

## 🔍 Find Documentation By Topic

### Camera & Image Capture
- [Setup - Camera Hardware](ARI-Selfie-Cam-Setup.md#hardware-requirements)
- [Setup - Camera Configuration](ARI-Selfie-Cam-Setup.md#hardware-configuration)
- [Configuration - Camera Settings](ARI-Selfie-Cam-Configuration.md#camera-configuration)
- [User Guide - Taking Selfies](ARI-Selfie-Cam-User-Guide.md#taking-selfies)
- [Troubleshooting - Camera Issues](ARI-Selfie-Cam-Troubleshooting.md#camera-issues)
- [Architecture - Camera Integration](ARI-Selfie-Cam-Architecture.md#3-camera-hardware-integration)

### Audio & Recording
- [Setup - Audio Device Setup](ARI-Selfie-Cam-Setup.md#audio-device-configuration)
- [Configuration - Audio Settings](ARI-Selfie-Cam-Configuration.md#audio-configuration)
- [User Guide - Recording Interviews](ARI-Selfie-Cam-User-Guide.md#recording-interviews)
- [Troubleshooting - Audio Issues](ARI-Selfie-Cam-Troubleshooting.md#audio-recording-issues)

### ARI Robot Integration
- [Configuration - ARI Settings](ARI-Selfie-Cam-Configuration.md#ari-robot-configuration)
- [Admin Control - Robot Control](ARI-Selfie-Cam-Admin-Control.md#ari-robot-control-section)
- [API - ARI Endpoints](ARI-Selfie-Cam-API.md#ari-robot-integration)
- [Troubleshooting - ARI Issues](ARI-Selfie-Cam-Troubleshooting.md#ari-robot-integration-issues)
- [Architecture - ARI Integration](ARI-Selfie-Cam-Architecture.md#4-ari-robot-integration)

### Admin Panel & Controls
- [Admin Control Features](ARI-Selfie-Cam-Admin-Control.md) (comprehensive)
- [User Guide - Admin Panel](ARI-Selfie-Cam-User-Guide.md#administrator-guide)
- [API - Admin Endpoints](ARI-Selfie-Cam-API.md#admin-control-api)

### Gallery & File Management
- [User Guide - Viewing Gallery](ARI-Selfie-Cam-User-Guide.md#viewing-the-gallery)
- [Admin Control - Gallery Nav](ARI-Selfie-Cam-Admin-Control.md#gallery-navigation)
- [API - Gallery API](ARI-Selfie-Cam-API.md#gallery-management-api)
- [Troubleshooting - Storage Issues](ARI-Selfie-Cam-Troubleshooting.md#storage--performance-issues)

### Installation & Setup
- [Setup Guide](ARI-Selfie-Cam-Setup.md) (complete)
- [Configuration - Startup](ARI-Selfie-Cam-Configuration.md#configuration--startup)

### Troubleshooting & Support
- [Troubleshooting Guide](ARI-Selfie-Cam-Troubleshooting.md) (comprehensive)
- Individual sections link to setup and configuration for solutions

### Performance & Optimization
- [Configuration - Performance Tuning](ARI-Selfie-Cam-Configuration.md#performance-tuning)
- [Troubleshooting - Performance Issues](ARI-Selfie-Cam-Troubleshooting.md#performance-optimization)
- [Architecture - Performance Characteristics](ARI-Selfie-Cam-Architecture.md#performance-characteristics)

---

## 🛠️ Technical Reference

### Configuration Options
See: [Configuration Reference](ARI-Selfie-Cam-Configuration.md)

**Key Settings**:
- `GALLERY_PATH` - Where media files are stored
- `INTERVIEW_DURATION` - Max interview length
- `ARI_BASE_URL` - Robot server address
- Flask `host` and `port` - Server listening address

### API Endpoints
See: [API Reference](ARI-Selfie-Cam-API.md)

**Main Routes**:
- `GET /` - Home
- `GET /admin` - Admin panel
- `GET /selfie` - Selfie page
- `GET /interview` - Interview page
- `GET /gallery` - Public gallery

**API Endpoints**:
- `POST /api/take_selfie` - Capture selfie
- `POST /api/start_interview` - Start recording
- `POST /api/stop_interview` - Stop recording
- `POST /api/admin/ari_speak` - Text-to-speech
- `POST /api/admin/ari_motion` - Motion control

### System Architecture
See: [Architecture & Design](ARI-Selfie-Cam-Architecture.md)

**Components**:
- Flask web application
- CameraManager class
- Camera hardware (CSI, UVC)
- ARI Robot integration
- File system organization

---

## 📋 Checklist for Common Tasks

### Setting Up the System
- [ ] Read: [Setup Guide](ARI-Selfie-Cam-Setup.md)
- [ ] Gather: All required hardware
- [ ] Install: System dependencies
- [ ] Configure: Camera and audio devices
- [ ] Set: ARI_BASE_URL
- [ ] Test: All cameras and microphone
- [ ] Verify: All systems operational

### Running an Event
- [ ] Review: [Admin Control Guide](ARI-Selfie-Cam-Admin-Control.md)
- [ ] Test: Admin panel access
- [ ] Verify: All cameras working
- [ ] Test: ARI robot connectivity
- [ ] Monitor: Gallery space available
- [ ] Have backup: Plan if issues occur
- [ ] Backup: Media after event

### Troubleshooting an Issue
- [ ] Check: [Quick Matrix](ARI-Selfie-Cam-Troubleshooting.md#quick-troubleshooting-matrix)
- [ ] Find: Specific issue section
- [ ] Follow: Step-by-step solutions
- [ ] Test: Each solution
- [ ] Collect: Debug info if persists
- [ ] Escalate: If needed

---

## 🔗 Related Resources

### Integration with Remote Slideshow
The ARI Selfie-Cam system can automatically upload captured photos to a remote display system:
- [Remote Slideshow Documentation Index](Remote-Slideshow-Index.md) - Complete guide to the display system
- [Remote Slideshow Integration Guide](Remote-Slideshow-Selfie-Cam-Integration.md) - How to connect and configure
- [Remote Slideshow API Reference](Remote-Slideshow-API.md) - Technical integration details
- [Remote Slideshow Setup Guide](Remote-Slideshow-Setup.md) - Hardware and installation for display system

**Use Case**: Capture selfies or interviews on the admin phone → Automatically display on a remote monitor/TV for immediate viewing

---

### Source Code
- **ARI Selfie-Cam Repository**: https://github.com/NM-TAFE/ARI-selfie-cam
- **Isabella Robot Doc Repository**: https://github.com/NM-TAFE/Isabella_Robot_Doc

### External Documentation
- **Flask**: https://flask.palletsprojects.com/
- **picamera2**: https://github.com/raspberrypi/picamera2
- **Raspberry Pi**: https://www.raspberrypi.com/documentation/
- **ARI Robot**: [Contact system administrator]

---

## 📞 Support

If you can't find an answer in the documentation:

1. **Search this index** for keywords related to your issue
2. **Check relevant guide** for step-by-step solutions
3. **Review troubleshooting** for common problems
4. **Collect debug info** from troubleshooting guide
5. **Contact administrator** with information

---

## 📝 Document Information

**Documentation Set Version**: 1.0
**Last Updated**: 2025-06-15
**Coverage**: ARI Selfie-Cam v1.0

**Includes**:
- ✅ Setup & Installation Guide
- ✅ User Guide (end-user and admin)
- ✅ Admin Control Features Guide
- ✅ API Reference
- ✅ Configuration Reference
- ✅ Architecture & Design Overview
- ✅ Troubleshooting Guide
- ✅ This Documentation Index

---

**Navigation**: [Home Index](index.md) | [Back to Main README](README.md)
