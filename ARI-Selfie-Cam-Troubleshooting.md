# ARI Selfie-Cam: Troubleshooting Guide

## Quick Troubleshooting Matrix

| Symptom | Possible Cause | Solution |
|---------|----------------|----------|
| Can't access admin page | Wrong IP or port | Check Pi IP: `hostname -I`, verify port 5000 |
| Selfie camera won't work | Camera not connected/detected | Check USB cable, run `lsusb`, `ls /dev/video*` |
| Interview camera won't work | CSI cable loose or not detected | Reseat ribbon cable, check `vcgencmd get_camera` |
| No audio in interview | Microphone not detected | Check `arecord -l`, verify USB connection |
| Preview won't start | Camera already in use | Reload page, restart Flask app |
| ARI not responding | Robot offline or wrong IP | Ping robot: `ping ari-Xc`, verify ARI_BASE_URL |
| Flask won't start | Port in use or permission issue | Kill process: `sudo pkill -f flask`, check permissions |
| Photos not saving | Disk full or permission denied | Check space: `df -h`, verify gallery permissions |

---

## Camera Issues

### CSI Camera Not Detected

**Error Message**: 
```
vcgencmd get_camera
supported=0 detected=0
```

**Causes**:
1. Ribbon cable not connected
2. Ribbon cable inserted backwards
3. Camera disabled in raspi-config
4. Defective camera or cable

**Solutions**:

**Step 1: Check Physical Connection**
```bash
# Power off first!
sudo shutdown -h now

# Then:
# 1. Open camera port
# 2. Reseat ribbon cable (shiny side up)
# 3. Close port firmly
# 4. Power on
```

**Step 2: Enable in Configuration**
```bash
sudo raspi-config
# Navigate: Interface Options → Camera → Enable
# Select "libcamera" when prompted
sudo reboot
```

**Step 3: Verify Detection**
```bash
vcgencmd get_camera
# Should show: supported=1 detected=1
```

**Step 4: Test Camera**
```bash
libcamera-hello --camera 0
# Should display live preview for 5 seconds
```

---

### UVC Camera Not Detected

**Error Message**: 
```
lsusb
# Camera not listed
```

**Causes**:
1. USB cable not connected
2. Camera not powered/functional
3. Wrong USB port
4. Camera driver not loaded

**Solutions**:

**Step 1: Verify Physical Connection**
```bash
# Check if camera appears in USB list
lsusb
# Should see camera listed

# Check video devices
ls -la /dev/video*
# Should show at least /dev/video0
```

**Step 2: Test with Different Port**
```bash
# Try different USB port on Pi
# Some ports may have power issues
```

**Step 3: Verify Driver**
```bash
# Check if camera detected
v4l2-ctl --list-devices

# Should list your camera
```

**Step 4: Test Camera Directly**
```bash
# Test with ffmpeg
ffmpeg -f v4l2 -i /dev/video0 -t 5 -y test.mp4

# Or with libcamera
libcamera-hello --camera 1
```

### Camera Index Incorrect

**Symptoms**:
- Admin panel shows camera ready but captures fail
- Preview shows wrong camera
- Some operations work, others don't

**Diagnosis**:
```bash
# Check which index is which
libcamera-hello --camera 0  # Test camera 0
libcamera-hello --camera 1  # Test camera 1

# Check app logs for camera info
python app_admin.py
# Look for: "Found CSI camera at index X"
#          "Found UVC camera at index X"
```

**Solution**:
Edit `app_admin.py` and manually set indices:

```python
# Around line 55-68 in CameraManager.detect_cameras()
# Add these lines after available_cameras loop:

self.csi_camera_num = 0  # Manually set
self.uvc_camera_num = 1  # Manually set
print(f"Manually set CSI to {self.csi_camera_num}, UVC to {self.uvc_camera_num}")
```

---

## Audio Recording Issues

### No Audio Captured

**Symptoms**:
- Interview video has no sound
- Recording creates MP4 but audio track is silent

**Check Audio Device**:
```bash
# List audio devices
arecord -l

# Example output:
# card 1: Device [USB Device], device 0: USB Audio [USB Audio]

# Format: plughw:CARD,DEVICE (so above is plughw:1,0)
```

