---
name: workflow-animator
description: Transforms any real-world problem, challenge, or process into a beautiful, animated HTML workflow visualization that non-technical users can immediately understand. Use this skill whenever a user describes a problem they want to automate, a suggestion for improving a process, a business challenge, or any workflow they want to visualize. Trigger on phrases like "automate this", "show how this works", "visualize my process", "make this into a workflow", "explain this automation", "how would this work step by step", or any time a user describes a multi-step problem and wants to see it mapped out visually. Also trigger when users share a challenge + solution and want it animated or made interactive for a presentation or stakeholder demo.
---

# Workflow Animator Skill

Turn any human problem + solution into a cinematic, animated workflow HTML page that anyone — including your grandma, your CEO, or a 10-year-old — can understand in under 30 seconds.

## What This Skill Does

Given:
- A **challenge** (the problem)
- **Suggestions / steps** (the proposed solution or automation)

It produces a single self-contained `workflow.html` file with:
- Smooth step-by-step animated flow
- Beautiful, minimal, opinionated visual design
- Icons, arrows, status indicators, and stage labels
- No frameworks, no dependencies — pure HTML/CSS/JS

---

## Step 1 — Parse the Input

Extract from the user's message:

| Field | Description |
|---|---|
| `problem_title` | Short name for the challenge (≤6 words) |
| `problem_description` | 1–2 sentence explanation |
| `steps[]` | Array of workflow steps, each with: `label`, `description`, `icon` (emoji), `actor` (who/what does it: human / ai / system / hybrid) |
| `outcome` | The happy-path end state |
| `domain` | (optional) e.g. HR, Finance, Healthcare, Logistics, Customer Support |

If the user gave vague input, **infer intelligently**. Don't ask — generate something smart and say "here's what I interpreted, let me know if you'd like adjustments."

Typical step count: **4–8 steps**. If user gives more, group related ones.

---

## Step 2 — Choose a Visual Theme

Pick a theme based on the domain. Be bold and committed — no generic purple-gradient slop.

| Domain | Recommended Aesthetic |
|---|---|
| Healthcare | Clean white + teal, soft rounded nodes, gentle pulse animations |
| Finance / Legal | Deep navy + gold, sharp edges, authoritative typography |
| Logistics / Supply Chain | Industrial dark + amber, bold blocky layout, fast transitions |
| HR / People Ops | Warm coral + cream, friendly bubbles, bouncy timing |
| Tech / Engineering | Dark terminal + neon green/cyan, monospace font, matrix feel |
| Retail / E-commerce | Vibrant + playful, card-based layout, pop animations |
| Education | Pastel soft tones, progress-bar style flow, encouraging copy |
| Generic / Unknown | Sophisticated dark slate + electric blue — always works |

**Font rules**: Never use Inter, Arial, Roboto. Use Google Fonts — Syne, Outfit, Space Mono, Fraunces, DM Sans, Plus Jakarta Sans, Cabinet Grotesk, etc.

---

## Step 3 — Build the HTML

Generate a single `workflow.html` file. Structure:

```
1. Hero section — Problem title + description, with subtle entrance animation
2. Workflow stage — Animated step-by-step flow (vertical on mobile, horizontal/diagonal on desktop)
3. Outcome section — The result, shown with a celebratory reveal
4. Optional footer — "Powered by automation" or domain-specific tagline
```

### Animation Choreography (CRITICAL)

Steps must animate **sequentially**, not all at once:
- Each step card slides/fades in with a delay: `animation-delay: calc(N * 0.3s)`
- Connector lines between steps **draw themselves** (stroke-dashoffset trick or height-grow)
- Active step pulses or glows while "processing"
- Outcome fires a celebration (scale + color burst, or confetti)
- Use `IntersectionObserver` to trigger animations only when visible

### Step Card Anatomy

Each step card must show:
```
[ ICON ]  Step N
[ LABEL — bold, large ]
[ Description — short, plain language ]
[ ACTOR BADGE — 🤖 AI  /  👤 Human  /  ⚙️ System  /  🤝 Hybrid ]
```

### Connector Visuals

Between steps, show an animated connector:
- Vertical layout: growing line + animated chevron arrow
- Horizontal layout: SVG path that draws from left to right
- Color: matches theme accent color
- Speed: 0.6s ease, triggered after previous card finishes appearing

### Interactivity

Add these simple interactions:
1. **Click any step** → expands with more detail (tooltip or inline expand)
2. **"Replay" button** → resets all animations and plays from start
3. **Progress indicator** → top bar or step counter showing position in flow
4. Hover states on every card

---

## Step 4 — Code Quality Rules

- **Single file** — all CSS + JS inline, no external dependencies except Google Fonts CDN
- **Responsive** — works on mobile (vertical stack) and desktop (horizontal or diagonal)
- **No scrolljacking** — smooth natural scroll
- **Accessible** — ARIA labels on animated elements, `prefers-reduced-motion` respected
- **Performance** — CSS animations only (no JS animation loops), GPU-friendly transforms

### Reduced Motion Support
```css
@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
}
```

---

## Step 5 — Save and Present

Save to `/mnt/user-data/outputs/workflow.html`

Then use `present_files` to share it with the user.

Tell the user:
> "Here's your animated workflow! Open it in any browser — no setup needed. Click any step to expand it, and use the Replay button to restart the animation."

---

## Example Input → Output Mapping

### Input
**Challenge**: Our customer support team gets 500 emails/day and manually categorizes them.  
**Suggestion**: Use AI to read each email, classify it (billing / tech / complaint), route to the right team, and auto-draft a reply.

### Extracted Steps
1. 📧 Email Arrives — `System` — New customer email hits the shared inbox
2. 🤖 AI Reads Email — `AI` — Language model analyzes tone, topic, urgency
3. 🏷️ Auto-Classify — `AI` — Tagged: Billing / Technical / Complaint / General
4. 🔀 Smart Route — `System` — Sent to the right team queue automatically
5. ✍️ Draft Reply — `AI` — AI writes a first-draft response in brand voice
6. 👤 Human Reviews — `Human` — Agent approves, tweaks, or overrides
7. 📤 Reply Sent — `System` — Customer receives response, ticket closes

**Outcome**: Response time drops from 4 hours → 12 minutes. Zero mis-routed emails.

---

## Design Don'ts

- ❌ No purple gradients on white — overdone, generic
- ❌ No flat gray boxes with thin borders — boring
- ❌ No lorem ipsum placeholder text — fill with real context
- ❌ No "Step 1, Step 2, Step 3" as the only label — give each step a real name
- ❌ No animations that all fire simultaneously on page load
- ❌ No font sizes smaller than 14px for body, 11px for labels

## Design Do's

- ✅ Commit to ONE strong color palette and own it
- ✅ Use emoji icons — they're universally understood and charming
- ✅ Make the outcome section feel like a WIN — big, bright, celebratory
- ✅ Show the "actor" (AI vs Human vs System) clearly — this is what excites non-tech people
- ✅ Use generous whitespace — less cluttered = more professional
- ✅ Add a subtle animated background (gradient mesh, floating particles, or noise texture)