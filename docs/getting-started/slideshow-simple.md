# Slideshow Simple Guide

Display photos automatically on a screen while ARI does other things.

## What is Remote Slideshow?

A system that:
1. **Captures photos** (via ARI or manually)
2. **Automatically uploads** them to a separate display
3. **Shows them on a big screen** in real-time
4. **Engages your audience** ("Hey, that's me on the screen!")

Perfect for events where you want media displayed in a different location from where ARI is operating.

## Why Use It?

### Problem
You're running a demo with ARI, but the photos only save to ARI's disk. Nobody sees them in real-time.

### Solution
```
ARI takes photo
    ↓
Automatically uploads
    ↓
Appears on big screen instantly
    ↓
Audience: Wow! That's us!
```

## Core Concept (Simple)

```
Location A: Where ARI operates
    │ (takes photos with Selfie-Cam)
    ↓
    Automatic upload
    ↓
Location B: Remote display
    └─> Big screen shows slideshow
```

**Key Point**: No manual file transfers. No USB sticks. Automatic.

---

## How It Works

### Setup Flow (One-Time)

```
1. Slideshow server running on separate Raspberry Pi
2. ARI configured to auto-upload photos
3. Both connected to same network
4. When ARI takes photo → Automatically appears on screen
```

### During Your Event

```
[ARI with Selfie-Cam running] ──(auto-upload)──> [Slideshow Server]
         ↓
    Captures photos              [Display Screen]
    (via app)                         ↓
    ↓                          Shows slideshow
    Photos appear                Real-time updates
    automatically
```

---

## What You See

### On ARI's Screen
- Normal ARI activity
- Photo capture indicator
- Control interface

### On Slideshow Screen
- Photos appear one at a time
- Each photo displays for 3-5 seconds (configurable)
- Smooth transitions between photos
- Shows most recent photos first

### Example Sequence
```
[Empty screen] → [Photo 1 appears] → [Wait 3 seconds] → [Photo 2 appears] → ...
```

---

## Setup (Simple Version)

### Requirements
- ARI robot
- Second Raspberry Pi or computer (for display)
- TV or monitor for display
- Same WiFi network

### Steps
```
1. Start slideshow server on Pi
2. Connect display to Pi
3. Configure ARI to send photos
4. Test one photo
5. Done!
```

For detailed setup: See technical documentation

---

## Using Slideshow During Events

### Before Event
```
✅ Slideshow server running
✅ Display turned on and visible
✅ Network connection verified
✅ Test: Take one photo, see it appear
```

### During Event
```
1. Use Selfie-Cam app to control ARI
2. Tap camera button to capture photos
3. Photos appear on slideshow screen automatically
4. Audience sees them in real-time
```

### After Event
```
1. Photos saved on slideshow server
2. Copy them for later use
3. Clean up storage (old photos auto-delete)
```

---

## Real-World Scenarios

### Trade Show Booth
```
Demo floor: ARI circulates
    ↓ (taking photos)
Booth screen: Shows slideshow
    ↓
Booth visitors see themselves
    ↓
"That's me!" moments
    ↓
Organic engagement
```

### Conference Event
```
Main stage: ARI presents
    ↓ (takes photos of audience)
Hallway screen: Shows slideshow
    ↓
Attendees see who's at event
    ↓
Networking catalyst
```

### Wedding Reception
```
Venue A: ARI circulates, captures candids
    ↓ (automatic upload)
Venue B: Big screen shows slideshow
    ↓
Guests see themselves having fun
    ↓
Extended engagement
```

### School Event
```
Auditorium: ARI demos new feature
    ↓ (photos of students)
Lobby screen: Shows slideshow
    ↓
Students see themselves
    ↓
Word spreads, excitement builds
```

---

## Features You Get

### Automatic Upload ✨
- No manual file management
- Photos transfer instantly
- No USB drives needed
- Hands-free operation

### Real-Time Display ⏱️
- Photos appear within seconds
- Configurable display duration
- Smooth transitions

### Auto-Cleanup 🧹
- Keeps only recent photos
- Prevents storage from filling up
- Configurable retention

