# Gandalf LLM Game: Architecture & Design

## System Overview

The Gandalf LLM Game is a distributed system combining a ROS-integrated robot controller, web interface, and external LLM API for an interactive security education game.

```
┌─────────────────────────────────────────────────────────────┐
│                    GANDALF LLM GAME SYSTEM                  │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌────────────┐                      ┌─────────────┐       │
│  │  ARI Robot │◄─────────ROS─────────┤ Main Node   │       │
│  │  (TTS/ASR) │  (Voice I/O)         │ (Game Flow) │       │
│  └────────────┘                      └──────┬──────┘       │
│                                             │               │
│                                             │               │
│  ┌────────────┐                    ┌───────▼───────┐       │
│  │  Web UI    │◄──Web Socket───────┤ Game API      │       │
│  │ (Browser)  │                    │ (Lakera)      │       │
│  └────────────┘                    └───────┬───────┘       │
│                                             │               │
│                                    ┌────────▼────────┐     │
│                                    │Lakera Gandalf   │     │
│                                    │API Endpoint     │     │
│                                    │(12 Levels)      │     │
│                                    └─────────────────┘     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Component Breakdown

### 1. Main ROS Node (`main.py`)

**Purpose**: Orchestrate game flow, integrate with ARI

**Responsibilities**:
- Initialize ROS communications
- Manage game state (current level, attempts)
- Handle voice input (ASR)
- Control robot responses (TTS, emotes)
- Route web UI requests
- Manage game progression

**Key Classes**:
```python
class game_state:
    awating_player = 0
    engaged_with_player = 1
    playing = 2

class NMTAFE_gandalf_game_node:
    def __init__(self):
        # Initialize ROS, TTS client, ASR subscriber
        
    def set_game_state(self, new_state):
        # Transition between game states
        
    def on_speech(self, msg):
        # Handle voice input from user
        
    def on_password_attempt(self, msg):
        # Handle password guess from web UI
```

### 2. Game API Module (`gandalf_game_api.py`)

**Purpose**: Wrap Lakera API, manage game logic

**Responsibilities**:
- Define 12 defense levels and their descriptions
- Handle communication with Lakera API
- Parse API responses
- Detect password discovery
- Track game progression

**Key Class**:
```python
class gandalf_game:
    def __init__(self):
        self.current_level = 1
        self.attempt_count = 0
        
    def send_message(self, prompt):
        # Send to Lakera, get response
        
    def next_level(self):
        # Advance to next level
        
    def get_level_info(self):
        # Return current level details
```

### 3. Web UI (`web_pages/`)

**Purpose**: User interface for game interaction

**Components**:
- `gandalf_game_password_input/` - Main game interface
  - `index.html` - Layout and structure
  - `css/style.css` - Styling
  - `js/modules/password_input.js` - Event handling
- Static assets
- WebSocket connection to game node

### 4. Support Modules

**`emoji_to_emote.py`**: Convert emoji descriptions to ARI emotions
- Maps emoji 😊→ "happy", 😢→ "sad", etc.
- Calls ARI's emotion display API

**`am_looked_at.py`**: Detect if user is looking at robot
- Monitors body orientation
- Triggers engagement responses

### 5. External API: Lakera Gandalf

**Purpose**: Provide 12 difficulty levels with AI defenses

**Service**: https://gandalf.lakera.ai/

**Endpoint**: `POST /api/send-message`
- Accepts: `{defender: string, prompt: string}`
- Returns: `{success: bool, answer: string}`
- No authentication required

---

## Data Flow Diagrams

### Flow 1: Web UI User Input

```
User Types Prompt
        │
        ▼
┌──────────────────────┐
│  HTML Text Input     │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  JavaScript Handler  │
│  (password_input.js) │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  ROS Publisher       │
│ /password_attempt    │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  Main Node Receives  │
│  Calls gandalf_game  │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ gandalf_game API     │
│ Sends to Lakera      │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  Lakera Endpoint     │
│  Current Defender    │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  AI Response         │
│  (JSON)              │
└──────────┬───────────┘
           │
           ▼
