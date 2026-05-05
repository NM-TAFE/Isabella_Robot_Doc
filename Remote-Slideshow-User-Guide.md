# Remote Slideshow: User Guide

## Overview

This guide provides instructions for using the Remote Slideshow system. Whether you're viewing the slideshow, uploading images, or managing the server, you'll find step-by-step instructions here.

## Getting Started

### Accessing the Slideshow Display

**On the Raspberry Pi (Direct Display)**:
1. The Pi is connected to a monitor via HDMI
2. Open a web browser and go to `http://localhost:8080`
3. You'll see the slideshow display

**From Another Device on the Network**:
1. Find the Pi's IP address: Ask the administrator or check the Pi terminal
2. Open a web browser
3. Go to `http://192.168.1.50:8080` (replace IP with actual Pi IP)
4. Bookmark this page for easy access

**Mobile Phone** (recommended for uploading):
1. Same URL as above
2. Works in any modern browser
3. Responsive design adapts to screen size

---

## End User Guide

### Viewing the Slideshow

**Display Behavior**:
- Images automatically display one at a time
- Each image shows for 10 seconds (default, configurable)
- Images loop through in order
- Newest uploaded images appear first
- Display shows current image and total count (e.g., "Image 1 of 42")

**Slideshow Pages**:
- **Main**: `http://192.168.1.50:8080/` - Fullscreen slideshow display
- **Gallery**: `http://192.168.1.50:8080/admin` - Grid view with thumbnails

### Understanding the Display

**On Main Slideshow Page**:
- Large image in center of screen
- Image title/filename shown
- Navigation info (current image/total)
- Keyboard controls available (see below)

**On Gallery Admin Page**:
- Grid of thumbnail images
- Click thumbnail to view full-size
- Shows image count and total storage size
- Delete buttons for each image

### Keyboard Controls

**On Main Slideshow Page**:
- **Spacebar**: Pause / Resume slideshow
- **Right Arrow (→)**: Next image
- **Left Arrow (←)**: Previous image
- **R**: Refresh image list
- **F11**: Toggle fullscreen (most browsers)
- **Esc**: Exit fullscreen

**Tips**:
- Use fullscreen mode for display presentations
- Spacebar useful for pausing on important images
- Arrow keys let you navigate manually through images

---

## Uploading Images

### Upload via Web Interface

**From Admin Gallery Page**:
1. Go to `http://192.168.1.50:8080/admin`
2. Scroll to "Upload Image" section
3. Click "Choose File" button
4. Select image from your device (JPG, PNG, GIF, etc.)
5. Optionally set "Display Duration" (default: 10 seconds)
6. Click "Upload" button
7. Wait for upload to complete
8. Image appears in slideshow after upload

**Upload Tips**:
- Landscape orientation works best for displays
- Image resolution: 1920x1080 or larger recommended
- File size: Under 5 MB recommended
- Format: JPG for photos, PNG for graphics

### Upload via Command Line

**From Raspberry Pi**:
```bash
# Basic upload
curl -X POST -F "image=@photo.jpg" http://localhost:8080/upload

# From another device (replace IP)
curl -X POST -F "image=@photo.jpg" http://192.168.1.50:8080/upload
```

**From Phone / Computer**:
```bash
# Send image to slideshow
curl -X POST -F "image=@myimage.jpg" http://192.168.1.50:8080/upload

# With custom duration (5 seconds)
curl -X POST -F "image=@myimage.jpg" -F "duration=5" http://192.168.1.50:8080/upload
```

### Batch Upload Multiple Images

**Using Script**:
```bash
# Upload all JPG files in current directory
for file in *.jpg; do
    curl -X POST -F "image=@$file" http://192.168.1.50:8080/upload
done
```

---

## Viewing Image List

**Gallery View Options**:

**Option 1: Web Admin Gallery**:
1. Go to `http://192.168.1.50:8080/admin`
2. See all uploaded images in grid view
3. Click image to view full-size
4. See file name, size, upload time

**Option 2: Image List API**:
```bash
# Get list of all images
curl http://192.168.1.50:8080/images | python -m json.tool

# Shows: filename, size, modified date, URL for each image
```

**Image Information Shown**:
- Image filename (with timestamp)
- File size in bytes
- When it was uploaded/modified
- Direct link to view individual image

---

## Downloading Images

### Download Individual Image

**From Web Admin Gallery**:
1. Go to `http://192.168.1.50:8080/admin`
2. Find image you want to download
3. Click download icon (or right-click → Save As)
4. Choose location on your device
5. Image saved

**From Command Line**:
```bash
# Download specific image
curl http://192.168.1.50:8080/image/20250615_143045_a1b2c3d4.jpg -o myimage.jpg

# Save with different name
curl http://192.168.1.50:8080/image/filename.jpg -o backup.jpg
```

### Backup All Images

```bash
# Create backup directory
mkdir slideshow_backup

# Download all images
curl http://192.168.1.50:8080/images -s | python3 << 'EOF'
import json, sys, urllib.request, os

data = json.load(sys.stdin)
os.makedirs('slideshow_backup', exist_ok=True)

for img in data['images']:
    url = 'http://192.168.1.50:8080' + img['url']
    filename = os.path.join('slideshow_backup', img['filename'])
    print(f"Downloading {filename}...")
    urllib.request.urlretrieve(url, filename)
EOF
```

---

## Managing Gallery

### View Server Status

**Check Storage and Image Count**:
```bash
curl http://192.168.1.50:8080/status | python -m json.tool

# Shows:
# - Total images uploaded
# - Total storage used (MB)
# - Max images limit
# - Allowed file formats
```

