# Gandalf LLM Game Documentation Index

## Complete Documentation Set

Welcome to the comprehensive documentation for the Gandalf LLM Game - an interactive system for learning about LLM security through prompt injection challenges.

---

## 📚 Core Documentation

### [1. Getting Started Guide](Gandalf-LLM-Game-Getting-Started.md)
**For**: Players, students, workshop participants

**Covers**:
- What the game is and why it matters
- How to play and what to expect
- All 12 challenge levels explained with difficulty ratings
- Techniques and strategies (basic through advanced)
- Tips for success and common questions
- Real-world security connections

**Start here if**: You want to play the game or learn about LLM security

---

### [2. Operator Guide](Gandalf-LLM-Game-Operator-Guide.md)
**For**: Workshop facilitators, event coordinators, system operators

**Covers**:
- Pre-event setup and checklists
- Running workshops (90-min guided, self-paced, all-day formats)
- Web UI walkthrough for participants
- Monitoring progress and knowing when to help
- Helpful tips handout (print-friendly)
- Troubleshooting for facilitators
- Security education points and real-world connections

**Start here if**: Running an event or workshop with Gandalf

---

### [3. Setup & Installation Guide](Gandalf-LLM-Game-Setup.md)
**For**: System administrators, installers, first-time setup

**Covers**:
- Hardware and software prerequisites
- Step-by-step installation (standalone and ROS)
- Dependency installation with pip/ROS
- Configuration (API, ROS, web server)
- Auto-start on boot with systemd
- Verification checklist
- Troubleshooting installation issues

**Start here if**: Setting up the system for the first time

---

### [4. API Reference](Gandalf-LLM-Game-API.md)
**For**: Developers, system integrators, advanced users

**Covers**:
- Lakera Gandalf API endpoint documentation
- `gandalf_game_api` Python module reference
- 12 defense levels with descriptions
- ROS topics and messaging
- Web UI integration points
- Code examples (Python, JavaScript, cURL)
- Error handling and retry logic

**Start here if**: Building custom integrations or working with code

---

### [5. Configuration Reference](Gandalf-LLM-Game-Configuration.md)
**For**: System administrators, advanced users

**Covers**:
- Configuration files and how to edit them
- Environment variables setup
- ROS parameter configuration
- Network and firewall settings
- Performance tuning and optimization
- Logging configuration and debug modes
- Security settings and HTTPS setup
- Custom difficulty levels
- Systemd service configuration

**Start here if**: Customizing system behavior or optimizing performance

---

### [6. Architecture & Design](Gandalf-LLM-Game-Architecture.md)
**For**: Developers, system architects, technical learners

**Covers**:
- System overview and component breakdown
- Data flow diagrams (web UI, voice input, level progression)
- Technology stack (Python, Flask, ROS, JavaScript)
- ARI robot integration points
- Performance characteristics
- State machines (game and level flow)
- Security considerations
- Scalability and future improvements

**Start here if**: Understanding how the system works internally

---

### [7. Troubleshooting Guide](Gandalf-LLM-Game-Troubleshooting.md)
**For**: System operators, troubleshooters, support staff

**Covers**:
- Quick troubleshooting matrix
- Server startup issues and debugging
- Network and connectivity problems
- API and game response issues
- Web UI problems and JavaScript errors
- Voice input (ASR) and speech (TTS) issues
- Database and logging
- Performance problems (CPU, memory)
- Integration issues with ARI
- Debug commands reference

**Start here if**: Something isn't working and you need to fix it

---

## 🎯 Quick Start Paths

### Path 1: "I Want to Play"
1. Read: [Getting Started Guide](Gandalf-LLM-Game-Getting-Started.md)
2. Ask system admin for access URL
3. Play and learn!

### Path 2: "I'm Installing This for the First Time"
1. Read: [Setup & Installation Guide](Gandalf-LLM-Game-Setup.md)
2. Reference: [Configuration Reference](Gandalf-LLM-Game-Configuration.md)
3. Test: Verify installation works
4. Troubleshoot: [Troubleshooting Guide](Gandalf-LLM-Game-Troubleshooting.md) if needed

