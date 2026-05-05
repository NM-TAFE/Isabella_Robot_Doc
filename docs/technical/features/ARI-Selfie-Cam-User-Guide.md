# ARI Selfie-Cam: User Guide

## Overview
This guide provides instructions for both end users and administrators on how to use the ARI Selfie-Cam application. The application provides three main modes of operation: Selfie capture, Interview recording, and Gallery management.

## Getting Started

### Accessing the Application

**On a Computer or Raspberry Pi (Direct)**:
1. Open a web browser
2. Go to `http://localhost:5000`
3. You'll see the main menu

**From a Phone on the Same Network**:
1. Find the Raspberry Pi's IP address
   - Ask the person running the setup, or
   - Look on the network router device list
2. Open a web browser on your phone
3. Go to `http://[PI_IP]:5000`
   - Example: `http://192.168.1.50:5000`
4. Bookmark this page for easy access

### Main Menu
The home page presents four options:
- **Take a Selfie** - Capture photos using USB camera
- **Interview** - Record video with audio questions
- **Gallery** - View all captured photos and videos
- **Admin** - Control panel with robot integration

---

## End User Guide

### Taking Selfies

**Access**: Click "Take a Selfie" from main menu

**Steps**:
1. Position yourself in front of the USB camera
2. Check that you're centered in the preview (if available)
3. Click the "Take Selfie" button
4. **Countdown**: Screen displays count from 6 to 1
   - Get ready during countdown
   - Final second shows "#issobellatherobot" on screen
5. **Photo Captured**: Picture displays for 5 seconds
   - Check if you like the photo
6. **Saved**: Photo is automatically saved to gallery

**Tips**:
- Good lighting helps photo quality
- Position camera at eye level
- Smile before the countdown completes
- Adjust distance if image is too close/far
- Photos are timestamped automatically

**Photo Naming**:
- Format: `selfie_YYYY-MM-DD_HH-MM-SS.jpg`
- Example: `selfie_2025-06-15_14-30-45.jpg`
- Unique filenames prevent overwriting

### Recording Interviews

**Access**: Click "Interview" from main menu

**Steps**:
1. Position yourself in front of the CSI camera
2. Ensure microphone is positioned for clear audio
3. **Read Question**: Display shows the interview prompt
4. Click "Start Interview" button
5. **Recording**: Video and audio are now being recorded
   - Timer shows how long you've been recording
   - Maximum duration: 5 minutes (or configured limit)
6. Answer the interview question(s)
7. Click "Stop Interview" when finished
8. **Processing**: Video and audio are combined
9. **Saved**: Combined video is saved to gallery

**Tips**:
- Speak clearly and loudly for better audio
- Face the camera directly
- Answer the question fully
- Don't move the microphone during recording
- Takes 1-2 minutes to combine video + audio after stopping

**Interview Questions** (Examples):
- "Nice outfit, tell me about it"
- "What's your favorite feature about ARI?"
- "Tell us about your robotics experience"
- Custom questions can be set by administrator

**Video Naming**:
- Format: `interview_YYYY-MM-DD_HH-MM-SS.mp4`
- Example: `interview_2025-06-15_14-20-15.mp4`

### Viewing the Gallery

**Access**: Click "Gallery" from main menu

**Public Gallery Features**:
- View all photos and videos in chronological order (newest first)
- Click on any image to view larger version
- Click on any video to play
- **Download**: Right-click image/video and select "Save As"
- Organized by date/time in filenames

**Admin Gallery** (see Admin Guide below):
- Additional management options
- Delete individual files
- Clear all files at once

**Photo Format**:
- JPG files (compressed photos)
- Viewable in any web browser
- Can be downloaded and printed

**Video Format**:
- MP4 files (compressed video with audio)
- Playable in any web browser
- Can be downloaded to save permanently

---

## Administrator Guide

### Admin Control Panel

**Access**: Click "Admin" from main menu or go to `http://[PI_IP]:5000/admin`

**Recommended**: Open on a phone for convenient mobile control

