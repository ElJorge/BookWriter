# Story Crew — README

A four-agent markdown spec system for generating a polished Chapter 1 from a short story prompt.

---

## File Structure

```
BookWriter/
├── .claude/
│   └── agents/                     ← Claude Code subagents (the four crew members)
│       ├── plot-architect.md       ← Senior Plot Architect
│       ├── character-crafter.md    ← Character & Dialogue Specialist
│       ├── scene-weaver.md         ← Creative Storyteller & Prose Weaver
│       └── chief-editor.md         ← Line and Executive Editor
│
├── docs/
│   ├── story-crew-README.md        ← you are here
│   └── orchestration.md            ← pipeline diagram, routing rules, kickoff instructions
│
└── outputs/                        ← created at runtime by the orchestrator
    ├── outline.md
    ├── characters.md
    ├── draft.md
    └── chapter_1_final.md          ← final deliverable
```

Each subagent is self-contained: its task spec (output format, requirements) is
embedded directly in the agent file, so there is no separate `tasks/` directory.

---

## How to Use These Files

Each crew member is a Claude Code subagent in `.claude/agents/`. Invoke one by name
and hand it the upstream artifacts as context, e.g.:

```
Use the plot-architect subagent on this prompt: <book_prompt>
Use the character-crafter subagent on outputs/outline.md
Use the scene-weaver subagent on outputs/outline.md + outputs/characters.md
Use the chief-editor subagent on outputs/draft.md (reference outline + characters)
```

See [`orchestration.md`](orchestration.md) for the full sequential flow, question routing rules, and re-run logic.

---

## Quick Reference: Question Rules

| Stage | Who can ask the user? | Who do agents ask instead? |
|-------|-----------------------|---------------------------|
| Pre-work (before outline starts) | `plot-architect` only | — |
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
