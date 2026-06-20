# Orchestration: Story Crew Pipeline

## Overview
A sequential four-agent pipeline that transforms a short story prompt into a polished Chapter 1. Each agent produces a saved markdown artifact. The pipeline is sequential — no agent begins until the previous task's output file exists.

---

## Pipeline Diagram

```
User prompt (5–100 words)
        │
        ▼
┌─────────────────────┐
│   plot-architect    │  ── outputs/outline.md
│   (outline)         │
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  character-crafter  │  ── outputs/characters.md
│  (character pass)   │
└─────────────────────┘
        │ (receives outline.md + characters.md)
        ▼
┌─────────────────────┐
│    scene-weaver     │  ── outputs/draft.md
│  (draft chapter)    │
└─────────────────────┘
        │ (receives draft.md; references outline.md + characters.md)
        ▼
┌─────────────────────┐
│    chief-editor     │  ── outputs/chapter_1_final.md  ← terminal output
│  (edit chapter)     │
└─────────────────────┘
```

---

## Agents

Each agent is a Claude Code subagent under `.claude/agents/`. Invoke by `name`.

| `name` | Role | Spec |
|--------|------|------|
| `plot-architect` | Senior Plot Architect | [`../.claude/agents/plot-architect.md`](../.claude/agents/plot-architect.md) |
| `character-crafter` | Character & Dialogue Specialist | [`../.claude/agents/character-crafter.md`](../.claude/agents/character-crafter.md) |
| `scene-weaver` | Creative Storyteller & Prose Weaver | [`../.claude/agents/scene-weaver.md`](../.claude/agents/scene-weaver.md) |
| `chief-editor` | Line and Executive Editor | [`../.claude/agents/chief-editor.md`](../.claude/agents/chief-editor.md) |

---

## Tasks

Each task spec is embedded in its agent's file (see the `## Task:` section there).

| Order | Task | Agent | Input | Output file |
|-------|------|-------|-------|-------------|
| 1 | Outline Chapter 1 | `plot-architect` | `{book_prompt}` | `outputs/outline.md` |
| 2 | Character & Dialogue Pass | `character-crafter` | `outline.md` | `outputs/characters.md` |
| 3 | Draft Chapter 1 | `scene-weaver` | `outline.md` + `characters.md` | `outputs/draft.md` |
| 4 | Edit Chapter 1 | `chief-editor` | `draft.md` (+ reference files) | `outputs/chapter_1_final.md` |

---

## Question Protocol

### Pre-Work (before any task begins)
- Only `plot-architect` may ask the user a question at this stage.
- All other agents receive their questions via inter-agent routing.
- Once `plot-architect` has begun producing the outline, the pre-work window is **closed**.

### During Work (after pipeline starts)
- **No agent may ask the user directly.**
- Questions must be directed to a specific agent by role using the inter-agent question format defined in each agent's spec.
- The receiving agent answers in their own output file under `## Inter-Agent Questions Raised` (for questions they send) or within the relevant beat annotation for answers they provide.
- The questioning agent must make a documented assumption and continue; the pipeline does not block.

### Routing Guide

| If the question is about… | Route to |
|---------------------------|----------|
| Original prompt intent, genre, tone | `plot-architect` |
| Story structure, beat order, timeline | `plot-architect` |
| Character psychology, motivation | `character-crafter` |
| Character voice, dialogue direction | `character-crafter` |
| Prose intent, scene meaning | `scene-weaver` |
| Editorial changes, what was cut and why | `chief-editor` |

---

## Output Files

| File | Produced by | Purpose |
|------|-------------|---------|
| `outputs/outline.md` | `plot-architect` | Structured beat-by-beat chapter outline |
| `outputs/characters.md` | `character-crafter` | Psychological annotations per character per beat |
| `outputs/draft.md` | `scene-weaver` | Raw first draft, min. 1,500 words |
| `outputs/chapter_1_final.md` | `chief-editor` | Polished, publication-ready chapter ← **final deliverable** |

---

## Kickoff Instructions

1. Collect `{book_prompt}` from the user (5–100 words).
2. Invoke the `plot-architect` subagent with the prompt. Its system prompt and task spec live in [`../.claude/agents/plot-architect.md`](../.claude/agents/plot-architect.md).
3. If `plot-architect` raises a pre-work question, surface it to the user and wait for a response before continuing.
4. On each subsequent step, invoke the next subagent with all upstream output files as context.
5. After `chief-editor` completes, surface `outputs/chapter_1_final.md` to the user along with any `[EDITOR FLAG]` blocks that require human decision.

---

## Re-Run / Revision Routing

If the final output contains `[EDITOR FLAG]` blocks:
- Flags directed at `plot-architect` → re-run from Task 1 with flag context appended to the prompt
- Flags directed at `character-crafter` → re-run from Task 2 with flag context
- Flags directed at `scene-weaver` → re-run from Task 3 with flag context
- Flags directed at `chief-editor` (self-flags) → re-run Task 4 only
