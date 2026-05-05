# Remote Slideshow: Selfie-Cam Integration Guide

## Overview

The Remote Slideshow integrates with the ARI Selfie-Cam system to automatically display captured photos and interview recordings on a dedicated display. This guide explains how to set up and use the integration.

## Architecture

### System Overview

```
┌─────────────────────────────────────────────────────┐
│ ARI Selfie-Cam System (Pi #1)                       │
│ ┌───────────────────────────────────────────────┐   │
│ │ Camera → Capture → Gallery                    │   │
│ │ - Takes selfies (UVC camera)                  │   │
│ │ - Records interviews (CSI camera + audio)     │   │
│ └───────────────────────────────────────────────┘   │
│          │                                           │
│          │ HTTP POST /upload (auto)                 │
│          ▼                                           │
└─────────────────────────────────────────────────────┘
          │
          │ Network (HTTP)
          │ 192.168.0.0/24
          │
┌─────────────────────────────────────────────────────┐
│ Remote Slideshow Server (Pi #2)                     │
│ ┌───────────────────────────────────────────────┐   │
│ │ Flask Server (Port 8080)                      │   │
│ │ - Receives uploads from selfie-cam            │   │
│ │ - Stores in slideshow_images/                 │   │
│ │ - Serves web interface                        │   │
│ └───────────────────────────────────────────────┘   │
│          │                                           │
│          │ HDMI Output                              │
│          ▼                                           │
│  [Monitor/Display]                                  │
│  Displays slideshow of captured photos              │
│  & interview thumbnails                             │
└─────────────────────────────────────────────────────┘
```

## Setup Requirements

### Hardware
- **Selfie-Cam Pi**: One Raspberry Pi running ARI Selfie-Cam
- **Slideshow Pi**: Separate Raspberry Pi running Remote Slideshow
- **Display**: HDMI monitor for slideshow display
- **Network**: Both Pi's on same network

### Network Connectivity
- Both Pi's must reach each other
- Example network: `192.168.0.0/24`
- Selfie-Cam Pi: `192.168.0.50`
- Slideshow Pi: `192.168.0.133`
- Both connected via WiFi or Ethernet

## Configuration

### Step 1: Install Remote Slideshow

On the **Slideshow Pi** (if not already done):

```bash
cd ~
git clone https://github.com/NM-TAFE/remote-slideshow.git
cd remote-slideshow
python3 -m venv venv
source venv/bin/activate
pip install flask pillow requests
python main.py
```

Verify it's running:
```bash
curl http://localhost:8080/status
# Should show: "status": "running"
```

### Step 2: Configure Selfie-Cam for Slideshow

On the **Selfie-Cam Pi**, edit `app_admin.py` or `droidcam.py`:

**Find and update these lines** (around line 53-55 in droidcam.py):

```python
# Slideshow/display server
SLIDESHOW = "http://192.168.0.133:8080"  # Change to your slideshow Pi IP
UPLOAD_ENDPOINT = f"{SLIDESHOW}/upload"
```

**Make sure**:
- `SLIDESHOW` URL is correct (replace IP with actual slideshow Pi IP)
- Port `8080` matches slideshow server port
- Both Pi's can reach each other on network

**Test connectivity**:
```bash
# From selfie-cam Pi, test slideshow connectivity
curl http://192.168.0.133:8080/status

# Should show slideshow server is running
```

### Step 3: Verify Integration

**Test from Selfie-Cam**:
1. Take a selfie via admin panel
2. After capture, check slideshow server
3. Image should appear in slideshow

**Check Slideshow**:
```bash
# List images on slideshow
curl http://192.168.0.133:8080/images

# Should show your captured selfie in list
```

## Auto-Upload Behavior

### When Integration is Enabled

**Selfie Capture Workflow**:
1. User takes selfie on selfie-cam
2. Image captured and saved locally in `static/gallery/`
3. Selfie-cam automatically uploads to slideshow server
4. Image appears in slideshow display

**Interview Workflow**:
1. User records interview (video + audio)
2. Interview MP4 file created in `static/gallery/`
3. Server generates thumbnail image
4. Thumbnail uploaded to slideshow
5. Interview thumbnail appears in slideshow (clickable to play)

**Code Location** (in `app_admin.py` or `droidcam.py`):

Around line 400-450 (after `take_selfie()` and `stop_interview()`):
```python
# After successful capture, auto-upload:
if UPLOAD_ENDPOINT:
    try:
        with open(file_path, 'rb') as f:
            requests.post(UPLOAD_ENDPOINT, files={'image': f})
    except Exception as e:
        print(f"Error uploading to slideshow: {e}")
```

## Control Commands

### Immediate Display

To display a specific selfie immediately (not in rotation):

```python
# In selfie-cam code, after upload
show_command = {
    'action': 'show_new',
    'filename': uploaded_filename,
    'duration': 5  # Show for 5 seconds
}
requests.post(f"{SLIDESHOW}/control", json=show_command)
```

### Pause/Resume

Pause slideshow during events:

```bash
# Pause
curl -X POST http://192.168.0.133:8080/control \
  -H "Content-Type: application/json" \
  -d '{"action":"pause"}'

# Resume
curl -X POST http://192.168.0.133:8080/control \
  -H "Content-Type: application/json" \
  -d '{"action":"resume"}'
```

### Clear Display

Clear all images between events:

```bash
# Delete all images
curl -X POST http://192.168.0.133:8080/clear
```