**Test Microphone**:
```bash
# Record 3 seconds of audio
arecord -D plughw:1,0 -f cd -t wav -d 3 test.wav

# Play it back
aplay test.wav

# If no sound, microphone isn't working
```

### Audio Device Not Found

**Symptoms**:
```
arecord: main:1476: audio open error: No such device
```

**Causes**:
1. USB microphone not connected
2. Wrong device specified
3. Audio drivers not loaded

**Solutions**:

**Step 1: Verify Connection**
```bash
# Check if USB device appears
lsusb | grep -i audio

# Connect/reconnect microphone
```

**Step 2: Find Correct Device**
```bash
# List all audio devices
arecord -l

# Each line shows: card N: Name [Description], device M: Info

# Common format: plughw:CARD,DEVICE
```

**Step 3: Update Configuration**
Edit `app_admin.py` in `start_interview()` function:

```python
# Find this line:
audio_process = subprocess.Popen([
    'arecord', '-D', 'plughw:1,0',  # Change 1,0 to your device

# Change 'plughw:1,0' to match your device from arecord -l
```

### Permissions Error

**Error Message**:
```
ALSA lib (sniff.c:631) arecord: cannot connect to server
Permission denied
```

**Solution**:
```bash
# Add user to audio group
sudo usermod -a -G audio $USER

# Apply changes immediately
newgrp - audio

# Or reboot for persistent changes
sudo reboot
```

---

## Network & Connectivity Issues

### Flask Server Won't Start

**Error Message**:
```
OSError: [Errno 98] Address already in use
```

**Cause**: Port 5000 already in use

**Solutions**:

**Option 1: Kill Existing Process**
```bash
# Find process using port 5000
sudo lsof -i :5000

# Kill the process by name
sudo pkill -f "python.*app_admin"

# Or kill by PID
sudo kill -9 <PID>
```

**Option 2: Use Different Port**
Edit `app_admin.py` (end of file):

```python
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5001, debug=True, threaded=True)
    # Changed port from 5000 to 5001
```

**Option 3: Wait for Port Release**
```bash
# Ports stay bound for ~60 seconds after close
# Wait a minute and retry
sleep 60
python app_admin.py
```

### Can't Access Admin Panel from Phone

**Symptom**: 
```
ERR_CONNECTION_REFUSED
or
Unable to connect
```

**Diagnosis**:

**Step 1: Verify Pi IP**
```bash
# On Pi, get IP address
hostname -I

# Example: 192.168.1.50
```

**Step 2: Test Connectivity**
```bash
# From phone (if possible)
# Or from computer on same network

ping 192.168.1.50
# Should get response
```

**Step 3: Verify Flask is Running**
```bash
# On Pi, check if Flask is running
ps aux | grep flask

# Or:
lsof -i :5000
# Should show Python process
```

**Step 4: Check Firewall**
```bash
# Check firewall status
sudo ufw status

# If enabled, allow port 5000
sudo ufw allow 5000
```

**Step 5: Test Locally First**
```bash
# On Pi, test locally
curl http://localhost:5000

# Should return HTML of home page
```

### Admin Commands Not Working (Timeouts)

**Symptoms**:
- Buttons don't respond
- Loading indicator stays spinning
- Browser shows "Failed to fetch" error

**Likely Cause**: Network lag or Flask crash

**Solutions**:

**Step 1: Check Network**
```bash
# Test Pi connectivity
ping 8.8.8.8

# Check signal strength (if WiFi)
iwconfig wlan0
```

**Step 2: Check Flask Process**
```bash
# SSH into Pi
# Verify Flask is still running
ps aux | grep flask

# If not running, start it
cd ~/ARI-selfie-cam
source venv/bin/activate
python app_admin.py
```

**Step 3: Refresh Browser**
- Reload admin page (F5 or ⌘R)
- Hard refresh (Ctrl+Shift+R or ⌘Shift+R)
- Clear cache: Browser settings → Clear browsing data

**Step 4: Restart Flask**
```bash
# On Pi terminal
Ctrl+C  # Stop Flask server

# Wait 2-3 seconds
python app_admin.py  # Restart
```

