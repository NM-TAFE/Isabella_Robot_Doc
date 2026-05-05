# ARI Selfie-Cam: Setup & Installation Guide

## Overview
The ARI Selfie-Cam is a Flask-based web application running on a Raspberry Pi that provides camera controls and ARI robot integration. It enables:
- Remote selfie capture via UVC USB camera
- Interview recording via CSI camera with audio
- Real-time camera preview streaming
- Phone-based admin control panel with robot interaction

## Hardware Requirements

### Computing Device
- **Raspberry Pi**: 3B or later (64-bit Raspberry Pi OS recommended)
- **MicroSD Card**: 32GB or larger (recommended)
- **Power Supply**: 5V 3A USB-C power adapter

### Camera Devices
- **CSI Camera**: For interview recording (ribbon cable connection)
  - Recommended: Official Raspberry Pi Camera Module 3
  - Alternative: Any compatible CSI camera (IMX or OV sensor)
- **UVC USB Camera**: For selfie capture (USB connection)
  - Any standard USB webcam compatible with UVC protocol
  - Recommended resolution: 1080p or higher

### Audio Equipment
- **USB Microphone**: For recording interview audio
  - Recommended: Standard USB condenser microphone
  - Ensure it's compatible with Linux ALSA audio system

### Network
- Ethernet or WiFi connectivity
- Router or local network for device discovery
- Same network for phone-based admin access

## Software Prerequisites

### Raspberry Pi OS Setup
```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install Python and package manager
sudo apt install -y python3-pip python3-venv

# Install system dependencies
sudo apt install -y ffmpeg alsa-utils libcamera-apps
sudo apt install -y python3-opencv

# Enable camera interface
sudo raspi-config
# Navigate to: Interface Options → Camera → Enable
# Select "libcamera" when prompted
```

### Enable Required Interfaces
```bash
sudo raspi-config
# Interface Options → Camera → Enable (libcamera)
# Interface Options → Audio → Enable (if not auto-enabled)
```

## Installation Steps

### 1. Clone or Download the Repository
```bash
cd ~/
git clone https://github.com/NM-TAFE/ARI-selfie-cam.git
cd ARI-selfie-cam
```

### 2. Set Up Python Virtual Environment
```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate

# Verify activation (prompt should show (venv))
which python
```

### 3. Install Python Dependencies
```bash
# Install required packages
pip install -r requirements.txt

# Typical requirements include:
# - Flask (web framework)
# - requests (HTTP client for ARI communication)
# - picamera2 (camera control)
# - Pillow (image processing)
```

### 4. Create Required Directories
```bash
# Create static gallery directory for saving media
mkdir -p static/gallery

# Verify directory structure
ls -la
# Should see: static/, templates/, app_admin.py, etc.
```

### 5. Configure Hardware Access Permissions
```bash
# Add current user to necessary groups
sudo usermod -a -G video $USER
sudo usermod -a -G audio $USER

# Apply group changes (requires logout/login or reboot)
newgrp video
newgrp audio

# Or reboot to apply permanently
sudo reboot
```

## Hardware Configuration

### Camera Device Detection
The application automatically detects cameras on startup:

```bash
# List detected cameras
vcgencmd get_camera

# Check for USB cameras
lsusb | grep -i camera
ls /dev/video*
```

### Configure Camera Devices

Edit `app_admin.py` or `droidcam.py` and update camera indices if needed:

```python
# In CameraManager.__init__():
# Camera detection automatically finds:
# - CSI_CAMERA_NUM: Interface cameras (IMX/OV sensors)
# - UVC_CAMERA_NUM: USB cameras
```

If cameras are not detected correctly, you can manually specify:

```python
# Manual configuration (if auto-detection fails)
self.csi_camera_num = 0    # CSI camera index
self.uvc_camera_num = 1    # UVC camera index
```

### Audio Device Configuration

Identify your USB microphone:

```bash
# List all audio devices
arecord -l

# Output example:
# card 1: Device [USB Device], device 0: USB Audio [USB Audio]

# Test recording from USB microphone
arecord -D plughw:1,0 -f cd -t wav -d 3 test.wav
aplay test.wav

# Update in app_admin.py - locate arecord command:
# Change 'plughw:1,0' to match your device
```

Configure audio device in `app_admin.py`:

```python
# Locate the start_interview() function
audio_process = subprocess.Popen([
    'arecord', '-D', 'plughw:1,0',  # Change device as needed
    '-f', 'cd', '-t', 'wav', audio_file
])
```

## Configuration

### Essential Configuration Variables

Edit `app_admin.py` or `droidcam.py` before running:

```python
# Flask Configuration
app.run(host='0.0.0.0', port=5000, debug=True, threaded=True)

# Camera Configuration
GALLERY_PATH = 'static/gallery'        # Where photos/videos are saved
INTERVIEW_DURATION = 300               # Max interview length (seconds)

# ARI Robot Configuration
ARI_BASE_URL = 'http://ari-Xc'         # ARI robot IP/hostname
                                       # Format: http://[IP] or http://[hostname]

# For droidcam.py only
DROIDCAM_IP = '192.168.0.126'          # DroidCam server IP
DROIDCAM_PORT = 4747                   # DroidCam port
DROIDCAM_URL = "http://192.168.0.126:4747/video"  # DroidCam video stream
```

