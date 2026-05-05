# Remote Slideshow: API Reference

## Overview

The Remote Slideshow provides a comprehensive REST API for uploading images, controlling the display, and managing stored media. All endpoints return JSON responses and support both HTTP and command-line access.

## Base URL

```
http://[PI_IP]:8080
```

## Response Format

### Success Response
```json
{
  "success": true,
  "message": "Operation successful",
  "data": {}
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
- `400 Bad Request` - Invalid parameters or missing data
- `404 Not Found` - Resource not found
- `500 Internal Server Error` - Server error

---

## Page Routes (HTML Returns)

### Home / Slideshow Display
**Route**: `GET /`
**Description**: Main slideshow display page
**Returns**: HTML page with embedded JavaScript slideshow
**Access**: All users
**Features**:
- Displays images in slideshow format
- Configurable display duration per image
- Keyboard controls (space, arrows, R)
- Full-screen compatible

---

## Image Upload & Management

### Upload Image
**Route**: `POST /upload`
**Description**: Upload image file to slideshow
**Access**: All users
**Content-Type**: `multipart/form-data`

**Request Parameters**:
```
- image (file, required): Image file to upload
- duration (integer, optional): Display duration in seconds (default: 10)
```

**Response - Success (200)**:
```json
{
  "success": true,
  "filename": "20250615_143045_a1b2c3d4.jpg",
  "timestamp": "2025-06-15T14:30:45.123456",
  "size": 245678,
  "action": "show_new",
  "duration": 10
}
```

**Response - Error (400)**:
```json
{
  "error": "Invalid file type. Allowed: png, jpg, jpeg, gif, bmp, webp",
  "status_code": 400
}
```

**Errors**:
- `400` - No file provided, empty filename, or invalid file type
- `500` - Save failed (disk space, permissions)

**Example Requests**:

cURL:
```bash
# Basic upload
curl -X POST -F "image=@photo.jpg" http://192.168.1.50:8080/upload

# With custom duration (5 seconds)
curl -X POST -F "image=@photo.jpg" -F "duration=5" http://192.168.1.50:8080/upload
```

Python:
```python
import requests

files = {'image': open('photo.jpg', 'rb')}
data = {'duration': 10}
response = requests.post('http://192.168.1.50:8080/upload', files=files, data=data)
print(response.json())
```

JavaScript/Fetch:
```javascript
const formData = new FormData();
formData.append('image', fileInput.files[0]);
formData.append('duration', 10);

fetch('http://192.168.1.50:8080/upload', {
  method: 'POST',
  body: formData
})
.then(r => r.json())
.then(data => console.log(data));
```

---

### List All Images
**Route**: `GET /images`
**Description**: Get list of all stored images
**Access**: All users
**Parameters**: None

**Response - Success (200)**:
```json
{
  "images": [
    {
      "filename": "20250615_143045_a1b2c3d4.jpg",
      "size": 245678,
      "modified": "2025-06-15T14:30:45.123456",
      "url": "/image/20250615_143045_a1b2c3d4.jpg"
    },
    {
      "filename": "20250615_142930_b2c3d4e5.png",
      "size": 185432,
      "modified": "2025-06-15T14:29:30.654321",
      "url": "/image/20250615_142930_b2c3d4e5.png"
    }
  ],
  "count": 2,
  "timestamp": "2025-06-15T14:35:00.000000"
}
```

**Example**:
```bash
curl http://192.168.1.50:8080/images | python -m json.tool
```

---

### Get Individual Image
**Route**: `GET /image/<filename>`
**Description**: Retrieve specific image file
**Access**: All users

**Path Parameters**:
- `filename` (string) - Name of image file (e.g., "20250615_143045_a1b2c3d4.jpg")

**Response - Success (200)**:
- Raw image file with appropriate Content-Type header

**Response - Error (404)**:
```json
{
  "error": "Image not found",
  "status_code": 404
}
```

**Example**:
```bash
# Download image
curl http://192.168.1.50:8080/image/20250615_143045_a1b2c3d4.jpg -o downloaded.jpg
```

---

### Delete Image
**Route**: `POST /delete/<filename>`
**Description**: Delete specific image from storage
**Access**: All users

**Path Parameters**:
- `filename` (string) - Name of image to delete

**Response - Success (200)**:
```json
{
  "success": true,
  "message": "Image deleted",
  "filename": "20250615_143045_a1b2c3d4.jpg"
}
```

**Response - Error (404)**:
```json
{
  "error": "Image not found",
  "status_code": 404
}
```

**Example**:
```bash
curl -X POST http://192.168.1.50:8080/delete/20250615_143045_a1b2c3d4.jpg
```

---

### Clear All Images
**Route**: `POST /clear`
**Description**: Delete all images from storage (careful!)
**Access**: All users
**Parameters**: None

**Response - Success (200)**:
```json
{
  "success": true,
  "cleared_count": 42,
  "timestamp": "2025-06-15T14:35:00.000000"
}
```

**Example**:
```bash
curl -X POST http://192.168.1.50:8080/clear
```

---

## Slideshow Control

### Control Slideshow
**Route**: `POST /control`
**Description**: Send control commands to slideshow
**Content-Type**: `application/json`

**Request Body** (JSON):
```json
{
  "action": "show_new",
  "filename": "20250615_143045_a1b2c3d4.jpg",
  "duration": 10
}
```

**Supported Actions**:
- `show_new` - Display specific image for duration
- `pause` - Pause current slideshow
- `resume` - Resume slideshow
- `next` - Go to next image
- `prev` - Go to previous image
- `first` - Go to first image
- `last` - Go to last image
- `refresh` - Reload image list

**Response - Success (200)**:
```json
{
  "success": true,
  "action": "show_new",
  "filename": "20250615_143045_a1b2c3d4.jpg",
  "duration": 10,
  "message": "Displaying image"
}
```

**Response - Error (400)**:
```json
{
  "error": "No data provided",
  "status_code": 400
}
```

**Example Requests**:

cURL - Show specific image:
```bash
curl -X POST http://192.168.1.50:8080/control \
  -H "Content-Type: application/json" \
  -d '{"action":"show_new","filename":"20250615_143045_a1b2c3d4.jpg","duration":5}'
