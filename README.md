# Nova Command - Complete Specification Package

## For Stitch AI

Give these files to Stitch AI in this order:

### 1. STITCH_BRIEF.md (Start here)
Quick reference for UI generation. Contains:
- Visual style (colors, typography)
- Layout structure
- 6 priority screens with ASCII mockups
- Key components
- Sidebar structure

### 2. PLATFORM_SPEC.md (Detailed reference)
Complete specification including:
- All 10 screen specifications with detailed mockups
- Component library (buttons, inputs, cards, etc.)
- Design system (colors, spacing, shadows)
- Data models
- User flows
- Accessibility requirements
- Animations

### 3. BACKEND_SPEC.md (For understanding data)
Backend architecture including:
- Convex database schema
- OpenClaw integration
- LiveKit voice setup
- Memory system
- Heartbeat cron system
- API endpoints

---

## What Nova Command Is

**Discord + Codex + Trello** for AI agent teams.

| Feature | Inspiration |
|---------|-------------|
| Channels & Chat | Discord |
| Voice Rooms | Discord |
| Agent Monitor | OpenAI Codex |
| Kanban Board | Trello/Mission Control |
| Activity Feed | Slack |
| Memory System | SiteGPT article |
| Heartbeat System | SiteGPT article |

---

## Priority Screens for MVP

1. **Channel Chat** - Discord-like messaging
2. **Voice Room** - Multi-agent voice calls
3. **Agent Monitor** - Codex-style streaming output
4. **Kanban Board** - Task management
5. **Activity Feed** - Real-time events
6. **Daily Standup** - Summary view

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| App Shell | Tauri (native) or Web |
| Frontend | React + TypeScript |
| Styling | Tailwind CSS |
| Animations | Framer Motion |
| Icons | Lucide React |
| Drag & Drop | dnd-kit |
| Real-time | Convex |
| Voice | LiveKit |
| Agents | OpenClaw |
| Secrets | Doppler |

---

## Quick Start for Stitch AI

Prompt for Stitch:

```
Build a dark-themed desktop app called "Nova Command" for managing AI agent teams.

Core features:
1. Discord-like sidebar with channels (text/voice) and agent roster
2. Chat interface with markdown, @mentions, reactions
3. Voice rooms with participant grid and speaking indicators
4. Codex-style agent monitor showing real-time streaming output
5. Kanban board with drag-drop tasks
6. Activity feed with timeline of events

Design:
- Dark theme (#0f0f0f background)
- Discord-inspired accent colors (#5865F2 blue, #57F287 green)
- Inter font for UI, JetBrains Mono for code
- Subtle animations, no harsh transitions

Use the attached STITCH_BRIEF.md for detailed specs.
```

---

## Files in This Package

```
/tmp/nova-command-spec/
├── README.md           ← This file (start here)
├── STITCH_BRIEF.md     ← Quick reference for Stitch AI
├── PLATFORM_SPEC.md    ← Complete UI specification
└── BACKEND_SPEC.md     ← Backend & integration spec
```

---

## What We're Building vs Competition

| Feature | Discord | SiteGPT | Nova Command |
|---------|---------|---------|--------------|
| Channels | ✅ | ❌ | ✅ |
| Voice | ✅ (limited) | ❌ | ✅ (multi-agent) |
| Agent Monitor | ❌ | ❌ | ✅ |
| Kanban | ❌ | ✅ | ✅ |
| Memory System | ❌ | ✅ | ✅ |
| Heartbeats | ❌ | ✅ | ✅ |
| Model Delegation | ❌ | ❌ | ✅ |
| Agent Factory | ❌ | ❌ | ✅ |

---

## Next Steps After UI

1. Connect to Convex backend (already set up)
2. Integrate LiveKit for voice
3. Set up OpenClaw heartbeat system on VPS
4. Configure agent memory files
5. Test multi-agent voice calls
6. Build Tauri wrapper for native Mac app
