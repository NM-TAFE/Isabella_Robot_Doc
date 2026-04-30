# Post-Upgrade Validation & Testing

Verification steps to confirm ARI 25.01 upgrade was successful and Isabella is fully operational.

## Quick Validation (5-10 minutes)

### System Access

- [ ] **SSH Access**
  ```bash
  ssh ari@ari-<id>
  # Should connect without errors
  ```

- [ ] **Verify OS Version**
  ```bash
  cat /etc/os-release
  # Should show ARI 25.01, Ubuntu 22.04/24.04
  ```

- [ ] **ROS 2 Verification**
  ```bash
  ros2 --version
  # Should show ROS 2 Humble
  ```

### Basic Health Check

- [ ] **Network Connectivity**
  ```bash
  ssh ari@ari-<id> "ip addr show"
  # Should show IP addresses
  ```

- [ ] **Disk Space**
  ```bash
  ssh ari@ari-<id> "df -h"
  # Should show adequate free space (> 5GB)
  ```

- [ ] **Memory**
  ```bash
  ssh ari@ari-<id> "free -h"
  # Should show available RAM for LLM
  ```

- [ ] **CPU Temperature**
  ```bash
  ssh ari@ari-<id> "sensors"
  # Should show normal temperatures
  ```

## Web UI Verification (10 minutes)

### Dashboard Access

1. **Open Web Interface**
   - Browser: `https://ari-<id>`
   - Login with admin credentials
   - Should see new ARI 25.01 dashboard

2. **Diagnostics Panel**
   - Click "Diagnostics"
   - View system health status
   - Expected: Most items green or stable
   - Note: Some higher-level items may show yellow (known PAL issue)

3. **System Information**
   - Check listed components
   - Verify all hardware visible:
     - [ ] Cameras
     - [ ] LiDAR
     - [ ] IMU
     - [ ] Batteries
     - [ ] Motors/Joints

4. **Device Management**
   - Check connected devices
   - Verify expected devices present
   - Note any new or missing devices

## ROS 2 System Validation (10-15 minutes)

### ROS 2 Connectivity

```bash
# From development machine

# Verify ROS 2 can see robot topics
ros2 topic list

# Should show topics like:
# /amcl_pose
# /scan
# /cmd_vel
# etc.

# Wait 1-2 minutes for full discovery
# Then verify again
```

### Critical Topics

- [ ] **Camera Topics**
  ```bash
  ros2 topic echo /head_front_camera/color/image_raw
  # Should show image data
  ```

- [ ] **LiDAR Topic**
  ```bash
  ros2 topic echo /scan
  # Should show distance data
  ```

- [ ] **Battery Status**
  ```bash
  ros2 topic echo /power/battery_level
  # Should show battery percentage
  ```

- [ ] **Odometry**
  ```bash
  ros2 topic echo /odom
  # Should show robot pose/velocity
  ```

### ROS 2 Nodes

```bash
# Verify key nodes are running
ros2 node list

# Expected nodes should include:
# - Camera nodes
# - LiDAR node
# - Localization node
# - Navigation nodes
# - Etc.
```

## Isabella Component Testing (20-30 minutes)

### Live Mode Testing

1. **Start Live Mode App**
   ```bash
   cd /path/to/live-mode
   npm start
   ```

2. **Verify Model Selection**
   - [ ] Models list populated
   - [ ] At least one model available

3. **Configure Connection**
   - [ ] WebSocket URL set correctly
   - [ ] STT server IP configured
   - [ ] TTS endpoint configured

4. **Start Live Mode**
   - [ ] Click "Start Live Mode"
   - [ ] Wait for "connected" status
   - [ ] Should show green "Ready" status

5. **Test Full Pipeline**
   - Speak a simple phrase
   - Observe Activity Log:
     - [ ] 🎤 icon appears (speech detected)
     - [ ] 🤖 icon appears (LLM processing)
     - [ ] 🔊 icon appears (TTS output)
     - [ ] Response appears in log
   - Verify audio output from robot

6. **Stop Live Mode**
   - [ ] Click "Stop Live Mode"
   - [ ] Should show "stopped" status

### LLM & Ollama Testing

```bash
# SSH to robot
ssh ari@ari-<id>

# Verify Ollama running
curl http://localhost:11434/api/tags

# Test model inference
curl -X POST http://localhost:11434/api/generate \
  -H "Content-Type: application/json" \
  -d '{"model": "llama2", "prompt": "Hello"}'

# Should return streaming response
```

### Social Perception Testing

```bash
# Verify person detection
ros2 launch social_perception social_perception.launch.py

# In another terminal, check detections
ros2 topic echo /person_detector/people_detections

# Move person in front of robot
# Should see detection data update
```

### ROS Nodes Testing

```bash
# Verify all custom Isabella nodes
ros2 node list | grep -i isabella

# Test each service
ros2 service call /isabella/service_name service_type "data"

# Verify responses
```

## Hardware Subsystem Testing (15-20 minutes)

### Camera Testing

```bash
# View camera stream
ros2 run rqt_image_view rqt_image_view

# Select topic: /head_front_camera/color/image_raw
# Should see video stream from robot's camera
# Verify image quality and clarity
```

### LiDAR Testing

