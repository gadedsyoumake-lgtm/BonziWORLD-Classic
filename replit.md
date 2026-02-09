# BonziWORLD Revival

## Overview

BonziWORLD is a real-time chat application featuring animated BonziBUDDY characters (the famous purple gorilla). Users join chat rooms, pick nicknames, and communicate through animated characters with text-to-speech capabilities. The project is a revival/clone of the original BonziWORLD.com site.

The application consists of two main parts:
- A **Node.js/Express server** using Socket.IO for real-time WebSocket communication
- A **static frontend client** built with jQuery, EaselJS (canvas-based animations), and speak.js (client-side text-to-speech)

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Server Architecture

- **Runtime**: Node.js with Express for HTTP serving and Socket.IO v2 for real-time WebSocket communication
- **Entry point**: `index.js` at the project root — initializes Express, Socket.IO, and loads settings
- **Key server modules**:
  - `index.js` — Server bootstrap, Express static file serving, Socket.IO initialization. Exports `io` for use by other modules. Listens on `process.env.PORT || 5000`
  - `meat.js` — Core chat logic. Handles socket connections, user management, room management (public/private rooms), message sanitization, and chat commands
  - `ban.js` — Ban system. Stores bans in `bans.json` file with IP-based banning, ban duration, and reasons
  - `console.js` — Server-side admin console for ban/kick/unban commands via stdin
  - `log.js` — Winston-based logging system configured via `settings.json`
  - `utils.js` — Utility functions (GUID generation, random range, argument parsing)

- **Configuration**: `settings.json` (auto-created from `settings.example.json` if missing). Contains room preferences (character limits, name limits, room max, pitch/speed settings), runlevel permissions for commands, and logging configuration
- **Data persistence**: File-based only — `bans.json` for bans, `settings.json` for config. No database is used.

### Frontend Architecture

- **Location**: `build/www/` contains the pre-built static frontend
- **Source**: `src/` directory (referenced in README but not included in repo contents) — uses Grunt for building and Sass (Ruby) for CSS preprocessing
- **Key client libraries**:
  - jQuery 2.1.3 for DOM manipulation
  - EaselJS (CreateJS) for HTML5 Canvas-based character animations
  - Socket.IO client 1.4.5 for WebSocket communication
  - speak.js (Web Worker-based eSpeak TTS) for client-side text-to-speech
  - PreloadJS for asset loading
  - seedrandom for deterministic randomization
  - jQuery contextMenu for right-click menus
- **Main client scripts**: `script.min.js` and `crossplay.min.js` contain the minified application logic
- **Service Worker**: `service-worker.js` at root provides basic offline caching support

### Real-time Communication

- Socket.IO handles all real-time messaging between clients and server
- Room-based architecture: users join public rooms (auto-assigned) or private rooms (by ID)
- Messages are sanitized server-side using `sanitize-html` to prevent XSS
- Custom HTML sanitization functions also exist for additional filtering

### Security

- IP-based banning system with configurable ban duration
- HTML sanitization on both input and output (sanitize-html library + custom sanitizers)
- Runlevel system in settings controls which commands are available (0 = everyone, higher = more restricted)
- "Godword" password system for admin/god mode access

### Important Notes for Development

- There are **two `package.json` files**: one at root level and one in `server/`. The root-level one has more dependencies (including discord-webhook-node, ngrok, etc.) and appears to be the active one
- The server uses Socket.IO v2 (not v4), which is important for client compatibility
- The client Socket.IO library is v1.4.5 — there's a version mismatch that works due to backward compatibility
- Express serves static files from `./build/www` when `settings.express.serveStatic` is true
- The `src/` directory for client source code is not present in the repo — only the built output in `build/www/`

## External Dependencies

### npm Packages (Server)
- **express** — HTTP server and static file serving
- **socket.io v2** — WebSocket real-time communication
- **sanitize-html** — HTML sanitization for chat messages
- **winston** — Logging framework
- **fs-extra** — Enhanced filesystem operations
- **discord-webhook-node** — Discord webhook integration (for notifications/logging)
- **ngrok** — Tunnel for exposing local server (development)
- **dgram** — UDP datagram support
- **merge** — Object merging utility

### Client-Side Libraries
- **jQuery 2.1.3** — DOM manipulation
- **EaselJS/CreateJS** — Canvas animations for BonziBUDDY characters
- **PreloadJS** — Asset preloading
- **speak.js** — Client-side text-to-speech via Web Workers (eSpeak engine compiled to JS)
- **Socket.IO Client 1.4.5** — WebSocket client
- **Font Awesome** — Icons (loaded from CDN: kit.fontawesome.com)

### External Services
- **Discord Webhooks** — Optional integration for logging/notifications
- **Google Play Store** — Links to Android app version
- No database service is used — all persistence is file-based (JSON files)