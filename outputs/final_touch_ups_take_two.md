# The Chain — Final Touch-Ups, Take Two
### Second whole-book continuity & editorial read-through (Ch1–27)

A fresh 6-reader parallel audit of all 27 chapters, run *after* the take-one fixes (Cold Security rename, Saturday weekday, Ch9/11/21 fixes, titles, Editorial-Notes split, `Final_Story.md` compile). Purpose: verify those fixes landed, and catch anything new or previously missed. It caught one real (and embarrassing) problem the first pass missed — now fixed.

---

## 🔴 NEW — and now FIXED this pass: leaked editorial metadata inside 5 chapter files
**What happened:** when the Editorial Notes were split out earlier, the split assumed the *first* `---` in each file was the notes/prose boundary. But a few chapters' notes used `---` as **internal section dividers**, so the cut landed mid-notes and **left editorial apparatus inside the chapter files**. The take-one "0-mismatch" verification was tautological (it compared each file's post-first-`---` content to itself) and so it missed this.

**Found in the prose files:**
- **Ch21** — full scrub block (lines 2–60) + `[POLISHED PROSE BEGINS HERE]` marker, above the prose.
- **Ch23** — full scrub block (lines 2–57) above the prose.
- **Ch26** — full scrub block (lines 2–57) + marker, above the prose.
- **Ch25** — stray `[POLISHED PROSE BEGINS HERE]` marker at the top.
- **Ch17** — trailing `[END CHAPTER 17 — pulls into Chapter 18]` marker after the prose.
- **`Final_Story.md`** — inherited all of the above (it was compiled from the leaked files).

**Fix applied (this pass):**
- Rebuilt all five chapter files to **title + prose only** (verified: no metadata signature remains in any of the 27 chapter files).
- **Repaired the 3 truncated notes files** (Ch21/23/26 had only partial notes — the rest had been stuck in the chapter files). Regenerated complete notes from the pre-split commit; all 27 `editorial_notes_chN.md` are now whole.
- **Regenerated `Final_Story.md`** from the cleaned chapter files (now ~78.3k words; the ~3.5k words of leaked metadata removed). Endpoints intact (opens Ch1; closes "…so the room would stay full.").

---

## ✅ Verified clean — all take-one fixes confirmed landed
The six readers re-checked every prior fix in the actual prose:
- **Cold Security rename** — no "Osbourne" anywhere; facility = "Cold Security," area = "Sylmar." Both eaten-word reveals intact and distinct (Ch8 "Syl—" = area; Ch10/11 "Cold Sec—" = facility); Ch17 "the access road"; Ch22 "COLD SECURITY, SYLMAR"; Ch24 door brand "COLD SECURITY."
- **Weekday = Saturday** — no story-day "Tuesday" anywhere. (Ch4 "it's a tuesday" idiom and Ch7 "on a Tuesday, over a brisket" past-day are intentional, not flags.)
- **Ch9 Cole fix** — both former spoken lines now wordless gesture ("tipped his chin…" / "flat-handed him to stay"); zero Cole speech in Ch9.
- **Ch11 ending** — now "The key had turned. Russ was —" (ends on the snatch).
- **Ch21 ending** — now membrane-correct `> marco: IT'S —`; the duplicate mid-burst trimmed.
- **Chapter titles** — all 27 headers are `# Chapter N — <Title>`, well-fitted; the 3 author-added titles (Ch1/4/5) read true.
- **Cold-room noun NEVER printed** — held across all 27, including every saved-room moment and the finale.
- **Cole's banked word** — wordless until "No." fires whole at the end of Ch24; spent thereafter (Ch25 only *recalls* it).
- **The three finale inversions** — Ch27 closes whole; the gap's meaning inverts to "kept"; the architect is resolved by connection, outside-only; Russ's brand stays (not exonerated).
- **POV rotation, Rhea spelling, format membrane, collector outside-only, genre lock** — all confirmed consistent.

---

## ✅ M1 — RESOLVED (fixed everywhere except `Final_Story_Chapter_1_to_14.md`, per request)
The 4 documented fictive-frame winks — plus **2 more chapter-counting winks the audits had missed** (both Ch4) — were scrubbed to in-world phrasing in `chapter_3_final.md`, `chapter_4_final.md`, and `Final_Story.md`. (Left intact in `Final_Story_Chapter_1_to_14.md` by request.)
- Ch3: "…in three chapters" → "…in hours"
- Ch4: "after two chapters of standing on a curb" → "after all that standing on a curb"  *(newly found)*
- Ch4: "off two chapters of stuck" → "off the long stretch of stuck"  *(newly found)*
- Ch4: "the thin sound **the prose could hear**" → "the thin sound that was there to be heard"
- Ch4: "Cole understood it a half-beat **after the reader did**" → "…a half-beat too late"
- Ch4: "the only thing **in the book** actually looking at the thing" → "…in the room actually looking…"

## 🟢 Minor — confirm-intent (likely all intentional; nothing broken)
- **Ch10** *"the cold room at Cold Sec—"* — "cold…Cold" proximity reads slightly doubled; defensible (the chilling corporate name). Optional: *"the cold room — Cold Sec—"*.
- **Ch10** *"read at 3:418… no… 3:41 a.m."* — deliberate misread-then-correct beat; confirm a proofreader won't "fix" it.
- **Ch20** — Marco's "twelve years" (career) vs. the card "blank for eleven years" vs. Rhea's "eleven years" (Ch17). Reads as intentional (card acquired a year into the job); confirm.
- **Ch23** — two "Friday dinner rush" *similes* (tempo figures, not a day-claim) in a Saturday-set book; harmless, optional to neutralize.
- **Ch22 / Ch24** — Devang gets a warm beat in each (one per chapter, rule honored); just noting two across the span.

---

## Note / lesson
The take-one L1 claim ("Editorial Notes moved… 0 mismatches") was **incomplete** — the verification method couldn't detect the mis-split. This pass re-verified by scanning for metadata *signatures* across every file (not a self-comparison), which is the reliable check. The take-one `final_touch_ups.md` L1 has been annotated to point here.

See **`final_touch_ups_take_two_ledger.md`** for the per-chapter table.