---

## Storage & Performance Issues

### Gallery Fills Up / Disk Full

**Symptoms**:
- Selfies stop being saved
- Error message about disk space
- Performance becomes very slow

**Check Disk Space**:
```bash
# View disk usage
df -h /

# Look at /home or root partition
# If 85%+ full, need to free space
```

**Solutions**:

**Option 1: Download & Delete**
```bash
# Access gallery via web browser
# Download important videos
# Delete files via admin gallery

# Or command line:
ls -lh static/gallery/
rm static/gallery/old_file.mp4
```

**Option 2: Archive to External Storage**
```bash
# Copy gallery to USB drive
cp -r static/gallery /mnt/usb/backup/

# Then delete original
rm -rf static/gallery/*
mkdir -p static/gallery
```

**Option 3: Expand Storage**
```bash
# Use larger microSD card
# Or external USB storage

# Mount USB:
sudo mount /dev/sda1 /mnt/external

# Symlink gallery to external storage:
rm -rf static/gallery
ln -s /mnt/external/gallery static/gallery
```

### Slow Preview / Laggy Camera Feed

**Symptoms**:
- Preview updates slowly
- Large delay between reality and screen
- Choppy video

**Causes**:
1. Network congestion
2. Pi CPU overloaded
3. Poor WiFi signal
4. Too many browser tabs

**Solutions**:

**Immediate**:
1. Close other browser tabs
2. Stop other applications on Pi
3. Move closer to WiFi router
4. Use wired Ethernet if possible

**Configuration** (edit `app_admin.py`):

```python
# Reduce frame rate (around line 327)
time.sleep(0.2)  # Was 0.1 (10 FPS), now 5 FPS

# Reduce JPEG quality (around line 321)
img.save(img_io, 'JPEG', quality=50)  # Was 70, now 50
```

---

## ARI Robot Integration Issues

### Robot Commands Fail

**Error Message**:
```json
{
  "error": "Failed to send TTS to ARI",
  "status_code": 500
}
```

**Diagnosis**:

**Step 1: Verify Robot Powered On**
- Check robot power indicator
- Look for status lights
- Listen for startup sounds

**Step 2: Check Network Connectivity**
```bash
# From Pi, ping robot
ping ari-Xc

# Or use IP
ping 192.168.1.100

# Should get replies
```

**Step 3: Test Robot API Directly**
```bash
# Test TTS
curl -X POST http://ari-Xc/action/tts \
  -H "Content-Type: application/json" \
  -d '{"rawtext":{"text":"Hello","lang_id":"en_GB"}}'

# Should get response (success or error)
```

**Step 4: Verify ARI_BASE_URL**
```bash
# Edit app_admin.py
# Check the ARI_BASE_URL setting

# It should match robot's actual IP or hostname
```

### Motion Commands Execute Slowly

**Symptoms**:
- 5+ second delay before motion starts
- TTS plays but motion lags

**Causes**:
1. Robot processing queue full
2. Network latency
3. Motion file large or complex

**Solutions**:
- Don't send rapid motion commands
- Wait 2-3 seconds between motions
- Check robot system status
- Verify network speed

### TTS Audio Quality Poor

**Symptoms**:
- Robot voice sounds robotic/synthesized
- Words mispronounced
- Accent option doesn't work

**Solutions**:

**Step 1: Verify Language**
```python
# In app_admin.py, check default language
def send_ari_tts(text, lang_id="en_GB"):

# Change to en_US if preferred
def send_ari_tts(text, lang_id="en_US"):
```

**Step 2: Adjust Text**
- Use simpler words
- Spell out acronyms
- Add punctuation for pauses
- Avoid special characters

**Step 3: Check Robot TTS Settings**
- Contact robot administrator
- Check ARI documentation for TTS options
- May need to update ARI configuration

---

## File & Permission Issues

### "Permission Denied" Saving Files

**Error Message**:
```
Permission denied: 'static/gallery/selfie_2025-06-15_14-30-45.jpg'
```

**Causes**:
1. Gallery directory not writable
2. Flask running as wrong user
3. File permissions too restrictive

