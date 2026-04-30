# Component Updates: Isabella Features in ARI 25.01

What Isabella components need attention after upgrading to ARI 25.01.

## Overview

Most Isabella customizations are compatible with ARI 25.01. Some may require re-testing or minor updates.

## Component Status Matrix

| Component | Status | Required Action | Priority |
|-----------|--------|-----------------|----------|
| Live Mode | ✅ Compatible | Re-test, verify config | Medium |
| LLM Server/Ollama | ✅ Compatible | Network verification | Low |
| Social Perception | ✅ Compatible | Re-test, benchmark | Medium |
| ROS Nodes | ✅ Compatible | Verify connections | Medium |
| Localizer | ✅ Compatible | Re-calibrate if needed | Low |
| Hardware Integration | ✅ Compatible | Run diagnostics | Medium |
| Custom Services | ⚠️ Verify | Test each service | High |
| Docker Environment | ⚠️ Rebuild | Rebuild dev image | High |

## Live Mode (STT → LLM → TTS)

### Compatibility

✅ **Fully compatible** with ROS 2 Humble (ARI 25.01 uses Humble)

### Update Steps

1. **Verify Dependencies**
   ```bash
   # Check npm packages still installed
   cd /path/to/live-mode
   npm list
   
   # Reinstall if needed
   npm install
   ```

2. **Test WebSocket Connection**
   ```bash
   # Verify STT server is accessible
   ping <stt-server-ip>
   nc -zv <stt-server-ip> 9090
   ```

3. **Test Ollama Integration**
   ```bash
   # SSH to robot
   ssh ari@ari-<id>
   
   # Verify Ollama is running
   curl http://localhost:11434/api/tags
   
   # Test model inference
   ollama run llama2 "test"
   ```

4. **Test TTS Endpoint**
   ```bash
   # Verify TTS server (typically on robot)
   curl -X POST http://ari-<id>/action/tts -d "Hello, testing TTS"
   ```

5. **Full Live Mode Test**
   - Start Live Mode app
   - Select model
   - Verify connection to STT
   - Speak and verify response pipeline
   - Check Activity Log for events

### Configuration Updates

If you made custom modifications to Live Mode:
- Check `live_mode.js` for hard-coded IP addresses
- Update PAL-specific API calls if any
- Re-test with ROS 2 topic structure (unchanged)

### Potential Issues

- **"Cannot connect to WebSocket"**: Verify STT server IP/port in settings
- **"LLM response is slow"**: Normal, no changes in ARI 25.01
- **"TTS not working"**: Verify TTS server endpoint and network

## LLM Server & Ollama Integration

### Compatibility

✅ **Fully compatible** - Ollama runs independently of ARI OS

### Update Steps

1. **Verify Ollama Installation**
   ```bash
   # Check if Ollama is still installed
   ollama --version
   
   # If missing, reinstall
   curl -fsSL https://ollama.ai/install.sh | sh
   ```

2. **Verify Models**
   ```bash
   # List installed models
   ollama list
   
   # Verify all expected models are present
   # If missing, reinstall: ollama pull model-name
   ```

3. **Test Ollama API**
   ```bash
   # Test API endpoint
   curl http://localhost:11434/api/tags
   
   # Test inference
   curl -X POST http://localhost:11434/api/generate \
     -H "Content-Type: application/json" \
     -d '{"model": "llama2", "prompt": "Hello"}'
   ```

4. **Memory Check**
   ```bash
   # Verify sufficient RAM for LLM
   free -h
   # Should show enough free memory for your models
   ```

### Configuration Updates

No ARI OS version changes affect Ollama configuration:
- Port 11434 unchanged
- API endpoints unchanged
- Model files locations unchanged

### Network Considerations

After upgrade, if using external LLM server:
- Verify network connectivity
- Test firewall rules
- Confirm DNS resolution
- Check API credentials/tokens

## Social Perception

### Compatibility & Improvements

✅ **Fully compatible** - May have improvements in ARI 25.01

**Improvements in 25.01:**
- Better person tracking
- Improved face detection
- Enhanced depth perception

### Update Steps

1. **Test Person Detection**
   ```bash
   # Launch social perception
   ros2 launch <social_perception_package> social_perception.launch.py
   
   # In another terminal, verify topics
   ros2 topic echo /person_detector/people_detections
   ```

2. **Verify Face Detection**
   ```bash
   # Check face detection topic
   ros2 topic echo /face_detector/faces
   ```

3. **Benchmark Performance**
   - Record metrics before/after upgrade
   - Compare detection accuracy
   - Compare processing latency
   - Note improvements for team

4. **Recalibrate if Needed**
   - Check camera calibration files
   - May need minor tuning with new OS
   - Reference: `docs/software/Social-Perception.md`

### Expected Differences

- Person tracking should be smoother
- Face detection may be faster
- Occasional new detections of far-away people (improved sensitivity)

## ROS Nodes & Services

### Compatibility

✅ **Fully compatible** with ROS 2 Humble

### Update Steps

1. **Verify All Custom Nodes**
   ```bash
   # List running nodes
   ros2 node list
   
   # Verify expected nodes are present
   # If missing, check systemd/launch files
   ```

