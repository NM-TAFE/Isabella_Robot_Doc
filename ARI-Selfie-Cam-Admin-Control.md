# ARI Selfie-Cam: Admin Control Features Guide

## Overview

The Admin Control page (`/admin`) provides a comprehensive phone-accessible interface for managing the selfie-cam system and controlling the ARI robot. This page is optimized for mobile devices and displays real-time status information.

## Accessing the Admin Panel

### URL
```
http://[PI_IP]:5000/admin
```

### From Different Devices

**Same Raspberry Pi**:
- Browser: `http://localhost:5000/admin`

**From Phone on Local Network**:
- Browser: `http://192.168.1.50:5000/admin` (replace with Pi's IP)
- Recommended: Bookmark for quick access

**Network Requirements**:
- Phone and Pi must be on same WiFi network
- Direct wired connection to same network preferred
- Firewall must not block port 5000

## Admin Panel Layout

The admin panel is organized into several sections, displayed from top to bottom:

### Section 1: System Status (Top)

**Display Location**: Header area

**Real-Time Indicators**:
- **Camera Status**: Green (Ready) or Red (Error)
  - Shows if USB camera is available
  - Updates every 2 seconds
  
- **Preview Status**: Green (Active) or Gray (Inactive)
  - Shows if camera preview is currently running
  - Indicates live stream availability
  
- **Recording Status**: Red (Recording) or Gray (Idle)
  - Shows if interview is currently being recorded
  - Indicates audio capture status
  
- **System Time**: Current date and time
  - Useful for logging events

**Tip**: Check all indicators are green before starting events

---

## Camera Control Section

### Live Camera Preview

**Purpose**: 
- See exactly what the USB camera sees
- Verify subject positioning before capture
- Test lighting conditions
- Confirm camera is functioning

**Controls**:

**Start Preview Button**:
```
[Start Preview]
```
- Initiates live camera feed
- Feed appears in preview window below
- Shows approximately 10 frames per second
- Uses MJPEG streaming protocol
- Display quality: 70% JPEG compression

**Stop Preview Button**:
```
[Stop Preview]
```
- Halts live camera stream
- Available after preview is started
- Releases camera for other operations
- Reduces network bandwidth

**Preview Window**:
- Displays live camera feed
- Updated approximately 10x per second
- Shows exactly what selfie camera captures
- Automatically scales to screen size

### Usage Tips

**Position Subject**:
1. Start preview
2. Direct subject to position
3. Verify they're centered and in focus
4. Adjust camera or distance as needed
5. Stop preview when ready for actual capture

**Check Lighting**:
- Look for shadows or glare
- Adjust room lights if needed
- Natural window light often works best
- Avoid backlighting (subject in shadow)

**Optimize Frame**:
- Subject should fill most of frame
- Leave some space around edges
- Head positioned in upper-center area
- Good framing = better photos

---

## Quick Action Buttons

### Take Selfie Button

**Button**: 
```
[📸 Take Selfie]
```

**Function**:
- Immediately triggers selfie capture
- Bypasses selfie page interface
- Executes standard 6-second countdown
- Displays flash screen on final second
- Automatically saves to gallery

**Workflow**:
1. Position subject using preview
2. Click "Take Selfie" button
3. Subject gets ready during 6-second countdown
4. Final frame shows "#issobellatherobot"
5. Photo captured and displayed for 5 seconds
6. Automatically saved to gallery

**Tip**: Use after verifying proper positioning with preview

### Interview Toggle

**Button** (Start):
```
[Start Interview]
```

**Button** (Stop):
```
[⏹️ Stop Interview]
```

**Function**:
- Toggles interview recording on/off
- Starts recording video (CSI camera) and audio (microphone)
- Records up to configured maximum (default: 5 minutes)
- Automatically combines video + audio when stopped

**Interview Workflow**:
1. Subject positioned in front of CSI camera
2. Microphone positioned for clear audio
3. Click "Start Interview"
4. Recording status indicator turns red
5. Interview question displayed on screen
6. Subject answers question
7. Click "Stop Interview" when done
8. Processing message appears (1-2 minutes)
9. Video saved to gallery when complete

**Interview Details**:
- Maximum duration: 5 minutes (configurable)
- Audio automatically synced with video
- Timestamp: Recording start time
- Format: MP4 video file with embedded audio
- Size: Typically 5-20 MB per interview

---

## ARI Robot Control Section

### Text-to-Speech (TTS) Control

**Purpose**: 
- Send messages for ARI to speak
- Demonstrate AI capabilities
- Provide audio feedback to participants
- Create interactive experiences

**Text Input Field**:
```
[                                         ]
  Type message here...
```

**Input Tips**:
- Type any message you want ARI to say
- Maximum length: Usually 500 characters
- Supports all languages configured on ARI
- Special characters supported (punctuation, numbers)
- Clear enunciation in spoken output

**Language Selection** (if available):
- Default: English (British) - `en_GB`
- Other options: en_US, es_ES, fr_FR, de_DE
- Selected language applies to TTS output

**"Send to ARI" Button**:
```
[Send to ARI]
```

**Operation**:
1. Type message in text input field
2. Click "Send to ARI" button
3. Message transmitted to ARI robot
4. Robot converts text to speech
5. Audio output from robot speakers
6. Confirmation appears on screen

**Response Time**: 
- Usually 1-2 seconds for robot to start speaking
- Longer for longer messages (more processing)
- Check network status if delay exceeds 5 seconds

**Example Messages**:
- "Hello everyone, welcome to the robotics showcase!"
- "Thank you for participating in this demonstration"
- "Nice to meet you, tell me about your interests"
- "Congratulations on your achievement"

**Error Handling**:
- If robot doesn't respond, check:
  - Robot is powered on
  - Network connectivity
  - ARI_BASE_URL configuration
  - Robot is not already speaking another message

### Motion Control Buttons

**Purpose**:
- Trigger robot gestures and movements
- Add personality and engagement
- Create memorable interactions
- Demonstrate robot expressiveness

**Motion Buttons** (arranged in grid):

#### Wave Hello Button
```
[👋 Wave Hello]
```
- Robot raises arm and waves
- Friendly greeting gesture
- Duration: ~2 seconds
- Use at start of interactions

#### Shake Left Button
```
[↙️ Shake Left]
```
- Robot shakes head/arm to left
- Negative or uncertain gesture
- Duration: ~1.5 seconds
- Use for "no" responses or doubt

#### Nod Yes Button
```
[✓ Nod Yes]
```
- Robot nods head vertically
- Affirmative gesture
- Duration: ~1 second
- Use for agreement or approval

#### Shake No Button
```
[↖️ Shake No]
```
- Robot shakes head side-to-side
- Negation gesture
- Duration: ~1.5 seconds
- Use for rejection or disagreement

**Motion Usage Tips**:
1. Allow 1-2 seconds between motions
2. Combine with TTS for complete responses
3. Test motions work before event
4. Use contextually for engagement
5. Don't spam motions (confusing for participants)

**Example Interaction Sequence**:
```
1. Participant: "Do you like robotics?"
2. Robot: [Wave Hello] (acknowledgment)
3. Send TTS: "I love robotics!"
4. [Nod Yes] (affirmation)
```

**Custom Motions**:
- Motion names are defined on ARI robot
- Administrator can add/configure motions
- Examples above are standard set
- Ask robot administrator for full list

---

## Real-Time Feedback System

### Action Confirmation Messages

**Feedback Indicators**:
- ✓ Success: Green checkmark, "Action completed"
- ⚠ Warning: Yellow warning icon, "Please check..."
- ✗ Error: Red X, "Failed to..."
- ⏳ Processing: Loading indicator, "Working..."

**Examples**:
- "Selfie captured successfully"
- "Interview started"
- "Message sent to ARI"
- "Motion executed"
- "Error: Camera not available"

### Response Time Expectations

| Action | Expected Time |
|--------|----------------|
| Start Preview | 1-2 seconds |
| Take Selfie | 6 seconds (countdown) + 1 second (capture) |
| Start Interview | 1 second |
| Stop Interview | 1-3 minutes (processing) |
| Send TTS | 1-2 seconds (network) + 1-3 seconds (robot processing) |
| Send Motion | 1-2 seconds (network) + 1-3 seconds (execution) |

---

## Status Indicators Reference

### Color Coding

**Green**:
- System ready
- Feature available
- Operation successful
- Camera ready

**Red**:
- Error or alert
- Recording in progress
- Requires attention
- Problem detected

**Yellow/Orange**:
- Warning
- Caution advised
- Check configuration
- Verify before proceeding

**Gray/Inactive**:
- Feature not active
- Standby mode
- Not available
- Disabled

---

## Admin Control Workflow Examples

### Example 1: Quick Photo Event

**Scenario**: Speed photo booth where participants take quick selfies

```
Setup:
1. Open admin panel on phone
2. Click "Start Preview"
3. Verify camera angle and lighting
4. Click "Stop Preview"

For Each Participant:
1. Direct them to stand in frame
2. Click "Take Selfie" button
3. 6-second countdown begins
4. Photo taken automatically
5. Show preview, repeat with next person
```

### Example 2: Interview + Robot Interaction

**Scenario**: Interactive interview with robot engagement

```
Setup:
1. Open admin panel on phone
2. Start preview to verify both cameras
3. Test ARI connection: Send TTS "Hello"
4. Test motion buttons

During Interview:
1. Subject sits in front of CSI camera
2. Click "Start Interview"
3. Display interview question
4. Subject answers
5. Mid-interview, send TTS message: "Great response!"
6. Robot: [Nod Yes] for engagement
7. After interview answer, click "Stop Interview"
8. Wait for processing (1-2 min)
9. Review in gallery

Cleanup:
1. Review recordings
2. Delete failed attempts if needed
3. Backup important videos
```

### Example 3: Multi-Subject Event

**Scenario**: Science fair with multiple participants

```
Setup Phase (Before Event):
1. Test all systems with preview
2. Clear gallery to make space
3. Verify ARI robot is charged and powered
4. Test one complete selfie + interview cycle

During Event (For Each Participant):
1. Show selfie page URL
2. Monitor with preview on admin panel
3. Have them proceed to interview
4. Use admin panel to monitor recording
5. Optional: Send encouraging TTS message
6. View gallery between participants

Cleanup Phase:
1. Download important videos
2. Review statistics (number of photos/videos)
3. Backup all media
4. Clear gallery if next event planned
```

---

## Network & Connectivity

### Optimizing Connection

**WiFi Connection**:
- Place Pi near WiFi router
- Use 5GHz band if available (less interference)
- Keep phone and Pi on same network
- Close other bandwidth-heavy apps

**Wired Connection** (if available):
- Ethernet cable for Pi = most stable
- Phone can stay on WiFi
- Best performance for preview and rapid capture

**Network Issues**:
- Lag in preview → Check WiFi signal
- Lost commands → Move closer to router
- Timeouts → Restart Flask app and admin panel

### Port Configuration

**Default Port**: 5000
- Standard Flask development port
- Change if port already in use
- Configured in `app_admin.py`
- Example: Use 5001 if 5000 occupied

---

## Accessibility Features

### Mobile Optimization

**Responsive Design**:
- Auto-scales to phone screen size
- Touch-friendly button sizing
- Vertical orientation (portrait) for menus
- Horizontal (landscape) for preview

**Touch Controls**:
- Large buttons for easy tapping
- Minimal accidental clicks
- Long-press support for some actions
- Swipe to scroll if needed

### Browser Requirements

**Recommended Browsers**:
- Chrome/Edge (best compatibility)
- Firefox (good support)
- Safari (iOS/Mac)
- Any modern mobile browser

**Compatibility Notes**:
- JavaScript must be enabled
- Cookies should be enabled
- Pop-ups may be blocked (allow if needed)
- Local storage may be used

---

## Troubleshooting Admin Panel

**Problem**: Admin page won't load
- Solution: Check URL is correct (`http://pi-ip:5000/admin`)
- Verify Pi IP address
- Restart Flask application
- Try different browser

**Problem**: Buttons don't respond
- Solution: Check network connection
- Refresh page
- Clear browser cache
- Restart Flask app

**Problem**: Status indicators always red
- Solution: Check camera connections
- Verify permissions (groups)
- Restart Flask application
- Check /dev/video* devices exist

**Problem**: ARI commands fail
- Solution: Check robot is powered on
- Verify correct IP/hostname
- Test network connectivity
- Check ARI_BASE_URL configuration

---

## Advanced Admin Features

### Multi-Device Admin

**Running Multiple Admins Simultaneously**:
- Multiple phones can access admin panel
- Each sees real-time status
- Commands from any device execute
- Useful for team coordination

### Mobile-Specific Features

**Offline Caching**:
- Page works in offline mode (limited)
- Status won't update
- Buttons may not function
- Refresh page when back online

---

## Best Practices

1. **Always test before events**:
   - Test preview works
   - Test TTS with ARI
   - Test all motion buttons
   - Verify camera positioning

2. **Monitor status indicators**:
   - Check green lights before starting
   - Watch for red errors during operation
   - Address warnings immediately

3. **Use preview strategically**:
   - Start preview to verify camera angle
   - Stop when not actively checking
   - Prevents unnecessary processing

4. **Provide feedback to participants**:
   - Use TTS to give instructions
   - Use motions for encouragement
   - Confirm successful captures

5. **Backup important media**:
   - Download videos daily
   - Remove once backed up
   - Prevent storage full errors

---

## Additional Resources

- [User Guide](ARI-Selfie-Cam-User-Guide.md) - End-user instructions
- [API Reference](ARI-Selfie-Cam-API.md) - Detailed API documentation
- [Configuration Guide](ARI-Selfie-Cam-Configuration.md) - Configuration options
- [Troubleshooting Guide](ARI-Selfie-Cam-Troubleshooting.md) - Problem solving
