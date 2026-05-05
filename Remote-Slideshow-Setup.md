# Remote Slideshow: Setup & Installation Guide

## Overview

The Remote Slideshow is a Flask-based server running on a Raspberry Pi that accepts image uploads via HTTP and displays them in a web-based slideshow. It can run standalone or as part of the ARI Selfie-Cam system for automatically displaying captured photos.

**Key Features**:
- Accept image uploads from any device on the network
- Web-based slideshow display with configurable image duration
- Remote control via API commands
- Automatic image cleanup (keeps latest 1000 images)
- Direct browser viewing or fullscreen display
- Support for common image formats (JPG, PNG, GIF, BMP, WebP)
- Lightweight, runs well on Raspberry Pi

## Hardware Requirements

### Computing Device
- **Raspberry Pi**: 3B or later (any model with WiFi/Ethernet)
- **MicroSD Card**: 16GB or larger
- **Power Supply**: 5V 2.5A USB-C or micro-USB

### Display Equipment
- **Monitor/TV**: Any HDMI display (required for slideshow viewing)
- **HDMI Cable**: To connect Pi to display
- **Display Mounting**: Optional but recommended for events

### Network
- Ethernet or WiFi connectivity
- Router or local network
- Same network as selfie-cam Pi (if integrated)

## Software Prerequisites

### Raspberry Pi OS Setup

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Python and dependencies
sudo apt install -y python3-pip python3-venv

# Install optional but recommended
sudo apt install -y git curl
```

## Installation Steps

### 1. Clone or Download Repository

```bash
# Navigate to home directory
cd ~

# Clone the remote-slideshow repository
git clone https://github.com/NM-TAFE/remote-slideshow.git
cd remote-slideshow
```

### 2. Create Virtual Environment

```bash
# Create virtual environment
python3 -m venv venv

# Activate it
source venv/bin/activate

# Verify activation (prompt should show (venv))
which python
```

### 3. Install Python Dependencies

```bash
# Install Flask and Pillow
pip install flask pillow requests

# Typical requirements:
# - Flask (web framework)
# - Pillow (image processing)
# - requests (optional, for client testing)
```

### 4. Create Image Directory

```bash
# Create directory for slideshow images
mkdir -p slideshow_images

# Verify creation
ls -la
# Should show: main.py, slideshow_images/, requirements.txt, etc.
```

### 5. Set Permissions

```bash
# Make main script executable
chmod +x main.py

# Ensure directory is writable
chmod 755 slideshow_images/
```

## Configuration

### Basic Configuration

Edit `main.py` and update these variables if needed:

```python
# Port configuration (default: 8080)
PORT = 8080

# Maximum images to store (default: 1000)
MAX_IMAGES = 1000

# Allowed file formats
ALLOWED_EXTENSIONS = {'png', 'jpg', 'jpeg', 'gif', 'bmp', 'webp'}

# Image storage folder
UPLOAD_FOLDER = Path('./slideshow_images')
```

### Network Configuration

**Find Your Pi's IP Address**:
```bash
# Get IP address
hostname -I

# Example output: 192.168.1.50
```

**Allow Network Access**:
```bash
# Check firewall status
sudo ufw status

# If enabled, allow port 8080
sudo ufw allow 8080/tcp
```

### Display Configuration

**Fullscreen Mode** (recommended for events):
1. Start the server
2. Open browser to `http://localhost:8080`
3. Press `F11` for fullscreen (most browsers)
4. Or configure display settings in Raspberry Pi OS

**Multiple Displays**:
- Server supports single display
- Configure display output in Raspberry Pi OS settings
- Use `xrandr` for advanced configuration

## Running the Application

### Start the Server

```bash
# Navigate to project directory
cd ~/remote-slideshow

# Activate virtual environment
source venv/bin/activate

# Run the server
python main.py

# Expected output:
# 🎬 Remote Slideshow Server
# 📍 Local IP: 192.168.1.50
# 🌐 Access: http://192.168.1.50:8080
# 📨 Upload: curl -X POST -F 'image=@photo.jpg' http://192.168.1.50:8080/upload
# 🚀 Starting server...
```

### Access the Slideshow

**From Same Raspberry Pi**:
- Browser: `http://localhost:8080`

