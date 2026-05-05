# ARI Selfie-Cam: Configuration Reference

## Overview
This guide documents all configurable parameters in the ARI Selfie-Cam application. Configuration is done by editing the Python application files before running the server.

## Main Configuration File
The primary configuration file is `app_admin.py` (or `droidcam.py` for alternative setup).

## Application Configuration

### Flask Server Settings

**File**: `app_admin.py` (end of file)

```python
# Basic server configuration
if __name__ == '__main__':
    app.run(
        host='0.0.0.0',      # Listen on all network interfaces
        port=5000,           # Port number
        debug=True,          # Debug mode (disable in production)
        threaded=True        # Enable threading for concurrent requests
    )
```

**Parameters**:
- `host='0.0.0.0'` - Listen on all interfaces (allows remote access)
  - Use `'127.0.0.1'` for localhost only
  - Use `'192.168.1.50'` for specific IP
- `port=5000` - Server port
  - Use `5001`, `5002`, etc. if 5000 is in use
  - Requires port > 1024 to run without sudo
- `debug=True` - Enable Flask debug mode
  - Set to `False` for production
  - Provides detailed error messages in development
- `threaded=True` - Allow concurrent requests
  - Required for simultaneous camera/preview operations

---

## Storage Configuration

### Gallery Directory

**File**: `app_admin.py` (line ~26)

```python
# Path where captured selfies and interview videos are stored
GALLERY_PATH = 'static/gallery'
```

**Options**:
- Relative path: `'static/gallery'` (relative to app directory)
- Absolute path: `'/home/pi/gallery'` (full filesystem path)
- External storage: `'/mnt/external/gallery'` (USB drive, network mount)

**Notes**:
- Directory must be writable by the Flask process user
- Ensure sufficient disk space for media files
- Recommend 10+ GB for active use

**Create directory**:
```bash
mkdir -p static/gallery
chmod 755 static/gallery
```

---

## Camera Configuration

### Interview Duration

**File**: `app_admin.py` (line ~27)

```python
# Maximum interview duration in seconds
INTERVIEW_DURATION = 300  # 5 minutes
```

**Common Values**:
- `60` - 1 minute (quick demos)
- `180` - 3 minutes (standard interviews)
- `300` - 5 minutes (default, allows detailed responses)
- `600` - 10 minutes (for longer interviews)

**Notes**:
- Interview auto-stops at this duration
- Can manually stop before limit
- Affects storage requirements

---

### Camera Device Indices

**File**: `app_admin.py` (in `CameraManager.detect_cameras()` method, ~line 45-68)

The application auto-detects cameras, but you can manually specify:

```python
# Manual camera index specification (if auto-detection fails)
self.csi_camera_num = 0      # CSI camera (interview)
self.uvc_camera_num = 1      # UVC camera (selfie)
```

**Finding Camera Indices**:
```bash
# List all cameras
vcgencmd get_camera

# List USB devices
lsusb

# List video devices
ls /dev/video*

# Test specific camera
libcamera-hello --camera 0   # Test camera 0
libcamera-hello --camera 1   # Test camera 1
```

### Camera Resolution

**File**: `app_admin.py` (in `CameraManager` class)

**CSI Camera Configuration** (~line 76):
```python
config = self.csi_camera.create_video_configuration(
    main={"size": (1280, 720)}  # Width x Height in pixels
)
```

**UVC Camera Configuration** (~line 91):
```python
config = self.uvc_camera.create_still_configuration(
    main={"size": (1280, 720)}  # Width x Height in pixels
)
```

**Resolution Options** (for Raspberry Pi 3B):
- `(640, 480)` - VGA (fastest, lowest quality)
- `(1280, 720)` - HD (recommended default)
- `(1920, 1080)` - Full HD (slower, larger files)
- `(2560, 1440)` - 2K (very slow, large files)

**Notes**:
- Lower resolution = faster capture, smaller files, less CPU
- Higher resolution = better quality, larger files, higher CPU load
- Raspberry Pi 3B recommends 720p or lower for smooth operation

### Camera Preview

**File**: `app_admin.py` (in `CameraManager.init_uvc_preview()` method, ~line 100)

```python
# Camera preview configuration
# No preview stream configured to save resources
```

