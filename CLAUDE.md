# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

QRQ is a high-speed Morse code (CW) trainer written in C, similar to the classic DOS "Rufz" program. It's designed to improve callsign copying abilities at high speeds for amateur radio operators, particularly for contesting. The program sends 50 random callsigns and adjusts speed based on user accuracy.

## Build System

This project uses a traditional C Makefile build system. The main commands are:

### Compilation
```bash
make                    # Build with default settings (PulseAudio on Linux)
make USE_PA=YES         # Build with PulseAudio support
make USE_CA=YES         # Build with Core Audio (macOS)
make USE_WIN32=YES      # Build for Windows with MinGW32
make OSX_PLATFORM=YES OSX_BUNDLE=YES USE_CA=YES  # Build macOS bundle
```

### Installation
```bash
make install            # Install to /usr by default
make install DESTDIR=/usr/local  # Install to custom prefix
```

### Cleanup
```bash
make clean              # Remove build artifacts
make uninstall          # Remove installed files
```

## Code Architecture

### Core Components

- **qrq.c** (main program): Contains the main application logic, UI handling with ncurses, scoring system, configuration management, and Morse code generation
- **Audio backends**: Modular audio system supporting multiple platforms:
  - **oss.c/oss.h**: Open Sound System (Linux/Unix)
  - **pulseaudio.c/pulseaudio.h**: PulseAudio support (modern Linux)
  - **coreaudio.c/coreaudio.h**: Core Audio support (macOS, MIT licensed)

### Key Features

- Multi-threaded audio output using pthreads (or Windows threads)
- Multiple waveform support (sine, sawtooth, square wave)
- Configurable CW parameters (speed, tone frequency, rise/fall times)
- Local and online high score tracking
- Multiple training modes (fixed speed, unlimited attempts, etc.)

### Configuration

- **qrqrc**: Main configuration file with CW parameters, audio settings, and training options
- **callbase.qcb**: Amateur radio callsign database
- **toplist**: Local high score file

## Platform Support

The codebase is designed for cross-platform compatibility:
- Linux/Unix (OSS or PulseAudio)
- macOS (Core Audio, with app bundle support)
- Windows (WinMM audio API)

Build flags control platform-specific features and audio backend selection.

## File Structure

Configuration and data files are searched in this order:
1. Current directory
2. ~/.qrq/ (user config directory)
3. /usr/share/qrq/ (system-wide installation)

The program creates ~/.qrq/ on first run and copies default configuration files there.