## Event Workflow

### Setup Phase
```
1. Start remote slideshow server on display Pi
2. Start selfie-cam server on capture Pi
3. Verify both servers running
4. Test one complete capture → display cycle
5. Clear slideshow gallery (/clear)
6. Open slideshow in fullscreen on display (F11)
```

### During Event
```
1. Participant takes selfie via selfie-cam interface
2. Photo automatically appears on slideshow display
3. Repeat for each participant
4. Slideshow cycles through all photos
5. Monitor network/storage if high volume
```

### End of Event
```
1. Stop capturing
2. Download important images from slideshow (backup)
3. Get images from selfie-cam gallery (local backup)
4. Clear slideshow (or keep for archive)
5. Shut down servers gracefully
```

## Troubleshooting Integration

### Upload Fails (Images Don't Appear)

**Check 1: Is slideshow server running?**
```bash
# From selfie-cam Pi
curl http://192.168.0.133:8080/status
# Should show "running"
```

**Check 2: Verify network connectivity**
```bash
# Ping slideshow Pi
ping 192.168.0.133

# Should get responses
```

**Check 3: Check firewall**
```bash
# On slideshow Pi
sudo ufw status
# If enabled: sudo ufw allow 8080
```

**Check 4: Review selfie-cam logs**
```bash
# Watch logs while taking selfie
python app_admin.py

# Look for error messages or confirmations
```

**Check 5: Verify SLIDESHOW URL**
```bash
# In app_admin.py or droidcam.py, verify:
SLIDESHOW = "http://192.168.0.133:8080"
# IP and port are correct
```

### Images Upload Slowly

**Causes**:
- Network congestion
- Image too large
- Pi CPU overloaded
- Storage full

**Solutions**:
- Reduce image resolution before upload
- Use wired Ethernet if possible
- Close other applications on Pi
- Clear old images from slideshow

### Slideshow Display Freezes

**Check**:
1. Is slideshow server still running? (try curl)
2. Network connectivity still good? (ping)
3. Storage full? (check `/status`)

**Fix**:
```bash
# Restart slideshow server
sudo systemctl restart remote-slideshow.service

# Or manually
# Kill: sudo pkill -f "python main.py"
# Restart: python main.py
```

## Advanced Configuration

### Custom Upload Location

To save uploads to different folder:

**On slideshow Pi**, edit `main.py`:
```python
# Change upload folder
UPLOAD_FOLDER = Path('/media/usb/slideshow')

# Or network share
UPLOAD_FOLDER = Path('/mnt/network/slideshow')
```

### Image Processing

To process images before display:

**In selfie-cam before upload**:
```python
from PIL import Image

# Resize/compress selfie
img = Image.open(selfie_path)
img.thumbnail((1920, 1080))
img.save(compressed_path, quality=80)

# Upload compressed version
with open(compressed_path, 'rb') as f:
    requests.post(UPLOAD_ENDPOINT, files={'image': f})
```

### Multiple Displays

To have multiple slideshow displays:

1. Run separate slideshow server instances on different ports
2. Configure each with different PORT in main.py
3. Update selfie-cam to upload to specific slideshow
4. Or upload to all:

```python
SLIDESHOWS = [
    "http://192.168.0.133:8080/upload",
    "http://192.168.0.134:8080/upload",
    "http://192.168.0.135:8080/upload"
]

# Upload to all
for slideshow_url in SLIDESHOWS:
    requests.post(slideshow_url, files={'image': f})
```

## Network Diagram

```
┌──────────────────┐                    ┌──────────────────┐
│  Admin Phone     │                    │  HDMI Display    │
│  http://....:5000│                    │                  │
└────────┬─────────┘                    └────────▲─────────┘
         │                                       │ HDMI
         │ WiFi                           ┌──────┴─────────┐
         │ :5000                          │   Slideshow    │
    ┌────▼──────────────────┐             │   Raspberry Pi │
    │  Selfie-Cam Pi        │             │  Flask :8080   │
    │  ┌────────────────┐   │    HTTP     │ ┌────────────┐ │
    │  │ App_admin.py   │───┼──POST/upload─→│ Remote     │ │
    │  │ Captures image │   │   :8080      │ │ Slideshow │ │
    │  └────────────────┘   │             │ └────────────┘ │
    │                        │             │                │
    │  Network: 192.168.0/24 │             │ 192.168.0.133  │
    │  IP: 192.168.0.50 :5000│             │                │
    └────────────────────────┘             └────────────────┘
         │
         │ Uses static/gallery
         │ for local backup
```

## Best Practices

**Network**:
- Use wired Ethernet for both Pi's (more reliable than WiFi)
- Keep both Pi's on same network segment
- Test connectivity before event

**Performance**:
- Monitor storage during long events
- Set image duration appropriately (5-15 seconds typical)
- Use consistent image size

**Backup**:
- Download photos from both servers at end of event
- Multiple copies of important photos
- Keep local backup on Pi and cloud/external drive

**Troubleshooting**:
- Test integration before deploying at event
- Have alternative display method ready
- Monitor logs during event

## Additional Resources

- [Remote Slideshow Setup](Remote-Slideshow-Setup.md)
- [Remote Slideshow API](Remote-Slideshow-API.md)
- [Selfie-Cam Setup](ARI-Selfie-Cam-Setup.md)
- [Selfie-Cam Configuration](ARI-Selfie-Cam-Configuration.md)
