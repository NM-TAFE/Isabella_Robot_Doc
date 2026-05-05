# Using Isabella

Isabella is the NMTAFE ARI robot. This guide covers what event staff can do with her.

---

## Connecting

1. On your phone or laptop, join the **`Isabella_AP`** WiFi network.
2. Open a browser and go to **`http://192.168.x.x:5000`** (IP shown on the robot's screen at startup).
3. The main menu shows three options: **Take Selfie**, **Interview**, **Gallery**.
4. For admin controls, go to **`http://192.168.x.x:5000/admin`**.

---

## Admin Control Panel

The admin panel is designed for use on a phone. Sections:

### Volume & Battery
Shows current battery level. Adjust Isabella's speaker volume.

### Soundboard
Six pre-set text slots. Type a phrase in a slot, press **Send** — Isabella speaks it. Useful for scripted event announcements.

### Gestures
Trigger a physical gesture:
- **Wave** — waves one arm
- **Shake Right** — shakes head right
- **Nod** — nods head
- **Show Left** — points/gestures left
- **Custom 1 / Custom 2** — site-configured gestures

### Interview Prompt
Type a question, press **Start Interview**. Isabella asks the question aloud and the camera begins recording the response. Recording saves to the gallery.

### Remote Display
Press **Send to Display** to push the current gallery to the remote slideshow screen.

### Navigation
Links to the admin gallery and back to the main menu.

---

## Taking Selfies

From the main menu, tap **Take Selfie**. A 6-second countdown runs. On the last second, the screen flashes **#issobellatherobot** and the photo is taken. The photo displays for 5 seconds, then saves to the gallery. If the remote slideshow Pi is running, the photo is automatically sent to the display screen.

---

## Interviews

From the main menu, tap **Interview** (or use the Interview Prompt in the admin panel to set a question first). Isabella asks the prompt aloud, then the camera records the response. Recordings save to the gallery.

---

## Gallery

Accessible at `http://192.168.x.x:5000/gallery` or via the admin panel. Shows all captured photos and videos. You can download or delete items from here.

---

## Remote Slideshow Display

A separate Raspberry Pi connected to an external screen runs the slideshow. Every photo taken during the event is automatically uploaded to it and added to the rotation. No manual steps needed — photos appear on the display within a few seconds of being taken.

---

## Gandalf Security Game

The Gandalf game runs on Isabella's touchscreen. A participant tries to trick an LLM into revealing a secret password by role-playing as the wizard Gandalf. Staff start the game from the touchscreen or terminal. See [Gandalf Operator Guide](../technical/features/Gandalf-LLM-Game-Operator-Guide.md) for setup.

---

## PAL WebGUI

The PAL WebGUI is Isabella's built-in browser interface, separate from the admin app. Access it at `http://[robot-ip]` (port 80). It provides:

- **Teleoperation** — joystick to drive Isabella remotely
- **Touchscreen Manager** — create and manage content on Isabella's screen
- **Motion Builder** — create and play arm/head motions

See the [PAL documentation](https://docs.pal-robotics.com/ari/legacy/) for full WebGUI details.
