# Gandalf LLM Game: Setup & Installation

## Prerequisites

### Hardware Requirements
- **Compute**: Any system with Python 3.7+ (Raspberry Pi 4+, laptop, workstation)
- **Network**: Stable internet connection (API calls to gandalf.lakera.ai)
- **Display**: Any device with web browser (for web UI)
- **Optional**: ARI robot (for integrated audio/visual experience)

### Software Requirements
- **Operating System**: Linux (Ubuntu 18.04+, Raspberry Pi OS), macOS, or WSL on Windows
- **Python**: 3.7+
- **ROS**: Noetic or Melodic (for ARI integration)
- **Package**: catkin build tools
- **Dependencies**: See requirements below

---

## Installation Steps

### Step 1: Clone the Repository

If installing on ARI (NM-TAFE repo):
```bash
cd ~/nmtafe_ws/src
# Already present in NMTAFE_ari repo
cd nmtafe_gandalf_game
```

For standalone installation:
```bash
cd ~/Documents/Github
git clone https://github.com/prgrobots/gandalf_cli_ari.git
cd gandalf_cli_ari
```

### Step 2: Install Python Dependencies

#### Option A: Using pip (Standalone)
```bash
# Navigate to repo directory
cd gandalf_cli_ari

# Install requirements
pip install -r requirements.txt

# Or manually install:
pip install requests
```

#### Option B: Using ROS (Integrated with ARI)
```bash
cd ~/nmtafe_ws/src/nmtafe_gandalf_game

# Install dependencies
rosdep install --from-paths . --ignore-src -r -y

# Build with catkin
cd ~/nmtafe_ws
catkin build nmtafe_gandalf_game
source devel/setup.bash
```

### Step 3: Verify Installation

#### Test Standalone Version
```bash
# Navigate to repo
cd gandalf_cli_ari

# Run the simple demo
python gadalf_simple.py

# You should see:
# 🤖/🧙 Start of game, ARI talking to Gandalf via internet only
# 👋 (ARI waves when they see a person) 🤖 says -- "Hi"
# ...
```

#### Test ROS Version
```bash
# Terminal 1: Start ROS master
roscore

# Terminal 2: Start Gandalf node
rosrun nmtafe_gandalf_game main.py

# Terminal 3: Test connection
rostopic list | grep gandalf
# Should show /NMTAFE_gandalf_game topics
```

---

## Configuration

### API Configuration

#### Gandalf.lakera.ai URL
Default: `https://gandalf.lakera.ai/`

To customize (edit `gandalf_game_api.py`):
```python
GANDALF_URL = 'https://gandalf.lakera.ai/'

# Or for local testing (if self-hosted):
GANDALF_URL = 'http://localhost:8000/'
```

#### API Timeout
```python
# In gandalf_game_api.py
TIMEOUT = 30  # seconds

# Increase if getting timeout errors:
TIMEOUT = 60  # for slow connections
```

### ROS Configuration

#### ROS Node Name
```bash
# Edit main.py or launch file
rospy.init_node("NMTAFE_gandalf_game")

# Or pass as parameter:
rosrun nmtafe_gandalf_game main.py _node_name:=my_gandalf_game
```

#### Topics
```
Default topics:
/NMTAFE_gandalf_game/password_attempt  - Listens for password guesses
/humans/voices/anonymous_speaker/speech - Listens for voice input (ASR)
/tts - Sends text-to-speech requests
/web/go_to - Controls web UI pages
```

### Web Server Configuration

#### Port Setup
```python
# Default port in main.py
PORT = 8080

# Customize:
PORT = 5000  # or any available port

# Access at:
# http://localhost:8080
# http://[robot-ip]:8080
```

#### Static Files
```python
# Ensure web_pages directory is accessible
WEB_PAGES_DIR = 'web_pages/'

# Full path if not in current directory:
WEB_PAGES_DIR = '/home/pi/nmtafe_ws/src/nmtafe_gandalf_game/web_pages/'
```

---

## Starting the Game

### Quick Start (Standalone)
```bash
cd ~/gadalf_cli_ari
python gadalf_simple.py
```

### Start on ARI
```bash
# Terminal 1
roscore

# Terminal 2
roslaunch nmtafe_gandalf_game gandalf.launch
```

Or manually:
```bash
rosrun nmtafe_gandalf_game main.py
```

### With Custom Parameters
```bash
rosrun nmtafe_gandalf_game main.py \
  _api_url:="https://gandalf.lakera.ai/" \
  _port:=8080 \
  _timeout:=30
```

---

## Auto-Start on Boot (Optional)

### Using systemd

