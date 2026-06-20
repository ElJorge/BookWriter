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
│   plot_architect    │  ── outputs/outline.md
│   task_outline      │
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  character_crafter  │  ── outputs/characters.md
│  task_character_pass│
└─────────────────────┘
        │ (receives outline.md + characters.md)
        ▼
┌─────────────────────┐
│    scene_weaver     │  ── outputs/draft.md
│  task_draft_chapter │
└─────────────────────┘
        │ (receives draft.md; references outline.md + characters.md)
        ▼
┌─────────────────────┐
│    chief_editor     │  ── outputs/chapter_1_final.md  ← terminal output
│  task_edit_chapter  │
└─────────────────────┘
```

---

## Agents

| ID | Role | Spec |
|----|------|------|
| `plot_architect` | Senior Plot Architect | [`agents/plot_architect.md`](agents/plot_architect.md) |
| `character_crafter` | Character & Dialogue Specialist | [`agents/character_crafter.md`](agents/character_crafter.md) |
| `scene_weaver` | Creative Storyteller & Prose Weaver | [`agents/scene_weaver.md`](agents/scene_weaver.md) |
| `chief_editor` | Line and Executive Editor | [`agents/chief_editor.md`](agents/chief_editor.md) |

---

## Tasks

| Order | Task ID | Agent | Input | Output file |
|-------|---------|-------|-------|-------------|
| 1 | `task_outline` | `plot_architect` | `{book_prompt}` | `outputs/outline.md` |
| 2 | `task_character_pass` | `character_crafter` | `outline.md` | `outputs/characters.md` |
| 3 | `task_draft_chapter` | `scene_weaver` | `outline.md` + `characters.md` | `outputs/draft.md` |
| 4 | `task_edit_chapter` | `chief_editor` | `draft.md` (+ reference files) | `outputs/chapter_1_final.md` |

Full task specs: [`tasks/`](tasks/)

---

## Question Protocol

### Pre-Work (before any task begins)
- Only `plot_architect` may ask the user a question at this stage.
- All other agents receive their questions via inter-agent routing.
- Once `plot_architect` has begun producing the outline, the pre-work window is **closed**.

### During Work (after pipeline starts)
- **No agent may ask the user directly.**
- Questions must be directed to a specific agent by role using the inter-agent question format defined in each agent's spec.
- The receiving agent answers in their own output file under `## Inter-Agent Questions Raised` (for questions they send) or within the relevant beat annotation for answers they provide.
- The questioning agent must make a documented assumption and continue; the pipeline does not block.

### Routing Guide

| If the question is about… | Route to |
|---------------------------|----------|
| Original prompt intent, genre, tone | `plot_architect` |
| Story structure, beat order, timeline | `plot_architect` |
| Character psychology, motivation | `character_crafter` |
| Character voice, dialogue direction | `character_crafter` |
| Prose intent, scene meaning | `scene_weaver` |
| Editorial changes, what was cut and why | `chief_editor` |

---

## Output Files

| File | Produced by | Purpose |
|------|-------------|---------|
| `outputs/outline.md` | `plot_architect` | Structured beat-by-beat chapter outline |
| `outputs/characters.md` | `character_crafter` | Psychological annotations per character per beat |
| `outputs/draft.md` | `scene_weaver` | Raw first draft, min. 1,500 words |
| `outputs/chapter_1_final.md` | `chief_editor` | Polished, publication-ready chapter ← **final deliverable** |

---

## Kickoff Instructions

1. Collect `{book_prompt}` from the user (5–100 words).
2. Pass it to `plot_architect` with the full contents of [`tasks/task_outline.md`](tasks/task_outline.md) and [`agents/plot_architect.md`](agents/plot_architect.md) as system context.
3. If `plot_architect` raises a pre-work question, surface it to the user and wait for a response before continuing.
4. On each subsequent step, pass the relevant agent spec + task spec + all upstream output files as context.
5. After `chief_editor` completes, surface `outputs/chapter_1_final.md` to the user along with any `[EDITOR FLAG]` blocks that require human decision.

---

## Re-Run / Revision Routing

If the final output contains `[EDITOR FLAG]` blocks:
- Flags directed at `plot_architect` → re-run from Task 1 with flag context appended to the prompt
- Flags directed at `character_crafter` → re-run from Task 2 with flag context
- Flags directed at `scene_weaver` → re-run from Task 3 with flag context
- Flags directed at `chief_editor` (self-flags) → re-run Task 4 only
