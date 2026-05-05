# Remote Slideshow: Configuration Reference

## Overview

This guide documents all configurable parameters in the Remote Slideshow application. Configuration is done by editing `main.py` before running the server.

## Configuration File

**File**: `main.py` (lines 18-22)

## Basic Configuration

### Port Configuration

**File**: `main.py` (line 22)

```python
PORT = 8080
```

**Purpose**: HTTP server listening port

**Options**:
- `8080` - Default, good for development/demo
- `80` - Standard HTTP (requires sudo/root)
- `5000` - Common Flask development port
- `3000` - Alternative
- Any port > 1024 (no root needed)

**Note**: If port is already in use, try different port or kill process:
```bash
sudo lsof -i :8080
sudo kill -9 <PID>
```

**Changing Port**:
```python
PORT = 8081  # Change to 8081
```

Then access at: `http://192.168.1.50:8081`

---

## Storage Configuration

### Upload Folder

**File**: `main.py` (line 20)

```python
UPLOAD_FOLDER = Path('./slideshow_images')
```

**Purpose**: Directory where uploaded images are stored

**Options**:
- Relative path: `'./slideshow_images'` (relative to app directory)
- Absolute path: `'/home/pi/slideshow_images'`
- External storage: `'/mnt/usb/slideshow_images'` (USB drive)
- Network share: `'/mnt/network/slideshow'` (if mounted)

**Changing Location**:
```python
# Store on USB drive
UPLOAD_FOLDER = Path('/mnt/external/slideshow')

# Or any accessible path
UPLOAD_FOLDER = Path('/home/pi/my_slideshow_images')
```

**Important**:
- Directory must exist or be auto-created
- Flask user must have write permissions
- Ensure sufficient disk space

### Maximum Images

**File**: `main.py` (line 21)

```python
MAX_IMAGES = 1000
```

**Purpose**: Maximum number of images to keep in storage

**Behavior**:
- When limit exceeded, oldest images auto-deleted
- Cleanup happens after each upload
- Prevents storage overflow

**Options** (adjust based on storage):
- `100` - Minimum (compact)
- `500` - Moderate
- `1000` - Default (balanced)
- `5000` - Large storage
- `10000` - Very large storage

**Calculation Guide**:
```
Average image size: ~200 KB
For 1000 images: ~200 MB
For 5000 images: ~1 GB
For 10000 images: ~2 GB

Adjust based on:
- Available SD card space
- Image resolution (higher res = larger files)
- Event duration (longer event = more images)
```

**Changing Limit**:
```python
MAX_IMAGES = 500  # Keep only 500 most recent images
```

---

## Image Format Configuration

### Allowed File Extensions

**File**: `main.py` (line 19)

```python
ALLOWED_EXTENSIONS = {'png', 'jpg', 'jpeg', 'gif', 'bmp', 'webp'}
```

**Purpose**: Which image formats can be uploaded

**Supported Formats**:
- `jpg`, `jpeg` - JPEG compressed images (most common)
- `png` - PNG lossless images
- `gif` - Animated GIF support
- `bmp` - Bitmap images
- `webp` - Modern web format

**Changing Formats**:

Only allow JPEG:
```python
ALLOWED_EXTENSIONS = {'jpg', 'jpeg'}
```

Add TIFF support:
```python
ALLOWED_EXTENSIONS = {'png', 'jpg', 'jpeg', 'gif', 'bmp', 'webp', 'tiff'}
```

Remove GIF (save space):
```python
ALLOWED_EXTENSIONS = {'png', 'jpg', 'jpeg', 'bmp', 'webp'}
```

---

## Flask Server Settings

### Host Configuration

**File**: `main.py` (bottom of file, in `app.run()`)

Default:
```python
app.run(host='0.0.0.0', port=PORT, debug=False)
```

**Options**:
- `'0.0.0.0'` - Listen on all network interfaces (default, allows remote access)
- `'127.0.0.1'` - Localhost only (no remote access)
- `'192.168.1.50'` - Specific IP address

**Changing Host**:
```python
# For local testing only
app.run(host='127.0.0.1', port=PORT, debug=False)

# For specific IP
app.run(host='192.168.1.50', port=PORT, debug=False)
```

### Debug Mode

**File**: `main.py` (bottom)

```python
app.run(debug=False)  # Default: disabled
```

**Options**:
- `False` - Production mode (recommended)
- `True` - Development mode (verbose output, auto-reload on code changes)

**Changing Debug**:
```python
# For development/troubleshooting
app.run(debug=True, port=PORT)

# For production
app.run(debug=False, port=PORT)
```

**Note**: Always use `debug=False` in production for security and stability.

---

## Performance Tuning

### For Slower Networks / Pi

**Reduce image quality** (in client code):
```python
# Before uploading, compress image
from PIL import Image
img = Image.open('photo.jpg')
img.save('compressed.jpg', quality=60)  # 60% quality
```

**Reduce storage requirements**:
```python
MAX_IMAGES = 300  # Fewer images kept
ALLOWED_EXTENSIONS = {'jpg'}  # Only JPEG, not PNG/GIF
```

### For Faster Response

**Increase cleanup frequency** (add to code):
```python
# Run cleanup more aggressively
cleanup_old_images()  # Call after each operation
```