**Solutions**:

**Step 1: Fix Directory Permissions**
```bash
# Check permissions
ls -ld static/gallery/

# Make writable
chmod 755 static/gallery/

# Or for more lenient permissions
chmod 777 static/gallery/
```

**Step 2: Check Ownership**
```bash
# Check who owns directory
ls -l static/ | grep gallery

# If owned by root, change:
sudo chown -R pi:pi static/gallery/
```

**Step 3: Verify User in Groups**
```bash
# User must be in video/audio groups
groups $USER

# Should show: video audio

# If not, add:
sudo usermod -a -G video,audio $USER
newgrp - $USER
```

---

## Application Crashes

### Flask Crashes Unexpectedly

**Symptoms**:
- Web interface stops responding
- Error appears in terminal
- Process exits

**Common Errors**:

**Memory Error**:
```
MemoryError: Unable to allocate...
```
Solution: Reduce preview resolution or frame rate

**Camera Error**:
```
libcamera error: ...
```
Solution: Camera disconnected, check connections, restart

**File Error**:
```
OSError: [Errno 28] No space left on device
```
Solution: Gallery disk full, delete files

### How to Debug

**Step 1: Check Logs**
```bash
# If running in terminal, read error output
# If running as service, check:
journalctl -u ari-selfie-cam.service -n 50

# Look for error message before crash
```

**Step 2: Reproduce Error**
- Try same operation that crashed
- Check admin logs
- Note exact error message

**Step 3: Review Recent Changes**
- What happened before crash?
- Any new photos/videos?
- Storage full?

**Step 4: Restart Safely**
```bash
# Stop cleanly
sudo systemctl stop ari-selfie-cam.service

# Wait 5 seconds
sleep 5

# Start again
sudo systemctl start ari-selfie-cam.service

# Check status
sudo systemctl status ari-selfie-cam.service
```

---

## Performance Optimization

### For Slow Pi / High Latency

**Network Optimization**:
```python
# In app_admin.py

# Reduce preview quality
img.save(img_io, 'JPEG', quality=40)  # Lower quality

# Reduce preview frame rate
time.sleep(0.25)  # ~4 FPS instead of 10

# Reduce camera resolution
main={"size": (640, 480)}  # Lower resolution
```

**Server Configuration**:
```python
# Single-threaded mode (if not needed)
app.run(threaded=False)

# Disable debug mode (production)
app.run(debug=False)
```

**System Optimization**:
```bash
# Stop unnecessary services
sudo systemctl disable bluetooth.service
sudo systemctl disable cups.service

# Check CPU temperature
vcgencmd measure_temp

# Monitor resources
top

# Reboot if needed
sudo reboot
```

---

## Getting Help

### Collect Debug Information

Before asking for help, collect:

```bash
# Hardware info
uname -a
cat /etc/os-release

# Python version
python3 --version

# Camera status
vcgencmd get_camera
libcamera-hello --camera 0  # Test

# Audio status
arecord -l
arecord -D plughw:1,0 -t wav -d 1 - | aplay

# Network status
hostname -I
ping ari-Xc
curl http://ari-Xc/api/status

# Flask logs
python app_admin.py 2>&1 | head -100

# Disk space
df -h

# Process info
ps aux | grep -i camera
ps aux | grep -i flask
```

### Resources

- **Flask Documentation**: https://flask.palletsprojects.com/
- **picamera2 Documentation**: https://github.com/raspberrypi/picamera2
- **Raspberry Pi Docs**: https://www.raspberrypi.com/documentation/
- **ARI Robot Docs**: Check robot documentation

---

## Escalation Path

If issue persists:
1. Review this entire guide thoroughly
2. Check all configuration options
3. Test each component individually
4. Review Flask application logs
5. Try alternative versions (app_admin.py vs droidcam.py)
6. Contact system administrator with debug info above

---

## Additional Resources

- [Setup Guide](ARI-Selfie-Cam-Setup.md)
- [Configuration Guide](ARI-Selfie-Cam-Configuration.md)
- [API Reference](ARI-Selfie-Cam-API.md)
