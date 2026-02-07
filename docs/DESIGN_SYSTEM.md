# Nova Command - Design System

> Complete specification for recreating the Nova Command "Cyber-Enterprise" aesthetic.

---

## 1. Core Tech Stack

| Technology | Purpose | Why |
|------------|---------|-----|
| **React 18+** | Framework | Modern hooks, concurrent features |
| **TypeScript** | Type safety | Better DX, fewer bugs |
| **Tailwind CSS** | Styling | Rapid utility-first development |
| **Lucide React** | Icons | Clean, consistent, technical look |
| **Framer Motion** | Animations | Gliding panels, glowing pulses |
| **Radix UI / shadcn/ui** | Components | Accessible, unstyled primitives |

---

## 2. Color Palette

The palette is built on **"True Dark"** principles with high-contrast neon accents.

### Backgrounds

| Name | Hex | Usage |
|------|-----|-------|
| Primary | `#0f0f0f` | Deep charcoal/black - main background |
| Secondary | `#18191c` | Sidebar, panels - creates depth |
| Card | `#1a1a1a` | Card backgrounds |
| Input | `#1e1f22` | Input fields, text areas |

### Accents

| Name | Hex | Usage |
|------|-----|-------|
| Nova Blue | `#5865F2` | Primary actions, focus states, links |
| Cyan | `#00e5ff` | Telemetry, data visualization, glow |
| Success | `#23a55a` | Online status, active, completed |
| Danger | `#f23f42` | Blockers, disconnect, errors |
| Warning | `#FEE75C` | Warnings, in-progress |

### Text

| Name | Hex | Usage |
|------|-----|-------|
| Primary | `#ffffff` | Pure white - main text |
| Muted | `#b5bac1` | Metadata, descriptions, secondary |
| Subtle | `#666666` | Very muted, tertiary info |

### Borders

| Name | Value | Usage |
|------|-------|-------|
| Subtle | `rgba(255, 255, 255, 0.08)` | Default borders, dividers |
| Hover | `rgba(255, 255, 255, 0.1)` | Hover state borders |
| Focus | `#5865F2` | Focus rings, active states |

---

## 3. Typography

### Font Stack

```css
/* Primary UI Font */
font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;

/* Monospace (Code, Logs, Terminal) */
font-family: 'JetBrains Mono', 'Fira Code', 'Consolas', monospace;
```

### Font Sizes

| Size | Pixels | Usage |
|------|--------|-------|
| xs | 10px | Labels, badges |
| sm | 12px | Metadata, timestamps |
| base | 14px | Standard UI text |
| lg | 16px | Subheadings |
| xl | 18px | Page headers |
| 2xl | 24px | Section titles |

### Font Weights

| Weight | Value | Usage |
|--------|-------|-------|
| Normal | 400 | Body text |
| Medium | 500 | Labels, buttons |
| Semibold | 600 | Subheadings |
| Bold | 700 | Headings, emphasis |

---

## 4. Layout Architecture

### Dimensions

```
┌─────────────────────────────────────────────────────────────────┐
│  Title Bar                                               _ □ X  │
├──────┬─────────┬────────────────────────────────┬───────────────┤
│ 72px │  240px  │           Flexible              │    320px      │
│      │         │                                 │               │
│ Icon │ Channel │        Main Content             │  Right Panel  │
│ Rail │  List   │                                 │ (collapsible) │
│      │         │                                 │               │
│      │         │                                 │               │
├──────┴─────────┴────────────────────────────────┴───────────────┤
│  Status Bar                                                     │
└─────────────────────────────────────────────────────────────────┘
```

### Spacing System (4px Grid)

| Token | Value | Tailwind |
|-------|-------|----------|
| 1 | 4px | `p-1`, `m-1` |
| 2 | 8px | `p-2`, `m-2` |
| 3 | 12px | `p-3`, `m-3` |
| 4 | 16px | `p-4`, `m-4` |
| 5 | 20px | `p-5`, `m-5` |
| 6 | 24px | `p-6`, `m-6` |
| 8 | 32px | `p-8`, `m-8` |

### Border Radius

| Token | Value | Usage |
|-------|-------|-------|
| sm | 4px | Small buttons, badges |
| md | 6px | Inputs, cards (default) |
| lg | 8px | Modals, large cards |
| xl | 12px | Feature cards |
| full | 9999px | Avatars, pills |

---

## 5. Component Styles

### Glassmorphism (Overlays, Floating Bars)

```css
.glass-panel {
  background: rgba(15, 15, 15, 0.8);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 8px;
}
```

### Shadow Glow (Instead of Standard Shadows)