**From Web Admin Gallery**:
1. Go to `http://192.168.1.50:8080/admin`
2. Status shown at top (e.g., "42 images, 125.5 MB used")

### Delete Individual Image

**From Web Admin Gallery**:
1. Go to `http://192.168.1.50:8080/admin`
2. Find image to delete
3. Click "×" or delete button
4. Confirm deletion
5. Image removed from slideshow

**From Command Line**:
```bash
curl -X POST http://192.168.1.50:8080/delete/20250615_143045_a1b2c3d4.jpg
```

### Clear All Images

**From Web Admin Gallery** (caution!):
1. Go to `http://192.168.1.50:8080/admin`
2. Click "Clear All Images" button
3. Confirm you want to delete all images
4. Gallery emptied (all images deleted!)

**From Command Line**:
```bash
# This deletes ALL images!
curl -X POST http://192.168.1.50:8080/clear

# Use with caution - no undo!
```

**⚠️ Warning**: Clearing gallery is permanent. Download/backup important images first.

---

## System Operator Guide

### Starting the Server

**Manual Start**:
```bash
cd ~/remote-slideshow
source venv/bin/activate
python main.py

# Should show:
# 🎬 Remote Slideshow Server
# 📍 Local IP: 192.168.1.50
# 🌐 Access: http://192.168.1.50:8080
```

**Auto-Start via Systemd** (if configured):
- Server starts automatically on Pi boot
- Check status: `sudo systemctl status remote-slideshow.service`

### Monitoring Server

**Check if Server is Running**:
```bash
# From command line
curl http://localhost:8080/status

# Or in browser
http://192.168.1.50:8080/status

# Should show status: "running"
```

**View Server Logs** (if running as service):
```bash
sudo journalctl -u remote-slideshow.service -f

# Shows real-time server activity
```

### Stopping the Server

**If Running Manually**:
- Press `Ctrl+C` in terminal
- Server will stop

**If Running as Service**:
```bash
sudo systemctl stop remote-slideshow.service
```

### Restarting the Server

**If Running Manually**:
```bash
# Stop (Ctrl+C) then restart
python main.py
```

**If Running as Service**:
```bash
sudo systemctl restart remote-slideshow.service
```

### Monitoring Storage

**Check Available Space**:
```bash
# View disk usage
df -h

# Check slideshow folder size
du -sh slideshow_images/

# Monitor in real-time
watch du -sh slideshow_images/
```

**Storage Management**:
- Server auto-deletes oldest images when limit (1000) is reached
- Manual cleanup via `/clear` endpoint if needed
- Move images to USB drive if permanent backup needed

### Performance Tuning

**If Server is Slow**:
1. Check storage (if full, delete old images)
2. Check network (WiFi signal strength)
3. Reduce image resolution on source
4. Reduce MAX_IMAGES if too many stored

**If Uploads are Failing**:
1. Check server status: `curl http://localhost:8080/status`
2. Check disk space: `df -h`
3. Verify network connectivity: `ping 8.8.8.8`
4. Restart server if needed

---

## Tips & Best Practices

### Event Usage

**Before Event**:
- Clear old images: `curl -X POST http://192.168.1.50:8080/clear`
- Test upload works
- Verify server is accessible from all locations
- Set appropriate image duration (5-10 seconds typical)

**During Event**:
- Monitor storage (check status endpoint)
- Have backup Pi in case of failure
- Keep power adapter plugged in (no battery issues)

**After Event**:
- Download important images
- Back up gallery to USB/cloud
- Clear images for next event

### Image Optimization

**For Better Performance**:
- Use JPEG format (smaller file size)
- Resize images to ~1920x1080 (display resolution)
- Compress images (aim for <2 MB each)
- Avoid animated GIFs (slower display)

**Command-line Resize** (requires imagemagick):
```bash
# Resize all JPG files to 1920x1080
for file in *.jpg; do
    convert "$file" -resize 1920x1080 "resized_$file"
done
```

### Presentation Tips

**For Professional Display**:
- Use landscape images (not portrait)
- Consistent image sizes
- Clean, professional photos
- Remove personal/test images before viewing

**Slideshow Settings**:
- 5 seconds: Quick scrolling (fast-paced event)
- 10 seconds: Default (standard viewing)
- 15-20 seconds: Detailed images (people need time to read/observe)

### Network Tips

**Remote Access**:
- Bookmark URL: `http://192.168.1.50:8080`
- Use static IP for reliable access
- If on different networks, may need VPN
- Always on same WiFi as Pi for local network

---

## Common Questions

**Q: How long do images stay in gallery?**
A: Until oldest images are auto-deleted when limit (1000) is reached, or manually cleared.

**Q: What happens if I upload while slideshow is playing?**
A: New image appears in list, automatically included in slideshow rotation.

**Q: Can multiple people upload simultaneously?**
A: Yes, server handles concurrent uploads. They queue automatically.

**Q: Is there a maximum image size?**
A: Technically no, but recommend keeping under 5 MB. Very large images may display slower.

**Q: Can I download all images at once?**
A: Yes, use the backup script in "Backup All Images" section above.

**Q: What if server crashes?**
A: Restart server (`python main.py` or `systemctl restart`). Images are saved, no data loss.

---

## Additional Resources

- [Setup Guide](Remote-Slideshow-Setup.md)
- [API Reference](Remote-Slideshow-API.md)
- [Integration Guide](Remote-Slideshow-Selfie-Cam-Integration.md)
- [Troubleshooting Guide](Remote-Slideshow-Troubleshooting.md)
