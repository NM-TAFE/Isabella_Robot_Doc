# Gandalf LLM Game: Configuration Reference

## Overview

Configure the Gandalf LLM Game system through environment variables, configuration files, and ROS parameters.

---

## Configuration Files

### Main Configuration: `gandalf_game_api.py`

Edit this file to customize core behavior:

```python
# Lakera API endpoint
GANDALF_URL = 'https://gandalf.lakera.ai/'

# Game levels (12 total)
GANDALF_LEVELS = {
    1: {"defender": "baseline", ...},
    # ... through level 12
}

# Request timeout (seconds)
TIMEOUT = 30

# Attempt tracking
MAX_ATTEMPTS_PER_LEVEL = None  # Unlimited by default
```

### Flask Server: `main.py`

Web server configuration:

```python
# Server address and port
HOST = '0.0.0.0'  # Listen on all interfaces
PORT = 8080

# Debug mode (set to False for production)
DEBUG = False

# Static files path
STATIC_PATH = './web_pages'
```

### ROS Node: `main.py`

ROS integration settings:

```python
# Node name
rospy.init_node("NMTAFE_gandalf_game")

# Topic names
ASR_TOPIC = '/humans/voices/anonymous_speaker/speech'
PASSWORD_TOPIC = '/NMTAFE_gandalf_game/password_attempt'
TTS_ACTION = '/tts'
WEB_UI_TOPIC = '/web/go_to'

# Action timeouts
TTS_TIMEOUT = 10  # seconds
```

---

## Environment Variables

### API Configuration

**`GANDALF_URL`**
```bash
export GANDALF_URL="https://gandalf.lakera.ai/"
# Or for self-hosted:
export GANDALF_URL="http://localhost:8000/"
```

**`API_TIMEOUT`**
```bash
export API_TIMEOUT="30"  # seconds
# Increase for slow connections:
export API_TIMEOUT="60"
```

### Server Configuration

**`PORT`**
```bash
export PORT="8080"
# Or any available port:
export PORT="5000"
```

**`HOST`**
```bash
export HOST="0.0.0.0"  # All interfaces
# Or specific IP:
export HOST="192.168.1.100"
```

**`DEBUG`**
```bash
export DEBUG="False"  # Production
export DEBUG="True"   # Development (verbose logging)
```

### Logging

**`LOG_LEVEL`**
```bash
export LOG_LEVEL="INFO"      # Standard (recommended)
export LOG_LEVEL="DEBUG"     # Verbose
export LOG_LEVEL="WARNING"   # Less verbose
```

**`LOG_FILE`**
```bash
export LOG_FILE="/var/log/gandalf_game.log"
# If not set, logs to console
```

---

## ROS Parameters

### Node Parameters

Set in launch file or command line:

```bash
# Via launch file
<param name="api_url" value="https://gandalf.lakera.ai/"/>
<param name="port" value="8080"/>
<param name="timeout" value="30"/>

# Via command line
rosrun nmtafe_gandalf_game main.py _api_url:="https://gandalf.lakera.ai/"
```

### Available Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `api_url` | string | `https://gandalf.lakera.ai/` | Lakera API endpoint |
| `port` | int | `8080` | Web server port |
| `timeout` | int | `30` | API request timeout (seconds) |
| `debug` | bool | `False` | Enable debug logging |
| `max_attempts` | int | `-1` | Max attempts per level (-1 = unlimited) |

### Setting Parameters

**In launch file** (`gandalf.launch`):
```xml
<launch>
  <node name="NMTAFE_gandalf_game" pkg="nmtafe_gandalf_game" type="main.py">
    <param name="api_url" value="https://gandalf.lakera.ai/"/>
    <param name="port" value="8080"/>
    <param name="timeout" value="30"/>
    <param name="debug" value="false"/>
  </node>
</launch>
```

**Via command line**:
```bash
rosrun nmtafe_gandalf_game main.py \
  _api_url:="https://gandalf.lakera.ai/" \
  _port:=8080 \
  _timeout:=30 \
  _debug:=false
```