```css
/* Primary glow */
.glow-primary {
  box-shadow: 0 0 15px rgba(88, 101, 242, 0.3);
}

/* Cyan data glow */
.glow-cyan {
  box-shadow: 0 0 15px rgba(0, 229, 255, 0.3);
}

/* Success glow (speaking indicator) */
.glow-success {
  box-shadow: 0 0 20px rgba(35, 165, 90, 0.4);
}

/* Danger glow */
.glow-danger {
  box-shadow: 0 0 15px rgba(242, 63, 66, 0.3);
}
```

### Input Fields

```css
.input-nova {
  background: #1e1f22;
  border: 1px solid transparent;
  border-radius: 6px;
  padding: 10px 12px;
  color: #ffffff;
  font-size: 14px;
  transition: border-color 0.15s ease;
}

.input-nova:focus {
  border-color: #5865F2;
  outline: none;
}

.input-nova::placeholder {
  color: #666666;
}
```

### Cards

```css
.card-nova {
  background: #1a1a1a;
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 8px;
  padding: 16px;
  transition: all 0.15s ease;
}

.card-nova:hover {
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(255, 255, 255, 0.1);
}
```

### Buttons

```css
/* Primary button */
.btn-primary {
  background: #5865F2;
  color: #ffffff;
  padding: 10px 16px;
  border-radius: 6px;
  font-weight: 500;
  transition: all 0.15s ease;
}

.btn-primary:hover {
  background: #4752c4;
  box-shadow: 0 0 15px rgba(88, 101, 242, 0.3);
}

/* Ghost button */
.btn-ghost {
  background: transparent;
  color: #b5bac1;
  padding: 10px 16px;
  border-radius: 6px;
  transition: all 0.15s ease;
}

.btn-ghost:hover {
  background: rgba(255, 255, 255, 0.05);
  color: #ffffff;
}
```

### Scrollbars

```css
/* Custom scrollbar - thin and dark */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}

::-webkit-scrollbar-track {
  background: transparent;
}

::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(255, 255, 255, 0.2);
}
```

---

## 6. Animation Rules

### Framer Motion Presets

```javascript
// Panel slide-in (spring animation)
export const panelTransition = {
  type: "spring",
  stiffness: 300,
  damping: 30
};

// Fade in
export const fadeIn = {
  initial: { opacity: 0 },
  animate: { opacity: 1 },
  exit: { opacity: 0 },
  transition: { duration: 0.15 }
};

// Slide up (for messages, cards)
export const slideUp = {
  initial: { opacity: 0, y: 10 },
  animate: { opacity: 1, y: 0 },
  exit: { opacity: 0, y: -10 },
  transition: { duration: 0.15 }
};

// Scale pop (for modals)
export const scalePop = {
  initial: { opacity: 0, scale: 0.95 },
  animate: { opacity: 1, scale: 1 },
  exit: { opacity: 0, scale: 0.95 },
  transition: { duration: 0.2 }
};
```

### Heartbeat Animation (Agent Activity)

```javascript
// Pulse every 2 seconds
export const heartbeat = {
  animate: {
    scale: [1, 1.05, 1],
  },
  transition: {
    duration: 2,
    repeat: Infinity,
    ease: "easeInOut"
  }
};
```

### Speaking Indicator Glow

```javascript
// Pulsing glow ring
export const speakingGlow = {
  animate: {
    boxShadow: [
      "0 0 0 0 rgba(35, 165, 90, 0.4)",
      "0 0 0 8px rgba(35, 165, 90, 0)",
    ],
  },
  transition: {
    duration: 1.5,
    repeat: Infinity,
  }
};
```

### Hover States

```javascript
// Standard hover
export const hoverBg = {
  whileHover: {
    backgroundColor: "rgba(255, 255, 255, 0.05)"
  },
  transition: { duration: 0.1 }
};

// Card lift
export const hoverLift = {
  whileHover: {
    y: -2,
    boxShadow: "0 4px 12px rgba(0, 0, 0, 0.3)"
  },
  transition: { duration: 0.15 }
};
```

---

## 7. Tailwind Configuration

