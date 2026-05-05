# Remote Slideshow: Architecture & Design

## System Overview

The Remote Slideshow is a lightweight Flask web application that serves as a centralized display server for images. It's designed to run on a Raspberry Pi and can operate standalone or integrated with the ARI Selfie-Cam system.

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────┐
│                Client Layer (Web Browser)                │
│  Admin: http://192.168.0.133:8080/admin                │
│  Display: http://192.168.0.133:8080/                   │
│  API Clients: cURL, Python, JavaScript                 │
└────────────────────────┬────────────────────────────────┘
                         │ HTTP/REST
┌────────────────────────▼────────────────────────────────┐
│          Flask Web Application Layer                     │
│  ┌────────────────────────────────────────────────────┐ │
│  │ Routes   │ API Endpoints   │ Response Handlers    │ │
│  └────────────────────────────────────────────────────┘ │
└────────────────────────┬────────────────────────────────┘
                         │
        ┌────────────────┼────────────────┐
        │                │                │
┌───────▼────────┐ ┌─────▼──────┐ ┌──────▼────────────┐
│ File System    │ │ Image      │ │ Display Interface│
│ (Slideshow)    │ │ Processing │ │ (HTML+JS+CSS)   │
│ slideshow_     │ │ (PIL)      │ │                │
│ images/        │ │            │ │                │
└────────────────┘ └────────────┘ └──────────────────┘
```

---

## Component Breakdown

### 1. Flask Web Application

**File**: `main.py` (~1012 lines)

**Responsibilities**:
- HTTP server hosting all endpoints
- Route handling for pages and API
- File upload processing
- Image list management
- Cleanup of old images
- Client request handling

**Key Functions**:
- `@app.route('/')` - Main slideshow display
- `@app.route('/upload', methods=['POST'])` - Image upload
- `@app.route('/admin')` - Admin gallery interface
- `@app.route('/control', methods=['POST'])` - Slideshow control
- `@app.route('/status')` - Server status

### 2. Storage Manager

**Location**: `slideshow_images/` directory

**Responsibilities**:
- Store uploaded images
- Track file metadata
- Auto-cleanup of oldest images when limit reached
- File permissions management

**Cleanup Logic**:
```python
def cleanup_old_images():
    """Remove oldest images if we exceed MAX_IMAGES"""
    # Get all images
    # Sort by modification time (oldest first)
    # Delete oldest when count > MAX_IMAGES
```

### 3. Image Processing

**Library**: PIL (Pillow)

**Functions**:
- Validate image format
- Read image metadata
- Display thumbnail generation
- Image resize/compression (if configured)

**Supported Formats**:
- JPG/JPEG - Compressed photos
- PNG - Lossless images
- GIF - Animated sequences
- BMP - Bitmap format
- WebP - Modern web format

### 4. Display Interface (Frontend)

**Technology**: HTML5, CSS3, JavaScript

**Components**:
- Slideshow viewer (main page)
- Admin gallery interface
- Image controls (pause, next, prev)
- Status display

**JavaScript Features**:
- Auto-rotate through images
- Keyboard event handling
- Image preloading
- Duration timer

### 5. API Layer

**Endpoints**:
- Image upload and management
- Slideshow control commands
- Gallery listing
- Server status monitoring

---

## Data Flow Diagrams

### Image Upload Flow

```
User selects image
    ↓
Upload via form/API (multipart)
    ↓
Flask: /upload POST handler
    ↓
Validate:
  ├─ File provided?
  ├─ Filename not empty?
  └─ File type allowed?
    ↓
Save to slideshow_images/
  ├─ Generate unique filename (timestamp + UUID)
  └─ Write file to disk
    ↓
Check MAX_IMAGES limit
    ↓ If exceeded:
    ├─ Get all images
    ├─ Sort by modification time
    └─ Delete oldest images
    ↓
