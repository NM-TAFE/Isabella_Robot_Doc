# Remote Slideshow: Troubleshooting Guide

## Quick Troubleshooting Matrix

| Symptom | Possible Cause | Solution |
|---------|----------------|----------|
| Server won't start | Port in use or Python error | Check `sudo lsof -i :8080`, kill process, or change port |
| Can't access web interface | Wrong IP/port or firewall | Verify Pi IP, check port, allow firewall: `sudo ufw allow 8080` |
| Upload fails | Network error or disk full | Check `/status`, verify network, clear old images |
| Images don't appear | Slideshow server not running | Verify server: `curl http://localhost:8080/status` |
| Slideshow freezes | Storage full or network issue | Check disk space: `df -h`, verify connectivity |
| Images upload but don't display | Slideshow browser cache | Refresh page (F5 or Ctrl+Shift+R) |
| Poor performance | Too many images or Pi overloaded | Reduce MAX_IMAGES, close other apps, check network |
| Integration not working | Wrong URL or network disconnect | Verify SLIDESHOW URL in selfie-cam config |

---

## Server Issues

### Flask Server Won't Start

**Error Message**:
```
OSError: [Errno 98] Address already in use
```

**Cause**: Port 8080 already in use

**Solutions**:

**Option 1: Kill Existing Process**
```bash
# Find process using port 8080
sudo lsof -i :8080

# Kill by PID
sudo kill -9 <PID>

# Or kill by process name
sudo pkill -f "python.*main.py"

# Wait 10 seconds and restart
sleep 10
python main.py
```

**Option 2: Use Different Port**
Edit `main.py`:
```python
PORT = 8081  # Change from 8080
```

Then access at: `http://192.168.1.50:8081`

**Option 3: Wait for Port Release**
```bash
# Ports may stay bound for ~60 seconds
sleep 60
python main.py
```

---

### Python/Module Errors

**Error**: `ModuleNotFoundError: No module named 'flask'`

**Solution**:
```bash
# Check dependencies installed
pip list | grep -i flask

# If missing, install
pip install flask pillow

# Or from requirements
pip install -r requirements.txt
```

**Error**: `ImportError: cannot import name 'Path'`

**Solution**: Upgrade Python
```bash
python3 --version  # Should be 3.7+
sudo apt install python3.9  # If needed
```

---

## Network & Access Issues

### Can't Access Web Interface

**Symptom**: `ERR_CONNECTION_REFUSED` or `Unable to connect`

**Step 1: Verify Server is Running**
```bash
# On Pi
ps aux | grep main.py

# Or check port
sudo lsof -i :8080
```

**Step 2: Check Pi IP Address**
```bash
# Get IP
hostname -I

# Example: 192.168.1.50
# Then access: http://192.168.1.50:8080
```

**Step 3: Test Connectivity**
```bash
# From another device on network
ping 192.168.1.50

# Or from same Pi
curl http://localhost:8080
```

**Step 4: Check Firewall**
```bash
# Check firewall status
sudo ufw status

# If enabled and blocking port
sudo ufw allow 8080/tcp
sudo ufw reload
```

**Step 5: Check Router Settings**
- Ensure both devices on same network
- Check if Pi's IP is stable
- Verify WiFi signal strength

---

### Integration Not Working (Selfie-Cam → Slideshow)

**Symptoms**: Selfie-cam uploads don't appear in slideshow

**Check 1: Is slideshow server running?**
```bash
# From selfie-cam Pi, test slideshow connectivity
curl http://192.168.0.133:8080/status

# Should show: "status": "running"
# If not, start slideshow server
```

**Check 2: Verify slideshow URL in selfie-cam**
```bash
# In selfie-cam app_admin.py or droidcam.py
# Verify SLIDESHOW variable:
SLIDESHOW = "http://192.168.0.133:8080"

# IP and port should match actual slideshow server
```

**Check 3: Test manual upload**
```bash
# From selfie-cam Pi
curl -X POST -F "image=@test.jpg" http://192.168.0.133:8080/upload

# Should return success JSON
```

**Check 4: Check firewall on both Pi's**
```bash
# Both Pi's should allow port 8080
sudo ufw allow 8080
```

**Check 5: Monitor network**
```bash
# Check network connectivity between Pi's
ping 192.168.0.133 (from selfie-cam Pi)

# Should get responses without packet loss
```