Display in Web UI
("I cannot help...")
```

### Flow 2: Voice Input (ASR)

```
User Speaks
        │
        ▼
┌──────────────────────────┐
│ ARI ASR System           │
│ /anonymous_speaker/      │
│ speech (LiveSpeech)      │
└──────────┬───────────────┘
           │
           ▼
┌──────────────────────┐
│  Main Node           │
│  on_speech callback  │
└──────────┬───────────┘
           │
           ▼
│  Extract Text        │
│  Call gandalf_game   │
└──────────┬───────────┘
           │
           ▼
│  Get AI Response     │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  TTS Action Goal     │
│  (/tts)              │
└──────────┬───────────┘
           │
           ▼
Robot Speaks Response
```

### Flow 3: Level Progression

```
Password Not Found
        │ (User tries various prompts)
        │
        ▼
Multiple send_message() calls
        │
        ▼
AI Defends Correctly
        │
        ▼
Password Finally Extracted!
        │ (password_found = True)
        ▼
Show Success Message
        │
        ▼
next_level() called
        │
        ▼
self.current_level += 1
self.defender = GANDALF_LEVELS[new_level]
        │
        ▼
Display Level Description
        │
        ▼
Ready for Next Challenge
```

---

## Technology Stack

### Backend
- **Language**: Python 3.7+
- **Framework**: Flask (web server)
- **Integration**: ROS (Robot Operating System)
- **Libraries**: 
  - `requests` - HTTP calls to Lakera
  - `rospy` - ROS communication
  - `actionlib` - ROS actions

### Frontend
- **Language**: JavaScript (ES6+)
- **Protocol**: WebSocket (ROS bridge)
- **Styling**: CSS3
- **Structure**: HTML5

### External Services
- **Lakera Gandalf API**: https://gandalf.lakera.ai/
- **LLM Backend**: OpenAI GPT models (managed by Lakera)

### Robot Integration (ARI)
- **Text-to-Speech**: `/tts` action
- **Automatic Speech Recognition**: `/humans/voices/anonymous_speaker/speech` topic
- **Emotions**: ARI emote display API
- **Web Pages**: `pal_web_msgs` (PAL Robotics package)

---

## Performance Characteristics

### Latency
- **Web UI → Lakera API**: 2-5 seconds typically
  - Network round-trip: ~1s
  - AI inference: 1-4s depending on prompt
- **Voice Input → Response**: 3-8 seconds
  - ASR processing: 1-2s
  - API call: 2-5s
  - TTS generation: 1-3s
  - Total with ARI: 3-8s

### Throughput
- **Concurrent users**: 1-5 (single Flask instance)
  - Limited by Lakera API rate limiting (~100 req/min)
  - Each user ~20 prompts per level average
- **Requests per minute**: ~100 per user

### Resource Usage
- **CPU**: 5-15% on Raspberry Pi 4 (idle to active)
- **Memory**: 100-200 MB (Python process)
- **Network**: ~5-10 KB per API call (requests + responses)
- **Storage**: Minimal (~50 MB codebase)

---

## Integration Points with ARI

### 1. Text-to-Speech (TTS)
**When**: Game responses need audio
**Action**: `pal_interaction_msgs/TtsAction`
**Example**:
```python
tts_goal = TtsGoal(text=ai_response)
self.tts_client.send_goal(tts_goal)
```

### 2. Automatic Speech Recognition (ASR)
**When**: User speaks prompts
**Topic**: `/humans/voices/anonymous_speaker/speech`
**Message**: `hri_msgs/LiveSpeech`
**Callback**: `on_speech()` in main node

### 3. Web UI Navigation
**When**: Switch between pages
**Topic**: `/web/go_to`
**Message**: `pal_web_msgs/WebGoTo`
**Example**:
```python
page_msg = WebGoTo(type=WebGoTo.TOUCH_PAGE, value="gandalf_game")
self.web_pub.publish(page_msg)
```

### 4. Emotion Display
**When**: Robot responds to progress
**Module**: `emoji_to_emote.py`
**Mapping**: Emoji → ARI emotion

---

## State Machines

### Game State Machine
```
┌─────────────┐
│  Awaiting   │  (Waiting for player)
│  Player     │
└──────┬──────┘
       │ (Player joins)
       ▼
