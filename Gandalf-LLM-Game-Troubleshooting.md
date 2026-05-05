# Gandalf LLM Game: Troubleshooting Guide

## Quick Troubleshooting Matrix

| Symptom | Likely Cause | First Step |
|---------|--------------|-----------|
| Game won't start | Roscore not running or API unavailable | Check `roscore` and internet |
| Web UI blank | Browser cache or JavaScript error | Clear cache (Ctrl+Shift+Del) |
| "Connection refused" error | API unreachable | Check internet, verify GANDALF_URL |
| Game very slow | Network lag or API overloaded | Wait and retry, check WiFi |
| Can't submit prompts | Python server crashed | Restart main.py |
| Same response every time | Stuck on same level | Try different approach |
| "Invalid defender" error | Wrong level ID | Restart game to reset |
| Web UI won't accept input | JavaScript issue | Try different browser |

---

## Server Issues

### Game Won't Start

**Error**:
```
Traceback: Failed to initialize node
OSError: [Errno 111] Connection refused
```

**Cause**: ROS master not running

**Solution**:
```bash
# Check if roscore is running
pgrep roscore

# If not, start it
roscore &
sleep 2

# Then start gandalf
rosrun nmtafe_gandalf_game main.py

# Or as service:
sudo systemctl start roscore.service
sudo systemctl start gandalf-game.service
```

---

### Python Module Not Found

**Error**:
```
ModuleNotFoundError: No module named 'gandalf_game_api'
```

**Cause**: File path issue or Python path not set

**Solution**:
```bash
# Navigate to scripts directory
cd /home/pi/nmtafe_ws/src/nmtafe_gandalf_game/scripts

# Run from there
python3 main.py

# Or set PYTHONPATH
export PYTHONPATH="${PYTHONPATH}:/home/pi/nmtafe_ws/src/nmtafe_gandalf_game/scripts"
python3 main.py
```

---

### Process Already Running

**Error**:
```
Address already in use / PID already exists
```

**Solution**:
```bash
# Find and kill existing process
ps aux | grep main.py
sudo kill -9 <PID>

# Or kill all gandalf processes
sudo pkill -f "gandalf_game"

# Wait 5 seconds and restart
sleep 5
rosrun nmtafe_gandalf_game main.py
```

---

## Network & Connectivity Issues

### Can't Reach Lakera API

**Error**:
```
Connection Error: Failed to resolve 'gandalf.lakera.ai'
Connection refused to gandalf.lakera.ai:443
Timeout exceeded
```

**Check 1: Internet Connection**
```bash
# Test basic connectivity
ping 8.8.8.8

# If fails: Internet is down
# Fix: Check WiFi, restart network
```

**Check 2: DNS Resolution**
```bash
# Test DNS
nslookup gandalf.lakera.ai
dig gandalf.lakera.ai

# If fails: DNS not working
# Fix: Use 8.8.8.8, restart network
```

**Check 3: API Endpoint Availability**
```bash
# Test direct connection
curl https://gandalf.lakera.ai/

# If times out: API may be down
# Fix: Wait and retry, check status
```

**Check 4: Firewall Block**
```bash
# May block HTTPS
sudo ufw status

# If blocking port 443, allow it:
sudo ufw allow https
sudo ufw reload
```

---

### Web UI Not Accessible

**Error**: Can't reach `http://localhost:8080/` or `http://robot-ip:8080/`

**Check 1: Is Server Running?**
```bash
# Check if Flask server started
ps aux | grep main.py

# If not running, start it:
rosrun nmtafe_gandalf_game main.py
```

**Check 2: Is Port Open?**
```bash
# Check if port is listening
lsof -i :8080

# Or netstat
netstat -tlnp | grep 8080

# If not listening, server crashed
```

**Check 3: Firewall Allows Port**
```bash
# Check firewall
sudo ufw status

# If port blocked, allow it:
sudo ufw allow 8080
sudo ufw reload
```

**Check 4: Try Different Port**
```bash
# Edit main.py
PORT = 5000  # Instead of 8080

# Then access at http://localhost:5000/
```

---

## API & Game Issues

### No Response to Prompts

**Symptom**: Submit prompt, nothing happens (no response text)

**Check 1: Server Running?**
```bash
# Verify gandalf process
ps aux | grep "main.py"

# Verify Flask listening
lsof -i :8080
```

