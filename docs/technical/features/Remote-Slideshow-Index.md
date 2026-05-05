# Remote Slideshow Documentation Index

## Complete Documentation Set

Welcome to the comprehensive documentation for the Remote Slideshow system. This index provides quick access to all documentation resources.

---

## 📚 Core Documentation

### [1. Setup & Installation Guide](Remote-Slideshow-Setup.md)
**For**: System administrators, first-time installers

**Covers**:
- Hardware requirements (Raspberry Pi, display, network)
- Software prerequisites and installation
- Port and network configuration
- Auto-start with systemd
- Verification checklist
- Troubleshooting installation issues

**Start here if**: Setting up the system for the first time

---

### [2. User Guide](Remote-Slideshow-User-Guide.md)
**For**: End users, event organizers, system operators

**Covers**:
- Accessing the slideshow from different devices
- Uploading images (web and command-line)
- Viewing image gallery
- Downloading and managing files
- Keyboard controls and display tips
- Best practices for events

**Start here if**: Using or managing the slideshow system

---

### [3. Selfie-Cam Integration Guide](Remote-Slideshow-Selfie-Cam-Integration.md)
**For**: System integrators, event coordinators

**Covers**:
- Architecture of integrated system
- Configuration for selfie-cam to slideshow
- Auto-upload behavior
- Control commands for immediate display
- Event workflows (setup, during, cleanup)
- Integration troubleshooting

**Start here if**: Using slideshow with ARI Selfie-Cam system

---

### [4. API Reference](Remote-Slideshow-API.md)
**For**: Developers, system integrators

**Covers**:
- All endpoint documentation
- Image upload endpoint
- Control commands (pause, play, show_new)
- Image management endpoints
- Server status endpoint
- cURL, Python, and JavaScript examples
- Response formats and error codes

**Start here if**: Building integrations or using API directly

---

### [5. Configuration Reference](Remote-Slideshow-Configuration.md)
**For**: Administrators, system customizers

**Covers**:
- Flask server settings (host, port)
- Storage configuration (folder, MAX_IMAGES)
- Image format settings
- Performance tuning
- Security considerations
- Environment variables

**Start here if**: Customizing system behavior or tuning performance

---

### [6. Architecture & Design](Remote-Slideshow-Architecture.md)
**For**: Developers, system architects

**Covers**:
- System architecture overview
- Component breakdown (Flask, storage, display)
- Data flow diagrams (upload, display, control)
- Technology stack
- Performance characteristics
- Threading model
- Future improvements

**Start here if**: Understanding internal system design

---

### [7. Troubleshooting Guide](Remote-Slideshow-Troubleshooting.md)
**For**: System operators, troubleshooters

**Covers**:
- Quick troubleshooting matrix
- Server startup issues
- Network and access problems
- Upload failures
- Storage and performance issues
- Display problems
- Integration issues
- Debug information collection

**Start here if**: Something isn't working

---

## 🎯 Quick Start Paths

