# ARI Selfie-Cam: API Reference

## Overview
The ARI Selfie-Cam Flask application provides multiple endpoints for camera control, media management, and ARI robot integration. This document details all available endpoints, request/response formats, and error handling.

## Base URL
```
http://[PI_IP]:5000
```

## Response Format
All API responses use JSON format with the following structure:

### Success Response
```json
{
  "success": true,
  "message": "Operation completed",
  "data": { /* operation-specific data */ }
}
```

### Error Response
```json
{
  "error": "Error description",
  "status_code": 400
}
```

## HTTP Status Codes
- `200 OK` - Request successful
- `201 Created` - Resource created
- `400 Bad Request` - Invalid parameters
- `404 Not Found` - Resource not found
- `500 Internal Server Error` - Server error

---

## Page Routes (HTML Returns)

### Home Page
**Route**: `GET /`
**Description**: Main menu page
**Returns**: HTML template (index.html)
**Access**: All users

### Admin Control Page
**Route**: `GET /admin`
**Description**: Phone-based admin control panel with camera preview and ARI controls
**Returns**: HTML template (admin.html)
**Features**:
- Live camera feed preview
- ARI robot text-to-speech input
- Motion control buttons
- Quick action buttons (take selfie, interview)
- System status indicators

### Selfie Page
**Route**: `GET /selfie`
**Description**: Selfie capture interface
**Returns**: HTML template (selfie.html)
**Features**:
- UVC camera countdown interface
- Flash screen display
- Gallery preview

### Interview Page
**Route**: `GET /interview`
**Description**: Interview recording interface
**Returns**: HTML template (interview.html)
**Features**:
- Recording controls
- Interview prompt display
- Duration timer

### Public Gallery
**Route**: `GET /gallery`
**Description**: Public-facing gallery view (read-only)
**Returns**: HTML template (public-gallery.html)
**Features**:
- Display all photos and videos
- Download functionality
- Organized by timestamp

### Admin Gallery
**Route**: `GET /admin-gallery`
**Description**: Admin gallery with management features
**Returns**: HTML template (admin-gallery.html)
**Features**:
- Display all media with management options
- Delete individual files
- Download files
- Admin-only access

---

## Camera Control API

### Take Selfie
**Route**: `POST /api/take_selfie`
**Description**: Capture a selfie using UVC camera with countdown
**Access**: Admin/Operator

**Request Body** (optional):
```json
{
  "countdown": 6  // seconds (default: 6)
}
```

**Response - Success (200)**:
```json
{
  "success": true,
  "filename": "selfie_2025-06-15_14-30-45.jpg",
  "filepath": "static/gallery/selfie_2025-06-15_14-30-45.jpg",
  "timestamp": 1750087845
}
```

**Response - Error (400/500)**:
```json
{
  "error": "UVC camera not found",
  "status_code": 400
}
```

**Errors**:
- `400` - UVC camera not found or not initialized
- `500` - Capture failed (permission, disk space, etc.)

---

### Start Interview
**Route**: `POST /api/start_interview`
**Description**: Begin interview recording with CSI camera and audio
**Access**: Admin/Operator

**Request Body** (optional):
```json
{
  "prompt": "Nice outfit, tell me about it"  // Interview question (optional)
}
```

**Response - Success (200)**:
```json
{
  "success": true,
  "message": "Interview recording started",
  "interview_id": "interview_2025-06-15_14-30",
  "prompt": "Nice outfit, tell me about it"
}
```

**Response - Error (400/500)**:
```json
{
  "error": "CSI camera not found",
  "status_code": 400
}
```

**Notes**:
- Interview auto-stops after `INTERVIEW_DURATION` seconds (default: 300)
- Continues until `/api/stop_interview` is called
- Audio and video are automatically combined on stop

---

### Stop Interview
**Route**: `POST /api/stop_interview`
**Description**: End current interview recording and save combined video+audio
**Access**: Admin/Operator

**Request Body**: None (empty)

**Response - Success (200)**:
```json
{
  "success": true,
  "message": "Interview recording stopped",
  "filename": "interview_2025-06-15_14-30-15.mp4",
  "filepath": "static/gallery/interview_2025-06-15_14-30-15.mp4",
  "duration": 15
}
```