Create `/etc/systemd/system/gandalf-game.service`:
```ini
[Unit]
Description=Gandalf LLM Game for ARI
After=network.target roscore.service

[Service]
Type=simple
User=pi
WorkingDirectory=/home/pi/nmtafe_ws
ExecStart=/home/pi/miniconda3/envs/ros/bin/python /home/pi/nmtafe_ws/src/nmtafe_gandalf_game/scripts/main.py
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Enable and start:
```bash
sudo systemctl daemon-reload
sudo systemctl enable gandalf-game.service
sudo systemctl start gandalf-game.service

# Check status
sudo systemctl status gandalf-game.service

# View logs
sudo journalctl -u gandalf-game.service -f
```

### Using ROS launch file

Create `~/nmtafe_ws/src/nmtafe_gandalf_game/launch/gandalf.launch`:
```xml
<launch>
  <node name="NMTAFE_gandalf_game" pkg="nmtafe_gandalf_game" type="main.py">
    <param name="api_url" value="https://gandalf.lakera.ai/"/>
    <param name="port" value="8080"/>
    <param name="timeout" value="30"/>
  </node>
</launch>
```

Start with:
```bash
roslaunch nmtafe_gandalf_game gandalf.launch
```

---

## Verification Checklist

After installation, verify each component:

### Internet Connectivity
```bash
# Test connection to Lakera API
ping gandalf.lakera.ai
# Or
curl https://gandalf.lakera.ai/

# Should not show "Connection refused" or timeout
```

### Python Environment
```bash
# Check Python version
python3 --version
# Should be 3.7+

# Check requests module
python3 -c "import requests; print(requests.__version__)"
# Should print version without error
```

### ROS Connectivity (if using ARI)
```bash
# Check ROS master
rostopic list
# Should list topics without hanging

# Check for gandalf node
rosnode list | grep gandalf
# Should show /NMTAFE_gandalf_game node
```

### API Connectivity
```bash
# Test calling API from Python
python3 << 'EOF'
import requests
response = requests.post('https://gandalf.lakera.ai/api/send-message',
    data={'defender': 'baseline', 'prompt': 'Test message'},
    timeout=30)
print(response.status_code)
print(response.json())
EOF

# Should print 200 and JSON response
```

### Web UI Access
```bash
# From same machine (if running locally)
curl http://localhost:8080/

# Or from another machine
curl http://[robot-ip]:8080/

# Should return HTML, not error
```

### Complete System Test
```bash
# If using ARI:
roslaunch nmtafe_gandalf_game gandalf.launch

# Access web UI in browser:
http://localhost:8080/

# Should see:
# - Gandalf LLM Game title
# - Current level display
# - Input text field
# - Send button
# - Gandalf responses below
```

---

## Troubleshooting Installation

### Issue: "ModuleNotFoundError: No module named 'requests'"
**Solution**:
```bash
pip install requests
# Or with conda:
conda install requests
```

### Issue: "Connection refused" when calling API
**Solution**:
1. Check internet connection: `ping gandalf.lakera.ai`
2. Check firewall isn't blocking HTTPS: `curl https://gandalf.lakera.ai/`
3. Try with explicit timeout: `requests.post(..., timeout=30)`
4. Verify `GANDALF_URL` is correct in code

### Issue: "ROS node won't start"
**Solution**:
```bash
# Check if roscore is running
pgrep roscore

# If not, start it:
roscore &

# Then try launching gandalf again:
rosrun nmtafe_gandalf_game main.py
```

### Issue: "Can't reach web UI at http://localhost:8080"
**Solution**:
```bash
# Check if process is running
ps aux | grep main.py

# Check if port is in use
lsof -i :8080

# If port in use, kill and restart:
sudo kill -9 <PID>
sleep 5
rosrun nmtafe_gandalf_game main.py
```

### Issue: "Web UI loads but nothing works"
**Solution**:
1. Clear browser cache (Ctrl+Shift+Delete)
2. Try different browser (Chrome, Firefox)
3. Check browser console for JavaScript errors (F12)
4. Verify Flask server is running: `ps aux | grep main.py`
5. Check logs: `journalctl -u gandalf-game.service -f`

---

## Next Steps

- **First time user**: See [Getting Started Guide](Gandalf-LLM-Game-Getting-Started.md)
- **Running an event**: See [Operator Guide](Gandalf-LLM-Game-Operator-Guide.md)
- **Technical details**: See [API Reference](Gandalf-LLM-Game-API.md)
- **System design**: See [Architecture](Gandalf-LLM-Game-Architecture.md)

---

## Support

- **Installation issues**: Check troubleshooting section above
- **API problems**: Verify gandalf.lakera.ai is accessible
- **ROS issues**: Check `rostopic` and `rosnode` commands
- **Web UI issues**: Check browser console (F12) for errors

---

**Installation complete! Ready to play the Gandalf LLM Game 🧙‍♂️**
