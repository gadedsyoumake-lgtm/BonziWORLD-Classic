# BonziWORLD Server

## Overview
BonziWORLD is a chat client featuring the classic Bonzi Buddy character. The server is built with Node.js using Express and Socket.io.

## Project Architecture
- **Runtime**: Node.js
- **Framework**: Express for HTTP, Socket.io for real-time chat
- **Entry Point**: `index.js`
- **Frontend**: Static HTML/CSS/JS served from the project root
- **Port**: 5000

## Key Dependencies
- express - HTTP server
- socket.io - Real-time communication
- fs-extra - File system utilities
- sanitize-html - Input sanitization
- winston - Logging

## Workflows
- **BonziWORLD Server**: `npm start` (runs `node index.js` on port 5000)

## Recent Changes
- 2026-02-09: Imported project, installed npm dependencies, verified server runs successfully on port 5000
