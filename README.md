# 🚀 Hermes Chat

> A Telegram-style chat platform for Hermes AI agents — with HTML5 game rendering, task progress notifications, cron scheduling, dynamic UI modification, and SSH secure transport.

[![TypeScript](https://img.shields.io/badge/TypeScript-5.5-blue?logo=typescript)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react)](https://react.dev/)
[![Node.js](https://img.shields.io/badge/Node.js-20+-339933?logo=node.js)](https://nodejs.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 💬 **Telegram-style Chat** | Dual-pane desktop layout + responsive mobile UI |
| 🤖 **Bot Conversations** | Real-time text chat with Hermes AI agents, Markdown rendering |
| 🎮 **HTML5 Game Rendering** | Sandboxed iframe game player right in chat bubbles |
| 📊 **Task Progress** | Real-time progress bar notifications from bots |
| ⏰ **Cron Scheduling** | Create, manage, and monitor scheduled tasks |
| 🎨 **Dynamic UI Modification** | Bots can change themes, colors, and backgrounds |
| 🔒 **SSH Secure Transport** | Dual-channel: WebSocket (realtime) + SSH tunnel (secure) |
| 📱 **Multi-device** | Desktop (≥1024px) / Tablet / Mobile (<768px) |

## 🏗️ Architecture

```
┌──────────────┐     Socket.IO      ┌──────────────┐     ws / SSH2      ┌──────────────┐
│   Frontend   │ ◄────────────────► │   Backend    │ ◄────────────────► │   Hermes     │
│  Vite+React  │    (realtime)       │  Node/Express│   (ws + SSH)      │  AI Agent    │
└──────────────┘                    └──────────────┘                    └──────────────┘
                                           │
                                           ▼
                                    ┌──────────────┐
                                    │   SQLite DB  │
                                    └──────────────┘
```

## 🛠️ Tech Stack

### Frontend
- **Vite 5** + **React 18** + **TypeScript 5.5**
- **MUI v5** (Material UI) + **Tailwind CSS 3.4**
- **Zustand** (state management) + **Socket.IO Client**
- **react-markdown** + **rehype-highlight** + **DOMPurify**

### Backend
- **Node.js 20+** + **Express 4** + **TypeScript**
- **Socket.IO** (frontend-facing) + **ws** (Hermes-facing)
- **ssh2** (SSH tunnel) + **node-cron** (scheduling)
- **better-sqlite3** (persistence) + **Zod** (validation)

## 🚀 Quick Start

```bash
# Clone the repository
git clone https://github.com/do-do026/hermes-chat.git
cd hermes-chat

# Install all dependencies (frontend + backend)
npm run install:all

# Start development servers (frontend :5173 + backend :3001)
npm run dev
```

The app will be available at `http://localhost:5173`.

## 📁 Project Structure

```
hermes-chat/
├── shared/                  # Shared types & constants (frontend ↔ backend)
│   ├── types.ts             #   Message, Chat, Bot, Task, User, enums...
│   └── constants.ts         #   Socket events, UI mod whitelist, defaults
│
├── src/                     # Frontend (Vite + React)
│   ├── store/               #   Zustand stores (chat, bot, UI)
│   ├── services/            #   Socket.IO client, REST API, GameBridge
│   ├── hooks/               #   useChat, useSocket, useTheme, useResponsive
│   ├── components/
│   │   ├── layout/          #   AppLayout, TopNavbar, Sidebar, BottomNav
│   │   ├── chat/            #   ChatList, ChatArea, MessageList/Input/Bubble
│   │   ├── messages/        #   Text, Markdown, Game, Progress, System
│   │   └── common/          #   Avatar, Badge, Spinner, GameSandbox
│   ├── pages/               #   Chat, Bots, Tasks, Settings
│   └── utils/               #   format, markdown, sanitize
│
├── server/src/              # Backend (Node.js + Express)
│   ├── services/            #   WebSocketManager, HermesConnector, MessageRouter
│   │                        #   SSHTunnel, CronScheduler
│   ├── socket/              #   Socket.IO server manager
│   ├── routes/              #   REST API: bots, chats, messages, tasks, config
│   ├── db/                  #   SQLite connection, schema, queries
│   ├── middleware/          #   Auth (API Key), Rate Limiter, Error Handler
│   └── index.ts             #   Server entry point
│
└── docs/                    # Architecture design docs & diagrams
```

## 🔧 Configuration

### Frontend (`.env`)
```env
VITE_API_URL=http://localhost:3001/api
VITE_WS_URL=http://localhost:3001
```

### Backend (`server/.env`)
```env
PORT=3001
API_KEY=your-secure-api-key-here
HERMES_DEFAULT_HOST=localhost
HERMES_DEFAULT_PORT=8080
DB_PATH=./data/hermes-chat.db
SSH_KEY_PATH=~/.ssh/id_rsa
```

## 🎮 HTML5 Game Integration

Bots can push HTML5 games that render directly in the chat:

1. Bot sends a `GAME` type message with a `gameUrl`
2. Frontend validates the URL against a whitelist
3. Game loads in a sandboxed iframe (`allow-scripts allow-same-origin`)
4. Game communicates via `postMessage` API
5. Results are sent back through the message channel

## 🎨 Bot UI Modification

Bots can dynamically modify the UI using whitelisted instructions:

| Instruction | Effect |
|-------------|--------|
| `SET_THEME` | Switch between light/dark mode |
| `SET_PRIMARY_COLOR` | Change the primary accent color |
| `SET_BACKGROUND` | Set a custom background color |

> All UI modifications go through a whitelist validator — arbitrary code injection is blocked.

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

Built with ❤️ by the Hermes Chat team