The admin panel provides complete control over:
- Camera operations
- ARI robot interaction
- Gallery management
- System status monitoring

### Admin Panel Sections

#### 1. System Status
**Location**: Top of admin panel

**Indicators Show**:
- Camera availability (ready/error)
- Preview status (active/inactive)
- Recording status (recording/idle)
- System time

**Status Updates**: Automatically refresh every 2 seconds

#### 2. Camera Preview

**Purpose**: Monitor what the UVC camera is seeing in real-time

**Controls**:
- **Start Preview**: Begins live camera stream (shows ~10 FPS)
- **Stop Preview**: Stops camera feed
- **Preview Window**: Shows live video from USB camera

**Tips**:
- Use to position camera before taking selfies
- Verify lighting conditions
- Ensure subject is in frame
- Preview uses minimal bandwidth

#### 3. Quick Action Buttons

**Take Selfie Button**:
- Triggers immediate selfie capture (bypasses selfie page)
- Follows standard countdown process
- Results in gallery

**Start/Stop Interview Toggle**:
- Starts interview recording
- Changes to "Stop Interview" while recording
- Combines audio+video when stopped

#### 4. Camera Control Card

**Sub-sections**:
- Preview controls (start/stop camera feed)
- Photo capture (quick selfie button)
- Interview controls (start/stop recording)
- Gallery navigation

#### 5. ARI Robot Control

**Text Input**:
- Type any message you want ARI to say
- Click "Send to ARI" button
- Robot speaks your text using text-to-speech

**Motion Buttons**:
- **Wave Hello**: Robot waves hand in greeting
- **Shake Left**: Robot shakes left
- **Nod Yes**: Robot nods affirmatively
- **Shake No**: Robot shakes head negatively

**Tips**:
- Confirm robot is powered on before commands
- Allow 1-2 seconds for motion execution
- Test connection before events
- Use simple, clear language for TTS

#### 6. Gallery Navigation

**Admin Gallery Button**:
- Opens admin gallery (with delete options)
- Allows file management

**Gallery Button**:
- Opens public gallery view
- Read-only, no delete options

### Admin Gallery Management

**Access**: "Admin Gallery" button from admin panel

**Features**:
- View all captured media (photos and videos)
- See file details (filename, size, date)
- **Delete Individual Files**:
  - Click trash icon next to file
  - Confirm deletion
  - File is permanently removed
- **Download Files**:
  - Click download icon
  - File saved to your computer

**Caution**: Deleted files cannot be recovered. Always backup important media.

### Common Admin Tasks

#### Task 1: Set Up Before Event
1. Open admin panel at `http://[PI_IP]:5000/admin`
2. Click "Start Preview" to verify camera positioning
3. Test ARI robot: Type "Hello everyone!" and click "Send to ARI"
4. Verify microphone is positioned for interview
5. Check gallery has space for new media

#### Task 2: Manage Participant Flow
1. Direct participant to selfie page
2. Monitor preview while they pose
3. Them direct to interview page
4. Use admin panel to monitor recording progress
5. View results in gallery afterward

#### Task 3: Troubleshoot During Event
1. If preview won't start:
   - Check USB camera connection
   - Verify no other app using camera
   - Restart Flask application
2. If ARI not responding:
   - Verify ARI is powered on
   - Check network connectivity
   - Confirm correct IP/hostname
3. If no audio:
   - Check microphone connection
   - Verify USB audio device detected
   - Test audio with system tools

#### Task 4: End-of-Event Cleanup
1. Review gallery with `INTERVIEW_DURATION` setting
2. Backup important videos
3. Clear gallery if needed (caution - irreversible)
4. Stop Flask server when done

---

## Navigation

### Menu Structure

```
Home (/)
├── Selfie (/selfie)
├── Interview (/interview)
├── Gallery (/gallery)
└── Admin (/admin)
    ├── Admin Gallery (/admin-gallery)
    └── Public Gallery (/gallery)
```

### Quick Access from Any Page
- All pages include links back to home menu
- Admin panel always accessible from home
- Gallery accessible from both user and admin modes

---

