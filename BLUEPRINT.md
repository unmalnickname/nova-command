# Nova Command - Project Blueprint

> **Version:** 1.0.0
> **Date:** February 7, 2026
> **Status:** UI Design Complete, Backend Ready, Integration Pending

---

## Executive Summary

Nova Command is a native desktop application for managing AI agent teams. It combines the best of Discord (channels, voice), OpenAI Codex (real-time monitoring), and Trello (kanban) into one unified command center.

**Core Value Proposition:** Multi-agent voice calls with real-time oversight - something Discord cannot do.

---

## Project Origin

### Problem Statement
- Discord limits voice calls to 1 bot per channel
- No visibility into what agents are doing in real-time
- Task management scattered across tools
- Agent memory/context not centralized

### Solution
A custom platform that:
1. Enables multi-agent voice rooms
2. Shows Codex-style streaming output
3. Integrates kanban task management
4. Centralizes agent memory (WORKING.md, daily notes)
5. Provides Discord-like chat organization

### Inspiration Sources
| Source | What We Took |
|--------|--------------|
| Discord | Channels, voice rooms, chat UI |
| OpenAI Codex | Real-time agent streaming |
| Trello/Linear | Kanban task boards |
| SiteGPT Article | Heartbeat system, memory files, standups |
| Mission Control (manish-raana) | Convex backend, basic kanban |

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                         NOVA COMMAND                                │
│                    (Tauri Desktop / Web App)                        │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
          ┌─────────────────────┼─────────────────────┐
          │                     │                     │
          ▼                     ▼                     ▼
   ┌─────────────┐      ┌─────────────┐      ┌─────────────┐
   │   CONVEX    │      │   LIVEKIT   │      │  OPENCLAW   │
   │  Database   │      │   Voice     │      │   Agents    │
   │  Real-time  │      │   WebRTC    │      │   Gateway   │
   └─────────────┘      └─────────────┘      └─────────────┘
          │                     │                     │
          └─────────────────────┼─────────────────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │      HOSTINGER VPS    │
                    │  • OpenClaw Gateway   │
                    │  • Agent Sessions     │
                    │  • Heartbeat Crons    │
                    │  • Memory Files       │
                    └───────────────────────┘