### Update ARI Robot URL

Find your ARI robot's IP address or hostname:

```bash
# From the Raspberry Pi (if on same network)
ping ari-Xc
# or
nmap -p 5000 192.168.0.0/24

# Get your own IP for reference
hostname -I
```

Update the URL in your app file:

```python
# Option 1: Using hostname (if mDNS available)
ARI_BASE_URL = 'http://ari-Xc'

# Option 2: Using static IP
ARI_BASE_URL = 'http://192.168.1.100'

# Verify connectivity
curl http://ari-Xc/api/status  # Test from command line
```

## Running the Application

### Start the Server

```bash
# Navigate to project directory
cd ~/ARI-selfie-cam

# Activate virtual environment
source venv/bin/activate

# Run the Flask application
python app_admin.py

# Expected output:
# * Running on http://0.0.0.0:5000
# * Debug mode: on
```

### Access the Application

**On the Raspberry Pi:**
- Open browser and navigate to `http://localhost:5000`

**From other devices on the network:**
- Find your Pi's IP: `hostname -I`
- Access from phone/computer: `http://[PI_IP]:5000`
- Example: `http://192.168.1.50:5000`

### Access Different Pages

- **Home Menu**: `http://[PI_IP]:5000/`
- **Admin Control**: `http://[PI_IP]:5000/admin` (phone controls, camera preview)
- **Selfie Mode**: `http://[PI_IP]:5000/selfie`
- **Interview Mode**: `http://[PI_IP]:5000/interview`
- **Gallery**: `http://[PI_IP]:5000/gallery` (public view)
- **Admin Gallery**: `http://[PI_IP]:5000/admin-gallery` (with delete options)

## Auto-Start on Boot (Optional)

Create a systemd service to automatically start the application:

```bash
# Create service file
sudo nano /etc/systemd/system/ari-selfie-cam.service
```

Add the following content (replace `pi` with your username if different):

```ini
[Unit]
Description=ARI Selfie-Cam Server
After=network.target

[Service]
Type=simple
User=pi
WorkingDirectory=/home/pi/ARI-selfie-cam
Environment=PATH=/home/pi/ARI-selfie-cam/venv/bin
ExecStart=/home/pi/ARI-selfie-cam/venv/bin/python app_admin.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Enable and start the service:

```bash
# Reload systemd daemon
sudo systemctl daemon-reload

# Enable service to start on boot
sudo systemctl enable ari-selfie-cam.service

# Start the service now
sudo systemctl start ari-selfie-cam.service

# Check status
sudo systemctl status ari-selfie-cam.service

# View logs
journalctl -u ari-selfie-cam.service -f
```

## Verification Checklist

After installation, verify everything is working:

- [ ] Raspberry Pi OS updated to latest version
- [ ] Python virtual environment created and activated
- [ ] Dependencies installed (`pip list` shows Flask, requests, picamera2, etc.)
- [ ] Camera permissions set (user in video and audio groups)
- [ ] CSI camera detected (`vcgencmd get_camera`)
- [ ] UVC camera detected (`lsusb` shows camera, `/dev/video*` exists)
- [ ] USB microphone detected (`arecord -l` shows device)
- [ ] ARI_BASE_URL configured correctly
- [ ] Flask server starts without errors
- [ ] Can access home page at `http://localhost:5000`
- [ ] Can access admin page at `http://localhost:5000/admin`
- [ ] Gallery directory created and writable

## Troubleshooting Installation Issues

### Python Version Issues
```bash
# Verify Python 3.7+
python3 --version

# If not available
sudo apt install -y python3.9 python3.9-venv
python3.9 -m venv venv
```

### Permission Denied Errors
```bash
# Add execute permissions to scripts
chmod +x app_admin.py

# Verify group membership
groups $USER

# If video/audio not listed, apply changes
sudo usermod -a -G video,audio $USER
newgrp - $USER
```

### Port Already in Use
```bash
# Check what's using port 5000
sudo lsof -i :5000

# Kill existing process
sudo pkill -f "python.*app_admin"

# Use different port in app code
app.run(port=5001)
```

### Camera Not Detected
```bash
# Diagnose camera issue
vcgencmd get_camera
# Should show: supported=1 detected=1

# If detected=0, check connection:
# 1. Reseat CSI ribbon cable (power off first)
# 2. Enable camera in raspi-config
# 3. Reboot
sudo reboot
```

## Next Steps

After successful installation:
1. Read [ARI-Selfie-Cam User Guide](ARI-Selfie-Cam-User-Guide.md) for usage instructions
2. Review [Admin Control Features](ARI-Selfie-Cam-Admin-Control.md) for phone controls
3. Consult [API Reference](ARI-Selfie-Cam-API.md) for integration details
4. Check [Troubleshooting Guide](ARI-Selfie-Cam-Troubleshooting.md) for common issues