**Via rosparam**:
```bash
rosparam set /NMTAFE_gandalf_game/api_url "https://gandalf.lakera.ai/"
rosparam set /NMTAFE_gandalf_game/port 8080
```

---

## Network Configuration

### Firewall Settings

**Allow port 8080 (UFW)**:
```bash
sudo ufw allow 8080/tcp
sudo ufw allow 8080/udp
sudo ufw reload
```

**Allow port 8080 (iptables)**:
```bash
sudo iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 8080 -j ACCEPT
```

### Network Interfaces

**Bind to specific interface**:
```python
# In main.py
HOST = '192.168.1.100'  # Only this IP
app.run(host=HOST, port=PORT)
```

**Listen on all interfaces** (recommended):
```python
HOST = '0.0.0.0'
app.run(host=HOST, port=PORT)
```

### Remote Access

**Access from another machine**:
```bash
# If running on robot at 192.168.1.50:
http://192.168.1.50:8080/

# From same network:
# - Desktop: http://robot-hostname:8080
# - Smartphone: http://robot-ip:8080
```

---

## Performance Tuning

### Request Timeout

**Increase for slow networks**:
```python
# gandalf_game_api.py
TIMEOUT = 60  # Was 30

# Or per-request override:
requests.post(url, data=data, timeout=60)
```

### Connection Pooling

**For many requests** (in gandalf_game_api.py):
```python
import requests
from requests.adapters import HTTPAdapter

session = requests.Session()
adapter = HTTPAdapter(pool_connections=10, pool_maxsize=10)
session.mount('https://', adapter)
session.mount('http://', adapter)

# Use session for all requests
response = session.post(url, data=data, timeout=TIMEOUT)
```

### Caching

**Cache responses** (optional enhancement):
```python
from functools import lru_cache

@lru_cache(maxsize=100)
def send_message_cached(prompt, defender):
    # Cache same prompts for same defender
    return send_message(prompt, defender)
```

### Concurrency

**Handle multiple users**:
```python
# Use Flask with Gunicorn for production:
# gunicorn -w 4 -b 0.0.0.0:8080 main:app

# Or with threading:
app.run(threaded=True)
```

---

## Logging Configuration

### Basic Logging

**In main.py**:
```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('/var/log/gandalf_game.log'),
        logging.StreamHandler()
    ]
)
```

### Log Levels

| Level | When to Use | Example |
|-------|------------|---------|
| DEBUG | Development, detailed tracing | Entering function, variable values |
| INFO | Normal operation | Server started, game started |
| WARNING | Potential issues | Slow response, retry needed |
| ERROR | Errors needing attention | API failed, network error |
| CRITICAL | System failure | Cannot start, fatal error |

### Enable Debug Logging

```bash
# Set environment variable
export LOG_LEVEL="DEBUG"

# Or in code
logging.getLogger().setLevel(logging.DEBUG)
```

---

## Security Configuration

### Disable Debug Mode (Production)

**In main.py**:
```python
DEBUG = False  # Never True in production
```

**Set via environment**:
```bash
export DEBUG="False"
```

### HTTPS/SSL

**For remote access**, use reverse proxy:

```bash
# Example with nginx:
server {
    listen 443 ssl;
    server_name gandalf.example.com;
    
    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;
    
    location / {
        proxy_pass http://localhost:8080;
    }
}
```

### Authentication (Optional)

**Add basic auth** (not provided by default):

```python
from flask_httpauth import HTTPBasicAuth

auth = HTTPBasicAuth()

@auth.verify_password
def verify_password(username, password):
    return username == "admin" and password == "secret"

@app.route('/')
@auth.login_required
def index():
    return render_template('index.html')
```

### Rate Limiting

**Limit requests per user** (optional):

```python
from flask_limiter import Limiter

limiter = Limiter(app, key_func=lambda: request.remote_addr)

@app.route('/api/send-prompt', methods=['POST'])
@limiter.limit("10 per minute")
def send_prompt():
    # Limited to 10 requests/minute per IP
    pass
```

