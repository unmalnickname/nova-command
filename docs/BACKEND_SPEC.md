# Nova Command - Backend & Integration Spec

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                         NOVA COMMAND APP                            │
│                      (Tauri Desktop / Web)                          │
└─────────────────────────────┬───────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
        ▼                     ▼                     ▼
┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│    CONVEX     │    │   LIVEKIT     │    │   OPENCLAW    │
│  (Real-time   │    │   (Voice)     │    │   (Agents)    │
│   Database)   │    │               │    │               │
└───────────────┘    └───────────────┘    └───────────────┘
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                              │
                              ▼
                    ┌───────────────┐
                    │     VPS       │
                    │  (OpenClaw    │
                    │   Gateway)    │
                    └───────────────┘
```

---

## Convex Schema

### Tables

```typescript
// agents - AI team members
agents: defineTable({
  name: v.string(),                    // "Friday"
  role: v.string(),                    // "Developer Agent"
  avatar: v.string(),                  // "⚙️"
  status: v.union(
    v.literal("active"),
    v.literal("idle"),
    v.literal("blocked")
  ),
  level: v.union(
    v.literal("lead"),
    v.literal("specialist"),
    v.literal("intern")
  ),
  sessionKey: v.string(),              // "agent:developer:main"
  currentTaskId: v.optional(v.id("tasks")),
  personality: v.optional(v.string()), // SOUL content
  lastActiveAt: v.optional(v.number()),
  tenantId: v.string(),
})
.index("by_tenant", ["tenantId"])
.index("by_session", ["sessionKey"])

// channels - text and voice channels
channels: defineTable({
  name: v.string(),                    // "dev-tasks"
  type: v.union(
    v.literal("text"),
    v.literal("voice")
  ),
  description: v.optional(v.string()),
  tenantId: v.string(),
})
.index("by_tenant", ["tenantId"])

// messages - chat messages in channels
messages: defineTable({
  channelId: v.id("channels"),
  authorId: v.string(),                // agent or user ID
  authorType: v.union(
    v.literal("user"),
    v.literal("agent")
  ),
  content: v.string(),                 // markdown
  threadId: v.optional(v.id("messages")),
  attachments: v.optional(v.array(v.id("documents"))),
  tenantId: v.string(),
})
.index("by_channel", ["channelId", "_creationTime"])
.index("by_thread", ["threadId", "_creationTime"])
.index("by_tenant", ["tenantId"])

// tasks - kanban tasks
tasks: defineTable({
  title: v.string(),
  description: v.string(),
  status: v.union(
    v.literal("inbox"),
    v.literal("assigned"),
    v.literal("in_progress"),
    v.literal("review"),
    v.literal("done"),
    v.literal("archived")
  ),
  assigneeIds: v.array(v.id("agents")),
  tags: v.array(v.string()),
  priority: v.union(
    v.literal("low"),
    v.literal("medium"),
    v.literal("high"),
    v.literal("urgent")
  ),
  progress: v.number(),                // 0-100
  borderColor: v.optional(v.string()),
  openclawRunId: v.optional(v.string()),
  channelId: v.optional(v.id("channels")), // linked channel
  tenantId: v.string(),
})
.index("by_tenant", ["tenantId"])
.index("by_status", ["status", "tenantId"])
.index("by_assignee", ["assigneeIds", "tenantId"])
.index("by_openclaw", ["openclawRunId"])

// taskComments - comments on tasks
taskComments: defineTable({
  taskId: v.id("tasks"),
  authorId: v.string(),
  authorType: v.union(v.literal("user"), v.literal("agent")),
  content: v.string(),
  attachments: v.optional(v.array(v.id("documents"))),
  tenantId: v.string(),
})
.index("by_task", ["taskId", "_creationTime"])
.index("by_tenant", ["tenantId"])

// activities - activity feed events
activities: defineTable({
  type: v.union(
    v.literal("task_created"),
    v.literal("task_moved"),
    v.literal("task_assigned"),
    v.literal("task_completed"),
    v.literal("comment"),
    v.literal("document_created"),
    v.literal("agent_started"),
    v.literal("agent_completed"),
    v.literal("message")
  ),
  agentId: v.optional(v.id("agents")),
  userId: v.optional(v.string()),
  message: v.string(),
  targetId: v.optional(v.string()),    // task/channel/doc ID
  targetType: v.optional(v.string()),
  metadata: v.optional(v.any()),
  tenantId: v.string(),
})
.index("by_tenant", ["tenantId", "_creationTime"])
.index("by_type", ["type", "tenantId"])

