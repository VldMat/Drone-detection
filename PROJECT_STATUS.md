# Project Status — Drone-vs-Bird Detection (Defense)

## Snapshot
- Current milestone: M3 complete → **M4 (evaluation) and M5 (tracking) now unblocked**
- Val-set results (2026-06-21): YOLOv11s mAP50=0.929, mAP50-95=0.566, P=0.928, R=0.866, 90.6 FPS, 19.2 MB
                                YOLOv11n mAP50=0.938, mAP50-95=0.582, P=0.926, R=0.882, 114.5 FPS, 5.5 MB
  Note: nano outperforms small — expected on a small dataset (bias-variance tradeoff). Test-set metrics pending (M4).
- best_s.pt and best_n.pt on Drive at /content/drive/MyDrive/!CVIS/models/
- Dataset: **Roboflow Universe "drone-vs-bird" v3** (still images, YOLOv11 format).
  Classes: [bird=0, drone=1] (verify in data.yaml). 2783 images total: 1950 train / 555 val / 278 test.
  Class balance: drone ~2020 boxes (63%) | bird ~1200 boxes (37%) → ratio ~1.7:1. No leakage detected.
  Processed data on Drive at: `/content/drive/MyDrive/!CVIS/data/processed`
- Days to July 1st: 10 (as of 2026-06-21)
- Last updated: 2026-06-21

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
- **[RESOLVED 2026-06-21]** Class ID order confirmed: bird=0, drone=1. Bug in
  `configs/yolo_drone_bird.yaml` corrected (names field was reversed). `configs/data.yaml` also
  confirmed correct. Any notebook cell referencing class IDs must use this order.
- **[RESOLVED 2026-06-21]** Colab Drive access: `!CVIS` is owned by Patrick and lives under
  "Shared with me". Each member must **Add shortcut to Drive** (into My Drive root) so the
  canonical path `/content/drive/MyDrive/!CVIS` resolves identically for everyone. The training
  notebook now asserts `data/processed` exists *before* any `os.makedirs`, so a missing shortcut
  fails loudly instead of creating a phantom empty `!CVIS`.
- **[OPEN]** Drive shared folder link needs to be added to `demo/README.md` for team access
  (include the "Add shortcut to Drive" instruction so Alberto and Vlad don't hit the same wall).
- **[OPEN]** Video clips in Drive VIDEOS/ folder — Vlad needs to know which clips are suitable for
  tracking (disjoint from training data). Patrick to confirm.
- **[RESOLVED]** Dataset chosen (Roboflow v3, 2783 images, no leakage, valid→val renamed, data.yaml written).

## Team & Roles (5 members)
- Patrick — **Data & Annotation** (M1, M2): Roboflow export, verify annotations, class balance,
  train/val/test split, source held-out video clip. *Bonus:* class-distribution plots +
  augmentation strategy (copy-paste for rarer class). Notebook: data + preprocessing.
- Jorge — **Detector Training** (M3): transfer-learn YOLO, log every hyperparameter, run
  training (rough checkpoint Jun 21). *Bonus:* compare 2 model scales (YOLOv11n vs s) + measure FPS.
  Notebook: model + training config.
- Alberto — **Evaluation & Failure Analysis** (M4): test-set mAP50, mAP50-95, P/R, confusion
  matrix. *Bonus:* failure-case gallery of drone↔bird confusions + error analysis by object size.
  Notebook: evaluation + results + limitations. **Co-owns slides.**
- Vlad — **Small-Object + Tracking** (M5 + small-object pitfall): SAHI tiling + tracking
  (ByteTrack/BoT-SORT) on the video clip. *Bonus:* tracking metrics (MOTA/IDF1) + stability
  discussion. Notebook: tracking + small-object handling.
- Felipe — **Demo + Integration/PM** (M8, M6 assembly, M9): reusable inference script, live
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
[x] M1 — Source dataset — **Roboflow v3 downloaded, annotations verified visually, class balance
         checked, no leakage, processed to Drive. Patrick complete.**
[x] M2 — Verify annotations + train/val/test split — 1950/555/278 split, valid→val renamed,
         class dist plot saved, data.yaml written. **Patrick complete.**
[x] M3 — Transfer-learn an object detector: drone vs bird (mandatory core)
         Notebook: `notebooks/02_detector_training.ipynb`. Both YOLOv11s and YOLOv11n trained
         and evaluated on val set (2026-06-21). best_s.pt + best_n.pt on Drive. M4 and M5 unblocked.
[ ] M4 — Report training params + test-set metrics
[ ] M5 — (Bonus) Object tracking on video (+ optional segmentation)
[ ] M6 — Build & document the Colab notebook
[ ] M7 — Build the PowerPoint
[ ] M8 — Prepare the live demo (drone vs bird) with a recorded fallback
[ ] M9 — Rehearse the 12-min presentation (all 5 members + Q&A)