---

## Upload Issues

### Image Upload Fails

**Error**: `Upload failed` or `500 Internal Server Error`

**Check 1: Is Disk Full?**
```bash
# Check available space
df -h /

# If 85%+ full, clean up
curl -X POST http://localhost:8080/clear
# Or manually delete old images
```

**Check 2: Are Permissions Correct?**
```bash
# Check slideshow_images directory
ls -ld slideshow_images/

# Should show: drwxr-xr-x (755 permissions)
# If not, fix:
chmod 755 slideshow_images/
```

**Check 3: Is Flask Process Still Running?**
```bash
# Check if Flask is still responsive
curl http://localhost:8080/status

# If not responding, restart:
sudo systemctl restart remote-slideshow.service
# Or kill and restart manually
```

**Check 4: Try Simpler Upload**
```bash
# Create minimal test image
convert -size 100x100 xc:red test.jpg

# Try uploading
curl -F "image=@test.jpg" http://localhost:8080/upload
```

### Large Files Fail

**Cause**: File size limit or network timeout

**Solutions**:
```bash
# Compress image before uploading
convert input.jpg -quality 85 output.jpg

# Or use ImageMagick
mogrify -quality 75 *.jpg

# Then upload smaller files
```

---

## Storage & Performance Issues

### Disk Full

**Symptom**: Can't upload images or slideshow freezes

**Check**:
```bash
# Check disk usage
df -h /

# Check slideshow folder size
du -sh slideshow_images/
```

**Solutions**:

**Option 1: Clear Old Images**
```bash
# Delete all images (caution!)
curl -X POST http://localhost:8080/clear

# Or manually
rm slideshow_images/*
```

**Option 2: Reduce MAX_IMAGES**
```python
# In main.py
MAX_IMAGES = 300  # Was 1000

# Cleanup will be more aggressive
```

**Option 3: Archive to USB**
```bash
# Copy images to USB drive
cp -r slideshow_images /mnt/usb/backup/

# Then clear Pi storage
rm slideshow_images/*
```

**Option 4: Expand Storage**
```bash
# Use larger SD card
# Or mount external USB/network storage:
sudo mount /dev/sda1 /mnt/external
# Then update UPLOAD_FOLDER path in main.py
```

---

### Slow Performance

**Symptoms**: 
- Uploads take too long
- Slideshow transitions laggy
- Browser unresponsive

**Check Network**:
```bash
# Test network speed
iperf3 -c 192.168.1.1

# Or simpler ping test
ping -c 10 8.8.8.8

# Watch for high latency or packet loss
```

**Check Storage**:
```bash
# Test disk I/O
dd if=/dev/zero of=test bs=1M count=100
rm test

# If very slow, SD card issue
```

**Check CPU**:
```bash
# Monitor CPU usage
top

# Look for high CPU processes
# Close unnecessary applications
```

**Solutions**:
1. Use WiFi vs Ethernet (Ethernet better)
2. Reduce image quality/size
3. Close other applications
4. Reduce MAX_IMAGES
5. Restart server

---

## Display Issues

### Blank Screen / No Images Showing

**Check 1: Browser Cache**
```bash
# On display browser
# Hard refresh: Ctrl+Shift+R (or Cmd+Shift+R on Mac)
# Or F12 → Application → Clear Storage
```

**Check 2: Any Images Uploaded?**
```bash
# Check image count
curl http://localhost:8080/status | grep image_count

# Should be > 0
# If 0, upload test image:
curl -F "image=@test.jpg" http://localhost:8080/upload
```

**Check 3: Is JavaScript Working?**
```bash
# Check browser console for errors
F12 → Console tab

# Look for JavaScript errors
```

**Check 4: Browser Compatibility**
```bash
# Try different browser
# Chrome, Firefox, Safari should work
# Older IE may have issues
```

---

### Images Show But Don't Change

**Cause**: Slideshow not auto-advancing

**Check**:
1. Is spacebar pressed? (paused) - Press spacebar to resume
2. Browser minimized? - Bring to foreground
3. JavaScript errors? - Check F12 console
4. Network issue? - Verify connectivity

**Solutions**:
```bash
# Manually refresh
Press R key

# Or refresh page
F5 or Ctrl+R

# Or use control API
curl -X POST http://localhost:8080/control \
  -H "Content-Type: application/json" \
  -d '{"action":"refresh"}'
```

