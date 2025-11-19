# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file web application called "Drum Timer" - a browser-based timer with an integrated drum machine for meditation, practice sessions, or timed activities. The entire application is contained in `index.html` as a self-contained HTML file with embedded CSS and JavaScript.

## Core Architecture

### Single-File Structure
- **HTML/CSS/JS**: All code exists in one file (`index.html`)
- **No build process**: Open the HTML file directly in a browser to run
- **No dependencies**: Pure vanilla JavaScript with Web Audio API

### Main Components

**DrumTimerApp Class** (line 1409)
The central application class that manages all functionality:

1. **Timer System** (lines 2694-2781)
   - Countdown timer with minutes/seconds input
   - Start/pause/reset functionality
   - Progress bar visualization

2. **Drum Machine** (lines 2070-2630)
   - 16-step sequencer (16th notes in 4/4 time)
   - Three drum tracks: kick, snare, hi-hat
   - Pattern editor with visual beat toggles
   - BPM control (60-200 BPM)
   - Preset patterns: Basic Rock, Disco, Funk, Reggae

3. **Audio System** (lines 2436-2552)
   - Web Audio API for sound generation
   - Two sound modes:
     - Synthesized drums (generated algorithmically)
     - Custom uploaded samples (user WAV/MP3/OGG files)

4. **Sample Compression & Storage** (lines 1500-1700)
   - Compresses uploaded audio to 16-bit PCM at 22kHz mono
   - Stores samples in localStorage as base64
   - Shows compression ratio and storage info
   - Automatic sample loading on page load

5. **Silent Bar System** (lines 2311-2400)
   - Generates random pattern of silent bars
   - First bar always plays (timing reference)
   - Last bar triggers a "gong" sound
   - Configurable probability slider (0-50%)

6. **Settings Persistence** (lines 1750-1900)
   - Saves/loads all settings to localStorage
   - Exports complete configuration as JSON
   - Custom pattern save/delete functionality

## Key Technical Details

### Audio Compression Flow
1. User uploads audio file → `handleSampleUpload()` (line 2824)
2. File converted to ArrayBuffer → `compressAudioData()` (line 1500)
3. Downsampled to 22kHz mono, converted to 16-bit PCM
4. Encoded as base64 and saved to localStorage
5. On page load: `loadStoredSamples()` (line 1673) decompresses and restores audio

### Pattern Playback Logic
- Beat duration calculated from BPM: `(60 / BPM / 4) * 1000` ms
- 16-step pattern loops continuously via `setInterval` (line 2599)
- Bar tracking: Every 16 beats = 1 bar
- Silent bars skip audio playback but continue visual animation

### LocalStorage Keys
- `drumTimerSettings`: All app settings (BPM, volume, patterns, etc.)
- `drumSample_kick`, `drumSample_snare`, `drumSample_hihat`: Compressed audio data

## Common Development Tasks

### Testing the Application
Simply open `index.html` in a modern web browser (Chrome, Firefox, Edge, Safari).

### Modifying Drum Patterns
Edit the preset objects in the `presets` property (line 1456):
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
- `createKickDrum()` (line 2436) - sine wave with frequency sweep
- `createSnareDrum()` (line 2459) - white noise burst
- `createHiHat()` (line 2486) - filtered noise with high-pass at 8kHz
- `createGong()` (line 2519) - multiple sine harmonics (200-1600Hz)

### Modifying Compression Settings
In `compressAudioData()` (line 1500), change:
- `targetSampleRate` (currently 22050 Hz) - lower = more compression
- `targetChannels` (currently 1 for mono) - stereo would be 2

## Important Constraints

### Browser Compatibility
- Requires Web Audio API support (all modern browsers)
- AudioContext may be suspended until user interaction
- LocalStorage has ~5-10MB limit per domain

### Performance Notes
- Timer uses `setInterval` at 1-second intervals (line 2694)
- Drum pattern uses `setInterval` at beat intervals (line 2599)
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
