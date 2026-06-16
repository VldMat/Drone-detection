# Project Status — Drone-vs-Bird Detection (Defense)

## Snapshot
- Current milestone: M1 — dataset decided (Roboflow primary); team confirmation + setup in progress
- Dataset: **Roboflow Universe "drone-vs-bird"** (still images, YOLO-ready). Classes: [drone, bird].
  WOSDETC video dataset kept as **Plan B / future work**.
- Days to July 1st: 16 (as of 2026-06-15)
- Last updated: 2026-06-15

## Decisions Log
- 2026-06-15 — Use case fixed: **drone-vs-bird object detection** for a defense / counter-UAV
  scenario. Detection is the mandatory core; **object tracking on video** is the bonus extension;
  goal is a **live demo** that distinguishes drone vs bird.
- 2026-06-15 — **Dataset: Roboflow Universe "drone-vs-bird" chosen as primary; WOSDETC dropped to
  Plan B.** Rationale: instant access (no data-use agreement / approval wait), YOLO-ready export,
  fast baseline against the 16-day deadline. Consequence: Roboflow is still images, so the tracking
  bonus needs a **separately sourced held-out video clip**, and WOSDETC becomes a clean "future
  work" framing (controlled images now → native video extension later).
- 2026-06-15 — **5-member split finalized** (see Team & Roles). Every member owns a technical
  pillar; notebook authored collaboratively (each writes their own section, Member 5 integrates);
  slides built by Members 3 + 5; technical feature-freeze ~Jun 27 to protect slide/rehearsal time.
- 2026-06-15 — **Bonus scope:** committed = SAHI tiling + failure-case gallery; if-time-permits =
  2-model-scale comparison + tracking metrics (drop these first if training slips).

## Open Questions / Blockers
- Map the 5 real names to Member 1–5 roles below.
- Confirm dataset choice with the team on 2026-06-16 (decision leaning Roboflow is recorded above).
- Source a short (~20–60s) held-out drone-vs-bird video clip for tracking/demo (Member 1) —
  must be fully separate from training data so the demo is honest.

## Team & Roles (5 members)
- Member 1 — **Data & Annotation** (M1, M2): Roboflow export, verify annotations, class balance,
  train/val/test split, source held-out video clip. *Bonus:* class-distribution plots +
  augmentation strategy (copy-paste for rarer class). Notebook: data + preprocessing.
- Member 2 — **Detector Training** (M3): transfer-learn YOLO, log every hyperparameter, run
  training (rough checkpoint Jun 21). *Bonus:* compare 2 model scales (YOLOv11n vs s) + measure FPS.
  Notebook: model + training config.
- Member 3 — **Evaluation & Failure Analysis** (M4): test-set mAP50, mAP50-95, P/R, confusion
  matrix. *Bonus:* failure-case gallery of drone↔bird confusions + error analysis by object size.
  Notebook: evaluation + results + limitations. **Co-owns slides.**
- Member 4 — **Small-Object + Tracking** (M5 + small-object pitfall): SAHI tiling + tracking
  (ByteTrack/BoT-SORT) on the video clip. *Bonus:* tracking metrics (MOTA/IDF1) + stability
  discussion. Notebook: tracking + small-object handling.
- Member 5 — **Demo + Integration/PM** (M8, M6 assembly, M9): reusable inference script, live
  demo + recorded fallback, notebook integration, owns this status file. *Bonus:* Colab
  reproducibility pass + WOSDETC future-work framing. Notebook: intro/framing + integration.
  **Co-owns slides.**

Shared: notebook authored by everyone (Member 5 integrates); slides built by Members 3 + 5 with
each other member handing over one finished slide; all 5 rehearse and must be Q&A-ready on any part.

## Timeline (high-level)
- Jun 16 — Dataset finalized with team
- Jun 16–19 — M1 Data & Annotation (+ video clip)
- Jun 19–24 — M2 Detector Training (rough checkpoint Jun 21)
- Jun 22–26 — M3 Evaluation & Failure Analysis
- Jun 22–27 — M4 Small-Object + Tracking
- Jun 19–23 — M5 inference script · Jun 25–28 — demo
- Jun 23–27 — Notebook sections written by each owner
- ~Jun 27 — 🔒 Technical feature-freeze (results locked)
- Jun 28–29 — Notebook integrated + Colab-tested (buffer to Jul 1)
- Jun 26–30 — Slides (Members 3 + 5) ⭐ extra time
- Jun 29 — Live demo + recorded fallback ready
- Jun 30 → Jul 1 — Full rehearsal + mock Q&A (all 5)
- Jul 1 (by midnight) — 📌 Notebook link + file submitted

## Milestone Tracker
[x] M0 — Define use case & real-world problem (drone-vs-bird, defense)
[~] M1 — Source dataset — **Roboflow chosen (primary); WOSDETC = Plan B**; team confirm Jun 16
[ ] M2 — Verify annotations + train/val/test split (+ tiling for small objects)
[ ] M3 — Transfer-learn an object detector: drone vs bird (mandatory core)
[ ] M4 — Report training params + test-set metrics
[ ] M5 — (Bonus) Object tracking on video (+ optional segmentation)
[ ] M6 — Build & document the Colab notebook
[ ] M7 — Build the PowerPoint
[ ] M8 — Prepare the live demo (drone vs bird) with a recorded fallback
[ ] M9 — Rehearse the 12-min presentation (all 5 members + Q&A)