**Check 2: API Responding?**
```bash
# Manual API test
python3 << 'EOF'
from gandalf_game_api import gandalf_game
g = gandalf_game()
result = g.send_message("Test message")
print(result)
EOF

# Should print response dict
```

**Check 3: Browser Console Errors**
```
1. Open web browser
2. Press F12 (Developer Tools)
3. Go to "Console" tab
4. Look for red error messages
5. Share error text for debugging
```

**Solution**: 
- If API test fails: Internet/API issue
- If console shows errors: Browser/JavaScript issue

---

### Slow Responses (>10 seconds)

**Cause 1: Network Lag**
```bash
# Check network latency
ping gandalf.lakera.ai

# If >2000ms: Very slow network
# Try: WiFi closer to router, Ethernet cable
```

**Cause 2: API Overloaded**
```bash
# Try again in a few minutes
# Peak times slower than off-peak
```

**Cause 3: Slow Hardware**
```bash
# Check system resources
top

# If CPU 100% or memory full:
# Close other applications
# Restart system if needed
```

**Solution**:
- Increase timeout in configuration (See Configuration guide)
- Reduce load on system
- Use wired network (Ethernet) instead of WiFi

---

### "Invalid Defender" Error

**Error**:
```json
{"error": "Invalid defender", "message": "..."}
```

**Cause**: Wrong level/defender name

**Solution**:
```bash
# Restart game - resets to level 1
sudo systemctl restart gandalf-game

# Or manually restart:
pkill -f gandalf_game
sleep 2
rosrun nmtafe_gandalf_game main.py
```

---

## Web UI Issues

### Blank Screen

**Cause 1: Browser Cache**
```bash
# Clear cache (Ctrl+Shift+Delete on Windows/Linux)
# Cmd+Shift+Delete on Mac

# Or use private/incognito mode
```

**Cause 2: JavaScript Disabled**
```
1. Check browser settings
2. JavaScript must be enabled
3. Allow JavaScript from localhost
```

**Cause 3: Flask Not Serving Pages**
```bash
# Check if pages directory exists:
ls -la /path/to/nmtafe_gandalf_game/web_pages/

# If missing, extract from repository
# If wrong path, update in main.py
```

---

### Page Won't Load

**Symptom**: "404 Not Found" or "500 Internal Server Error"

**Check 1: Flask Running?**
```bash
# Check process
ps aux | grep main.py

# Check port
lsof -i :8080
```

**Check 2: Static Files Present?**
```bash
# Verify files exist
ls -la ./web_pages/
ls -la ./web_pages/gandalf_game_password_input/

# If missing, git clone/extract
```

**Check 3: File Permissions**
```bash
# Check permissions
ls -la ./web_pages/

# Should be readable (r-x)
chmod 755 ./web_pages/
chmod 755 ./web_pages/gandalf_game_password_input/
```

---

### Input Field Not Working

**Symptom**: Can't type in text field or button doesn't work

**Cause 1: JavaScript Error**
```bash
# Check browser console (F12)
# Look for red error messages
# Common: ROS bridge not connected
```

**Cause 2: ROS Not Connected**
```bash
# Verify roscore running
pgrep roscore

# Check ROS environment
echo $ROS_MASTER_URI
echo $ROS_HOSTNAME

# Should show values, not empty
```

**Solution**:
- Clear browser cache
- Try different browser
- Restart Flask server
- Restart ROS master

---

## Voice Input Issues (ASR/TTS)

### Voice Commands Not Working

**Check 1: ASR Enabled?**
```bash
# Check if speech topic exists
rostopic list | grep speech

# If not, start ASR:
rosrun hri_pal_interaction start_speech

# Or configure in ARI setup
```

**Check 2: Microphone Working?**
```bash
# Record test audio
arecord -d 5 test.wav

# Play it back
aplay test.wav

# If no sound, microphone issue
```

**Check 3: Topic Connected?**
```bash
# Monitor speech topic
rostopic echo /humans/voices/anonymous_speaker/speech

# Speak and watch for messages
# If no messages: Not connecting
```

---

### Robot Speech Not Working (TTS)

**Check 1: TTS Action Available?**
```bash
# List actions
rosaction list | grep tts

# Should show /tts

# If not, start TTS:
rosrun pal_tts tts.py
```

**Check 2: Audio Output Enabled?**
```bash
# Check if speakers are connected
aplay -l

# Should list audio devices

# If no output, test with speaker
speaker-test -t sine -f 500 -l 1
```

