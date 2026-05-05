# ARI Selfie-Cam: Architecture & Design Overview

## System Architecture

The ARI Selfie-Cam is a web-based application built with Flask, designed to run on a Raspberry Pi and provide remote camera control with ARI robot integration.

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────┐
│          Client Layer (Web Browser)                      │
│  ┌──────────────────────────────────────────────────┐   │
│  │ Admin Panel │ Selfie │ Interview │ Gallery       │   │
│  └──────────────────────────────────────────────────┘   │
└──────────────────────┬──────────────────────────────────┘
                       │ HTTP/WebSocket
┌──────────────────────▼──────────────────────────────────┐
│        Flask Web Application Layer                       │
│  ┌──────────────────────────────────────────────────┐   │
│  │ Routes    │ API Endpoints │ WebSocket Handlers   │   │
│  └──────────────────────────────────────────────────┘   │
└──────────────────────┬──────────────────────────────────┘
                       │
       ┌───────────────┼───────────────┐
       │               │               │
┌──────▼──────┐  ┌────▼────┐  ┌──────▼────────┐
│ CameraManager│  │ ARI Robot│  │ File System   │
│             │  │Interface │  │ (Gallery)     │
└──────┬──────┘  └────┬────┘  └──────┬────────┘
       │              │              │
   ┌───▼──┐      ┌────▼────┐    ┌──▼───────┐
   │CSI   │      │REST API │    │JPG/MP4   │
   │UVC   │      │(TTS,    │    │files     │
   │Audio │      │Motion)  │    │          │
   └──────┘      └─────────┘    └──────────┘
```

---

## Component Overview

### 1. Flask Web Application

**File**: `app_admin.py` (or `droidcam.py` for alternative)

**Responsibilities**:
- HTTP server hosting web interface
- Route handling for all pages
- API endpoint implementation
- Session management
- Static file serving (CSS, JS, images)

**Key Functions**:
- `app.route()` decorators define routes
- Request/response handling
- JSON API for frontend communication
- Error handling and logging

### 2. CameraManager Class

**File**: `app_admin.py` (lines ~33-150+)

**Purpose**: Manage all camera and audio operations

**Attributes**:
```python
self.csi_camera          # CSI camera object (interviews)
self.uvc_camera          # UVC USB camera object (selfies)
self.recording           # Recording state flag
self.audio_process       # Audio recording subprocess
self.current_video       # Current video file path
self.current_audio       # Current audio file path
self.available_cameras   # List of detected cameras
self.preview_active      # Preview stream state
self.csi_camera_num      # CSI camera index
self.uvc_camera_num      # UVC camera index
```

**Key Methods**:

| Method | Purpose |
|--------|---------|
| `detect_cameras()` | Auto-detect available cameras on startup |
| `init_csi_camera()` | Initialize CSI camera for interview recording |
| `init_uvc_camera()` | Initialize UVC camera for selfie capture |
| `init_uvc_preview()` | Setup camera for live preview streaming |
| `cleanup_csi_camera()` | Cleanup and release CSI camera |
| `cleanup_uvc_camera()` | Cleanup and release UVC camera |

### 3. Camera Hardware Integration

**CSI Camera** (Interview Recording):
- Direct ribbon cable connection
- Captures video (1280x720 default)
- Used via picamera2 library
- Process flow:
  1. Initialize with video configuration
  2. Start recording on demand
  3. Capture frames to video file
  4. Stop recording when complete

**UVC USB Camera** (Selfie):
- USB connection (any standard webcam)
- Captures still images
- Used via picamera2 library
- Process flow:
  1. Initialize with still configuration
  2. Capture single frame on request
  3. Save to JPEG file with timestamp
  4. Return file path and success status

**Audio Recording**:
- USB microphone via ALSA
- Records PCM audio to WAV file
- Uses `arecord` subprocess
- Runs parallel to video recording
- Post-processing combines audio+video via ffmpeg

### 4. ARI Robot Integration

**Endpoints Used**:
```
POST /action/tts              # Text-to-speech
POST /action/motion_manager   # Motion commands
```

**TTS Function**:
```python
def send_ari_tts(text, lang_id="en_GB"):
    """Send text-to-speech to ARI"""
    url = f"{ARI_BASE_URL}/action/tts"
    payload = {
        "rawtext": {
            "text": text,
            "lang_id": lang_id
        }
    }
    response = requests.post(url, json=payload, timeout=10)
    return response.json() if response.status_code == 200 else None
```

**Motion Function**:
```python
def send_ari_motion(motion_name):
    """Send motion command to ARI"""
    url = f"{ARI_BASE_URL}/action/motion_manager"
    payload = {"filename": motion_name}
    response = requests.post(url, json=payload, timeout=10)
    return response.json() if response.status_code == 200 else None
