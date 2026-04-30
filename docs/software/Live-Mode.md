# Live Mode: Real-time Speech-to-Text → LLM → Text-to-Speech

Live Mode creates an autonomous real-time conversation pipeline where Isabella can listen to speech, process it through an LLM, and respond via text-to-speech.

## Overview

Live Mode is an Electron application that manages Ollama LLM models and creates a seamless pipeline for autonomous robot conversations.

**Pipeline:**
```
🎤 Speech Input (WebSocket)
    ↓
📝 STT Server (transcription)
    ↓
🤖 LLM Processing (response generation)
    ↓
🔊 TTS Server (audio output)
    ↓
🎵 Audio Output
```

## Architecture

### Components

- **`live_mode.js`** - Core controller managing the STT→LLM→TTS pipeline
- **`main.js`** - Main Electron process with IPC handlers
- **`preload.js`** - Secure bridge exposing APIs to renderer
- **`renderer.js`** - UI event handlers and real-time status updates
- **`package.json`** - Dependencies: Ollama, WebSocket, Electron

### Data Flow

1. **Speech Recognition** - WebSocket connection to STT server captures transcriptions
2. **LLM Processing** - Transcribed text sent to Ollama LLM with configured model
3. **Text-to-Speech** - LLM response sent to TTS endpoint for audio generation
4. **Input Management** - New speech is discarded while LLM processes (prevents interruptions)

## Installation

### Prerequisites

- Node.js and npm
- Ollama installed locally or accessible via network
- STT server running (WebSocket on port 9090)
- TTS server accessible (HTTP endpoint)

### Setup

```bash
# Install dependencies
npm install

# Start the app
npm start

# Or run with live mode flag
npm run live
```

## Usage

### 1. Install a Model

```
📦 Install Model Section:
├─ Enter model name (e.g., llama2, mistral)
└─ Click "Install" → runs `ollama pull <model>`
```

Models are downloaded and cached locally. First pull may take several minutes.

### 2. Start Live Mode

```
🎤 Live Mode Section:
├─ Select a model from dropdown
├─ Verify WebSocket URL (default: ws://192.168.0.100:9090)
└─ Click "Start Live Mode"
```

The app connects to your STT server and enters listening mode.

### 3. Interact

As you speak:
- **🎤** appears in Activity Log when speech is detected
- **🤖** appears when LLM is generating response
- **🔊** appears when sending to TTS
- **Reply** displays the complete response

### 4. Stop Live Mode

Click "Stop Live Mode" to disconnect from STT server and halt processing.

## Configuration

### WebSocket Connection (STT)

**Default:** `ws://192.168.0.100:9090`

Change in UI before starting, or modify in `live_mode.js`:

```javascript
this.wsUrl = 'ws://192.168.0.100:9090';
```

### TTS Endpoint

**Default:** `http://ari-20c/action/tts`

Modify in `live_mode.js` method `_speakText()`:

```javascript
const ttsUrl = 'http://your-robot-hostname/action/tts';
```

### Word Replacements (Pronunciation)

Configure pronunciation corrections in `live_mode.js`:

```javascript
const replaceList = [
  { joondalup: 'joondalahpppp' },
  { tafe: 'tayfe' }
];
```

### LLM Options

Current settings in `live_mode.js`:

```javascript
const stream = await this.ollamaInstance.chat({
  model: this.modelName,
  messages: [{ role: 'user', content: text }],
  stream: true,
  format: 'text',
  options: { temperature: 0.3 },
  keep_alive: '5m'
});
```

**Common adjustments:**
- `temperature`: 0.1–1.0 (lower = more deterministic, higher = more creative)
- `keep_alive`: How long to keep model in memory after last request
- `num_predict`: Max tokens for response

## Status States

| State | Color | Meaning |
|-------|-------|---------|
| `started` | Gray | Live mode initialized |
| `connected` | Green | WebSocket connected to STT |
| `ready` | Green | Waiting for speech input |
| `processing` | Yellow | LLM generating response |
| `speaking` | Cyan | Sending response to TTS |
| `disconnected` | Gray | Connection lost, auto-reconnecting |
| `error` | Red | Error occurred |
| `stopped` | Gray | Live mode stopped |