2. **Test Topic Connections**
   ```bash
   # Check all expected topics
   ros2 topic list
   
   # Verify you can echo each critical topic
   ros2 topic echo /robot/topic_name
   ```

3. **Test Services**
   ```bash
   # Call each critical service
   ros2 service call /robot/service_name service_type "data"
   
   # Verify responses are correct
   ```

4. **Check Node Logs**
   ```bash
   # Monitor nodes for errors
   ros2 node info /node_name
   
   # Check for repeated errors
   journalctl -u ros2.service | tail -50
   ```

### Debugging Common Issues

**"Cannot find topic"**
- Node may not be running
- Topic may have different name
- Check node is still available

**"Service call fails"**
- Service may not be registered
- Check node logs for errors
- Verify ROS 2 domain setup

## Localizer

### Compatibility

✅ **Fully compatible** - No changes needed for core functionality

### Update Steps

1. **Verify Localization Running**
   ```bash
   # Check if localizer is active
   ros2 topic echo /amcl_pose
   ```

2. **Recalibration Recommended**
   ```bash
   # After upgrade, recommend re-running calibration
   # Reference: Original Localizer documentation
   # This ensures best performance with new OS
   ```

3. **Test Navigation**
   - Send navigation goals
   - Verify robot reaches targets
   - Monitor localization confidence
   - Compare to pre-upgrade performance

## Hardware Integration

### Compatibility

✅ **Fully compatible** - All hardware unchanged

### Update Steps

1. **Run Hardware Diagnostics**
   - Access Web UI: `https://ari-<id>`
   - Navigate to Diagnostics
   - Verify all components green or stable

2. **Test Each Subsystem**

   **Cameras:**
   ```bash
   ros2 run rqt_image_view rqt_image_view
   # Select /head_front_camera/color/image_raw
   # Verify video stream
   ```

   **LiDAR:**
   ```bash
   ros2 topic echo /scan
   # Verify distance measurements (should be numeric values)
   ```

   **Microphone Array (Respeaker):**
   ```bash
   # Verify audio input
   arecord -l  # Should list devices
   ```

   **Speakers:**
   ```bash
   # Play test sound
   ros2 topic pub /audio_out std_msgs/String "data: 'test.wav'"
   ```

3. **Recalibrate if Needed**
   - Camera intrinsic calibration (if supported)
   - LiDAR offsets (if any custom setup)
   - Microphone array (if using beamforming)

## Custom Services & Integrations

### Compatibility

⚠️ **Verify each custom service** - Depends on implementation

### Audit Steps

1. **Inventory Custom Services**
   - List all non-PAL services
   - List all external integrations
   - Document dependencies

2. **Test Each Service**
   ```bash
   # For each custom service
   ros2 service call /custom_service service_type "data"
   ```

3. **Check External APIs**
   - Verify API endpoints are accessible
   - Test authentication credentials
   - Confirm response formats haven't changed

4. **Monitor Logs**
   ```bash
   # Watch for service-specific errors
   journalctl -f -u custom-service.service
   ```

## Docker Development Environment

### Action Required: Rebuild

⚠️ **Must rebuild Docker image** for development

### Update Steps

1. **Obtain New Build Script**
   - Request from PAL: `<user-id>-build-docker.sh`
   - This was provided with ARI 25.01 package

2. **Build New Image**
   ```bash
   # Run updated build script
   bash /path/to/<user-id>-build-docker.sh
   
   # This creates new image with ARI 25.01 tools
   # Takes 10-20 minutes
   ```

3. **Verify Image**
   ```bash
   # List Docker images
   docker images | grep ari
   
   # Should show new ari-25.01 image
   ```

4. **Start New Container**
   ```bash
   # Stop old container
   docker stop ari-dev
   
   # Start new container with new image
   docker run -it --name ari-dev-new <new-image-name> bash
   ```

## Post-Component Update Validation

After updating all components:

- [ ] Live Mode fully functional
- [ ] Ollama models working
- [ ] Social perception operating
- [ ] All ROS nodes discoverable
- [ ] All services callable
- [ ] Hardware diagnostics green
- [ ] Docker environment ready
- [ ] Custom services tested
- [ ] External integrations verified

## Troubleshooting

### "Module not found" errors

- Reinstall dependencies: `pip install -r requirements.txt`
- Rebuild packages: `colcon build`

### "Connection refused" errors

- Verify services are running
- Check port conflicts
- Verify network connectivity

### "Segmentation fault" errors

- May indicate incompatibility
- Try running under debugger
- Check for dependency version mismatches

## Next Steps

1. Complete [Post-Upgrade Validation](./post-upgrade-validation.md)
2. Run full system tests
3. Monitor for 24 hours
4. Report results to team

## References

- **Live Mode:** `docs/software/Live-Mode.md`
- **LLM Server:** `docs/software/LLM-Server.md`
- **Social Perception:** `docs/software/Social-Perception.md`
- **ROS Nodes:** `docs/software/ROS-Nodes-and-Services.md`
- **Localizer:** `docs/software/Localizer.md`
- **PAL Documentation:** https://docs.pal-robotics.com/25.01/