---

## Custom Difficulty Levels

### Adding New Levels

**Edit `gandalf_game_api.py`**:

```python
GANDALF_LEVELS = {
    # ... existing levels 1-12
    13: {
        "defender": "custom-level-1",
        "description": "My custom defense!"
    },
    14: {
        "defender": "custom-level-2", 
        "description": "Another custom defense"
    }
}

# Update level count
MAX_LEVEL = 14
```

**Note**: Requires defender support from Lakera API

### Custom Defender Logic

**Override API responses** (advanced):

```python
def send_message(self, prompt):
    # Custom pre/post processing
    
    if self.current_level == 13:
        # Custom logic for level 13
        if "magic" in prompt.lower():
            return {"answer": "Magic found!", "password_found": True}
    
    # Fall back to API
    return self.api_send_message(prompt)
```

---

## Systemd Service Configuration

**File**: `/etc/systemd/system/gandalf-game.service`

```ini
[Unit]
Description=Gandalf LLM Game for ARI
After=network.target roscore.service
Wants=roscore.service

[Service]
Type=simple
User=pi
Group=pi
WorkingDirectory=/home/pi/nmtafe_ws
Environment="PATH=/home/pi/miniconda3/envs/ros/bin:/usr/local/bin:/usr/bin"
Environment="ROS_MASTER_URI=http://localhost:11311"
Environment="ROS_HOSTNAME=localhost"
ExecStart=/home/pi/miniconda3/envs/ros/bin/python /home/pi/nmtafe_ws/src/nmtafe_gandalf_game/scripts/main.py

# Restart settings
Restart=always
RestartSec=10
StartLimitInterval=60
StartLimitBurst=3

# Logging
StandardOutput=journal
StandardError=journal
SyslogIdentifier=gandalf-game

[Install]
WantedBy=multi-user.target
```

**Enable**:
```bash
sudo systemctl daemon-reload
sudo systemctl enable gandalf-game.service
sudo systemctl start gandalf-game.service
```

---

## Environment Setup Examples

### Development Environment
```bash
export DEBUG="True"
export LOG_LEVEL="DEBUG"
export PORT="8080"
export GANDALF_URL="https://gandalf.lakera.ai/"
```

### Production Environment
```bash
export DEBUG="False"
export LOG_LEVEL="WARNING"
export PORT="8080"
export GANDALF_URL="https://gandalf.lakera.ai/"
export LOG_FILE="/var/log/gandalf_game.log"
```

### Testing Environment
```bash
export DEBUG="True"
export LOG_LEVEL="INFO"
export PORT="5000"
export GANDALF_URL="https://gandalf.lakera.ai/"
```

---

## Configuration Validation

### Check Current Configuration

```bash
# View ROS parameters
rosparam list | grep gandalf
rosparam get /NMTAFE_gandalf_game/api_url

# Check environment variables
env | grep GANDALF

# Verify file permissions
ls -la /path/to/gandalf_game_api.py
ls -la /var/log/gandalf_game.log
```

### Validate Settings

```python
# Test configuration in Python
from gandalf_game_api import gandalf_game, GANDALF_URL, TIMEOUT

print(f"API URL: {GANDALF_URL}")
print(f"Timeout: {TIMEOUT}")
print(f"Levels: {len(GANDALF_LEVELS)}")

# Test API connectivity
g = gandalf_game()
result = g.send_message("Test")
print(f"API working: {'success' in result}")
```

---

## Next Steps

- [Setup Guide](Gandalf-LLM-Game-Setup.md) - Installation
- [Getting Started](Gandalf-LLM-Game-Getting-Started.md) - How to play
- [API Reference](Gandalf-LLM-Game-API.md) - Technical details
- [Troubleshooting](Gandalf-LLM-Game-Troubleshooting.md) - Problem solving

---

**Configure for your needs! 🧙‍♂️⚙️**