Return success JSON
  ├─ filename
  ├─ size
  ├─ timestamp
  └─ action: "show_new"
    ↓
Response sent to client
    ↓
Browser updates gallery view
```

### Slideshow Display Flow

```
Browser loads http://192.168.0.133:8080
    ↓
Flask returns HTML template
    ↓
HTML page loaded in browser
    ↓
JavaScript executes:
  ├─ Fetch /images (get list)
  ├─ Parse image list
  └─ Start auto-rotate
    ↓
Display loop:
  ├─ Show current image
  ├─ Wait X seconds (configurable)
  ├─ Go to next image
  └─ Repeat
    ↓
Keyboard input handler:
  ├─ Space → Pause/Resume
  ├─ → → Next image
  ├─ ← → Previous image
  └─ R → Refresh list
```

### Slideshow Control Flow

```
Admin sends POST /control
  {action: "show_new", filename: "...", duration: 10}
    ↓
Flask: /control handler
    ↓
Parse action type
    ↓
If action = "show_new":
  ├─ Get filename from request
  ├─ Verify file exists
  ├─ Get duration (default: 10s)
  └─ Return success
    ↓
Response sent to client
    ↓
Frontend receives action
    ↓
Browser stops auto-rotate
    ↓
Display specific image
    ↓
After duration expires:
  └─ Resume auto-rotate
```

---

## Technology Stack

### Backend
- **Language**: Python 3.7+
- **Framework**: Flask (web framework)
- **Image Processing**: PIL/Pillow (image validation, metadata)
- **File System**: pathlib (Path handling)
- **Date/Time**: datetime (timestamps)

### Frontend
- **HTML5**: Page structure
- **CSS3**: Styling and responsive layout
- **JavaScript (ES6)**: Image rotation, keyboard controls, API calls
- **Fetch API**: Client-side HTTP requests

### System
- **Operating System**: Raspberry Pi OS (Linux)
- **Server**: Flask development server (suitable for local/event use)
- **Network**: HTTP via port 8080 (configurable)

---

## Request/Response Cycle

### Typical Upload Request

**1. Client Request**:
```
POST /upload HTTP/1.1
Content-Type: multipart/form-data

--boundary
Content-Disposition: form-data; name="image"; filename="photo.jpg"
Content-Type: image/jpeg

[binary image data]
--boundary
Content-Disposition: form-data; name="duration"