## Keyboard Shortcuts & Mobile Tips

### Desktop Keyboard
- Spacebar: Take selfie (on selfie page)
- Enter: Start/Stop interview (on interview page)

### Mobile Phone (Recommended for Admin)
- Use portrait orientation for menu
- Landscape orientation for preview
- Tap buttons firmly (touch screen may be sensitive)
- Bookmark admin page for quick access

---

## File Management

### Gallery Storage Location
- All files saved in: `static/gallery/`
- Accessible via web interface
- Backed up or exported via download

### Accessing via File System (Advanced)
From Raspberry Pi command line:
```bash
# View files
ls -lh static/gallery/

# Download all to USB drive
cp -r static/gallery /mnt/usb/

# Archive for backup
tar -czf gallery_backup.tar.gz static/gallery/
```

### File Formats
- **Selfies**: JPG (compressed image format)
- **Interviews**: MP4 (compressed video with audio)
- **Sizes**: JPG ~200-500 KB, MP4 ~5-20 MB

---

## Troubleshooting Common Issues

### Camera Issues

**Selfie camera not working**:
- Check USB cable connection
- Verify camera detected: `lsusb` on Pi terminal
- Restart Flask application
- Try different USB port

**Interview camera not working**:
- Check CSI ribbon cable connection (power off first)
- Run: `vcgencmd get_camera` on Pi terminal
- Restart Flask application

### Audio Issues

**No audio in interview**:
- Check microphone connection
- List audio devices: `arecord -l`
- Test microphone: `arecord -D plughw:1,0 -t wav -d 3 test.wav`
- Verify correct device in app configuration

### ARI Robot Issues

**Robot not responding to commands**:
- Check robot is powered on
- Ping robot: `ping ari-Xc` from Pi terminal
- Verify ARI_BASE_URL is correct
- Check network connectivity

### Preview Not Starting

**Error: "Preview already active"**:
- Stop preview first, then restart
- Or restart Flask application

**Error: "Failed to initialize camera"**:
- Camera may be in use by another application
- Restart Flask application
- Check camera permissions

### Performance Issues

**Slow preview/preview lagging**:
- Close other browser tabs
- Use lower resolution preview
- Check network bandwidth
- Reduce preview frame rate (technical configuration)

---

## Tips for Best Results

### Photography Tips
- **Lighting**: Natural or bright lighting produces best results
- **Distance**: Position 1-2 meters from camera for good framing
- **Backdrop**: Clean, uncluttered background looks professional
- **Positioning**: Center yourself in frame before countdown

### Interview Tips
- **Audio**: Speak clearly and loudly
- **Pace**: Speak naturally at normal pace
- **Duration**: Keep answers concise (2-4 minutes ideal)
- **Energy**: Show enthusiasm and genuine engagement
- **Eye Contact**: Face camera direction throughout

### Admin Tips
- **Backup**: Download important videos regularly
- **Space**: Monitor available storage regularly
- **Testing**: Test all features before running an event
- **Timing**: Allow 1-2 minutes for video+audio combination
- **Documentation**: Note timestamps of important recordings

---

## Data Privacy & Storage

### Where Are Files Stored?
- On Raspberry Pi in `static/gallery/` directory
- Not backed up automatically
- Lost if SD card fails without backup

### Download & Backup
1. Open gallery page
2. Click download on video/photo
3. Files saved to your computer
4. Recommended: Back up after each session

### File Retention
- No automatic deletion
- Must manually delete files or clear gallery
- Gallery can fill up storage if left unchecked

### Security Notes
- No password protection on current setup
- Anyone with network access can view gallery
- Production deployments should add authentication
- See admin for security setup

---

## Additional Resources

- [Setup Guide](ARI-Selfie-Cam-Setup.md) - Installation instructions
- [Admin Control Features](ARI-Selfie-Cam-Admin-Control.md) - Detailed admin controls
- [API Reference](ARI-Selfie-Cam-API.md) - Technical API documentation
- [Troubleshooting Guide](ARI-Selfie-Cam-Troubleshooting.md) - Detailed troubleshooting