```bash
# Launch LiDAR viewer (if available)
ros2 launch pointcloud_to_laserscan pointcloud_to_laserscan_launch.py

# Or check raw data
ros2 topic echo /scan

# Should show distance measurements
# Verify range values are realistic
```

### Motor Control Testing

```bash
# Keyboard teleop (if available)
ros2 run teleop_twist_keyboard teleop_twist_keyboard

# Or publish manually
ros2 topic pub /cmd_vel geometry_msgs/Twist "linear: {x: 0.1}"

# Robot should move forward
# Verify motors respond smoothly
```

### Microphone Array Testing

```bash
# Test audio input
arecord -l
# Should list audio devices

# Test audio recording
arecord -d 5 test.wav
aplay test.wav

# Should hear recorded audio
```

## System Stability Testing (optional, 30 minutes)

### Performance Monitoring

1. **CPU Usage**
   ```bash
   ssh ari@ari-<id> "top -b -n 1"
   # Verify not maxed out
   # Expected: 30-60% under load
   ```

2. **Memory Usage**
   ```bash
   ssh ari@ari-<id> "free"
   # Verify swap not heavily used
   # Expected: < 20% swap usage
   ```

3. **Temperature**
   ```bash
   ssh ari@ari-<id> "watch -n 1 sensors"
   # Monitor for 5 minutes
   # Expected: < 80°C under normal load
   ```

### Network Performance

```bash
# From development machine
ping -c 100 ari-<id>

# Analyze results
# Expected packet loss: 0%
# Expected latency: < 50ms typical
```

### Long-Running Test

- [ ] Keep Live Mode running for 10 minutes
- [ ] Monitor for errors or disconnections
- [ ] Check memory usage doesn't creep up
- [ ] Verify consistent response times

## Validation Checklist

| Category | Item | Status | Notes |
|----------|------|--------|-------|
| **System** | SSH access | ☐ | |
| | OS version | ☐ | |
| | ROS 2 version | ☐ | |
| | Disk space | ☐ | |
| | Memory available | ☐ | |
| **Web UI** | Dashboard loads | ☐ | |
| | Diagnostics panel | ☐ | |
| | Hardware visible | ☐ | |
| **ROS 2** | Topics available | ☐ | |
| | Camera topic | ☐ | |
| | LiDAR topic | ☐ | |
| | Battery status | ☐ | |
| **Live Mode** | App starts | ☐ | |
| | Connects to STT | ☐ | |
| | LLM responds | ☐ | |
| | TTS working | ☐ | |
| **Hardware** | Cameras working | ☐ | |
| | LiDAR working | ☐ | |
| | Motors responding | ☐ | |
| | Microphone active | ☐ | |
| **Stability** | CPU normal | ☐ | |
| | Memory stable | ☐ | |
| | Temperature normal | ☐ | |
| | Network stable | ☐ | |

## Troubleshooting

### "Cannot connect to robot"

**Solution:**
```bash
# Verify network connectivity
ping ari-<id>

# Check if it was renamed during setup
nmap -p 22 192.168.1.0/24

# If found, SSH to IP address
ssh ari@192.168.1.XXX
```

### "Topics not appearing"

**Solution:**
```bash
# Wait for ROS 2 discovery (can take 1-2 minutes)
# Verify DDS configuration
echo $ROS_DOMAIN_ID

# Check for network issues
ros2 doctor

# May need to reconfigure networking
```

### "Live Mode won't start"

**Solution:**
- Verify STT server is running and accessible
- Check WebSocket URL configuration
- Verify network firewall rules
- Check Ollama is running: `curl localhost:11434`

### "High CPU/Memory Usage"

**Solution:**
- Verify model size isn't too large for hardware
- Check for runaway processes: `top`
- Monitor temperature: `sensors`
- Consider smaller LLM model

### "Audio Issues"

**Solution:**
- Verify microphone array connected
- Check volume levels: `alsamixer`
- Test audio input: `arecord test.wav`
- Verify TTS endpoint accessible

## Post-Validation Steps

After confirming all systems operational:

1. **Update Documentation**
   - Note any observed differences from 23.12
   - Document new features being used
   - Update Isabella feature list

2. **Team Notification**
   - Announce successful upgrade
   - Share any new capabilities
   - Update runbooks/procedures

3. **Continuous Monitoring**
   - Watch system diagnostics for 24-48 hours
   - Monitor application logs
   - Report any issues immediately

4. **Backup New System**
   - Create backup of working 25.01 system
   - Store on external drive
   - Document backup date/procedures

## Success Criteria

Upgrade is successful when:

✅ All systems accessible (SSH, Web UI, ROS 2)  
✅ All hardware subsystems operational  
✅ Isabella components functioning  
✅ Live Mode pipeline working end-to-end  
✅ No console errors or warnings  
✅ System stable over 24-hour period  
✅ Performance meets or exceeds previous version  

## Next Steps

- Monitor system performance over next week
- Gradually resume normal operations
- Report issues to PAL support if discovered
- Plan future maintenance windows
- Share lessons learned with team

---

**Estimated Time:** 1-2 hours (comprehensive validation)  
**Estimated Time:** 15-20 minutes (quick validation)  
**Continue To:** Normal operations or troubleshooting as needed