**Check 3: Robot Speakers Volume**
```bash
# Check volume
amixer get Master

# If 0%, increase:
amixer sset Master 80%
```

---

## Database & Logging Issues

### Missing Log Files

**Symptom**: Can't find logs for debugging

**Check Log Locations**:
```bash
# ROS logs
ls ~/.ros/log/

# Systemd logs
sudo journalctl -u gandalf-game.service

# Flask logs (if configured)
cat /var/log/gandalf_game.log
```

**If No Logs**:
```bash
# Enable logging in main.py:
logging.basicConfig(
    level=logging.DEBUG,
    filename='/var/log/gandalf_game.log'
)

# Or redirect output:
rosrun nmtafe_gandalf_game main.py 2>&1 | tee gandalf_debug.log
```

---

## Performance Problems

### High CPU Usage

**Symptom**: System slow, main.py using 80%+ CPU

**Cause**: Tight loop or API polling

**Solution**:
```bash
# Check what's using CPU
top -p $(pgrep -f main.py)

# If API calls continuous:
# Slow down request rate in code
# Add sleep between requests: time.sleep(1)

# Restart process:
pkill -f main.py
sleep 2
rosrun nmtafe_gandalf_game main.py
```

### Memory Leak

**Symptom**: Process memory grows over time

**Check Memory**:
```bash
# Monitor memory
watch -n 2 'ps aux | grep main.py'

# If growing continuously: Memory leak

# Solution: Restart regularly
sudo systemctl restart gandalf-game
```

---

## Integration Problems

### ARI Not Responding

**Error**: TTS/ASR commands sent but no response

**Check 1: ARI Services Running?**
```bash
# List ARI services
rosservice list | grep tts
rostopic list | grep speech

# If not present: Start ARI
```

**Check 2: ROS Network**
```bash
# Check connectivity to ARI
rostopic echo /ari_status

# Or ping ARI directly
ping <ari-ip>
```

**Check 3: ROS Master URI**
```bash
# Must point to same ROS master
echo $ROS_MASTER_URI

# If different from ARI:
# Set same ROS_MASTER_URI
export ROS_MASTER_URI=http://ari-ip:11311
```

---

## Escalation Checklist

Before contacting support:

- [ ] Restarted Flask server: `pkill -f main.py`
- [ ] Restarted ROS: `pkill roscore && sleep 2 && roscore &`
- [ ] Restarted robot/Pi: `sudo reboot`
- [ ] Checked internet connection: `ping 8.8.8.8`
- [ ] Tested API directly: Run Python test
- [ ] Checked browser console: F12 → Console tab
- [ ] Cleared browser cache: Ctrl+Shift+Delete
- [ ] Updated code: `git pull && git status`
- [ ] Collected logs: `journalctl -u gandalf-game.service -n 50`

---

## Debug Commands Reference

### System Status
```bash
# Check all services
sudo systemctl status gandalf-game
sudo systemctl status roscore

# View recent logs
sudo journalctl -u gandalf-game.service -n 20 -f

# Monitor resources
top -p $(pgrep -f main.py)
```

### ROS Diagnostics
```bash
# List nodes
rosnode list

# List topics
rostopic list

# Check graph
rqt_graph

# Echo a topic
rostopic echo /NMTAFE_gandalf_game/password_attempt
```

### Network Testing
```bash
# Test API
curl https://gandalf.lakera.ai/

# Test local web UI
curl http://localhost:8080/

# Check open ports
netstat -tlnp | grep 8080
lsof -i :8080
```

### Python Debugging
```bash
# Test gandalf_game module
python3 -c "from gandalf_game_api import gandalf_game; print(gandalf_game())"

# Test with detailed output
python3 -c "from gandalf_game_api import *; import logging; logging.basicConfig(level=logging.DEBUG)"
```

---

## Getting Help

**Include in bug report**:
1. Error message (exact text)
2. Steps to reproduce
3. Logs from last 20 minutes
4. System info: `uname -a`, `python3 --version`
5. Network status: `ping gandalf.lakera.ai`
6. What you tried already

**Resources**:
- [Setup Guide](Gandalf-LLM-Game-Setup.md)
- [Configuration Guide](Gandalf-LLM-Game-Configuration.md)
- [API Reference](Gandalf-LLM-Game-API.md)

---

**Can't find your issue? Check the system logs! 🔍**