### Multi-Screen Support 🖥️
- Works with any TV or monitor
- Supports common resolutions
- Full-screen display

### Web Interface 🌐
- Manage from browser
- No special software needed
- Monitor from anywhere

---

## Photo Management

### Where Photos Go
```
[ARI takes photo]
    ↓
[Uploaded to slideshow server]
    ↓
[Stored on Pi disk]
    ↓
[Displayed on screen]
    ↓
[Auto-deleted after retention period]
```

### Retention Settings
- **Default**: Keep last 100 photos
- **Configurable**: Adjust based on storage
- **Manual cleanup**: Delete any time

### Exporting Photos
- Copy from slideshow server after event
- Create slideshow video
- Share on social media

---

## Frequently Asked Questions

### Q: Can I use just ARI without Selfie-Cam?
A: Yes! You can upload photos manually via the web interface.

### Q: Does the display need to be connected to ARI?
A: No—it's on a separate Pi/computer.

### Q: What if the slideshow server crashes?
A: Photos stop uploading. Restart server to resume.

### Q: Can I control the slideshow timing?
A: Yes—configure how long each photo displays (3-10 seconds typical).

### Q: Is there a limit to photo size?
A: Yes—optimize for screen resolution to keep upload fast.

### Q: Can I run slideshow on a phone or tablet?
A: Not easily—designed for dedicated display.

### Q: What happens when storage fills up?
A: Old photos auto-delete to make room for new ones.

### Q: Can I have multiple displays?
A: Not with current setup—but technically possible (see technical docs).

---

## Troubleshooting

### Photos not appearing on screen
- ❌ Slideshow server running? → Start it
- ❌ Network connected? → Check WiFi
- ❌ ARI configured for upload? → Check settings
- ❌ Storage full? → Clean up old photos

### Display blank or black
- ❌ Pi powered on? → Check power
- ❌ Display connected? → Check cable
- ❌ Display resolution correct? → Adjust settings
- ❌ Server running? → Check status

### Photos slow to appear
- ❌ Large image files? → Compress images
- ❌ Slow network? → Check WiFi signal strength
- ❌ Server busy? → Check CPU/memory usage
- ❌ Too many files? → Clean up storage

### Connection drops
- ❌ WiFi unstable? → Move closer to router
- ❌ Too far from WiFi? → Extend range
- ❌ Interference? → Change WiFi channel

---

## Tips for Best Results

### 1. **Test Before the Event**
```
1. Start slideshow server
2. Take test photo with Selfie-Cam
3. Verify it appears on screen
4. Check timing and visibility
```

### 2. **Position Display Visibly**
- Place screen where people can see it
- Angle for best viewing
- Ensure good lighting on screen

### 3. **Configure Display Duration**
- Too fast: People miss photos
- Too slow: Becomes boring
- Sweet spot: 3-5 seconds per photo

### 4. **Keep WiFi Strong**
- Position router centrally
- Minimize interference
- Test range before event

### 5. **Monitor Storage**
- Check available space before event
- Set auto-cleanup appropriately
- Clean up after event

### 6. **Have Backup Plan**
- Know how to restart slideshow server
- Have spare USB cable for display
- Know manual reset procedure

---

## Event Day Checklist

✅ Slideshow server powered on  
✅ Display monitor/TV on  
✅ WiFi network operational  
✅ ARI battery charged  
✅ Selfie-Cam app tested  
✅ Test photo verified on display  
✅ Storage space available  
✅ Display positioning optimal  
✅ Audience can see screen clearly  
✅ Backup power available  

---

## Next Steps

- **Want detailed setup?** → [Technical Setup Guide](../Remote-Slideshow-Setup.md)
- **Need integration help?** → [Selfie-Cam Integration](../Remote-Slideshow-Selfie-Cam-Integration.md)
- **Try Selfie-Cam?** → [Selfie-Cam Guide](./selfie-cam-simple.md)
- **Curious about Gandalf?** → [Gandalf Game Guide](./gandalf-simple.md)

---

**Level**: Beginner  
**Time to read**: 5 minutes  
**Prerequisites**: Second Raspberry Pi or computer, monitor/TV