```

cURL - Pause slideshow:
```bash
curl -X POST http://192.168.1.50:8080/control \
  -H "Content-Type: application/json" \
  -d '{"action":"pause"}'
```

Python:
```python
import requests

data = {
  'action': 'show_new',
  'filename': '20250615_143045_a1b2c3d4.jpg',
  'duration': 10
}
response = requests.post('http://192.168.1.50:8080/control', json=data)
print(response.json())
```

JavaScript:
```javascript
const data = {
  action: 'show_new',
  filename: '20250615_143045_a1b2c3d4.jpg',
  duration: 10
};

fetch('http://192.168.1.50:8080/control', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(data)
})
.then(r => r.json())
.then(data => console.log(data));
```

---

## Server Status

### Get Server Status
**Route**: `GET /status`
**Description**: Get server and storage status
**Access**: All users
**Parameters**: None

**Response (200)**:
```json
{
  "status": "running",
  "image_count": 42,
  "total_size_mb": 125.45,
  "upload_folder": "/home/pi/remote-slideshow/slideshow_images",
  "max_images": 1000,
  "allowed_extensions": ["png", "jpg", "jpeg", "gif", "bmp", "webp"],
  "timestamp": "2025-06-15T14:35:00.000000"
}
```

**Example**:
```bash
curl http://192.168.1.50:8080/status | python -m json.tool

# Output:
# {
#   "status": "running",
#   "image_count": 42,
#   "total_size_mb": 125.45,
#   ...
# }
```

---

## Admin Gallery

### Admin Gallery Interface
**Route**: `GET /admin`
**Description**: Web interface for gallery management
**Returns**: HTML page with management controls
**Access**: All users

**Features**:
- View all images in grid
- Delete individual images
- Delete all images
- View image metadata
- Download images
- Image count and storage stats

---

## Integration Workflows

### Upload from Selfie-Cam

When integrated with ARI Selfie-Cam, images are automatically uploaded:

```python
# Selfie-cam automatically does this after capturing
UPLOAD_ENDPOINT = "http://192.168.0.133:8080/upload"
files = {'image': open('selfie_2025-06-15_14-30-45.jpg', 'rb')}
response = requests.post(UPLOAD_ENDPOINT, files=files)
```

### Display Selfie Immediately

Control API to display captured selfie:

```python
show_command = {
    'action': 'show_new',
    'filename': uploaded_filename,
    'duration': 10  # Show for 10 seconds
}
requests.post('http://192.168.0.133:8080/control', json=show_command)
```

---

## Error Handling

### Common Errors

**Invalid File Type**:
```json
{
  "error": "Invalid file type. Allowed: png, jpg, jpeg, gif, bmp, webp",
  "status_code": 400
}
```
**Solution**: Upload image in allowed format

**Storage Full**:
```json
{
  "error": "Storage limit reached",
  "status_code": 500
}
```
**Solution**: Delete old images via `/clear` or `/delete/<filename>`

**Network Timeout**:
- Check Pi network connectivity
- Verify firewall allows port 8080
- Try again with larger timeout

---

## CORS Headers

All API responses include CORS headers for cross-origin requests:
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, DELETE, OPTIONS
Access-Control-Allow-Headers: Content-Type
```

---

## Rate Limiting & Performance

**No built-in rate limiting** - implement in production.

**Performance Guidelines**:
- ~1000 images max (before automatic cleanup)
- Upload speed: ~1-5 seconds per image
- Slideshow: ~10 FPS updates
- Network: Recommend 10+ Mbps for smooth operation

---

## Additional Resources

- [Setup Guide](Remote-Slideshow-Setup.md)
- [User Guide](Remote-Slideshow-User-Guide.md)
- [Integration Guide](Remote-Slideshow-Selfie-Cam-Integration.md)
- [Configuration Reference](Remote-Slideshow-Configuration.md)