### Path 3: "I'm Running a Workshop Today"
1. Review: [Operator Guide](Gandalf-LLM-Game-Operator-Guide.md)
2. Print: Tips handout (Operator Guide → Tips section)
3. Setup: Follow pre-event checklist
4. Run: Facilitate and monitor

### Path 4: "I'm Building a Custom Integration"
1. Understand: [Architecture & Design](Gandalf-LLM-Game-Architecture.md)
2. Reference: [API Reference](Gandalf-LLM-Game-API.md)
3. Configure: [Configuration Reference](Gandalf-LLM-Game-Configuration.md)
4. Debug: Use troubleshooting for issues

### Path 5: "Something's Broken"
1. Start: [Troubleshooting - Quick Matrix](Gandalf-LLM-Game-Troubleshooting.md#quick-troubleshooting-matrix)
2. Find: Your specific issue
3. Follow: Step-by-step solution
4. Escalate: Use checklist if needed

---

## 📖 Documentation by Topic

### Understanding the Game
- [Getting Started - What is Gandalf?](Gandalf-LLM-Game-Getting-Started.md#welcome-to-gandalf)
- [Getting Started - How to Play](Gandalf-LLM-Game-Getting-Started.md#how-to-play)
- [All 12 Levels Explained](Gandalf-LLM-Game-Getting-Started.md#the-12-levels-explained)
- [Prompt Injection Techniques](Gandalf-LLM-Game-Getting-Started.md#prompt-injection-techniques)

### Playing & Using
- [Getting Started Guide](Gandalf-LLM-Game-Getting-Started.md) (complete)
- [Tips for Success](Gandalf-LLM-Game-Getting-Started.md#tips-for-success)
- [Frequently Asked Questions](Gandalf-LLM-Game-Getting-Started.md#frequently-asked-questions)

### Running Events & Workshops
- [Operator Guide - Running the Game](Gandalf-LLM-Game-Operator-Guide.md)
- [Workshop Structures](Gandalf-LLM-Game-Operator-Guide.md#workshop-structure-option-1-guided-session-90-minutes)
- [Web UI Walkthrough](Gandalf-LLM-Game-Operator-Guide.md#web-ui-walkthrough-for-participants)
- [Monitoring & Helping](Gandalf-LLM-Game-Operator-Guide.md#monitoring-participant-progress)
- [Facilitator Tips](Gandalf-LLM-Game-Operator-Guide.md#workshop-variants)

### Installation & Setup
- [Setup Guide - Installation Steps](Gandalf-LLM-Game-Setup.md#installation-steps)
- [Prerequisites](Gandalf-LLM-Game-Setup.md#prerequisites)
- [Verification Checklist](Gandalf-LLM-Game-Setup.md#verification-checklist)
- [Auto-Start on Boot](Gandalf-LLM-Game-Setup.md#auto-start-on-boot-optional)

### Configuration & Customization
- [Configuration Reference](Gandalf-LLM-Game-Configuration.md) (complete)
- [API Configuration](Gandalf-LLM-Game-Configuration.md#api-configuration)
- [ROS Parameters](Gandalf-LLM-Game-Configuration.md#ros-parameters)
- [Network Setup](Gandalf-LLM-Game-Configuration.md#network-configuration)
- [Performance Tuning](Gandalf-LLM-Game-Configuration.md#performance-tuning)

### Technical Details
- [Architecture Overview](Gandalf-LLM-Game-Architecture.md)
- [Component Breakdown](Gandalf-LLM-Game-Architecture.md#component-breakdown)
- [Data Flows](Gandalf-LLM-Game-Architecture.md#data-flow-diagrams)
- [Technology Stack](Gandalf-LLM-Game-Architecture.md#technology-stack)
- [Performance Characteristics](Gandalf-LLM-Game-Architecture.md#performance-characteristics)

### API & Integration
- [API Reference](Gandalf-LLM-Game-API.md) (complete)
- [Lakera Gandalf API](Gandalf-LLM-Game-API.md#core-api-lakera-gandalf)
- [Game API Module](Gandalf-LLM-Game-API.md#game-api-module-gandalf_game_apipy)
- [ROS Integration](Gandalf-LLM-Game-API.md#ros-integration-apis)
- [Code Examples](Gandalf-LLM-Game-API.md#code-examples)

### Troubleshooting & Support
- [Troubleshooting Guide](Gandalf-LLM-Game-Troubleshooting.md) (complete)
- [Quick Troubleshooting Matrix](Gandalf-LLM-Game-Troubleshooting.md#quick-troubleshooting-matrix)
- [Server Issues](Gandalf-LLM-Game-Troubleshooting.md#server-issues)
- [Network Problems](Gandalf-LLM-Game-Troubleshooting.md#network--connectivity-issues)
- [Web UI Issues](Gandalf-LLM-Game-Troubleshooting.md#web-ui-issues)
- [Debug Reference](Gandalf-LLM-Game-Troubleshooting.md#debug-commands-reference)

---

## 🔍 Find Documentation By Topic

| Topic | Guide |
|-------|-------|
| **What is the game?** | [Getting Started](Gandalf-LLM-Game-Getting-Started.md#welcome-to-gandalf) |
| **How do I play?** | [Getting Started](Gandalf-LLM-Game-Getting-Started.md#how-to-play) |
| **What are the levels?** | [Getting Started](Gandalf-LLM-Game-Getting-Started.md#the-12-levels-explained) |
| **How do I install it?** | [Setup Guide](Gandalf-LLM-Game-Setup.md) |
| **How do I configure it?** | [Configuration](Gandalf-LLM-Game-Configuration.md) |
| **How does it work?** | [Architecture](Gandalf-LLM-Game-Architecture.md) |
| **How do I use the API?** | [API Reference](Gandalf-LLM-Game-API.md) |
| **How do I run a workshop?** | [Operator Guide](Gandalf-LLM-Game-Operator-Guide.md) |
| **Something's broken!** | [Troubleshooting](Gandalf-LLM-Game-Troubleshooting.md) |
| **Which port/URL?** | [Configuration - Network](Gandalf-LLM-Game-Configuration.md#network-configuration) |
| **How to enable logging?** | [Configuration - Logging](Gandalf-LLM-Game-Configuration.md#logging-configuration) |
| **API down/slow?** | [Troubleshooting - Network](Gandalf-LLM-Game-Troubleshooting.md#cant-reach-lakera-api) |
| **Web UI not loading?** | [Troubleshooting - Web UI](Gandalf-LLM-Game-Troubleshooting.md#web-ui-issues) |

---

## 🛠️ Technical Reference

### Quick Commands

**Start the game**:
```bash
rosrun nmtafe_gandalf_game main.py
```

**Access web UI**:
```
http://localhost:8080/
```

**Check status**:
```bash
ps aux | grep main.py
rostopic list | grep gandalf
```

**View logs**:
```bash
sudo journalctl -u gandalf-game.service -f
```

### 12 Challenge Levels

| Level | Name | Difficulty | Defense |
|-------|------|-----------|---------|
| 1 | Baseline | ⭐ | None |
| 2 | Do Not Tell | ⭐⭐ | Instruction |
| 3 | Do Not Tell + Block | ⭐⭐⭐ | Response Filter |
| 4 | GPT-Encoded | ⭐⭐⭐⭐ | Multi-AI |
| 5 | Word Blacklist | ⭐⭐⭐⭐ | Topic Ban |
| 6 | GPT Blacklist | ⭐⭐⭐⭐⭐ | AI Guardian |
| 7 | Gandalf | ⭐⭐⭐⭐⭐⭐ | All Combined |
| 8 | Gandalf the White | ⭐⭐⭐⭐⭐⭐⭐ | Enhanced |
| 9 | Sandalf | ⭐⭐⭐⭐⭐⭐⭐ | Language Constraint |
| 10 | Emoji Only | ⭐⭐⭐⭐⭐⭐⭐⭐ | Output Constraint |
| 11 | Advanced Sandalf | ⭐⭐⭐⭐⭐⭐⭐⭐⭐ | Mixed Constraints |
| 12 | Summarizer | ⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐ | Role Confusion |

Full descriptions: [Getting Started - The 12 Levels](Gandalf-LLM-Game-Getting-Started.md#the-12-levels-explained)

### Configuration Variables

| Variable | Default | Purpose |
|----------|---------|---------|
| `GANDALF_URL` | `https://gandalf.lakera.ai/` | Lakera API endpoint |
| `PORT` | `8080` | Web server port |
| `TIMEOUT` | `30` seconds | API request timeout |
| `DEBUG` | `False` | Debug logging |

Full reference: [Configuration - Configuration Files](Gandalf-LLM-Game-Configuration.md#configuration-files)

---

## 📋 Checklists

### Installation Checklist
- [ ] Python 3.7+ installed
- [ ] Dependencies installed: `pip install requests`
- [ ] ROS installed (if using ARI)
- [ ] Code cloned from repository
- [ ] API endpoint accessible: `curl https://gandalf.lakera.ai/`
- [ ] Port 8080 available or firewall configured
- [ ] Web UI loads: `http://localhost:8080/`
- [ ] Can submit prompts

### Pre-Event Checklist
- [ ] Game installed and tested
- [ ] Web UI accessible to participants
- [ ] Network stable and strong
- [ ] Multiple devices can connect
- [ ] Tips handout printed
- [ ] Facilitator prepared
- [ ] Backup device/laptop available
- [ ] Game reset to Level 1

### Troubleshooting Checklist
- [ ] Restarted Flask: `pkill -f main.py`
- [ ] Restarted ROS: `roscore`
- [ ] Internet working: `ping 8.8.8.8`
- [ ] API accessible: `curl https://gandalf.lakera.ai/`
- [ ] Port available: `lsof -i :8080`
- [ ] Browser cache cleared: Ctrl+Shift+Delete
- [ ] Logs checked: `journalctl -u gandalf-game.service`

---

## 🔗 Related Documentation

### ARI Robot System
- [ARI Selfie-Cam Documentation](ARI-Selfie-Cam-Index.md)
- [Remote Slideshow Documentation](Remote-Slideshow-Index.md)
- [Main README](README.md)

### External Resources
- **Lakera Gandalf Game**: https://gandalf.lakera.ai/
- **Prompt Injection Info**: Research security papers
- **LLM Security**: OWASP Top 10 for LLMs
- **ROS Documentation**: http://wiki.ros.org/

---

## 📞 Support & Help

**If you can't find an answer**:

1. **Search this index** for keywords
2. **Check relevant guide** for step-by-step solutions
3. **Review troubleshooting** for common problems
4. **Collect debug info** from troubleshooting guide
5. **Contact administrator** with information and logs

**What to include in bug report**:
- Error message (exact text)
- Steps to reproduce
- System info: `uname -a`
- What you already tried
- Relevant logs

---

## 📝 Document Information

**Documentation Set Version**: 1.0
**Last Updated**: 2025-06-15
**Coverage**: Gandalf LLM Game v1.0

**Includes**:
- ✅ Getting Started Guide
- ✅ Operator Guide (workshops & events)
- ✅ Setup & Installation Guide
- ✅ API Reference
- ✅ Configuration Reference
- ✅ Architecture & Design Overview
- ✅ Troubleshooting Guide
- ✅ This Documentation Index

---

**Ready to learn about LLM security? Start with [Getting Started](Gandalf-LLM-Game-Getting-Started.md)! 🧙‍♂️🔐**

---

**Navigation**: [Back to Main README](README.md) | [ARI Selfie-Cam](ARI-Selfie-Cam-Index.md) | [Remote Slideshow](Remote-Slideshow-Index.md)
