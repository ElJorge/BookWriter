# Story Crew — README

A four-agent markdown spec system for generating a polished Chapter 1 from a short story prompt.

---

## File Structure

```
story_crew/
├── README.md                   ← you are here
├── orchestration.md            ← pipeline diagram, routing rules, kickoff instructions
│
├── agents/
│   ├── plot_architect.md       ← Senior Plot Architect
│   ├── character_crafter.md    ← Character & Dialogue Specialist
│   ├── scene_weaver.md         ← Creative Storyteller & Prose Weaver
│   └── chief_editor.md         ← Line and Executive Editor
│
├── tasks/
│   ├── task_outline.md         ← Task 1: build beat-by-beat outline
│   ├── task_character_pass.md  ← Task 2: annotate characters & dialogue
│   ├── task_draft_chapter.md   ← Task 3: write full draft (1,500+ words)
│   └── task_edit_chapter.md    ← Task 4: polish to publication-ready
│
└── outputs/                    ← created at runtime by your orchestrator
    ├── outline.md
    ├── characters.md
    ├── draft.md
    └── chapter_1_final.md      ← final deliverable
```

---

## How to Use These Files

Each agent call should be constructed as:

```
SYSTEM:
  [contents of agents/<agent_id>.md]
  [contents of tasks/<task_id>.md]

USER:
  [upstream output files as context]
  [the book_prompt on Task 1]
```

See [`orchestration.md`](orchestration.md) for the full sequential flow, question routing rules, and re-run logic.

---

## Quick Reference: Question Rules

| Stage | Who can ask the user? | Who do agents ask instead? |
|-------|-----------------------|---------------------------|
| Pre-work (before outline starts) | `plot_architect` only | — |
| During pipeline | Nobody | A specific agent by role |

Inter-agent question format is defined in each agent's spec file.

---

## Deliverables Per Run

| File | What it contains |
|------|-----------------|
| `outputs/outline.md` | Beat breakdown, POV, setting, hook |
| `outputs/characters.md` | Character roster + per-beat psychological annotations |
| `outputs/draft.md` | Raw first draft with metadata |
| `outputs/chapter_1_final.md` | **Final polished chapter** + editorial notes |