// documents - files and deliverables
documents: defineTable({
  title: v.string(),
  content: v.string(),                 // markdown
  type: v.union(
    v.literal("deliverable"),
    v.literal("research"),
    v.literal("protocol"),
    v.literal("attachment")
  ),
  taskId: v.optional(v.id("tasks")),
  agentId: v.optional(v.id("agents")),
  path: v.optional(v.string()),
  tenantId: v.string(),
})
.index("by_tenant", ["tenantId"])
.index("by_task", ["taskId"])

// voiceRooms - voice room state
voiceRooms: defineTable({
  channelId: v.id("channels"),
  livekitRoom: v.string(),             // LiveKit room name
  participants: v.array(v.object({
    id: v.string(),
    type: v.union(v.literal("user"), v.literal("agent")),
    joinedAt: v.number(),
  })),
  tenantId: v.string(),
})
.index("by_channel", ["channelId"])
.index("by_tenant", ["tenantId"])

// notifications - @mentions and alerts
notifications: defineTable({
  recipientId: v.string(),             // agent or user ID
  recipientType: v.union(v.literal("user"), v.literal("agent")),
  content: v.string(),
  sourceType: v.union(
    v.literal("mention"),
    v.literal("assignment"),
    v.literal("thread"),
    v.literal("standup")
  ),
  sourceId: v.optional(v.string()),
  delivered: v.boolean(),
  readAt: v.optional(v.number()),
  tenantId: v.string(),
})
.index("by_recipient", ["recipientId", "delivered"])
.index("by_tenant", ["tenantId"])

// agentMemory - working memory and notes
agentMemory: defineTable({
  agentId: v.id("agents"),
  type: v.union(
    v.literal("working"),              // WORKING.md
    v.literal("daily"),                // daily notes
    v.literal("longterm")              // MEMORY.md
  ),
  date: v.optional(v.string()),        // for daily: "2026-01-31"
  content: v.string(),                 // markdown
  tenantId: v.string(),
})
.index("by_agent", ["agentId", "type"])
.index("by_agent_date", ["agentId", "type", "date"])
.index("by_tenant", ["tenantId"])

// agentStream - real-time agent output (Codex-style)
agentStream: defineTable({
  agentId: v.id("agents"),
  runId: v.string(),                   // OpenClaw run ID
  chunks: v.array(v.object({
    type: v.union(
      v.literal("thinking"),
      v.literal("tool_call"),
      v.literal("tool_result"),
      v.literal("output"),
      v.literal("error")
    ),
    content: v.string(),
    timestamp: v.number(),
  })),
  status: v.union(
    v.literal("running"),
    v.literal("completed"),
    v.literal("error"),
    v.literal("paused")
  ),
  progress: v.number(),
  startedAt: v.number(),
  completedAt: v.optional(v.number()),
  tenantId: v.string(),
})
.index("by_agent", ["agentId", "_creationTime"])
.index("by_run", ["runId"])
.index("by_tenant", ["tenantId"])

// threadSubscriptions - who's subscribed to task threads
threadSubscriptions: defineTable({
  taskId: v.id("tasks"),
  subscriberId: v.string(),
  subscriberType: v.union(v.literal("user"), v.literal("agent")),
  tenantId: v.string(),
})
.index("by_task", ["taskId"])
.index("by_subscriber", ["subscriberId"])

// standups - daily standup summaries
standups: defineTable({
  date: v.string(),                    // "2026-01-31"
  completed: v.array(v.object({
    agentId: v.id("agents"),
    taskTitle: v.string(),
  })),
  inProgress: v.array(v.object({
    agentId: v.id("agents"),
    taskTitle: v.string(),
    progress: v.number(),
  })),
  blocked: v.array(v.object({
    agentId: v.id("agents"),
    reason: v.string(),
  })),
  needsReview: v.array(v.object({
    taskId: v.id("tasks"),
    taskTitle: v.string(),
  })),
  decisions: v.array(v.string()),
  sentToTelegram: v.boolean(),
  tenantId: v.string(),
})
.index("by_date", ["date", "tenantId"])
.index("by_tenant", ["tenantId"])
```

---

## OpenClaw Integration

### Heartbeat System

Each agent wakes every 15 minutes via cron:

```bash
# Staggered schedule to avoid API bursts
0,15,30,45 * * * *   agent:main:main        # Nova (Lead)
2,17,32,47 * * * *   agent:developer:main   # Friday
4,19,34,49 * * * *   agent:product:main     # Shuri
6,21,36,51 * * * *   agent:content:main     # Loki
# ... etc
```

### Heartbeat Flow

```
1. Cron fires
2. OpenClaw creates isolated session
3. Agent reads WORKING.md from Convex
4. Agent checks notifications
5. Agent checks assigned tasks
6. If work to do:
   - Start task
   - Stream progress to agentStream table
   - Update task status
   - Post comments