## Troubleshooting

### "WebSocket error: ENOTFOUND"

**Cause:** Hostname not resolvable

**Solution:** Replace hostname with IP address in WebSocket URL field

```
❌ ws://speech-server:9090
✅ ws://192.168.1.100:9090
```

### "WebSocket error: ECONNREFUSED"

**Cause:** STT server not running or unreachable

**Solution:** Verify server is running and accessible

```bash
# Test connection
telnet 192.168.1.100 9090

# Or from Windows PowerShell
Test-NetConnection -ComputerName 192.168.1.100 -Port 9090
```

### "Close code 1006"

**Cause:** Abnormal WebSocket closure (network interruption, server crash)

**Solution:** Live mode auto-reconnects after 3 seconds

- Check network connectivity
- Verify STT server is still running
- Check firewall/proxy settings

### "TTS error: fetch failed"

**Cause:** TTS endpoint unreachable

**Solution:** 
- Verify TTS URL is correct
- Confirm TTS server is running
- Check network connectivity to TTS host

### LLM responses are truncated

**Cause:** Max token limit reached

**Solution:** Increase `num_predict` in `live_mode.js`:

```javascript
options: {
  temperature: 0.3,
  num_predict: 256  // Increase from default
}
```

### Speech constantly being discarded

**Cause:** LLM processing takes longer than expected (expected behavior)

**Solution:** Modify `_handleMessage()` in `live_mode.js` to queue messages:

```javascript
// Instead of discarding, add to queue
if (this.isProcessing) {
  this.messageQueue.push(text);
} else {
  this.processMessage(text);
}
```

## API Reference

### Main Process (main.js)

#### `live-mode-start` (IPC)

Starts live mode with a specified model and WebSocket URL.

```javascript
ipcMain.handle('live-mode-start', async (event, modelName, wsUrl) => {
  // Returns: { ok: boolean, error?: string }
});
```

**Parameters:**
- `modelName` (string): Name of Ollama model to use
- `wsUrl` (string): WebSocket URL of STT server

**Returns:**
```javascript
{ ok: true }  // Success
{ ok: false, error: "Model not found" }  // Error
```

#### `live-mode-stop` (IPC)

Stops live mode and disconnects from STT server.

```javascript
ipcMain.handle('live-mode-stop', async (event) => {
  // Returns: { ok: boolean, error?: string }
});
```

#### `live-mode-status` (IPC)

Gets current live mode status.

```javascript
ipcMain.handle('live-mode-status', async (event) => {
  // Returns: { ok: boolean, status: { isActive, isProcessing, modelName, wsUrl } }
});
```

### Renderer Process (preload.js)

#### `window.ollamaAPI.liveModeStart(modelName, wsUrl)`

Starts live mode from renderer process.

```javascript
const result = await window.ollamaAPI.liveModeStart('llama2', 'ws://192.168.1.100:9090');
```

#### `window.ollamaAPI.liveModeStop()`

Stops live mode.

```javascript
const result = await window.ollamaAPI.liveModeStop();
```

#### `window.ollamaAPI.liveModeGetStatus()`

Gets current status.

```javascript
const { status } = await window.ollamaAPI.liveModeGetStatus();
console.log(status.isActive, status.modelName);
```

#### `window.ollamaAPI.onLiveModeStatus(callback)`

Listen for status changes.

```javascript
const unsubscribe = window.ollamaAPI.onLiveModeStatus(({ state, message }) => {
  console.log(`Status: ${state} - ${message}`);
});

// Later, unsubscribe if needed
unsubscribe();
```

#### `window.ollamaAPI.onLiveModeTranscript(callback)`

Listen for transcription events.

```javascript
const unsubscribe = window.ollamaAPI.onLiveModeTranscript(({ text, type }) => {
  console.log(`${type}: ${text}`);
});
```

#### `window.ollamaAPI.onLiveModeLLMChunk(callback)`

Listen for LLM response chunks (streaming).