```

---

## Tech Stack

| Layer | Technology | Status |
|-------|------------|--------|
| **Desktop App** | Tauri (Rust + Web) | Planned |
| **Web App** | React + TypeScript | In Progress |
| **Styling** | Tailwind CSS | Ready |
| **Animations** | Framer Motion | Planned |
| **Icons** | Lucide React / Tabler | Ready |
| **Drag & Drop** | dnd-kit | Ready (from MC) |
| **Real-time DB** | Convex | ✅ Connected |
| **Voice** | LiveKit | Planned |
| **Agents** | OpenClaw | ✅ Running on VPS |
| **Secrets** | Doppler | ✅ Configured |

---

## Feature Matrix

### Core Features (MVP)

| Feature | Description | Status |
|---------|-------------|--------|
| **Channel Chat** | Discord-style text channels | UI: ⚠️ Partial |
| **Voice Rooms** | Multi-agent voice calls | UI: ✅ Done |
| **Agent Monitor** | Codex-style streaming | UI: ✅ Done |
| **Kanban Board** | Task management | UI: ✅ Done |
| **Daily Standup** | Automated summaries | UI: ✅ Done |
| **Memory System** | WORKING.md, daily notes | UI: ✅ Done |
| **Agent Factory** | Spawn new agents | UI: ✅ Done |
| **Settings** | Doppler integration | UI: ✅ Done |

### Missing Features (Phase 2)

| Feature | Description | Priority |
|---------|-------------|----------|
| **Text Channels** | Discord-style #channels | High |
| **Thread Replies** | Reply in thread | Medium |
| **@Mentions** | Autocomplete + notifications | High |
| **Reactions** | Emoji reactions | Low |
| **Activity Feed** | Timeline view | Medium |
| **Task Comments** | Comment on tasks | High |
| **Right Panel** | Context sidebar | Low |

---

## UI Screens (Stitch Generated)

### 1. Mission Control Dashboard
**File:** `nova_command_mission_control_dashboard/`

Primary hub showing:
- Operations channels sidebar
- Voice uplink with active room
- Live telemetry (agents, cost)
- Swarm roster with status
- Task queue summary
- Real-time message feed
- Chat input with voice indicator

### 2. Agent Codex Monitoring
**File:** `nova_command_agent_codex_monitoring/`

Real-time agent view:
- Live Feed / Errors / History tabs
- Streaming log (SYSTEM, THOUGHT, ACTION, SUCCESS, MEMORY)
- Timestamps for each event
- Live metrics (tokens, time, model, cost)
- Generated artifacts list
- Pause Stream / Auto-scroll controls
- Delegate New Task / Force Stop buttons

### 3. Voice Operations Room
**File:** `nova_command_voice_operations_room/`

Multi-agent voice:
- Voice channels sidebar
- Participant grid (avatars + status)
- Speaking indicator (animated)
- Processing state for AI agents
- Chat transcript at bottom
- Summon Agent button
- Share Screen button
- Volume controls

### 4. Task Kanban Board
**File:** `nova_command_task_kanban_board/`

Task management:
- Backlog / In Progress / AI Processing columns
- Task cards with ID, title, tags
- Agent assignment with avatar
- Progress bars on AI tasks
- Priority badges (Low/Med/High)
- Filter and search
- Add Task button

### 5. Automated Daily Standup
**File:** `nova_command_automated_daily_standup/`

Daily summary:
- Standup pulse timeline (history)
- Yesterday's Wins section
- Today's Objectives section
- Critical Blockers with actions
- Team Velocity chart
- AI Insight panel
- Token burn metrics
- Listen to Narration (audio!)
- Active agents list

### 6. Memory & Knowledge Base
**File:** `nova_command_memory_&_knowledge_base/`

Agent memory:
- File tree (CORE_DIRECTIVES, PROJECT_OS, WORKING.md)
- Daily notes folder
- Markdown editor with syntax highlighting
- Revisions history
- Auto-save indicator
- Recent Ingestions timeline
- Sync Memory button
- Storage usage meter

### 7. Agent Factory Builder
**File:** `nova_command_agent_factory_builder/`

Agent creation:
- Core Identity (name, role)
- Intelligence Model dropdown
- Creativity/Temperature slider
- Prime Directive (system prompt)
- Neural Extensions toggles (Web Search, Code Interpreter, etc.)
- Network status
- Architecture visualization
- System log
- Test Config / Initialize Agent buttons

### 8. Secrets & Platform Settings
**File:** `nova_command_secrets_&_platform_settings/`

Configuration:
- Doppler Secrets Management
- API Keys table (masked)
- Sync with Doppler button
- Connected Platforms (LiveKit, Convex, OpenClaw)
- Platform status badges
- Security Score
- Add Integration button

---

## Data Models

### Convex Schema

```typescript
// Core tables
agents          // AI team members
channels        // Text and voice channels
messages        // Chat messages
tasks           // Kanban tasks
taskComments    // Comments on tasks
activities      // Activity feed events
documents       // Files and deliverables
voiceRooms      // Voice room state
notifications   // @mentions and alerts
agentMemory     // WORKING.md, daily notes
agentStream     // Real-time agent output
threadSubscriptions // Thread notification subscriptions
standups        // Daily standup summaries
```

See `BACKEND_SPEC.md` for complete schema.

---

## Integration Points

### OpenClaw → Nova Command

```
POST /openclaw/event
{
  "runId": "abc-123",
  "action": "start" | "progress" | "end" | "error",
  "sessionKey": "agent:developer:main",
  "chunk": { "type": "thinking", "content": "..." },
  "progress": 48
}
```

### Heartbeat System

```bash
# Cron schedule (staggered every 15 min)
0,15,30,45 * * * *   agent:main:main        # Nova
2,17,32,47 * * * *   agent:developer:main   # Friday
4,19,34,49 * * * *   agent:product:main     # Shuri
# ... etc
```

### Memory Files

```
/workspace/
├── WORKING.md       # Current task state
├── MEMORY.md        # Long-term memory
├── SOUL.md          # Agent personality
├── HEARTBEAT.md     # Wake-up checklist
└── memory/
    └── 2026-02-07.md  # Daily notes