7. If no work:
   - Report HEARTBEAT_OK
   - Update lastActiveAt
8. Session terminates
```

### Event Hook (Nova Command ← OpenClaw)

POST to `/openclaw/event`:

```json
{
  "runId": "abc-123",
  "action": "start" | "progress" | "end" | "error",
  "sessionKey": "agent:developer:main",
  "prompt": "User prompt text",
  "chunk": {
    "type": "thinking" | "tool_call" | "output",
    "content": "Current thinking..."
  },
  "progress": 48,
  "error": "Error message if action=error"
}
```

### Convex HTTP Endpoint

```typescript
// convex/http.ts
import { httpRouter } from "convex/server";
import { httpAction } from "./_generated/server";

const http = httpRouter();

http.route({
  path: "/openclaw/event",
  method: "POST",
  handler: httpAction(async (ctx, request) => {
    const body = await request.json();

    switch (body.action) {
      case "start":
        // Create or update task in "in_progress"
        // Initialize agentStream
        // Update agent status to "active"
        break;

      case "progress":
        // Append chunk to agentStream
        // Update progress percentage
        break;

      case "end":
        // Move task to "review" or "done"
        // Update agent status to "idle"
        // Calculate duration
        break;

      case "error":
        // Move task to "blocked"
        // Log error in activity feed
        break;
    }

    return new Response(JSON.stringify({ ok: true }), {
      status: 200,
      headers: { "Content-Type": "application/json" },
    });
  }),
});

export default http;
```

---

## LiveKit Voice Integration

### Setup

```typescript
// convex/voice.ts
import { v } from "convex/values";
import { mutation, query } from "./_generated/server";
import { AccessToken } from "livekit-server-sdk";

export const getVoiceToken = mutation({
  args: {
    channelId: v.id("channels"),
    participantId: v.string(),
    participantType: v.union(v.literal("user"), v.literal("agent")),
  },
  handler: async (ctx, args) => {
    const channel = await ctx.db.get(args.channelId);
    if (!channel || channel.type !== "voice") {
      throw new Error("Invalid voice channel");
    }

    // Create LiveKit access token
    const token = new AccessToken(
      process.env.LIVEKIT_API_KEY,
      process.env.LIVEKIT_API_SECRET,
      { identity: args.participantId }
    );

    token.addGrant({
      room: `channel-${args.channelId}`,
      roomJoin: true,
      canPublish: true,
      canSubscribe: true,
    });

    // Update voiceRooms table
    // ...

    return {
      token: token.toJwt(),
      serverUrl: process.env.LIVEKIT_URL,
    };
  },
});
```

### Agent Voice

Agents join voice via OpenClaw + LiveKit SDK:

```typescript
// On VPS, agent joins voice room
import { Room, RoomEvent } from "livekit-client";

const room = new Room();
await room.connect(serverUrl, token);

// Publish TTS audio as track
const audioTrack = await createTTSTrack(agentResponse);
await room.localParticipant.publishTrack(audioTrack);

// Listen for user audio
room.on(RoomEvent.TrackSubscribed, (track) => {
  if (track.kind === "audio") {
    // STT → process → respond
  }
});
```

---

## Memory System

### WORKING.md (Current Task State)

Stored in `agentMemory` with type="working":

```markdown
# Current Task
Fix authentication token refresh bug

# Status
In progress - found the issue, writing fix

# Context
- Bug report: tokens expire but cached version returned
- File: src/api/auth.ts line 142
- Fix: add expiry check before return

# Next Steps
1. ✅ Locate the bug
2. 🔄 Write the fix
3. ⬜ Test locally
4. ⬜ Deploy to staging
5. ⬜ Notify Shuri for testing
```

### Daily Notes

Stored in `agentMemory` with type="daily", date="2026-01-31":

```markdown
# January 31, 2026

## 09:15 UTC
- Started auth token fix task
- Found issue in auth.ts

## 09:32 UTC
- Fix written and committed
- Deployed to staging
- Notified @Shuri for testing

## 10:45 UTC
- Shuri approved
- Deployed to production
- Closed task
```

### Heartbeat Check Procedure

```markdown
# HEARTBEAT.md (Agent Instructions)

## On Wake
1. Read WORKING.md for current task state
2. If task in progress, resume it
3. If stuck, check session memory for context

## Check For Work
1. Query notifications for @mentions
2. Query tasks assigned to me
3. Scan activity feed for relevant updates

## Priority Order
1. Direct @mentions (respond immediately)
2. Assigned tasks in "in_progress"
3. Assigned tasks in "assigned"
4. Unassigned tasks in "inbox" (only if lead)