```javascript
// tailwind.config.js
module.exports = {
  darkMode: 'class',
  theme: {
    extend: {
      colors: {
        nova: {
          bg: '#0f0f0f',
          secondary: '#18191c',
          card: '#1a1a1a',
          input: '#1e1f22',
          blue: '#5865F2',
          cyan: '#00e5ff',
          success: '#23a55a',
          danger: '#f23f42',
          warning: '#FEE75C',
        },
        text: {
          primary: '#ffffff',
          muted: '#b5bac1',
          subtle: '#666666',
        },
        border: {
          subtle: 'rgba(255, 255, 255, 0.08)',
          hover: 'rgba(255, 255, 255, 0.1)',
        },
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
        mono: ['JetBrains Mono', 'Fira Code', 'monospace'],
      },
      fontSize: {
        '2xs': '10px',
      },
      spacing: {
        '18': '72px',  // Icon rail width
        '60': '240px', // Channel list width
        '80': '320px', // Right panel width
      },
      borderRadius: {
        'nova': '6px',
      },
      boxShadow: {
        'glow-blue': '0 0 15px rgba(88, 101, 242, 0.3)',
        'glow-cyan': '0 0 15px rgba(0, 229, 255, 0.3)',
        'glow-success': '0 0 20px rgba(35, 165, 90, 0.4)',
        'glow-danger': '0 0 15px rgba(242, 63, 66, 0.3)',
      },
      animation: {
        'heartbeat': 'heartbeat 2s ease-in-out infinite',
        'glow-pulse': 'glow-pulse 1.5s ease-in-out infinite',
      },
      keyframes: {
        heartbeat: {
          '0%, 100%': { transform: 'scale(1)' },
          '50%': { transform: 'scale(1.05)' },
        },
        'glow-pulse': {
          '0%': { boxShadow: '0 0 0 0 rgba(35, 165, 90, 0.4)' },
          '100%': { boxShadow: '0 0 0 8px rgba(35, 165, 90, 0)' },
        },
      },
    },
  },
  plugins: [],
}
```

---

## 8. CSS Variables (Global)

```css
/* globals.css */
:root {
  /* Backgrounds */
  --nova-bg: #0f0f0f;
  --nova-bg-secondary: #18191c;
  --nova-bg-card: #1a1a1a;
  --nova-bg-input: #1e1f22;

  /* Accents */
  --nova-blue: #5865F2;
  --nova-cyan: #00e5ff;
  --nova-success: #23a55a;
  --nova-danger: #f23f42;
  --nova-warning: #FEE75C;

  /* Text */
  --nova-text: #ffffff;
  --nova-text-muted: #b5bac1;
  --nova-text-subtle: #666666;

  /* Borders */
  --nova-border: rgba(255, 255, 255, 0.08);
  --nova-border-hover: rgba(255, 255, 255, 0.1);

  /* Transitions */
  --nova-transition: 0.15s ease;

  /* Spacing */
  --sidebar-icon-width: 72px;
  --sidebar-channel-width: 240px;
  --panel-right-width: 320px;
}

body {
  background: var(--nova-bg);
  color: var(--nova-text);
  font-family: 'Inter', sans-serif;
}
```

---

## 9. Component Examples

### Agent Status Badge

```jsx
// Tailwind classes
<span className="inline-flex items-center gap-1.5 px-2 py-0.5 rounded-full text-xs font-medium bg-nova-success/20 text-nova-success">
  <span className="w-1.5 h-1.5 rounded-full bg-nova-success animate-heartbeat" />
  Online
</span>
```

### Glowing Card

```jsx
<div className="bg-nova-card border border-border-subtle rounded-nova p-4
                hover:bg-white/5 hover:border-border-hover hover:shadow-glow-blue
                transition-all duration-150">
  {/* Content */}
</div>
```

### Input Field

```jsx
<input
  className="w-full bg-nova-input border border-transparent rounded-nova
             px-3 py-2.5 text-nova-text placeholder:text-text-subtle
             focus:border-nova-blue focus:outline-none transition-colors"
  placeholder="Message #general"
/>
```

### Speaking Indicator

```jsx
<motion.div
  className="w-12 h-12 rounded-full border-2 border-nova-success"
  animate={{
    boxShadow: [
      "0 0 0 0 rgba(35, 165, 90, 0.4)",
      "0 0 0 8px rgba(35, 165, 90, 0)",
    ],
  }}
  transition={{ duration: 1.5, repeat: Infinity }}
>
  <img src={avatar} className="w-full h-full rounded-full" />
</motion.div>
```

---

## 10. Best Practices

### Do's ✅

- Use `backdrop-filter: blur()` for overlays
- Apply subtle glows instead of harsh shadows
- Keep spacing consistent (4px grid)
- Use spring animations for panels
- Make all interactive elements have hover states
- Use the cyan accent for data/telemetry elements

### Don'ts ❌

- Don't use pure black (`#000000`) - use `#0f0f0f`
- Don't use white borders - use `rgba(255,255,255,0.08)`
- Don't use standard box-shadows - use glow effects
- Don't make animations too fast (min 150ms)
- Don't forget focus states for accessibility

---

*Nova Command Design System v1.0 - February 2026*
