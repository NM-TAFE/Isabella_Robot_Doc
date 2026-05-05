# PAL Docs Reference Bridge

New to ARI? Here's a simplified guide to the official PAL Robot documentation, plus links to dive deeper.

## What is PAL Robotics?

PAL Robotics is the company that makes ARI. They have extensive technical documentation. This page helps you find what you need without getting overwhelmed.

**Key Point**: You don't need to read everything. Start with your use case below.

---

## Quick Start (Choose Your Path)

### 🆕 I'm brand new to ARI
```
1. Read: "What is ARI?" (below)
2. Read: "Getting Started" (PAL section)
3. Read: "Power Management" (PAL section)
4. Try: Basic movement using web interface
→ Official link: Section 5 on PAL docs
```

### 🎮 I want to control ARI manually
```
1. Read: "Motion Builder" (below)
2. Read: "WebGUI" (PAL section)
3. Try: Using joystick or motion controls
→ Official link: Section 10.7 on PAL docs
```

### 📱 I want to use our Selfie-Cam app
```
1. Read: Selfie-Cam Simple Guide (in this site)
2. No need to read PAL advanced controls
3. Our app handles the complexity
→ See: Selfie-Cam Simple Guide
```

### 🎤 I want to make ARI talk
```
1. Read: "Text-to-Speech" (below)
2. Read: "Speakers and Microphones" (PAL section)
3. Test: Making ARI speak
→ Official link: Section 13 on PAL docs
```

### 🖥️ I need technical details
```
1. Read: Technical Documentation (on this site)
2. Reference: PAL WebGUI docs
3. Reference: PAL API documentation
→ See: Technical Documentation
```

---

## What is ARI? (Official Overview)

ARI is a mobile manipulation platform designed for Human-Robot Interaction (HRI) research and real-world applications.

### Key Components

| Component | What It Is | What It Does |
|-----------|-----------|------------|
| **Body** | Mobile base | Drives around autonomously |
| **Arm** | Manipulator (optional) | Pick up or interact with objects |
| **Head** | Sensors + display | See, listen, speak |
| **Battery** | Rechargeable | Powers everything |
| **Network** | WiFi + Ethernet | Connects to external systems |

### Physical Specs (Simple Version)
- **Size**: Small wheeled robot (approx. 40cm tall)
- **Weight**: Lightweight, portable
- **Battery**: 2-3 hours typical use
- **Range**: WiFi dependent (usually 30-50m)
- **Cost**: Research/enterprise robot (not a toy)