### For Large Storage

**Increase image limit**:
```python
MAX_IMAGES = 5000  # Keep more images
```

---

## Integration Configuration

### For Selfie-Cam Integration

In **selfie-cam** (e.g., `app_admin.py`), configure:

```python
# Slideshow server location
SLIDESHOW = "http://192.168.0.133:8080"
UPLOAD_ENDPOINT = f"{SLIDESHOW}/upload"

# After capturing selfie, auto-upload:
requests.post(UPLOAD_ENDPOINT, files={'image': open(selfie_file, 'rb')})
```

### Server URL Configuration

In **remote-slideshow** (`main.py`), ensure port matches:

```python
PORT = 8080  # Must match selfie-cam SLIDESHOW URL
```

If using different port:
```python
# In remote-slideshow
PORT = 8888

# In selfie-cam app_admin.py, update to:
SLIDESHOW = "http://192.168.0.133:8888"
```

---

## Security Configuration

### For Local Network Only (Default)

Current setup is suitable for:
- Local workshops and events
- Trusted networks
- Closed environments (no public internet)

### For Production Deployment

**Add Authentication** (requires code modification):
```python
from flask_httpauth import HTTPAuth
auth = HTTPAuth()

@auth.verify_password
def verify_password(username, password):
    if username == "admin" and password == "secret_password":
        return True
    return False

@app.route('/upload', methods=['POST'])
@auth.login_required
def upload_image():
    # Protected endpoint
```

**Add HTTPS**:
```python
# Requires SSL certificate
app.run(ssl_context='adhoc')  # Needs pyopenssl

# Or better, use reverse proxy (nginx, Apache)
```

**Rate Limiting** (Flask-Limiter):
```python
from flask_limiter import Limiter
limiter = Limiter(app, key_func=lambda: request.remote_addr)

@app.route('/upload', methods=['POST'])
@limiter.limit("5/minute")  # Max 5 uploads per minute per IP
def upload_image():
    # Limited endpoint
```

### For Public Internet

**Must implement**:
- HTTPS/SSL encryption
- Authentication (API keys, passwords)
- Rate limiting
- Input validation
- CORS restrictions
- Logging and monitoring

See deployment guides for production setup.

---

## Slideshow Behavior Configuration

### Image Display Duration

**Default** (in test script):
```python
play_duration = 10  # 10 seconds per image
```

**Client Control** (via upload):
```bash
curl -F "image=@photo.jpg" -F "duration=5" http://192.168.1.50:8080/upload
```

**Control API**:
```python
requests.post('http://192.168.1.50:8080/control', json={
    'action': 'show_new',
    'filename': 'image.jpg',
    'duration': 7  # 7 seconds
})
```

### Automatic Slideshow Speed

In HTML (client-side configuration):
- Modify JavaScript in main.py template to adjust:
  - Transition speed
  - Fade effects
  - Auto-advance timing

---

## Environment Variables (Optional)

For more flexible configuration without editing code:

```python
import os

PORT = int(os.getenv('SLIDESHOW_PORT', 8080))
MAX_IMAGES = int(os.getenv('MAX_IMAGES', 1000))
UPLOAD_FOLDER = Path(os.getenv('UPLOAD_FOLDER', './slideshow_images'))
```

**Usage**:
```bash
export SLIDESHOW_PORT=9000
export MAX_IMAGES=500
python main.py
```

Or in systemd service:
```ini
[Service]
Environment="SLIDESHOW_PORT=8080"
Environment="MAX_IMAGES=1000"
```

---

## Configuration Validation

**Verify Configuration**:
```bash
# Check status endpoint
curl http://localhost:8080/status

# Should show:
# {
#   "status": "running",
#   "upload_folder": "/home/pi/remote-slideshow/slideshow_images",
#   "max_images": 1000,
#   ...
# }
```

**Common Configuration Errors**:

**Port Already in Use**:
```
Error: Address already in use
Solution: Change PORT or kill existing process
```

**Permission Denied**:
```
Error: Permission denied: slideshow_images
Solution: chmod 755 slideshow_images/
```

**Disk Full**:
```
Error: No space left on device
Solution: Increase storage or reduce MAX_IMAGES
```

---

## Configuration Checklist

Before running in production:
- [ ] `PORT` configured (default 8080 is fine)
- [ ] `UPLOAD_FOLDER` exists and is writable
- [ ] `MAX_IMAGES` set appropriate for storage
- [ ] `ALLOWED_EXTENSIONS` includes desired formats
- [ ] `debug=False` for production
- [ ] Firewall allows port 8080 (if enabled)
- [ ] Network interface set correctly (`0.0.0.0` for remote, `127.0.0.1` for local)
- [ ] Storage space available (3x MAX_IMAGES * avg_image_size)
- [ ] Integration URLs match if using with selfie-cam
- [ ] Security settings reviewed for deployment

---

## Additional Resources

- [Setup Guide](Remote-Slideshow-Setup.md)
- [API Reference](Remote-Slideshow-API.md)
- [User Guide](Remote-Slideshow-User-Guide.md)
- [Troubleshooting Guide](Remote-Slideshow-Troubleshooting.md)