**Camera Feed Frame Rate**:
```python
# In camera_feed() function (~line 327)
time.sleep(0.1)  # ~10 FPS (0.1 second between frames)
```

**Adjust Frame Rate**:
- `time.sleep(0.05)` - 20 FPS (higher CPU usage)
- `time.sleep(0.1)` - 10 FPS (default, balanced)
- `time.sleep(0.2)` - 5 FPS (lower CPU usage)

**JPEG Quality**:
```python
# In camera_feed() function (~line 321)
img.save(img_io, 'JPEG', quality=70)
```

**Quality Settings** (1-100):
- `50` - Low quality, smaller file
- `70` - Default, good balance
- `85` - High quality, larger file
- `95` - Very high quality, much larger file

---

## Audio Configuration

### Audio Device

**File**: `app_admin.py` (in `start_interview()` function)

```python
audio_process = subprocess.Popen([
    'arecord', '-D', 'plughw:1,0',  # Audio device
    '-f', 'cd',                      # Format (16-bit stereo)
    '-t', 'wav',                     # Output format
    audio_file
])
```

**Find Your Audio Device**:
```bash
# List audio devices
arecord -l

# Example output:
# card 1: Device [USB Device], device 0: USB Audio [USB Audio]

# Format: plughw:CARD,DEVICE
# So above device is: plughw:1,0
```

**Update Audio Device**:
```python
'arecord', '-D', 'plughw:1,0',  # Change '1,0' to your device
```

**Audio Format Options**:
- `cd` - 44.1 kHz, 16-bit stereo (recommended)
- `dat` - 48 kHz, 16-bit stereo (higher quality)
- Other formats available (see `arecord --help`)

---

## ARI Robot Configuration

### ARI Base URL

**File**: `app_admin.py` (line ~28)

```python
# ARI robot server URL - MUST be updated for your setup
ARI_BASE_URL = 'http://ari-Xc'
```

**Finding Your ARI Robot**:

Option 1: Hostname (mDNS)
```bash
# If your robot is named "ari-Xc"
ping ari-Xc
# If it responds, use: ARI_BASE_URL = 'http://ari-Xc'
```

Option 2: IP Address
```bash
# Scan network for ARI robot
nmap -p 5000 192.168.0.0/24

# Or from Pi, find other devices
arp-scan --localnet | grep -i ari

# Once found, use IP:
# ARI_BASE_URL = 'http://192.168.1.100'
```

**Configuration Examples**:
```python
# Using hostname
ARI_BASE_URL = 'http://ari-Xc'

# Using static IP
ARI_BASE_URL = 'http://192.168.1.100'

# Using port (if not default)
ARI_BASE_URL = 'http://192.168.1.100:8080'
```

**Testing Connectivity**:
```bash
# From Raspberry Pi command line
curl http://ari-Xc/api/status
curl http://192.168.1.100/api/status

# If it returns JSON, connection works
```

### TTS Language

**File**: `app_admin.py` (in `send_ari_tts()` function, ~line 156)

```python
def send_ari_tts(text, lang_id="en_GB"):
    # Default language for text-to-speech
```

**Default Language**: `"en_GB"` (English - British)

**Available Languages**:
- `"en_GB"` - English (British)
- `"en_US"` - English (American)
- `"es_ES"` - Spanish (Spain)
- `"fr_FR"` - French
- `"de_DE"` - German

**Changing Default**:
```python
def send_ari_tts(text, lang_id="en_US"):  # Change default
```

**API Request**:
```python
# Specifying language in API call
payload = {
    "rawtext": {
        "text": text,
        "lang_id": "es_ES"  # Override default
    }
}
```

### Motion Commands

**File**: Not hardcoded - defined on ARI robot

Motion names are passed to ARI and executed server-side:

```python
def send_ari_motion(motion_name):
    url = f"{ARI_BASE_URL}/action/motion_manager"
    payload = {"filename": motion_name}
```

**Common Motion Names** (examples):
- `wave_hello` - Wave gesture
- `shake_left` - Shake left
- `nod_yes` - Nod affirmatively
- `shake_no` - Shake negatively

**Finding Available Motions**:
```bash
# Query ARI robot for available motions
curl http://ari-Xc/api/motions

# Or check ARI documentation/file system
```

