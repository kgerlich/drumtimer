# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file web application called "Drum Timer" - a browser-based timer with an integrated drum machine for meditation, practice sessions, or timed activities. The entire application is contained in `Drumtimer3.html` as a self-contained HTML file with embedded CSS and JavaScript.

## Core Architecture

### Single-File Structure
- **HTML/CSS/JS**: All code exists in one file (`Drumtimer3.html`)
- **No build process**: Open the HTML file directly in a browser to run
- **No dependencies**: Pure vanilla JavaScript with Web Audio API

### Main Components

**DrumTimerApp Class** (line 821)
The central application class that manages all functionality:

1. **Timer System** (lines 1965-2103)
   - Countdown timer with minutes/seconds input
   - Start/pause/reset functionality
   - Progress bar visualization

2. **Drum Machine** (lines 1475-1963)
   - 16-step sequencer (16th notes in 4/4 time)
   - Three drum tracks: kick, snare, hi-hat
   - Pattern editor with visual beat toggles
   - BPM control (60-200 BPM)
   - Preset patterns: Basic Rock, Disco, Funk, Reggae

3. **Audio System** (lines 1475-1895)
   - Web Audio API for sound generation
   - Two sound modes:
     - Synthesized drums (generated algorithmically)
     - Custom uploaded samples (user WAV/MP3/OGG files)

4. **Sample Compression & Storage** (lines 909-1159)
   - Compresses uploaded audio to 16-bit PCM at 22kHz mono
   - Stores samples in localStorage as base64
   - Shows compression ratio and storage info
   - Automatic sample loading on page load

5. **Silent Bar System** (lines 1687-1778)
   - Generates random pattern of silent bars
   - First bar always plays (timing reference)
   - Last bar triggers a "gong" sound
   - Configurable probability slider (0-50%)

6. **Settings Persistence** (lines 1160-1409)
   - Saves/loads all settings to localStorage
   - Exports complete configuration as JSON
   - Custom pattern save/delete functionality

## Key Technical Details

### Audio Compression Flow
1. User uploads audio file → `handleSampleUpload()` (line 2168)
2. File converted to ArrayBuffer → `compressAudioData()` (line 911)
3. Downsampled to 22kHz mono, converted to 16-bit PCM
4. Encoded as base64 and saved to localStorage
5. On page load: `loadStoredSamples()` (line 1084) decompresses and restores audio

### Pattern Playback Logic
- Beat duration calculated from BPM: `(60 / BPM / 4) * 1000` ms (line 1943)
- 16-step pattern loops continuously (line 1945)
- Bar tracking: Every 16 beats = 1 bar (line 1922)
- Silent bars skip audio playback but continue visual animation

### LocalStorage Keys
- `drumTimerSettings`: All app settings (BPM, volume, patterns, etc.)
- `drumSample_kick`, `drumSample_snare`, `drumSample_hihat`: Compressed audio data

## Common Development Tasks

### Testing the Application
Simply open `Drumtimer3.html` in a modern web browser (Chrome, Firefox, Edge, Safari).

### Modifying Drum Patterns
Edit the preset objects in the `presets` property (lines 867-893):
```javascript
presets = {
    basic: {
        kick: [1, 0, 0, 0, ...],  // 16 values: 1 = play, 0 = silent
        snare: [0, 0, 0, 0, ...],
        hihat: [1, 1, 1, 1, ...]
    }
}
```

### Adjusting Audio Synthesis
Synthesized drum sounds are in:
- `createKickDrum()` (line 1782) - sine wave with frequency sweep
- `createSnareDrum()` (line 1805) - white noise burst
- `createHiHat()` (line 1832) - filtered noise with high-pass at 8kHz
- `createGong()` (line 1865) - multiple sine harmonics (200-1600Hz)

### Modifying Compression Settings
In `compressAudioData()` (line 911), change:
- `targetSampleRate` (currently 22050 Hz) - lower = more compression
- `targetChannels` (currently 1 for mono) - stereo would be 2

## Important Constraints

### Browser Compatibility
- Requires Web Audio API support (all modern browsers)
- AudioContext may be suspended until user interaction (line 2197)
- LocalStorage has ~5-10MB limit per domain

### Performance Notes
- Timer uses `setInterval` at 1-second intervals (line 2046)
- Drum pattern uses `setInterval` at beat intervals (line 1945)
- All audio samples must fit in localStorage after compression

### State Management
- No external state management - all state in `DrumTimerApp` instance
- Settings auto-save on changes (BPM, volume, patterns)
- Manual "Save All Settings" for complete state preservation

## User Features

### Keyboard Shortcuts
- **Space**: Start/pause timer (line 2205)
- **Escape**: Reset timer (line 2212)

### Pattern Editor
- Click beat toggles to enable/disable drum hits
- Visual feedback shows which drums play on each beat
- Real-time pattern preview in beat indicator grid

### Sample Management
- Upload custom drum samples (WAV/MP3/OGG)
- Samples compressed ~50-80% for localStorage
- Test individual samples or all at once
- Clear samples to revert to synthesized drums

### Bar Timeline Visualization
- Shows all bars for the entire timer duration
- Green = playing bar, Red = silent bar
- Current bar highlighted and auto-scrolls into view