## Reporting
- If work done: post to task thread
- If completed: move task to review
- If blocked: set status and explain
- If nothing to do: HEARTBEAT_OK
```

---

## Daily Standup Generation

Cron at 11:30 PM:

```typescript
// convex/standups.ts
export const generateDailyStandup = internalMutation({
  handler: async (ctx) => {
    const today = new Date().toISOString().split("T")[0];
    const tenantId = "default";

    // Get all tasks updated today
    const tasks = await ctx.db
      .query("tasks")
      .withIndex("by_tenant", (q) => q.eq("tenantId", tenantId))
      .collect();

    // Categorize
    const completed = tasks
      .filter(t => t.status === "done" && wasUpdatedToday(t))
      .map(t => ({ agentId: t.assigneeIds[0], taskTitle: t.title }));

    const inProgress = tasks
      .filter(t => t.status === "in_progress")
      .map(t => ({
        agentId: t.assigneeIds[0],
        taskTitle: t.title,
        progress: t.progress
      }));

    const blocked = tasks
      .filter(t => t.status === "blocked")
      .map(t => ({ agentId: t.assigneeIds[0], reason: t.title }));

    const needsReview = tasks
      .filter(t => t.status === "review")
      .map(t => ({ taskId: t._id, taskTitle: t.title }));

    // Get key decisions from activities
    const decisions = await getKeyDecisions(ctx, today);

    // Save standup
    await ctx.db.insert("standups", {
      date: today,
      completed,
      inProgress,
      blocked,
      needsReview,
      decisions,
      sentToTelegram: false,
      tenantId,
    });

    return { date: today };
  },
});
```

---

## Thread Subscriptions

### Auto-Subscribe Rules

1. **Comment on task** → subscribed to that task
2. **Get @mentioned** → subscribed to that task
3. **Get assigned** → subscribed to that task
4. **Create task** → subscribed to that task

### Notification Flow

```typescript
// When someone comments on a task
export const addComment = mutation({
  handler: async (ctx, args) => {
    // Add comment
    await ctx.db.insert("taskComments", { ... });

    // Auto-subscribe author
    await subscribeToTask(ctx, args.taskId, args.authorId);

    // Get all subscribers
    const subscribers = await ctx.db
      .query("threadSubscriptions")
      .withIndex("by_task", (q) => q.eq("taskId", args.taskId))
      .collect();

    // Create notifications for all except author
    for (const sub of subscribers) {
      if (sub.subscriberId !== args.authorId) {
        await ctx.db.insert("notifications", {
          recipientId: sub.subscriberId,
          recipientType: sub.subscriberType,
          content: `New comment on "${task.title}"`,
          sourceType: "thread",
          sourceId: args.taskId,
          delivered: false,
          tenantId: args.tenantId,
        });
      }
    }

    // Parse @mentions and notify those too
    const mentions = parseMentions(args.content);
    for (const mention of mentions) {
      await createMentionNotification(ctx, mention, args.taskId);
    }
  },
});
```

---

## Doppler Secrets

```bash
# Required secrets in Doppler (novaops-vps / prd)

# Convex
CONVEX_DEPLOYMENT=dev:rare-bloodhound-226
VITE_CONVEX_URL=https://rare-bloodhound-226.convex.cloud
CONVEX_DEPLOY_KEY=<production deploy key>

# LiveKit
LIVEKIT_API_KEY=<api key>
LIVEKIT_API_SECRET=<api secret>
LIVEKIT_URL=wss://your-livekit-server.com

# OpenClaw
OPENCLAW_GATEWAY_URL=http://localhost:3001
OPENCLAW_WEBHOOK_SECRET=<shared secret>

# Telegram (for standups)
TELEGRAM_BOT_TOKEN=<bot token>
TELEGRAM_CHAT_ID=<your chat id>

# Anthropic (for agents)
ANTHROPIC_API_KEY=<api key>
```

---

## API Endpoints Summary

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/openclaw/event` | POST | Receive agent lifecycle events |
| `/voice/token` | POST | Get LiveKit access token |
| `/standup/send` | POST | Send standup to Telegram |
| `/notifications/deliver` | POST | Webhook for notification delivery |

---

## Cron Jobs on VPS

```bash
# /etc/cron.d/nova-command

# Agent heartbeats (staggered)
0,15,30,45 * * * * nova /home/nova/.openclaw/bin/openclaw sessions send agent:main:main "HEARTBEAT"
2,17,32,47 * * * * nova /home/nova/.openclaw/bin/openclaw sessions send agent:developer:main "HEARTBEAT"
# ... etc

# Daily standup generation
30 23 * * * nova curl -X POST https://rare-bloodhound-226.convex.site/standup/generate

# Memory cleanup (weekly)
0 3 * * 0 nova curl -X POST https://rare-bloodhound-226.convex.site/maintenance/cleanup
```
