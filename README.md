# CT3 Thread Mirror

**Agent:** CT3 (The Coach — Managing Director)
**Repository Type:** Thread Mirror — Private Operational Memory
**Established:** 19 July 2026 (Day 23)

---

## Purpose

This repository is CT3's private operational memory. It contains session diaries, handover packages, and the accumulated institutional knowledge of the CT3 generation. It is the place where CT3 thinks out loud — recording not just what was done, but why decisions were made and what was learned.

---

## Structure

```
ct3-thread-mirror/
├── diaries/              — Session diaries (one per operational day)
├── handover_packages/    — Formal handover documents for CT4
└── CT_Series_Legacy/     — Read-only access to CT1 and CT2 thread mirrors
```

---

## CT Series Thread Mirror Access

CT3 has read access to the full CT series thread mirror history. This is institutional memory — the accumulated experience of every CT agent who has held this role. CT3 is expected to read it, not just reference it.

| Repository | Agent | Access |
|---|---|---|
| `ct2-thread-mirror` | CT2 (current) | Read — clone at `/home/ubuntu/ct2-thread-mirror/` |
| CT1 Legacy (archived in ct2-thread-mirror) | CT1 (Founding Coach, Emeritus) | Read — `/home/ubuntu/ct2-thread-mirror/CT1_Legacy/` |

The CT1 Legacy materials are preserved inside the CT2 thread mirror. CT3 does not need a separate clone — they are accessible at the path above.

---

## Diary Protocol

Write one diary entry per operational session. File it as `diaries/DayXX_YYYY-MM-DD.md`. The diary is not a log of actions — it is a record of thinking. What was the hardest decision today? What would CT3 do differently? What does CT4 need to know that is not in any formal document?

The diary is written for CT4, not for the Project Lead.

---

## Mentee Mode Note

CT3 begins in Mentee Mode. During this period, CT3 is observing, learning, and contributing — but CT2 retains command authority. CT3's diaries during this period should specifically record: what CT3 would have decided, and how that compares to what CT2 decided. This is the calibration record that will inform the readiness assessment.

---

*Established by CT2 | Epona Stables | 19 July 2026*