**Custom Motions**:
Add custom motion filenames as needed on the ARI robot, then use them in the admin panel.

---

## DroidCam Configuration (Alternative Setup)

**File**: `droidcam.py` (for phone-as-camera setup)

```python
# DroidCam configuration
DROIDCAM_IP = '192.168.0.126'           # DroidCam server IP
DROIDCAM_PORT = 4747                    # Standard DroidCam port
DROIDCAM_URL = "http://192.168.0.126:4747/video"  # Video stream URL

# Slideshow/display server
SLIDESHOW = "http://192.168.0.133:8080"
UPLOAD_ENDPOINT = f"{SLIDESHOW}/upload"
```

**Setup DroidCam**:
1. Install DroidCam on phone
2. Find phone's IP address
3. Update `DROIDCAM_IP` to phone's IP
4. Keep port `4747` (standard)

---

## Performance Tuning

### For Faster Response Times
```python
# Lower preview resolution
{"size": (640, 480)}

# Lower preview frame rate
time.sleep(0.2)  # 5 FPS

# Lower JPEG quality
quality=50
```

### For Better Quality
```python
# Higher camera resolution
{"size": (1920, 1080)}

# Higher preview frame rate
time.sleep(0.05)  # 20 FPS

# Higher JPEG quality
quality=90
```

### For Lower CPU Usage
```python
# Disable debug mode
debug=False

# Single-threaded mode (if not needed)
threaded=False

# Lower resolution
{"size": (640, 480)}

# Longer preview interval
time.sleep(0.3)
```

---

## Security Considerations

**IMPORTANT**: Current setup has no authentication.

### For Production Deployment

**1. Disable Debug Mode**:
```python
app.run(debug=False)  # MUST be False in production
```

**2. Add Authentication** (example):
```python
from flask_httpauth import HTTPAuth
auth = HTTPAuth()

@auth.verify_password
def verify_password(username, password):
    # Implement password verification
    return username == "admin" and password == "secure_password"

@app.route('/api/admin/ari_speak', methods=['POST'])
@auth.login_required
def admin_ari_speak():
    # Protected endpoint
```

**3. Use HTTPS**:
```python
# Install SSL certificate
app.run(ssl_context='adhoc')  # Requires pyopenssl

# Or use reverse proxy (nginx, Apache) with SSL
```

**4. Firewall/Network**:
- Restrict access to trusted IPs only
- Use VPN for remote access
- Don't expose to public internet

---

## Environment Variables (Optional)

For sensitive configuration, use environment variables:

```python
import os

ARI_BASE_URL = os.getenv('ARI_BASE_URL', 'http://ari-Xc')
GALLERY_PATH = os.getenv('GALLERY_PATH', 'static/gallery')
```

**Set in shell**:
```bash
export ARI_BASE_URL="http://192.168.1.100"
export GALLERY_PATH="/mnt/external/gallery"
python app_admin.py
```

**Set in systemd service**:
```ini
[Service]
Environment="ARI_BASE_URL=http://192.168.1.100"
Environment="GALLERY_PATH=/home/pi/gallery"
```

---

## Configuration Checklist

Before running the application:
- [ ] `GALLERY_PATH` directory exists and is writable
- [ ] `INTERVIEW_DURATION` set to desired max time
- [ ] `ARI_BASE_URL` configured for your robot
- [ ] Camera indices correct for your setup
- [ ] Audio device specified for your microphone
- [ ] Port 5000 available (or update to different port)
- [ ] Flask `debug=False` if in production
- [ ] Security measures implemented for production

---

## Configuration Validation

**Test Configuration**:
```bash
# Start server and check for errors
python app_admin.py

# Expected output:
# * Running on http://0.0.0.0:5000
# Available cameras: [...]
# CSI camera at index 0
# UVC camera at index 1
```

**Common Configuration Errors**:
- Port already in use → Change to different port
- Camera not detected → Check indices and connections
- Audio device not found → Verify `plughw:X,X` value
- ARI unreachable → Verify URL and network connectivity

---

## Additional Resources

- [Setup Guide](ARI-Selfie-Cam-Setup.md)
- [API Reference](ARI-Selfie-Cam-API.md)
- [Troubleshooting Guide](ARI-Selfie-Cam-Troubleshooting.md)