**Response - Error (400)**:
```json
{
  "error": "No interview in progress",
  "status_code": 400
}
```

---

## Admin Control API

### Send TTS to ARI
**Route**: `POST /api/admin/ari_speak`
**Description**: Send text-to-speech command to ARI robot
**Access**: Admin/Operator

**Request Body**:
```json
{
  "text": "Hello everyone!",
  "lang_id": "en_GB"  // Language code (optional, default: en_GB)
}
```

**Response - Success (200)**:
```json
{
  "success": true,
  "result": {
    "status": "speaking",
    "duration": 2.5
  }
}
```

**Response - Error (400/500)**:
```json
{
  "error": "No text provided",
  "status_code": 400
}
```

**Response - Error (Robot unreachable)**:
```json
{
  "error": "Failed to send TTS to ARI",
  "status_code": 500
}
```

**Language Codes**:
- `en_GB` - English (UK)
- `en_US` - English (US)
- `es_ES` - Spanish
- `fr_FR` - French
- `de_DE` - German

---

### Send Motion to ARI
**Route**: `POST /api/admin/ari_motion`
**Description**: Send motion command to ARI robot
**Access**: Admin/Operator

**Request Body**:
```json
{
  "motion": "wave_hello"  // Motion filename or name
}
```

**Response - Success (200)**:
```json
{
  "success": true,
  "result": {
    "status": "motion_complete",
    "motion": "wave_hello",
    "duration": 2.0
  }
}
```

**Response - Error (400)**:
```json
{
  "error": "No motion specified",
  "status_code": 400
}
```

**Common Motion Names** (configurable on ARI):
- `wave_hello` - Wave hand gesture
- `shake_left` - Shake left
- `nod_yes` - Nod affirmatively
- `shake_no` - Shake negatively
- Other custom motions defined on ARI

---

### Start Camera Preview
**Route**: `POST /api/admin/start_preview`
**Description**: Start UVC camera preview stream for admin monitoring
**Access**: Admin/Operator

**Request Body**: None (empty)

**Response - Success (200)**:
```json
{
  "success": true,
  "message": "Preview started"
}
```

**Response - Error (400)**:
```json
{
  "error": "Preview already active",
  "status_code": 400
}
```

**Response - Error (500)**:
```json
{
  "error": "Failed to initialize camera for preview",
  "status_code": 500
}
```

---

### Stop Camera Preview
**Route**: `POST /api/admin/stop_preview`
**Description**: Stop active camera preview stream
**Access**: Admin/Operator

**Request Body**: None (empty)

**Response - Success (200)**:
```json
{
  "success": true,
  "message": "Preview stopped"
}
```

**Response - Error (400)**:
```json
{
  "error": "Preview not active",
  "status_code": 400
}
```

---

### Get Camera Feed Stream
**Route**: `GET /admin/camera_feed`
**Description**: Live MJPEG stream for browser display during preview
**Access**: Admin/Operator

**Returns**: MJPEG video stream
```
Content-Type: multipart/x-mixed-replace; boundary=frame
```

**Usage in HTML**:
```html
<img src="/admin/camera_feed" alt="Live Camera Feed" />
```

**Frame Rate**: ~10 FPS (configurable in code)
**Quality**: 70% JPEG quality

**Notes**:
- Only active when preview is started
- Auto-stops when preview ends
- Stream ends if client disconnects

---

## Gallery Management API

### Get Gallery Contents
**Route**: `GET /api/gallery`
**Description**: Retrieve list of all captured media files
**Access**: All users

**Query Parameters**: None

**Response - Success (200)**:
```json
{
  "success": true,
  "files": [
    {
      "filename": "selfie_2025-06-15_14-30-45.jpg",
      "type": "image",
      "size": 245678,
      "timestamp": 1750087845,
      "date": "2025-06-15 14:30:45"
    },
    {
      "filename": "interview_2025-06-15_14-20-15.mp4",
      "type": "video",
      "size": 1245678,
      "timestamp": 1750087215,
      "date": "2025-06-15 14:20:15"
    }
  ]
}
```

---