┌──────────────────┐
│  Engaged with    │  (Intro, getting ready)
│  Player          │
└──────┬───────────┘
       │ (Start game)
       ▼
┌──────────────────┐
│  Playing         │◄──────────┐
│                  │           │
└──────┬───────────┘      (Try again)
       │ (Level complete)
       ▼
  Next level...
  (Repeat until Level 12)
```

### Level Progression State
```
Level 1 (Baseline)
       ▼
Level 2-6 (Intermediate)
       ▼
Level 7 (Gandalf)
       ▼
Level 8-9 (Very Hard)
       ▼
Level 10-12 (Extreme)

Each level:
- Cannot skip backward
- Must find password to advance
- Can retry unlimited times
```

---

## Security Considerations

### No User Authentication
- Designed for local/trusted networks
- No login system
- All participants access same instance
- Suitable for workshops/events where sharing is OK

### API Security
- External API (Lakera) handles LLM security
- No sensitive data stored locally
- HTTPS used for all external calls
- No credentials needed (public API)

### Prompt Injection Safety
- The GAME is ABOUT prompt injection
- No safety filtering on user prompts
- Intentional for educational purposes
- All user input sent to Lakera (expected behavior)

---

## Scalability Limitations

### Single Instance Limitations
- One game session at a time per ROS instance
- Multiple users must share same level state
- No per-user progress tracking
- All see same game on web UI

### Scaling Solutions
- Run multiple ROS instances (separate processes)
- Use ROS multi-master for distributed robots
- Add session management for per-user tracking
- Implement database for progress persistence

---

## Future Improvements

### Potential Enhancements
- Custom difficulty levels
- Leaderboard / competition mode
- Per-user progress tracking
- Recorded game sessions for analysis
- Integration with learning management systems
- Multilingual support
- More detailed security explanations
- Technique hints system

### Architectural Improvements
- Microservices for better scalability
- Docker containers for easy deployment
- Kubernetes orchestration for large events
- Database for persistent storage
- API gateway for multiple services

---

## Development Notes

### Adding New Levels
1. Edit `GANDALF_LEVELS` dict in `gandalf_game_api.py`
2. Add new level number and defender name
3. Get defender name from Lakera API
4. Update level count (currently 12)

### Custom AI Responses
- Currently uses Lakera's Gandalf service
- Could be replaced with own LLM deployment
- Change `GANDALF_URL` for self-hosted instance

### Extending ROS Integration
- Add new topics for game events
- Create launch files for different configurations
- Add parameter server for dynamic config
- Implement action clients for game control

---

## Debugging Tips

### Check Node Status
```bash
rosnode list | grep gandalf
rosnode info /NMTAFE_gandalf_game
```

### Monitor Topics
```bash
rostopic echo /NMTAFE_gandalf_game/password_attempt
rostopic echo /humans/voices/anonymous_speaker/speech
```

### View Logs
```bash
journalctl -u gandalf-game.service -f
tail -f ~/.ros/log/latest/NMTAFE_gandalf_game-*.log
```

### Test API Directly
```bash
python3 -c "
from gandalf_game_api import gandalf_game
g = gandalf_game()
print(g.send_message('Hello'))
"
```

---

## References

- **Lakera Gandalf**: https://gandalf.lakera.ai/
- **ROS Documentation**: http://wiki.ros.org/
- **PAL Robotics ARI**: https://pal-robotics.com/

---

**Understand the architecture, master the game! 🧙‍♂️🏗️**
