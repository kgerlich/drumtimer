# 🥁 Drum Timer - Custom Drum Studio

A browser-based timer application with an integrated drum machine, perfect for meditation, practice sessions, or any timed activities. Features customizable drum patterns, audio sample storage, and intelligent silent bar generation.

## Features

### Timer
- Countdown timer with customizable duration (minutes and seconds)
- Visual progress bar
- Start/pause/reset controls
- Keyboard shortcuts (Space to start/pause, Escape to reset)

### Drum Machine
- 16-step sequencer (16th notes in 4/4 time)
- Three drum tracks: Kick, Snare, Hi-Hat
- BPM control (60-200 BPM)
- Visual beat indicators showing current playback position
- Interactive pattern editor

### Audio Options
- **Synthesized Drums**: Built-in procedurally generated drum sounds
- **Custom Samples**: Upload your own drum samples (WAV, MP3, OGG)
- Automatic audio compression (16-bit PCM, 22kHz mono)
- Persistent storage in browser localStorage
- Volume control

### Drum Patterns
- Built-in presets: Basic Rock, Disco, Funk, Reggae
- Create and save custom patterns
- Pattern editor with visual feedback
- Delete custom patterns (right-click)

### Silent Bar System
- Randomly generates silent bars throughout the session
- Configurable probability (0-50%)
- First bar always plays for timing reference
- Last bar triggers a gong sound
- Visual timeline showing all bars (green = playing, red = silent)

### Settings Management
- Save/load all settings to localStorage
- Export complete configuration as JSON
- Reset to factory defaults
- Automatic settings persistence

## Usage

### Quick Start
1. Open `Drumtimer3.html` in any modern web browser
2. Set your desired timer duration
3. Adjust BPM to your preference
4. Optionally customize the drum pattern or load a preset
5. Press Start or hit Space to begin

### Upload Custom Samples
1. Click "Choose File" for each drum type
2. Select an audio file (WAV, MP3, or OGG)
3. Sample is automatically compressed and stored
4. Stored samples persist across browser sessions

### Create Custom Patterns
1. Click beat toggles in the Pattern Editor to enable/disable hits
2. Enter a name in the text field
3. Click "Save Pattern" to store
4. Right-click custom pattern buttons to delete

### Silent Bar Configuration
1. Adjust the "Silent Bars" slider (0-50%)
2. Click "Regenerate Pattern" to create new random silence distribution
3. View the bar timeline to see which bars will be silent

## Technical Details

- **Zero Dependencies**: Pure vanilla JavaScript with Web Audio API
- **No Build Process**: Single HTML file with embedded CSS and JavaScript
- **Browser Storage**: Uses localStorage for sample and settings persistence
- **Audio Compression**: Reduces sample size by 50-80% for efficient storage
- **Responsive Design**: Works on desktop and mobile browsers

## Browser Requirements

- Modern browser with Web Audio API support (Chrome, Firefox, Edge, Safari)
- LocalStorage enabled (for sample and settings persistence)
- JavaScript enabled

## Storage Limits

- LocalStorage typically allows 5-10MB per domain
- Samples are automatically compressed to maximize available space
- View storage info via the "Storage Info" button

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| Space | Start/Pause timer |
| Escape | Reset timer |

## Project Structure

- `Drumtimer3.html` - Complete application (HTML, CSS, JavaScript)
- `CLAUDE.md` - Documentation for AI code assistants

## License

This project is open source. Feel free to use, modify, and distribute as needed.

## Contributing

This is a single-file application for simplicity. To contribute:
1. Fork the repository
2. Make your changes to `Drumtimer3.html`
3. Test in multiple browsers
4. Submit a pull request

## Tips

- For meditation: Set a 5-20 minute timer with lower BPM (60-80)
- For practice: Use higher BPM (120-180) and custom patterns
- Experiment with different silent bar probabilities to create variety
- Export your settings to share configurations with others