```

### 5. File System Organization

```
ARI-selfie-cam/
├── app_admin.py              # Main Flask application
├── droidcam.py               # Alternative (DroidCam) version
├── requirements.txt          # Python dependencies
├── static/
│   ├── bitmap.png            # Logo/image asset
│   └── gallery/              # Photos and videos
│       ├── selfie_*.jpg      # Captured selfies
│       └── interview_*.mp4   # Recorded interviews
├── templates/
│   ├── base.html             # Base template (inherited)
│   ├── index.html            # Home page
│   ├── selfie.html           # Selfie capture page
│   ├── interview.html        # Interview recording page
│   ├── admin.html            # Admin control panel
│   ├── admin-gallery.html    # Admin gallery (with delete)
│   ├── public-gallery.html   # Public gallery (read-only)
│   └── [backup files]        # Previous versions
└── [config/setup files]      # install_script.sh, start_server.sh
```

---

## Data Flow Diagrams

### Selfie Capture Flow

```
User clicks "Take Selfie"
    ↓
POST /api/take_selfie
    ↓
CameraManager.init_uvc_camera()
    ↓
Camera initialized with picamera2
    ↓
Countdown timer (6 seconds)
    ↓
capture_array() → PIL Image
    ↓
Image.save() → JPG file
    ↓
File: static/gallery/selfie_TIMESTAMP.jpg
    ↓
Return success JSON with filename
    ↓
Frontend displays captured image (5 seconds)
    ↓
Gallery updated with new file
```

### Interview Recording Flow

```
User clicks "Start Interview"
    ↓
POST /api/start_interview
    ↓
CameraManager.init_csi_camera() [video]
    ↓
subprocess.Popen(['arecord']) [audio]
    ↓
Both recording started in parallel
    ↓
User clicks "Stop Interview"
    ↓
POST /api/stop_interview
    ↓
Stop video recording → video_file.h264
    ↓
Stop audio recording → audio_file.wav
    ↓
ffmpeg combines:
    video_file.h264 + audio_file.wav
    ↓
Output: interview_TIMESTAMP.mp4
    ↓
File: static/gallery/interview_TIMESTAMP.mp4
    ↓
Temporary files cleaned up
    ↓
Return success JSON
    ↓
Gallery updated with new video
```

### ARI Robot TTS Flow

```
Admin enters text in TTS input
    ↓
Frontend: POST /api/admin/ari_speak
    {text: "Hello!", lang_id: "en_GB"}
    ↓
Backend: send_ari_tts() called
    ↓
requests.post(ARI_BASE_URL/action/tts, json=payload)
    ↓
Network transmission to ARI robot
    ↓
ARI: TTS engine processes text
    ↓
ARI: Text converted to speech audio
    ↓
ARI: Plays audio through speakers
    ↓
Response: HTTP 200 + status JSON
    ↓
Frontend displays: "Message sent successfully"
    ↓
User hears robot speak the message
```

### Preview Streaming Flow

```
Admin clicks "Start Preview"
    ↓
POST /api/admin/start_preview
    ↓
CameraManager.init_uvc_preview()
    ↓
UVC camera initialized
    ↓
camera.start() begins capture
    ↓
preview_active = True
    ↓
Frontend requests GET /admin/camera_feed
    ↓
Backend: generate() function
    Loop: while preview_active:
        capture_array() → frame
        PIL Image conversion
        JPEG encode (70% quality)
        Yield MJPEG frame boundary
    ↓
Browser: <img src="/admin/camera_feed" />
    ↓
Display updates ~10 FPS
    ↓
Admin clicks "Stop Preview"
    ↓
POST /api/admin/stop_preview
    ↓
preview_active = False
    ↓
generate() loop exits
    ↓
Camera cleanup/release
    ↓
Stream ends
```

---

## Threading Model

### Single-Threaded Main Loop
- Flask development server uses single thread by default
- `threaded=True` enabled in `app.run()` for concurrent requests
- Each HTTP request handled in separate thread

### Concurrent Operations

**Supported Simultaneously**:
- Multiple preview requests → Shared camera object
- Take selfie while preview running → Camera switch/initialization
- Multiple admin panel browsers → Read same status variables

**Potential Conflicts**:
- Two simultaneous captures from same camera
- Preview + capture at same time (blocked with checks)
- Interview + selfie simultaneously (separate cameras avoid conflict)

**Synchronization**:
- `threading.Lock()` used for interview_events list
- Camera state flags checked before operations
- State transitions (recording, preview_active) are critical sections

---

## Technology Stack

### Backend
- **Python 3.7+** - Programming language
- **Flask** - Web framework
- **picamera2** - Raspberry Pi camera library
- **requests** - HTTP client (for ARI communication)
- **PIL/Pillow** - Image processing
- **subprocess** - For arecord and ffmpeg

### Frontend
- **HTML5** - Page structure
- **CSS3** - Styling and responsive design
- **JavaScript** - Client-side logic and API calls
- **Fetch API** - Client-server communication

### System Libraries
- **ffmpeg** - Video/audio processing
- **ALSA** - Audio recording and playback
- **libcamera** - Raspberry Pi camera abstraction layer

### Deployment
- **Raspberry Pi OS** - Operating system
- **systemd** - Service management (optional auto-start)

---

## Request/Response Cycle

### Typical Selfie API Call

**1. Client Request**:
```
POST /api/take_selfie
Content-Type: application/json