10
--boundary--
```

**2. Flask Processing**:
- Parse multipart form
- Extract 'image' file and 'duration' parameter
- Validate file type and size
- Generate unique filename

**3. File Save**:
- Write to `slideshow_images/<filename>`
- Record file stats (size, timestamp)

**4. Cleanup Check**:
- Count files in directory
- If > MAX_IMAGES: delete oldest
- Update disk space

**5. Response**:
```json
{
  "success": true,
  "filename": "20250615_143045_a1b2c3d4.jpg",
  "size": 245678,
  "action": "show_new",
  "duration": 10
}
```

---

## Performance Characteristics

### Resource Usage (Typical)

| Metric | Idle | Active Upload | Slideshow Display |
|--------|------|----------------|-------------------|
| CPU | 2-5% | 10-15% | 8-12% |
| Memory | 30 MB | 50 MB | 60 MB |
| Network | ~0 Mbps | 1-5 Mbps | 0.1 Mbps |
| Disk I/O | Minimal | High (write) | Minimal |

### Speed Benchmarks

| Operation | Time |
|-----------|------|
| Image upload | 1-3 seconds (depends on size/network) |
| File save | <100 ms |
| Cleanup scan | <200 ms |
| List images | <500 ms |
| Serve image | <100 ms |
| Slideshow transition | <1 second |

### Scalability Limits (Raspberry Pi 3B)

- Maximum images: 1000 (configurable)
- Maximum image size: Limited by storage
- Concurrent uploads: ~2-5 simultaneous
- Slideshow FPS: ~10 updates/second
- Storage capacity: Depends on SD card size

---

## Threading Model

**Single-threaded** (Flask default):
- Handles one request at a time
- Sufficient for local/event use
- No threading locks needed
- Simple implementation

**Limitations**:
- Slow uploads can block other requests
- For production: Consider concurrent processing
- Multiple simultaneous users may experience delays

---

## File Naming Convention

### Uploaded Image Filename

**Format**: `YYYYMMDD_HHMMSS_XXXXXXXX.EXT`

**Example**: `20250615_143045_a1b2c3d4.jpg`

**Components**:
- `YYYYMMDD` - Date (2025-06-15)
- `HHMMSS` - Time (14:30:45)
- `XXXXXXXX` - 8-char UUID suffix (prevents collisions)
- `.EXT` - File extension (jpg, png, gif, etc.)

**Benefits**:
- Unique identifier (no overwrites)
- Sortable by timestamp
- Human-readable date/time
- Chronological ordering in filesystems

---

## Image Cleanup Mechanism

### Auto-Cleanup Process

**Trigger**: After each image upload

**Logic**:
```python
def cleanup_old_images():
    # 1. Get all image files in directory
    images = list(UPLOAD_FOLDER.glob('*'))
    images = [img for img in images if allowed_file(img.name)]
    
    # 2. Check count
    if len(images) > MAX_IMAGES:
        # 3. Sort by modification time (oldest first)
        images.sort(key=lambda x: x.stat().st_mtime)
        
        # 4. Delete oldest files
        for img in images[:-MAX_IMAGES]:  # Keep only MAX_IMAGES newest
            img.unlink()  # Delete file
```

**Benefits**:
- Prevents storage overflow
- Automatic management (no manual intervention)
- Keeps newest images (most relevant for events)
- Invisible to users

---

## Error Handling

### Upload Errors

**No file provided**:
```json
{"error": "No image file provided", "status_code": 400}
```

**Invalid file type**:
```json
{"error": "Invalid file type. Allowed: ...", "status_code": 400}
```

**Save failed**:
```json
{"error": "Failed to save file", "status_code": 500}
```

### API Errors

**File not found**:
```json
{"error": "Image not found", "status_code": 404}
```

**Server error**:
```json
{"error": "Internal server error", "status_code": 500}
```

---

## Security Considerations

### Current Implementation
- **Authentication**: None (open access)
- **Authorization**: All users can upload/delete
- **Encryption**: HTTP only (not HTTPS)
- **Input validation**: Basic file type check
- **Suitable for**: Trusted local networks, events, workshops

### Production Hardening

**Would require**:
- HTTPS/SSL encryption
- API key or user authentication
- Rate limiting
- Input sanitization
- CORS restrictions
- Access logging
- Reverse proxy (nginx/Apache)

---

## Future Architecture Improvements

### Possible Enhancements

1. **Async Processing**:
   - Celery for background tasks
   - Non-blocking uploads
   - Queued image processing

2. **Database**:
   - SQLite for metadata
   - Image tagging/categorization
   - Usage statistics

3. **Advanced Features**:
   - Image editing tools
   - Collage/montage creation
   - Video support
   - Real-time collaboration

4. **Performance**:
   - Image caching
   - CDN integration
   - Compression optimization
   - Load balancing

---

## Integration Points

### With ARI Selfie-Cam

- **Upload endpoint**: `http://192.168.0.133:8080/upload`
- **Control API**: `POST /control` for immediate display
- **Network protocol**: HTTP REST
- **Payload**: Multipart form (image file)

### With Admin Dashboard
- **Gallery view**: `GET /admin`
- **Management**: Delete, clear, download via web interface
- **Status monitoring**: `GET /status`

---

## Additional Resources

- [Setup Guide](Remote-Slideshow-Setup.md)
- [API Reference](Remote-Slideshow-API.md)
- [Configuration Reference](Remote-Slideshow-Configuration.md)