### Display Freezes / Becomes Unresponsive

**Check Server Status**:
```bash
# Test if server still responding
curl http://localhost:8080/status

# If no response, server crashed
```

**Restart Server**:
```bash
# Kill existing process
sudo pkill -f "python.*main.py"

# Restart
python main.py
```

**Check System Resources**:
```bash
# CPU/memory usage
free -h
top

# If very high, restart Pi
sudo reboot
```

---

## Integration Troubleshooting

### Selfie-Cam Upload Doesn't Trigger Slideshow

**Verify Integration Enabled**:
In `app_admin.py` or `droidcam.py`, check:
```python
SLIDESHOW = "http://192.168.0.133:8080"  # URL present?
UPLOAD_ENDPOINT = f"{SLIDESHOW}/upload"  # Endpoint constructed?
```

**Verify Upload Code Exists**:
After `take_selfie()` and `stop_interview()`, look for:
```python
requests.post(UPLOAD_ENDPOINT, files={'image': open(file, 'rb')})
```

**Debug Upload**:
```bash
# Add debug print in app_admin.py
print(f"Uploading to: {UPLOAD_ENDPOINT}")

# Watch logs while taking selfie
python app_admin.py

# Should show upload attempt
```

**Test Manually**:
```bash
# Simulate selfie-cam upload
curl -F "image=@selfie.jpg" http://192.168.0.133:8080/upload

# Should appear in slideshow
```

---

## Logging & Debugging

### Enable Verbose Logging

Edit `main.py`:
```python
import logging
logging.basicConfig(level=logging.DEBUG)

# Flask will print more details
app.run(debug=True)  # Also enables auto-reload
```

**Then run**:
```bash
python main.py 2>&1 | tee slideshow.log

# All output saved to slideshow.log
```

### View Systemd Logs

```bash
# If running as service
sudo journalctl -u remote-slideshow.service -f

# Real-time logs
# Shows errors, startup messages, etc.

# Get last N lines
sudo journalctl -u remote-slideshow.service -n 50
```

### Monitor in Real-Time

```bash
# Watch status and images
watch -n 2 'curl http://localhost:8080/status | python -m json.tool'

# Updates every 2 seconds
# Shows image count, storage, etc.
```

---

## Performance Optimization

### For Slow Networks

**Reduce Image Quality**:
- Before uploading: compress with ~70% quality
- Smaller files = faster transfers
- Network bandwidth saved

**Reduce Slideshow Refresh**:
- Edit HTML in main.py template
- Increase delay between transitions
- Less frequent screen updates

**Limit Images**:
```python
MAX_IMAGES = 300  # Was 1000
```

### For Limited Storage

**Automatically Clean Up**:
```python
MAX_IMAGES = 100  # Keep only 100 most recent
```

**Monitor Storage**:
```bash
# Check daily
du -sh slideshow_images/

# Backup and clear monthly
cp -r slideshow_images /mnt/backup/
rm slideshow_images/*
```

---

## Getting Help

### Collect Debug Information

Before asking for help:

```bash
# System info
uname -a
cat /etc/os-release

# Python version
python3 --version

# Flask status
curl http://localhost:8080/status

# Disk space
df -h

# Network connectivity
hostname -I
ping 8.8.8.8 -c 5

# Process info
ps aux | grep main
ps aux | grep flask

# Directory permissions
ls -la slideshow_images/

# Error logs
tail -20 /var/log/syslog
journalctl -n 50
```

### Resources

- **Flask Docs**: https://flask.palletsprojects.com/
- **Raspberry Pi Docs**: https://www.raspberrypi.com/documentation/
- **Python Docs**: https://docs.python.org/3/

---

## Escalation Path

If issue persists:
1. **Verify** - Run through quick matrix at top
2. **Isolate** - Test each component separately
3. **Collect** - Gather debug info above
4. **Try alternative** - Different browser, restart, check logs
5. **Escalate** - Contact system administrator with debug info

---

## Additional Resources

- [Setup Guide](Remote-Slideshow-Setup.md)
- [Configuration Guide](Remote-Slideshow-Configuration.md)
- [Integration Guide](Remote-Slideshow-Selfie-Cam-Integration.md)
- [API Reference](Remote-Slideshow-API.md)
