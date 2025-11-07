# BonziWORLD

## Overview

BonziWORLD is a real-time chat application featuring animated character agents (originally modeled after BonziBUDDY, the infamous purple gorilla). The application provides a browser-based chat room experience with text-to-speech capabilities, user commands, and room-based conversations. Users can join public or private rooms, customize their characters, and interact through various commands and animations.

The project consists of a Node.js/Socket.IO backend server and a client-side web application built with vanilla JavaScript, jQuery, and Canvas-based animations using EaselJS.

## Recent Changes

**November 7, 2025** - Added media command support:
- Added `/image [URL]` command to display images in Bonzi dialog bubbles
- Added `/video [URL]` command to play videos with controls in Bonzi dialog bubbles
- Added `/audio [URL]` command to play audio files with controls in Bonzi dialog bubbles
- Updated `meat.js` with server-side handlers for image, video, and audio commands
- Updated `script.min.js` with client-side media rendering methods
- Updated `settings.json` to include new commands in runlevel configuration for both public and private rooms
- Configured server to run on port 5000 for Replit deployment

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Backend Architecture

**Framework & Runtime**
- Node.js server using Express for HTTP serving and Socket.IO for real-time WebSocket communication
- The server handles static file serving (optional via settings) and WebSocket connections for real-time chat

**Core Modules**
- `index.js`: Main server entry point, initializes Express, Socket.IO, and loads configuration
- `meat.js`: Core chat logic and WebSocket event handling (appears to be main business logic)
- `ban.js`: Ban management system with IP-based banning, temporary bans with expiration times, and kick functionality
- `log.js`: Winston-based logging system with configurable transports (file and console)
- `console.js`: Command-line interface for server administration (ban, kick, unban commands)
- `utils.js`: Utility functions including GUID generation and random number helpers

**Configuration Management**
- Settings loaded from `settings.json` (auto-created from `settings.example.json` if missing)
- Supports both public and private room configurations with different permission levels
- Runlevel system for controlling feature access (godmode, youtube, image, video, audio, joke, etc.)
- Configurable character limits, room sizes, pitch/speed ranges
- Server configured to run on port 5000 for Replit webview compatibility

**Room System**
- Public and private room support
- Room-based user isolation
- Maximum user limits per room (configurable)

**User Management**
- Socket-based user tracking
- IP-based identification for moderation
- Ban system with reason and duration tracking (stored in `bans.json`)

### Frontend Architecture

**Core Technologies**
- Vanilla JavaScript with jQuery for DOM manipulation
- EaselJS for Canvas-based 2D graphics and animations
- Socket.IO client for real-time communication
- speak.js for client-side text-to-speech synthesis using Web Audio API

**Key Features**
- Character animation system using sprite sheets and Canvas
- Context menu system (right-click interactions)
- Client-side speech synthesis using Web Workers
- Multiple platform support (desktop web, mobile web, Cordova app capability)

**Client Files**
- `script.min.js`: Main application logic for standard web version
- `crossplay.min.js`: Cross-platform variant (appears to be Cordova-compatible)
- `platform.js`: Platform detection and compatibility checks
- HTML sanitization on client side for XSS protection

**Speech Synthesis**
- Web Worker-based speech generation (`speakWorker.js`)
- Fallback support for different browsers
- Configurable pitch and speed parameters

### Data Storage

**File-Based Storage**
- `bans.json`: Persistent storage for IP bans with expiration timestamps and reasons
- `settings.json`: Application configuration (auto-generated from example template)
- Winston file-based logging (configured per logger in settings)

**In-Memory State**
- User sessions maintained in Socket.IO connection objects
- Room membership tracked in memory
- Active connections stored in socket registry

**No Traditional Database**
- Application uses simple JSON file storage rather than a database system
- This approach is lightweight but not horizontally scalable

### Authentication & Authorization

**No Traditional Authentication**
- Users identified by IP address for moderation purposes
- No user account system or password authentication
- Nickname-based identification (user-chosen display names)

**Permission System**
- Runlevel-based feature access control
- "Godmode" system with configurable godword for elevated permissions
- IP-based ban enforcement at connection level

### External Dependencies

**Third-Party Services**
- Discord Webhook integration for notifications (webhook URL hardcoded in `meat.js`)
- ngrok support for tunneling (listed in dependencies, likely for development/demo purposes)

**Client Libraries**
- EaselJS: Canvas animation framework
- PreloadJS: Asset loading
- jQuery: DOM manipulation and utilities
- Socket.IO client: WebSocket communication
- speak.js: Text-to-speech synthesis
- jquery.contextMenu: Right-click menu system

**Server Libraries**
- Express: HTTP server framework
- Socket.IO: WebSocket server
- Winston: Logging framework
- sanitize-html: HTML sanitization for user input
- fs-extra: Enhanced file system operations

**Build Tools**
- Grunt: Build automation (referenced in README)
- Sass: CSS preprocessing (Ruby dependency)
- npm: Package management

**Optional Platform Extensions**
- Cordova: Mobile app packaging capability (iOS/Android)
- Service Worker: PWA/offline support implementation

**Note on Sanitization**
- Multiple HTML sanitization implementations exist in the codebase (custom `sanitizeHTML` functions and sanitize-html library)
- This suggests content security is a primary concern due to user-generated content