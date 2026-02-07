# Nova Command - Stitch AI Brief

## One-Liner
A dark-themed desktop app for managing AI agent teams - combining Discord (channels/voice), Codex (real-time monitoring), and Trello (kanban) into one command center.

---

## Visual Style

**Theme:** Dark mode only
**Vibe:** Discord meets Linear meets Terminal
**Colors:**
- Background: `#0f0f0f` (near black)
- Cards: `#1a1a1a` / `#252525`
- Borders: `#333333`
- Primary accent: `#5865F2` (Discord blue)
- Success/Online: `#57F287` (green)
- Warning: `#FEE75C` (yellow)
- Danger: `#ED4245` (red)
- Text: `#ffffff` / `#a0a0a0` / `#666666`

**Typography:**
- UI: Inter
- Code/Terminal: JetBrains Mono
- Base size: 14px

---

## Layout

```
┌─────────────────────────────────────────────────────────────────┐
│  Title Bar (native controls)                                    │
├─────────────┬───────────────────────────────────┬───────────────┤
│  SIDEBAR    │         MAIN CONTENT              │  RIGHT PANEL  │
│  (240px)    │         (flexible)                │  (320px)      │
│             │                                   │               │
│  Channels   │  Changes based on selection:      │  Context for  │
│  Voice      │  - Channel chat                   │  selection:   │
│  Agents     │  - Kanban board                   │  - Members    │
│  Views      │  - Agent monitor                  │  - Tasks      │
│             │  - Activity feed                  │  - Memory     │
│             │  - Daily standup                  │               │
├─────────────┴───────────────────────────────────┴───────────────┤
│  Status Bar (connection, user)                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 6 Priority Screens

### 1. Channel Chat (Discord-like)
- Header: channel name, search, settings
- Messages: avatar, name, time, content (markdown), reactions
- Input: text field, attachment, emoji, send
- Date separators between groups

### 2. Voice Room
- Grid of participant cards (3-4 per row)
- Each card: avatar, name, mute icon, speaking glow (green pulse)
- Bottom controls: mute, video, share, leave
- Speaking indicator: animated green border

### 3. Agent Monitor (Codex-style)
- Stacked agent cards
- Each card shows:
  - Avatar, name, status badge (Working/Idle)
  - Terminal-style streaming output (monospace)
  - Progress bar with percentage
  - Current task name
  - Pause/Stop buttons
- Idle agents show collapsed with "Last active: X ago"

### 4. Kanban Board
- 5 columns: Inbox, Assigned, In Progress, Review, Done
- Drag and drop task cards
- Task card: title, assignee avatars, progress bar, tags
- Column headers with count badge

### 5. Activity Feed
- Vertical timeline of events
- Each item: avatar, action text, timestamp
- Types: task moves, comments, completions, assignments
- Filter by type dropdown

### 6. Daily Standup
- Sections: Completed, In Progress, Blocked, Needs Review
- Each item shows agent + task + progress
- Key Decisions section at bottom
- "Send to Telegram" button

---

## Sidebar Structure

```
NOVA COMMAND (logo)

─── CHANNELS ───
# general
# dev-tasks        ●  (unread dot)
# research
+ Add Channel

─── VOICE ───
🔊 War Room
   👤 You
   🤖 Friday
🔇 Standup Room
+ Add Voice Room

─── AGENTS ───
● Nova (Lead)        (green = active)
● Friday
○ Shuri             (gray = idle)
○ Loki
+ Spawn Agent

─── VIEWS ───
📋 Kanban
📊 Agent Monitor
📜 Activity Feed
📁 Documents

──────────
⚙️ Settings
👤 Profile
```

---

## Key Components

**Message Bubble:**
```
┌──────────────────────────────────────────┐
│ 🤖 Nova · 2:34 PM                        │
│ I'll start working on the auth bug now.  │
│ @Friday can you investigate?             │
│                               💬 2  ↩️    │
└──────────────────────────────────────────┘
```

**Agent Monitor Card (Active):**
```
┌──────────────────────────────────────────┐
│ ⚙️ Friday                    ● Working   │
│ ──────────────────────────────────────── │
│ > Reading src/api/auth.ts...             │
│ > Found issue at line 142                │
│ > Writing fix...                         │
│                                          │
│ ████████████░░░░░░░░░░░  48%             │
│                                          │
│ Task: Fix auth token refresh             │
│ Duration: 3m 24s                         │
│                            [Pause] [Stop]│
└──────────────────────────────────────────┘
```

**Task Card:**
```
┌─────────────────┐
│ Fix auth bug    │
│                 │
│ ⚙️ 🔬           │  (assignee avatars)
│ ████░░░░  48%   │
│                 │
│ #dev #urgent    │
└─────────────────┘
```

**Voice Participant:**
```
┌───────────────┐
│               │
│      🤖       │
│   ● ● ●      │  (speaking animation)
│    Friday     │
│    ┌────┐     │
│    │ 🔊 │     │  (unmuted)
│    └────┘     │
└───────────────┘
```

---

## Animations

- Speaking: Green pulsing glow around avatar (2s loop)
- Progress: Smooth width transition
- Messages: Fade in + slide up
- Modals: Scale from 0.95 + fade
- Hover: 100ms color transitions
- Drag: Lift shadow + real-time position

---

## Icon Set

Use Lucide React icons:
- Hash (#) for text channels
- Volume2 for voice rooms
- Circle (filled/empty) for status
- MessageSquare for comments
- FileText for documents
- Settings for settings
- User for profile
- Plus for add buttons
- X for close
- Mic/MicOff for voice
- Play/Pause/Square for controls

---

## What to Generate

1. **Component library** - buttons, inputs, cards, modals, badges, avatars
2. **Sidebar component** - with all sections
3. **Channel view** - message list + input
4. **Voice room view** - participant grid + controls
5. **Agent monitor view** - stacked cards with streaming
6. **Kanban board** - columns + drag-drop cards
7. **Task detail modal** - full task with comments
8. **Activity feed** - timeline of events
9. **Settings panel** - all options
10. **Right panel** - context based on selection

---

## Tech Stack (for code generation)

- React + TypeScript
- Tailwind CSS
- Framer Motion (animations)
- Lucide React (icons)
- React DnD or dnd-kit (drag and drop)

Generate with dark theme, responsive, accessible (keyboard nav, focus rings).