### For More Details
👉 [PAL Docs - What is ARI](https://docs.pal-robotics.com/ari/legacy/)

---

## Getting Started (Official PAL Process)

### Step 1: Unboxing
- Check all components
- Verify battery included
- Read safety warnings

### Step 2: Power Management
Most important! You must understand:
- **How to charge** ARI safely
- **How to turn on/off** properly
- **Emergency stop** location and use
- **Battery warning** indicators

### Step 3: First Startup
```
1. Charge battery to 100%
2. Press power button
3. Wait for boot (1-2 minutes)
4. Connect to WiFi network
5. Access web interface
```

### Step 4: Basic Operation
- Drive using joystick or web interface
- Make ARI talk (Text-to-Speech)
- Observe how it moves and responds

### For Detailed Steps
👉 [PAL Docs - Getting Started](https://docs.pal-robotics.com/ari/legacy/)

---

## Power Management (Critical!)

### Battery
- **Type**: Rechargeable lithium-ion
- **Charging time**: 3-4 hours (full empty to full)
- **Duration**: 2-3 hours typical use
- **Indication**: LED shows charge level

### Charging Safety
```
✅ DO:
  • Charge in well-ventilated area
  • Use official charger
  • Monitor first charge
  • Let cool between heavy use

❌ DON'T:
  • Overcharge (leave plugged in 24/7)
  • Expose battery to extreme heat
  • Use damaged charger
  • Charge in enclosed space
```

### Emergency Stop (E-Stop)
ARI has a physical emergency stop button.
- **Location**: Usually on the back or top
- **Purpose**: Stops all movement immediately
- **When to use**: Safety hazard detected
- **Finding it**: Check physical robot or ask operator

### For More Details
👉 [PAL Docs - Power Management](https://docs.pal-robotics.com/ari/legacy/)

---

## WebGUI (Control Interface)

### What It Is
A web browser interface to control ARI from any computer on the network.

```
Open browser → Type ARI's IP address → Get control dashboard
```

### Main Features Available

| Feature | What It Does | When You Use It |
|---------|------------|----------------|
| **Homepage** | Status overview | Check if ARI is working |
| **Information Panel** | System details | See battery, network, temperature |
| **Motion Builder** | Create movements | Design custom robot movements |
| **Touchscreen Manager** | Custom interfaces | Make ARI screen show your content |
| **Command Desks** | Control panels | Create custom control layouts |
| **Teleoperation** | Remote control | Drive ARI from web browser |
| **Joystick** | Game controller input | Natural control method |

### Basic Workflow
```
1. Open WebGUI in browser
2. Login (if required)
3. Select feature (e.g., "Motion Builder")
4. Make changes
5. Test/execute
6. Save if needed
```

### For More Details
👉 [PAL Docs - WebGUI](https://docs.pal-robotics.com/ari/legacy/)

---

## Motion Builder (Creating Movements)

### What It Is
A visual editor for creating and recording robot movements.

### Basic Idea
```
Instead of programming code,
you:
1. Position robot manually
2. Record position as keyframe
3. Repeat for different positions
4. Play back the sequence
5. Refine timing and movement
```

### Why Use It
- ✅ No coding required
- ✅ Visual/intuitive
- ✅ See movement before executing
- ✅ Reusable motion libraries

### Example: Wave Motion
```
1. Position arm down
2. Click "Record Frame"
3. Move arm to side
4. Click "Record Frame"
5. Move arm up
6. Click "Record Frame"
7. Click "Play"
8. ARI waves!
```

### For More Details
👉 [PAL Docs - Motion Builder](https://docs.pal-robotics.com/ari/legacy/)

---

## Text-to-Speech (Making ARI Talk)

### What It Is
ARI can speak text out loud in natural voice.

### Basic Use
```python
# Pseudocode (actual implementation varies)
ari.speak("Hello, welcome to our event!")
```

### Capabilities
- **Languages**: Multiple languages supported
- **Voice**: Natural-sounding synthesis
- **Speed**: Configurable (slow to fast)
- **Pitch**: Adjustable if needed

### Common Uses
- Greetings: "Welcome!"
- Instructions: "Please move to the left"
- Q&A: Answer questions out loud
- Announcements: "The show is about to start"

### For More Details
👉 [PAL Docs - Text-to-Speech](https://docs.pal-robotics.com/ari/legacy/)

---

## Animated Eyes

### What It Is
ARI's "eyes" display emotions via animated expressions.

### Expressions
- **Happy**: Smile, bright eyes
- **Confused**: Questioning look
- **Sad**: Downturned expression
- **Surprised**: Wide open eyes
- **Neutral**: Resting state

### Why It Matters
- Makes ARI feel more "alive"
- Communicates state to users
- Increases engagement
- More natural interaction

### For More Details
👉 [PAL Docs - Animated Eyes](https://docs.pal-robotics.com/ari/legacy/)

---

## Speakers & Microphones

### What It Is
Audio input/output system for listening and speaking.

### Speakers
- **Quality**: Clear, intelligible
- **Volume**: Adjustable
- **Use**: TTS output, notifications, sounds

### Microphones
- **Purpose**: Listen to users
- **Use**: Speech recognition, voice commands
- **Placement**: On head (good pickup)

### Considerations
- Loud environments: Harder to hear
- Far distance: Prefer closer interaction
- Accent/language: May affect recognition

### For More Details
👉 [PAL Docs - Speakers & Microphones](https://docs.pal-robotics.com/ari/legacy/)

---

## Connectivity

### WiFi
- **Range**: Typical 30-50 meters
- **Speed**: Standard WiFi speeds sufficient
- **Security**: Secured by your network password

### Ethernet (Optional)
- More stable than WiFi
- Better for high-bandwidth tasks
- Less convenient (physical cable)

### Network Setup
```
1. ARI connects to your WiFi network
2. You need to know: Network name (SSID) and password
3. Access via: Browser at ARI's IP address
```

### Troubleshooting
- **Won't connect to WiFi?** → Check network name/password
- **Connection drops?** → Move closer to router
- **Can't reach WebGUI?** → Check if same network
- **Slow response?** → Check WiFi signal strength

### For More Details
👉 [PAL Docs - Connectivity](https://docs.pal-robotics.com/ari/legacy/)

---

## Components Overview (Physical)

### Main Components
```
┌─────────────────────┐
│   Display/Screen    │ ← Touchscreen
│                     │
│  [O] [O] = Eyes     │ ← Animated eyes
│   ||||||  = Mouth   │
├─────────────────────┤
│    Head Unit        │ ← Sensors, camera, speaker
├─────────────────────┤
│                     │
│  Mobile Base        │ ← Wheels, drives around
│                     │
└─────────────────────┘
```

### Key Parts
- **Base**: Locomotion (movement)
- **Head**: Sensing, interaction
- **Screen**: Information display
- **Battery**: Powers everything
- **Network**: Connects to external systems

### For More Details
👉 [PAL Docs - Components](https://docs.pal-robotics.com/ari/legacy/)

---

## Command Desks (Custom Interfaces)

### What It Is
Create custom control panels for ARI.

### Example: Conference Demo Panel
```
┌──────────────────┐
│  Conference Mode │
├──────────────────┤
│ [Greet Visitors] │
│ [Show Slides]    │
│ [Take Photo]     │
│ [End Greeting]   │
└──────────────────┘
```

### Why Use It
- Simplified controls for demonstrations
- Hide complex options
- Pre-program common tasks
- Faster operation

### For More Details
👉 [PAL Docs - Command Desks](https://docs.pal-robotics.com/ari/legacy/)

---

## Touchscreen Manager

### What It Is
Customize what appears on ARI's touchscreen.

### Options
- **Buttons**: Large easy-to-tap buttons
- **Slideshows**: Display images, photos
- **Text**: Static or dynamic text
- **Custom HTML**: Advanced layouts
- **Web Embeds**: Display web content

### Use Cases
- Show company logo while greeting
- Display slideshow of products
- Show instructions to users
- Display real-time information

### For More Details
👉 [PAL Docs - Touchscreen Manager](https://docs.pal-robotics.com/ari/legacy/)

---

## Teleoperation (Remote Control)

### What It Is
Control ARI from a distance via web interface.

### How It Works
```
[You at computer] ──(WiFi)──> [ARI robot]
   Web browser              Executes commands
```

### Common Controls
- Movement: Arrow keys or joystick
- Speed: Slider
- Rotation: Turn controls
- Special functions: Custom buttons

### For More Details
👉 [PAL Docs - Teleoperation](https://docs.pal-robotics.com/ari/legacy/)

---

## Key Takeaways

### What to Know
1. ✅ ARI is powerful and flexible
2. ✅ WebGUI makes control easy
3. ✅ No coding needed for basic use
4. ✅ Battery management is important
5. ✅ Our custom features extend capabilities

### What Not to Worry About (Yet)
- ❌ Advanced programming (unless needed)
- ❌ Network security (handled by WiFi password)
- ❌ Sensor fusion details
- ❌ Kinematics calculations

---

## Where to Go Next

### By Role

**Event Organizer**
- [Getting Started](./index.md) (this site)
- [What ARI Does](./answering-what-does-ari-do.md) (this site)
- [Selfie-Cam Guide](./selfie-cam-simple.md) (this site)

**Educator**
- [Gandalf Game Guide](./gandalf-simple.md) (this site)
- [Motion Builder](https://docs.pal-robotics.com/ari/legacy/) (PAL docs)
- [Technical Setup](../technical/) (this site)

**Developer**
- [Technical Documentation](../technical/) (this site)
- [WebGUI Details](https://docs.pal-robotics.com/ari/legacy/) (PAL docs)
- [API Reference](../ARI-Selfie-Cam-API.md) (this site)

---

## Official PAL Resources

📖 **Main PAL ARI Documentation**  
https://docs.pal-robotics.com/ari/legacy/

**Sections Available**:
1. ARI User Manual & Disclaimers
2. What is ARI / Package Contents / Components
3. Battery, Connectivity, Description
4. Getting Started & Power Management
5. First Time Startup & Welcoming Screen
6. Animated Eyes, Speakers, Microphones
7. WebGUI & All Features
8. Touchscreen Manager, Command Desks
9. Teleoperation, Text-to-Speech

---

**Level**: Beginner  
**Time to read**: 10 minutes  
**Best for**: Understanding where PAL docs fit in, quick reference to official material