**From Other Device on Network**:
- Browser: `http://192.168.1.50:8080` (replace with actual Pi IP)

**Recommended for Displays**:
- Full URL for easier network access
- Bookmark the URL for quick return

## Auto-Start on Boot (Optional)

### Create Systemd Service

```bash
# Create service file
sudo nano /etc/systemd/system/remote-slideshow.service
```

Add this content (replace `pi` with your username if different):

```ini
[Unit]
Description=Remote Slideshow Server
After=network.target

[Service]
Type=simple
User=pi
WorkingDirectory=/home/pi/remote-slideshow
Environment=PATH=/home/pi/remote-slideshow/venv/bin
ExecStart=/home/pi/remote-slideshow/venv/bin/python main.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Save and exit (Ctrl+X, then Y, then Enter).

### Enable Service

```bash
# Reload systemd
sudo systemctl daemon-reload

# Enable service (start on boot)
sudo systemctl enable remote-slideshow.service

# Start service now
sudo systemctl start remote-slideshow.service

# Check status
sudo systemctl status remote-slideshow.service

# View logs
journalctl -u remote-slideshow.service -f
```

### Manage Service

```bash
# Stop service
sudo systemctl stop remote-slideshow.service

# Restart service
sudo systemctl restart remote-slideshow.service

# View last 50 lines of log
journalctl -u remote-slideshow.service -n 50
```

## Testing Installation

### Quick Test

```bash
# Check server is running
curl http://localhost:8080

# Should return HTML of the slideshow page
```

### Upload Test Image

```bash
# From command line on the Pi
curl -X POST -F "image=@test.jpg" http://localhost:8080/upload

# Or from another device
curl -X POST -F "image=@test.jpg" http://192.168.1.50:8080/upload

# Response should be:
# {"success": true, "filename": "YYYYMMDD_HHMMSS_xxxxx.jpg", ...}
```

### Check Server Status

```bash
# Get server status
curl http://localhost:8080/status

# Response shows image count, total size, etc.
```

## Verification Checklist

After installation, verify:

- [ ] Raspberry Pi OS updated to latest
- [ ] Python virtual environment created and activated
- [ ] Flask and Pillow installed (pip list shows them)
- [ ] slideshow_images directory created and writable
- [ ] main.py is executable
- [ ] Flask server starts without errors
- [ ] Can access http://localhost:8080 from Pi
- [ ] Can access http://[PI_IP]:8080 from phone/computer
- [ ] Test upload successful (see status shows 1 image)
- [ ] Firewall allows port 8080 if enabled
- [ ] HDMI monitor connected (if for display mode)

## Troubleshooting Installation

### Port Already in Use

```bash
# Check what's using port 8080
sudo lsof -i :8080

# Kill process if needed
sudo kill -9 <PID>

# Or use different port in main.py:
PORT = 8081
```

### Permission Issues

```bash
# Add user to necessary groups
sudo usermod -a -G video $USER
newgrp - $USER

# Check permissions on image directory
chmod 755 slideshow_images/
```

### Dependencies Not Installing

```bash
# Make sure pip is updated
pip install --upgrade pip

# Try installing individually
pip install Flask==2.0.0 Pillow requests

# Or install from requirements.txt if available
pip install -r requirements.txt
```

### Flask Won't Start

```bash
# Check for Python errors
python main.py

# Look for ImportError or SyntaxError messages

# Verify Flask is installed
python -c "import flask; print(flask.__version__)"
```

## Next Steps

After successful installation:
1. Read [User Guide](Remote-Slideshow-User-Guide.md) for basic operation
2. Review [Integration Guide](Remote-Slideshow-Selfie-Cam-Integration.md) if using with selfie-cam
3. Check [API Reference](Remote-Slideshow-API.md) to upload from other systems
4. See [Configuration Reference](Remote-Slideshow-Configuration.md) for tuning

## Security Notes

**Current Setup**:
- No authentication - anyone with network access can upload/delete
- No HTTPS - suitable for local networks only
- Suitable for events, demos, workshops on trusted networks

**For Production/Public Deployment**:
- Add API key authentication
- Use HTTPS with SSL certificate
- Implement rate limiting
- Add access control lists
- Run behind reverse proxy (nginx, Apache)

See [Configuration Reference](Remote-Slideshow-Configuration.md) for security settings.