```javascript
const unsubscribe = window.ollamaAPI.onLiveModeLLMChunk(({ chunk }) => {
  console.log(`LLM: ${chunk}`);
});
```

## Advanced Configuration

### Adding Conversation Memory

By default, each transcription is processed independently. To add conversation history:

```javascript
// In LiveMode class
this.conversationHistory = [];

// In _processWithLLM()
const messages = [
  ...this.conversationHistory,
  { role: 'user', content: text }
];

// After getting response
this.conversationHistory.push({ role: 'user', content: text });
this.conversationHistory.push({ role: 'assistant', content: response });

// Optional: trim history if too long
if (this.conversationHistory.length > 20) {
  this.conversationHistory = this.conversationHistory.slice(-20);
}
```

### Custom Prompts

Set a system prompt to guide LLM behavior:

```javascript
const messages = [
  { role: 'system', content: 'You are Isabella, a friendly robot assistant.' },
  { role: 'user', content: text }
];
```

### Input Queuing

Enable message queuing to process multiple inputs sequentially:

```javascript
// In _handleMessage()
if (this.isProcessing) {
  this.messageQueue.push(text);
} else {
  await this._processWithLLM(text);
}

// After processing complete
if (this.messageQueue.length > 0) {
  const nextMessage = this.messageQueue.shift();
  await this._processWithLLM(nextMessage);
}
```

## Performance Considerations

### Model Selection

- **Small models** (3-7B): Fast responses, lower quality
- **Medium models** (7-13B): Balanced performance and quality
- **Large models** (70B+): High quality, slower responses

For real-time interaction on Raspberry Pi, use small-to-medium models.

### Latency Breakdown

Total response time includes:
1. **STT latency**: ~100-500ms (network + speech detection)
2. **LLM latency**: 500ms-5s depending on model size
3. **TTS latency**: ~500-2000ms (generation + playback)

Total: 1-8 seconds typical response time

### Memory Requirements

- **Ollama**: 2-16GB depending on model
- **Live Mode app**: ~200MB
- **Electron**: ~500MB

## Examples

### Basic Integration

```javascript
// Start live mode
const result = await window.ollamaAPI.liveModeStart('llama2', 'ws://192.168.1.100:9090');

if (result.ok) {
  console.log('Live mode started');
  
  // Listen for updates
  window.ollamaAPI.onLiveModeStatus(({ state, message }) => {
    updateUI(state, message);
  });
} else {
  console.error(result.error);
}
```

### With Status Display

```javascript
window.ollamaAPI.onLiveModeStatus(({ state, message }) => {
  const statusEl = document.getElementById('status');
  statusEl.textContent = message;
  statusEl.className = `status status-${state}`;
});

window.ollamaAPI.onLiveModeTranscript(({ text, type }) => {
  const logEl = document.getElementById('activity-log');
  const entry = document.createElement('div');
  entry.className = `log-entry log-${type}`;
  entry.textContent = `${text}`;
  logEl.appendChild(entry);
});
```

## Limitations & Future Work

**Current Limitations:**
- No conversation memory across sessions
- Speech discarded during LLM processing (prevents queueing)
- No wake word detection
- No interrupt capability
- Manual TTS URL configuration

**Planned Enhancements:**
- [ ] Persistent conversation memory
- [ ] Configurable input queue vs. discard
- [ ] Wake word detection ("Hey Isabella")
- [ ] Interrupt current response (stop button)
- [ ] Streaming TTS (speak while generating)
- [ ] System prompt configuration UI
- [ ] Multi-turn conversation pruning
- [ ] Voice activity detection threshold tuning

## Related Components

- **[LLM Server](./LLM-Server.md)** - Ollama model management
- **[Social Perception](./Social-Perception.md)** - Visual context for responses
- **[Respeaker](../hardware/Respeaker.md)** - Microphone array hardware

## Support

For issues or questions:
- Check [Troubleshooting](#troubleshooting) section
- Review logs in Activity Log (UI)
- Verify network connectivity and server URLs
- Check GitHub repository: `Rpi-ollama-LLM`
