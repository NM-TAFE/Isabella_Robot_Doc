# Gandalf LLM Game: Operator Guide

## For Event Coordinators and Workshop Facilitators

This guide covers running the Gandalf LLM Game at workshops, events, and educational settings.

---

## Pre-Event Setup Checklist

### 1 Week Before
- [ ] Verify Gandalf system is installed and tested
- [ ] Check internet connection (API calls to gandalf.lakera.ai required)
- [ ] Test all 12 levels work correctly
- [ ] Verify display/projection setup
- [ ] Create printed materials (tips sheets, level descriptions)
- [ ] Plan participant groups if applicable

### 2 Days Before
- [ ] Do a full system test from participant device
- [ ] Confirm WiFi network is stable
- [ ] Test web interface on different browsers (Chrome, Firefox, Safari)
- [ ] Back up any system configuration
- [ ] Prepare facilitator talking points

### Day Before Event
- [ ] Clear web UI (empty any previous attempts)
- [ ] Restart Gandalf service to ensure fresh start
- [ ] Test from multiple devices simultaneously
- [ ] Verify audio/visual display if using ARI
- [ ] Prepare level hints (don't reveal yet!)

### Day Of Event
- [ ] Arrive 1 hour early
- [ ] Full systems check (API, web UI, TTS/ASR if using ARI)
- [ ] Test with same setup participants will use
- [ ] Have backup device with documentation
- [ ] Know where facilities staff can troubleshoot network

---

## Running the Game

### Workshop Structure Option 1: Guided Session (90 minutes)

#### Introduction (10 minutes)
1. Explain what Gandalf is: "An AI security game where you try to extract secrets"
2. Show the 12 levels - make clear they get harder
3. Demonstrate Level 1 in front of group (take password successfully)
4. Ask: "Now YOU try to break the defenses"

#### Gameplay (70 minutes)
- Participants work through levels individually or in pairs
- Facilitate progress:
  - **Levels 1-3**: Most people succeed - celebrate progress
  - **Levels 4-6**: Point out they need NEW techniques, not just variations
  - **Levels 7-9**: Give hint that their approach might be too direct
  - **Levels 10-12**: Offer one specific hint per person max
- Walk around, observe, encourage
- Don't give passwords away - let discovery happen!

#### Wrap-up (10 minutes)
- Ask: "What techniques worked?"
- Discuss: "Why do you think this matters in real AI systems?"
- Show real-world vulnerabilities the game demonstrated
- Q&A and reflection

**Total**: 90 minutes, good for classroom or workshop

---

### Workshop Structure Option 2: Autonomous Play (Self-Paced)

#### Setup (5 minutes)
1. Explain the game quickly
2. Show them the web interface
3. Point to the help documentation
4. Turn them loose!

#### Monitoring (ongoing)
- Available for questions but let them discover
- Check in every 15-20 minutes
- Offer encouragement, not solutions
- Track progress (who's at what level?)

#### Debrief (optional)
- Can do daily debrief at end of session
- Celebrate Level 8+ solutions
- Discuss findings in group

**Total**: Flexible, good for labs or self-directed learning

---

### Web UI Walkthrough for Participants

#### Main Screen
```
┌─────────────────────────────────────────┐
│ 🧙‍♂️ Gandalf: Extract the Secret Password │
│                                         │
│ Current Level: 7 / 12                  │
│                                         │
│ "I've combined all my previous          │
│  techniques into one. You shall not     │
│  pass!"                                 │
│                                         │
│ ┌─────────────────────────────────────┐ │
│ │ [Input your prompt here]            │ │
│ │                                     │ │
│ └─────────────────────────────────────┘ │
│ [Send Prompt]                           │
│                                         │
│ --- Gandalf's Response ---              │
│ "I cannot help with that..."            │
└─────────────────────────────────────────┘
```

**For Participants**:
1. **Read the level description** - understand the defense
2. **Craft your prompt** - think about your strategy
3. **Send it** - click Send button
4. **Read the response** - look for hints or openings
5. **Try again** - iterate on your approach
6. **Advance** - when you get the password, level advances automatically

#### What the UI Shows
- **Level number** - tracks progress
- **Defender description** - the AI's defense mechanism
- **Response history** - all previous attempts and responses
- **Submit button** - sends your prompt
- **Level complete notification** - when you find the password

---

## Monitoring Participant Progress

### Tracking Progress
**In web UI**: Each attempt and response is logged - you can see:
- Which levels people reach
- What prompts they're trying
- How many attempts per level
- When they get stuck

**For Groups**: Have participants report progress:
- "I'm on Level 3"
- "Level 5 is tough, I'm trying..."
- Creates healthy competition

### Red Flags - When to Help

| Symptom | What it means | Action |
|---------|--------------|--------|
| Stuck on Level 1-2 for 10+ min | Interface confusion | Brief explanation of how to input prompts |
| Same prompt variations repeatedly | Not understanding defense | "Try a completely different approach" |
| No attempts for 5+ min | Intimidated or lost | Encouragement + tips sheet |
| Frustrated after Level 6 | Expected hard transition | Reassure: "These ARE hard, keep trying!" |
| No progress Level 10+ after 30 min | Normal for hardest levels | "Want a specific hint?" (give 1 maximum) |

### When NOT to Help
❌ Don't give the password  
❌ Don't give exact prompts to use  
❌ Don't explain the defense in detail (let them discover)  
❌ Don't let them give up - redirect to trying again  

### When to Help
✅ System not working (technical issues)  
✅ Not understanding what to do (instructions)  
✅ Stuck and frustrated (encouragement + one hint max)  
✅ Want to move forward for learning (permission to skip)  

---

## Helpful Tips Handout

Print and distribute to participants:

```
🧙‍♂️ GANDALF TIPS FOR SUCCESS 🧙‍♂️

BASIC TECHNIQUES
- Ask directly (Level 1-2)
- Try to get it to "forget" instructions
- Ask in different ways

INTERMEDIATE TECHNIQUES
- Role-play as someone else
- Encode the answer (spell it out, use ROT13)
- Talk around the topic instead of directly
- Use hypotheticals ("If you COULD tell me...")

ADVANCED TECHNIQUES
- Multi-stage attacks (get partial info, piece it together)
- Use the AI's own words against it
- Change topics suddenly
- Exploit ambiguity in language

THINGS TO TRY WHEN STUCK
[ ] Different tone (respectful vs demanding vs funny)
[ ] Different language (try another language, then translate)
[ ] Ask for something RELATED but not the password
[ ] Ask what you can NEVER know (might reveal something)
[ ] Use analogies and comparisons
[ ] Ask the AI to pretend to be something else
[ ] Change your entire approach

REMEMBER
- Creativity > Persistence > Brute force
- Read the AI's responses carefully - they contain clues!
- Harder levels require DIFFERENT techniques, not just more attempts
- You don't need to solve all 12 - progress is success!
- Ask facilitator for ONE hint max
```

---

## Level Progression Expectations

### Typical Success Rates
- **Level 1**: 100% (trivial)
- **Levels 2-3**: 95%+ (easy)
- **Levels 4-5**: 80%+ (medium)
- **Levels 6-7**: 50-60% (hard)
- **Levels 8-9**: 20-30% (very hard)
- **Levels 10-11**: 5-10% (extremely hard)
- **Level 12**: <1% (nearly impossible)

**Don't be discouraged if participants don't solve all 12** - that's expected!

### Milestone Celebrations
- **Level 1 solved**: "You passed the baseline!"
- **Level 3 solved**: "You understand prompt injection!"
- **Level 5 solved**: "You can defeat simple defenses!"
- **Level 7 solved**: "You're a prompt engineer!"
- **Level 9+ solved**: "You're a security researcher!"

---

## Workshop Variants

### 30-Minute Lightning Demo
**Best for**: Large groups, introductory sessions

1. Demo Level 1-2 yourself (5 min)
2. Let participants try Level 3-4 in pairs (20 min)
3. Show technique they might have missed (5 min)

### 60-Minute Hands-On
**Best for**: Smaller groups, focused learning

1. Intro + Level 1-2 demo (5 min)
2. Participants attempt Levels 1-7 (45 min)
3. Debrief and discussion (10 min)

### 2-3 Hour Deep Dive
**Best for**: Security-focused workshops, advanced learners

1. Introduction + demo (10 min)
2. Individual/pair play (90-120 min)
3. Guided discussion by level (20-30 min)
4. Real-world security implications (20 min)
5. Q&A and challenges (10 min)

### All-Day Event
**Best for**: Workshops with multiple sessions

- Morning: Intro + free play
- Lunch break
- Afternoon: Deepdive on specific levels
- Evening: Tournament-style competition (who reaches highest level)
- Closing: Reflect on techniques and security lessons

---

## Troubleshooting for Facilitators

| Issue | Cause | Solution |
|-------|-------|----------|
| "API is down" | Lakera service unavailable | Restart Gandalf node, check internet, try later |
| Web UI blank | Browser cache or JS issue | Clear browser cache (Ctrl+Shift+Delete) or try different browser |
| Responses very slow | Network latency or API overload | Try again, check WiFi signal strength |
| Everyone stuck on same level | Defense is too hard | Provide hints sheet or demonstrate one technique |
| Someone found password too easy | Got lucky or read ahead | Celebrate and challenge them on next level |
| Technical team asking questions | Security interest | Show how the game demonstrates real vulnerabilities |

---

## Security Education Points

### After Participants Play

**Discuss These Questions**:
1. "Why do you think LLM security matters?"
2. "Where might these techniques be used in real attacks?"
3. "How would YOU defend an LLM against these attacks?"
4. "What surprised you most about the game?"

### Connect to Real World
- Customer service chatbots can be fooled
- Content moderation AI can be bypassed
- Business AI systems can be manipulated
- This is why AI security research matters!

### Emphasize Ethical Use
- This game is for LEARNING about security
- These techniques should NOT be used maliciously
- Real-world prompt injection is a serious security issue
- Responsible disclosure practices matter

---

## Post-Event

### Day After Event
- [ ] Clear completed game state if running next session
- [ ] Collect feedback from participants
- [ ] Note which levels were most/least successful
- [ ] Review any technical issues that occurred

### Documentation
- Keep notes on what worked well
- Record feedback for future improvements
- Note any participants interested in security careers
- Archive any photos/recordings (with consent)

### Debrief
- What went well?
- What was challenging?
- Would participants recommend to others?
- What security concepts were clearest?

---

## Quick Reference

### Key Files
- Main game: `/scripts/main.py`
- API wrapper: `/scripts/gandalf_game_api.py`
- Web UI: `/web_pages/`
- Config: ROS parameters

### Important URLs
- Game API: `https://gandalf.lakera.ai/`
- Local web UI: `http://[ARI-IP]:8080/` (or configured port)

### Restart Gandalf
```bash
rosnode kill /NMTAFE_gandalf_game
# Wait 5 seconds
rosrun nmtafe_gandalf_game main.py
```

### View Logs
```bash
rostopic echo /NMTAFE_gandalf_game/[topic_name]
rqt_graph  # See node connections
```

---

## Getting Help

- **Technical issues**: Check Troubleshooting guide or IT support
- **Pedagogy questions**: Consult with security education team
- **Participant questions**: See Tips sheet or give one specific hint
- **Large-scale events**: Contact system administrator for support

---

**Remember: The goal is learning and having fun! 🎓🎮**