```

---

## Doppler Secrets

**Project:** `novaops-vps` | **Config:** `prd`

| Key | Purpose |
|-----|---------|
| `CONVEX_DEPLOYMENT` | Convex project ID |
| `VITE_CONVEX_URL` | Convex cloud URL |
| `VITE_CONVEX_SITE_URL` | Convex HTTP endpoint |
| `MISSION_CONTROL_WEBHOOK_URL` | OpenClaw → Convex webhook |
| `LIVEKIT_API_KEY` | Voice API key |
| `LIVEKIT_API_SECRET` | Voice API secret |
| `ANTHROPIC_API_KEY` | Claude API key |

---

## Development Phases

### Phase 1: UI Foundation (Current)
- [x] Write platform spec
- [x] Generate UI with Stitch AI
- [x] Set up Convex backend
- [ ] Convert Stitch HTML to React
- [ ] Add missing 7 features to Stitch
- [ ] Integrate with Convex

### Phase 2: Agent Integration
- [ ] Install OpenClaw hook on VPS
- [ ] Configure webhook URL
- [ ] Set up heartbeat cron jobs
- [ ] Implement memory system
- [ ] Test agent streaming

### Phase 3: Voice Integration
- [ ] Set up LiveKit server
- [ ] Implement voice room join/leave
- [ ] Add agent voice (TTS)
- [ ] Add user voice (STT)
- [ ] Test multi-agent calls

### Phase 4: Polish & Native
- [ ] Tauri desktop wrapper
- [ ] Native Mac features
- [ ] PWA for mobile
- [ ] Push notifications
- [ ] Performance optimization

---

## Cost Analysis

### Development Savings (Stitch AI)

| Metric | Without Stitch | With Stitch | Savings |
|--------|----------------|-------------|---------|
| Design time | 2-3 weeks | 2 hours | ~95% |
| UI coding | 4-6 weeks | 2 weeks | ~70% |
| Tokens used | ~350K | ~65K | ~80% |
| Total time | 7-10 weeks | ~3 weeks | ~70% |

### Ongoing Costs (Estimated)

| Service | Monthly Cost |
|---------|--------------|
| Convex | Free tier / $25 |
| LiveKit | ~$20-50 |
| Anthropic API | ~$50-100 |
| Hostinger VPS | $12 |
| Doppler | Free tier |
| **Total** | ~$100-200/mo |

---

## Repository Structure

```
nova-command/
├── README.md
├── BLUEPRINT.md          # This file
├── docs/
│   ├── PLATFORM_SPEC.md  # Complete UI spec
│   ├── BACKEND_SPEC.md   # Convex + integrations
│   └── STITCH_BRIEF.md   # Quick reference for Stitch
├── stitch-output/        # Generated HTML/PNG from Stitch
│   ├── mission_control_dashboard/
│   ├── agent_codex_monitoring/
│   ├── voice_operations_room/
│   ├── task_kanban_board/
│   ├── automated_daily_standup/
│   ├── memory_knowledge_base/
│   ├── agent_factory_builder/
│   └── secrets_platform_settings/
├── src/                  # React source (to be created)
├── convex/               # Convex functions
└── public/
```

---

## References

- **Convex Dashboard:** https://dashboard.convex.dev/d/rare-bloodhound-226
- **OpenClaw Docs:** https://github.com/anthropics/openclaw
- **LiveKit Docs:** https://docs.livekit.io
- **SiteGPT Article:** Mission Control architecture reference
- **Mission Control (original):** https://github.com/manish-raana/openclaw-mission-control

---

## Team

- **Project Lead:** Luis (nova_corp)
- **UI Generation:** Stitch AI
- **Backend:** Convex
- **Agents:** OpenClaw on Hostinger VPS
- **AI Assistant:** Claude (Opus 4.5)

---

*Nova Command // AI Fleet Management // 2026*