### Path 1: "I'm Installing This for the First Time"
1. Read: [Setup Guide](Remote-Slideshow-Setup.md)
2. Reference: [Configuration Reference](Remote-Slideshow-Configuration.md)
3. Test: [User Guide - Accessing](Remote-Slideshow-User-Guide.md#getting-started)
4. Troubleshoot: [Troubleshooting Guide](Remote-Slideshow-Troubleshooting.md) if needed

### Path 2: "I'm Using This at an Event"
1. Quick review: [User Guide](Remote-Slideshow-User-Guide.md#system-operator-guide)
2. Upload workflow: [User Guide - Upload](Remote-Slideshow-User-Guide.md#uploading-images)
3. Integration: [Integration Guide](Remote-Slideshow-Selfie-Cam-Integration.md#event-workflow) if using with selfie-cam
4. Troubleshoot: [Troubleshooting Guide](Remote-Slideshow-Troubleshooting.md) if issues arise

### Path 3: "I'm Integrating with Selfie-Cam"
1. Reference: [Integration Guide](Remote-Slideshow-Selfie-Cam-Integration.md)
2. Understand: [Architecture - Integration Points](Remote-Slideshow-Architecture.md#integration-points)
3. Configuration: [Configuration Reference](Remote-Slideshow-Configuration.md)
4. Test: [Integration Guide - Verification](Remote-Slideshow-Selfie-Cam-Integration.md#step-3-verify-integration)

### Path 4: "Something's Broken"
1. Start: [Troubleshooting - Quick Matrix](Remote-Slideshow-Troubleshooting.md#quick-troubleshooting-matrix)
2. Detail: Find specific issue section
3. Reference: Link to relevant configuration/setup doc
4. Debug: Collect information from troubleshooting guide

---

## 📖 Documentation by Topic

### Getting Started
- [Setup Guide](Remote-Slideshow-Setup.md) - Installation
- [User Guide - Getting Started](Remote-Slideshow-User-Guide.md#getting-started) - Basic access

### Using the System
- [User Guide](Remote-Slideshow-User-Guide.md) - Complete user instructions
- [Keyboard Controls](Remote-Slideshow-User-Guide.md#keyboard-controls)
- [Uploading Images](Remote-Slideshow-User-Guide.md#uploading-images)
- [Gallery Management](Remote-Slideshow-User-Guide.md#managing-gallery)

### System Administration
- [Configuration Reference](Remote-Slideshow-Configuration.md) - All settings
- [Storage Management](Remote-Slideshow-User-Guide.md#monitoring-storage)
- [Performance Tuning](Remote-Slideshow-Configuration.md#performance-tuning)

### API & Integration
- [API Reference](Remote-Slideshow-API.md) - All endpoints
- [Integration Guide](Remote-Slideshow-Selfie-Cam-Integration.md) - Selfie-cam integration
- [Architecture - Integration](Remote-Slideshow-Architecture.md#integration-points)

### Technical Details
- [Architecture & Design](Remote-Slideshow-Architecture.md)
- [Configuration Reference](Remote-Slideshow-Configuration.md)
- [API Reference](Remote-Slideshow-API.md)

### Troubleshooting
- [Troubleshooting Guide](Remote-Slideshow-Troubleshooting.md)
- [Quick Matrix](Remote-Slideshow-Troubleshooting.md#quick-troubleshooting-matrix)
- [Integration Issues](Remote-Slideshow-Troubleshooting.md#integration-troubleshooting)

---

## 🔍 Find Documentation By Topic

### Installation & Setup
- [Setup - Hardware Requirements](Remote-Slideshow-Setup.md#hardware-requirements)
- [Setup - Installation Steps](Remote-Slideshow-Setup.md#installation-steps)
- [Setup - Auto-Start Configuration](Remote-Slideshow-Setup.md#auto-start-on-boot-optional)
- [Setup - Troubleshooting](Remote-Slideshow-Setup.md#troubleshooting-installation)

### Network & Access
- [Setup - Network Configuration](Remote-Slideshow-Setup.md#network-configuration)
- [Troubleshooting - Network Issues](Remote-Slideshow-Troubleshooting.md#network--access-issues)
- [Integration - Network Diagram](Remote-Slideshow-Selfie-Cam-Integration.md#network-diagram)

### Image Management
- [User Guide - Uploading](Remote-Slideshow-User-Guide.md#uploading-images)
- [User Guide - Gallery Management](Remote-Slideshow-User-Guide.md#managing-gallery)
- [API - Upload Endpoint](Remote-Slideshow-API.md#upload-image)
- [API - Image Management](Remote-Slideshow-API.md#image-upload--management)

### Slideshow Display
- [User Guide - Viewing Slideshow](Remote-Slideshow-User-Guide.md#viewing-the-slideshow)
- [API - Display Control](Remote-Slideshow-API.md#slideshow-control)
- [Architecture - Display Flow](Remote-Slideshow-Architecture.md#slideshow-display-flow)

### Integration & Automation
- [Integration Guide](Remote-Slideshow-Selfie-Cam-Integration.md) (complete)
- [Integration - Auto-Upload](Remote-Slideshow-Selfie-Cam-Integration.md#auto-upload-behavior)
- [Integration - Event Workflow](Remote-Selfie-Cam-Integration.md#event-workflow)

### Configuration & Tuning
- [Configuration - Port](Remote-Slideshow-Configuration.md#port-configuration)
- [Configuration - Storage](Remote-Slideshow-Configuration.md#storage-configuration)
- [Configuration - Performance](Remote-Slideshow-Configuration.md#performance-tuning)
- [Configuration - Security](Remote-Slideshow-Configuration.md#security-configuration)

### Troubleshooting & Support
- [Troubleshooting - Quick Matrix](Remote-Slideshow-Troubleshooting.md#quick-troubleshooting-matrix)
- [Troubleshooting - Server Issues](Remote-Slideshow-Troubleshooting.md#server-issues)
- [Troubleshooting - Upload Issues](Remote-Slideshow-Troubleshooting.md#upload-issues)
- [Troubleshooting - Display Issues](Remote-Slideshow-Troubleshooting.md#display-issues)

### Performance & Optimization
- [Configuration - Performance Tuning](Remote-Slideshow-Configuration.md#performance-tuning)
- [Troubleshooting - Performance](Remote-Slideshow-Troubleshooting.md#slow-performance)
- [Architecture - Performance](Remote-Slideshow-Architecture.md#performance-characteristics)

---

## 🛠️ Technical Reference

### API Endpoints Quick Reference
- `GET /` - Main slideshow display
- `GET /admin` - Admin gallery interface
- `POST /upload` - Upload image
- `GET /images` - List all images
- `GET /image/<filename>` - Get specific image
- `POST /delete/<filename>` - Delete image
- `POST /clear` - Clear all images
- `POST /control` - Control slideshow (pause, play, show_new)
- `GET /status` - Server status

See [API Reference](Remote-Slideshow-API.md) for full details.

### Configuration Variables Quick Reference
- `PORT = 8080` - Server port
- `UPLOAD_FOLDER = Path('./slideshow_images')` - Storage location
- `MAX_IMAGES = 1000` - Image limit before cleanup
- `ALLOWED_EXTENSIONS` - Allowed file types

See [Configuration Reference](Remote-Slideshow-Configuration.md) for full details.

---

## 📋 Checklists

### Pre-Event Checklist
- [ ] Remote slideshow server installed and tested
- [ ] Connected to display via HDMI
- [ ] Network connectivity verified
- [ ] Firewall allows port 8080
- [ ] Disk space available (check `/status`)
- [ ] Browser at correct URL
- [ ] Gallery cleared from previous event (`/clear`)
- [ ] Selfie-cam configured for upload (if using)
- [ ] Test capture → display workflow
- [ ] Backup plan if system fails

### Event Checklist
- [ ] All systems running and responsive
- [ ] Display in fullscreen mode
- [ ] Test upload from selfie-cam
- [ ] Monitor gallery size during event
- [ ] Check for network issues
- [ ] Have troubleshooting guide ready
- [ ] Note any errors or issues

### Post-Event Checklist
- [ ] Download important images (backup)
- [ ] Clear gallery for next event (`/clear`)
- [ ] Shut down servers gracefully
- [ ] Review any issues that occurred
- [ ] Archive images if needed

### Troubleshooting Checklist
- [ ] Is server running? (`curl http://localhost:8080`)
- [ ] Can access from phone/computer? (correct URL?)
- [ ] Are images uploaded? (`/status` shows count > 0)
- [ ] Is network working? (ping test)
- [ ] Is disk space available? (df -h)
- [ ] Any error messages? (check logs)
- [ ] Browser cache clear? (Ctrl+Shift+R)

---

## 🔗 Related Documentation

### ARI Selfie-Cam System
- [ARI Selfie-Cam Index](ARI-Selfie-Cam-Index.md)
- [Selfie-Cam Setup](ARI-Selfie-Cam-Setup.md)
- [Selfie-Cam Admin Control](ARI-Selfie-Cam-Admin-Control.md)

### External Resources
- **Flask Documentation**: https://flask.palletsprojects.com/
- **Raspberry Pi**: https://www.raspberrypi.com/documentation/
- **Python**: https://docs.python.org/3/

---

## 📞 Support

**If you can't find an answer**:

1. **Search this index** for keywords
2. **Check relevant guide** for step-by-step solutions
3. **Review troubleshooting** for common problems
4. **Collect debug info** from troubleshooting guide
5. **Contact administrator** with information

---

## 📝 Document Information

**Documentation Set Version**: 1.0
**Last Updated**: 2025-06-15
**Coverage**: Remote Slideshow v1.0

**Includes**:
- ✅ Setup & Installation Guide
- ✅ User Guide (end-user and operator)
- ✅ Selfie-Cam Integration Guide
- ✅ API Reference
- ✅ Configuration Reference
- ✅ Architecture & Design Overview
- ✅ Troubleshooting Guide
- ✅ This Documentation Index

**Next Steps**: Start with the [Setup Guide](Remote-Slideshow-Setup.md) or choose your path above.

---

**Navigation**: [Back to Main README](README.md) | [ARI Selfie-Cam Index](ARI-Selfie-Cam-Index.md)