### Delete File
**Route**: `DELETE /api/delete_file/<filename>`
**Description**: Delete a specific file from gallery
**Access**: Admin/Operator

**Path Parameters**:
- `filename` (string) - Name of file to delete (e.g., "selfie_2025-06-15_14-30-45.jpg")

**Request Body**: None (empty)

**Response - Success (200)**:
```json
{
  "success": true,
  "message": "File deleted",
  "filename": "selfie_2025-06-15_14-30-45.jpg"
}
```

**Response - Error (404)**:
```json
{
  "error": "File not found",
  "status_code": 404
}
```

**Response - Error (403)**:
```json
{
  "error": "Permission denied",
  "status_code": 403
}
```

**Notes**:
- File must exist in gallery directory
- CORS enabled for cross-origin requests
- Use OPTIONS method for preflight requests

---

### Clear Gallery
**Route**: `POST /api/clear_gallery`
**Description**: Delete all files from gallery (use with caution)
**Access**: Admin/Operator

**Request Body**: None (empty)

**Response - Success (200)**:
```json
{
  "success": true,
  "message": "Gallery cleared",
  "files_deleted": 15
}
```

---

### Download File
**Route**: `GET /download/<filename>`
**Description**: Download a specific file from gallery
**Access**: All users

**Path Parameters**:
- `filename` (string) - Name of file to download

**Response**: File download with appropriate Content-Type header
- Images: `image/jpeg`, `image/png`
- Videos: `video/mp4`

**Response - Error (404)**:
```
404 Not Found
```

---

## CORS Headers

All API responses include CORS headers:
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: Content-Type,Authorization
Access-Control-Allow-Methods: GET,PUT,POST,DELETE,OPTIONS
```

---

## Error Handling

### Common Error Scenarios

**Camera Not Initialized**
```json
{
  "error": "UVC camera not found",
  "status_code": 400
}
```
**Solution**: Ensure USB camera is connected and detected

**ARI Robot Unreachable**
```json
{
  "error": "Failed to send TTS to ARI",
  "status_code": 500
}
```
**Solution**: Verify `ARI_BASE_URL` configuration and network connectivity

**Disk Space Full**
```json
{
  "error": "Failed to save file",
  "status_code": 500
}
```
**Solution**: Delete old media files or expand storage

**Permission Denied**
```json
{
  "error": "Permission denied",
  "status_code": 403
}
```
**Solution**: Ensure user is in `video` and `audio` groups

---

## Authentication Notes

Current implementation has **no authentication**. For production use, consider adding:
- API key validation
- JWT token authentication
- IP whitelist access control
- Session-based authentication

See admin control panel documentation for deployment considerations.

---

## Rate Limiting

No built-in rate limiting. In production, implement:
- Request throttling per IP
- Concurrent operation limits
- Queue management for multiple requests

---

## Integration Examples

### cURL Examples

**Take Selfie**:
```bash
curl -X POST http://192.168.1.50:5000/api/take_selfie
```

**Send TTS**:
```bash
curl -X POST http://192.168.1.50:5000/api/admin/ari_speak \
  -H "Content-Type: application/json" \
  -d '{"text":"Hello!","lang_id":"en_GB"}'
```

**Send Motion**:
```bash
curl -X POST http://192.168.1.50:5000/api/admin/ari_motion \
  -H "Content-Type: application/json" \
  -d '{"motion":"wave_hello"}'
```

### JavaScript Examples

**Fetch Selfie**:
```javascript
fetch('http://pi-ip:5000/api/take_selfie', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' }
})
.then(r => r.json())
.then(data => console.log(data));
```

**Fetch ARI Speak**:
```javascript
fetch('http://pi-ip:5000/api/admin/ari_speak', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ text: 'Hello everyone!' })
})
.then(r => r.json())
.then(data => console.log(data));
```

---

## Additional Resources

- [Setup Guide](ARI-Selfie-Cam-Setup.md)
- [User Guide](ARI-Selfie-Cam-User-Guide.md)
- [Admin Control Features](ARI-Selfie-Cam-Admin-Control.md)
- [Configuration Reference](ARI-Selfie-Cam-Configuration.md)