{}
```

**2. Flask Routing**:
- Route handler: `@app.route('/api/take_selfie', methods=['POST'])`
- Route function: `def take_selfie():`

**3. Processing**:
- Verify UVC camera available
- Initialize camera
- Capture frame
- Save to JPEG
- Get file path

**4. Response**:
```json
{
  "success": true,
  "filename": "selfie_2025-06-15_14-30-45.jpg",
  "filepath": "static/gallery/selfie_2025-06-15_14-30-45.jpg",
  "timestamp": 1750087845
}
```

**5. Client Processing**:
- Parse JSON response
- Display success message
- Update gallery
- Show image preview

---

## Error Handling

### Camera Errors
- Try/except blocks around camera operations
- Return error JSON with HTTP status codes
- Log errors to stdout for debugging

### Network Errors
- Timeout on ARI requests (10 seconds)
- Return error if robot unreachable
- Display error message to admin

### File System Errors
- Permission denied → Check directory permissions
- Disk full → Check available space
- Path not found → Create directories if missing

### Recovery Strategies
1. Check device status indicators
2. Review Flask logs for error details
3. Verify configurations
4. Restart Flask application if needed
5. Check hardware connections

---

## Performance Characteristics

### Resource Usage (Typical)

| Resource | Idle | Preview | Recording |
|----------|------|---------|-----------|
| CPU | 5-10% | 20-30% | 40-50% |
| Memory | 50 MB | 80 MB | 150 MB |
| Network | ~0 Mbps | 2-3 Mbps | 0.1 Mbps |

### Response Times

| Operation | Time |
|-----------|------|
| Preview start | 1-2 seconds |
| Selfie capture | 7 seconds (6s countdown + capture) |
| Interview start | 1 second |
| Interview stop | 1-3 minutes (processing) |
| TTS send | 1-2 seconds (network) + 1-3s (robot) |

### Scalability Limits (Raspberry Pi 3B)
- Concurrent connections: ~5-10 browsers
- Simultaneous cameras: 2 (CSI + UVC)
- Preview stream: Single consumer at ~10 FPS
- Interview duration: 5 minutes max (configurable)

---

## Dependencies

### Python Packages (from requirements.txt)
```
Flask>=2.0.0
requests>=2.25.0
picamera2>=0.3.0
Pillow>=8.0.0
```

### System Packages
```bash
ffmpeg              # Video/audio processing
alsa-utils          # Audio recording
libcamera-apps      # Camera interface tools
python3-opencv      # Optional: Computer vision
```

---

## Configuration & Startup

### Application Startup Sequence

```
1. app = Flask(__name__)
   └─ Create Flask instance

2. app.after_request(after_request)
   └─ Configure CORS headers

3. camera_manager = CameraManager()
   └─ Initialize camera manager
   └─ detect_cameras()
     └─ Load available cameras
     └─ Identify CSI and UVC

4. @app.route() decorators
   └─ Register all routes

5. if __name__ == '__main__':
   └─ app.run(host='0.0.0.0', port=5000, ...)
     └─ Start Flask development server
     └─ Listen on port 5000
     └─ Enable threading
     └─ Begin accepting connections
```

### Configuration Loading
- Hardcoded in `app_admin.py`:
  - `GALLERY_PATH = 'static/gallery'`
  - `INTERVIEW_DURATION = 300`
  - `ARI_BASE_URL = 'http://ari-Xc'`
- Can be overridden by editing Python file
- Can be read from environment variables (not default)

---

## Testing & Debugging

### Debug Output
- Enable `debug=True` in Flask
- Print statements output to console
- Access via SSH or direct terminal

### Logging Queries
```bash
# Check Flask logs (if running in terminal)
# CPU/memory usage
top
# Camera status
vcgencmd get_camera
# Audio devices
arecord -l
# Network connectivity
ping ari-Xc
curl http://ari-Xc/api/status
```

---

## Future Architecture Improvements

### Possible Enhancements

1. **Database**:
   - SQLite for media metadata
   - Gallery indexing and search
   - User management

2. **Async Operations**:
   - Celery for background tasks
   - Video processing queue
   - Non-blocking TTS

3. **Streaming**:
   - WebSocket for real-time updates
   - Automatic preview refresh
   - Live status notifications

4. **Security**:
   - Authentication (JWT, OAuth)
   - Encrypted API endpoints
   - Input validation/sanitization

5. **Scalability**:
   - Multiple camera support per device
   - Load balancing
   - Distributed recording

---

## Additional Resources

- [Setup Guide](ARI-Selfie-Cam-Setup.md)
- [API Reference](ARI-Selfie-Cam-API.md)
- [Configuration Guide](ARI-Selfie-Cam-Configuration.md)
