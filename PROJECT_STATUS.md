# Project Status — Drone-vs-Bird Detection (Defense)

## Snapshot
- Current milestone: **M6 (Colab integration) — CRITICAL PATH, due tonight Jul 1 midnight**
- Dataset: Roboflow Universe "drone-vs-bird" v3, 2783 images (1950/555/278), classes [bird=0, drone=1], no leakage. Drive at `/content/drive/MyDrive/!CVIS/data/processed`
- Test-set results (authoritative, 278 images, 302 GT objects):
  - YOLOv11s: mAP50=0.9796, mAP50-95=0.6169, P=0.9547, R=0.9711, drone recall=1.0000 ← **USE THESE IN SLIDES**
  - YOLOv11n: mAP50=0.9713 (scale comparison)
  - s outperforms n on test set (reversal of val-set ranking — document as honest limitation)
- Weights: `best_s.pt` (primary) and `best_n.pt` at `/content/drive/MyDrive/!CVIS/models/`
- Fallback demo video: committed at `runs/track_demo/...playable.mp4` via Git LFS
- Days to July 1st: **0 — SUBMISSION DAY**
- Last updated: 2026-07-01

## Decisions Log
- 2026-06-15 — Use case fixed: **drone-vs-bird object detection** for a defense / counter-UAV scenario. Detection is the mandatory core; **object tracking on video** is the bonus extension; goal is a **live demo** that distinguishes drone vs bird.
- 2026-06-15 — **Dataset: Roboflow Universe "drone-vs-bird" chosen as primary; WOSDETC dropped to Plan B.** Rationale: instant access (no data-use agreement / approval wait), YOLO-ready export. Consequence: tracking bonus uses a separately sourced held-out video clip; WOSDETC = "future work" framing.
- 2026-06-15 — **5-member split finalized** (see Team & Roles). Every member owns a technical pillar; Felipe integrates final notebook; Alberto + Felipe co-own slides.
- 2026-06-15 — **Bonus scope:** committed = SAHI tiling + failure-case gallery + object tracking + scale comparison.
- 2026-06-21 — Class ID order confirmed: bird=0, drone=1. Bug in `configs/yolo_drone_bird.yaml` fixed.
- 2026-06-21 — Drive access pattern established: `!CVIS` is a shared folder owned by Patrick; each member must add a shortcut to My Drive; assert before makedirs pattern enforced in all notebooks.
- 2026-06-29 — Test-set evaluation complete: YOLOv11s primary (mAP50=0.9796 > YOLOv11n 0.9713). Test-set ranking reverses val-set ranking; document as val vs test variance on small datasets.
- 2026-06-30 — M5 complete locally. Notebook 04 has critical bug: `PRIMARY_WEIGHTS = best_n.pt` — must be changed to `best_s.pt` before integration.

## Open Questions / Blockers
- **[CRITICAL — TONIGHT]** M6: Felipe must assemble `FINAL_drone_vs_bird.ipynb` and submit link by midnight. Notebook 04 integration checklist must be applied first (see below).
- **[OPEN — Vlad]** Notebook 04 critical bug: `PRIMARY_WEIGHTS` is hardcoded to `best_n.pt`. Must be changed to `best_s.pt` (small outperforms nano on the authoritative test set). Push before Felipe integrates.
- **[OPEN — Felipe]** Notebook 04 integration checklist: (1) Remove Section A (duplicates M4). (2) Fix `data.yaml` mutation — use temp file, not overwrite of committed file. (3) Change `!uv pip install sahi` → `!pip install sahi`. (4) Replace base64 video display with `IPython.display.Video` to avoid OOM. (5) Switch `PRIMARY_WEIGHTS` to `best_s.pt`.
- **[OPEN — Drive cleanup]** Move `class_distribution.png` to `results/`. Remove stray `IMG-20260615-WA0013.jpg`. Move `drone-vs-bird-object-detection-3/` to `data/raw/` or delete. After integration: move pillar notebooks 01–04 to `notebooks/archive/` so only `FINAL_drone_vs_bird.ipynb` sits at Drive root.
- **[OPEN]** Drive shared folder link not yet added to `demo/README.md`.
- **[OPEN]** Notebook 04 writing review not yet done (notebooks 01–03 already reviewed).
- **[RISK]** Live demo depends on Colab GPU availability on Jul 2. Mitigation: fallback video at `runs/track_demo/...playable.mp4` (committed via Git LFS).

## Team & Roles (5 members)
- Patrick — **Data & Annotation** (M1, M2): complete. Notebook: `notebooks/01_data_annotation.ipynb` (writing-reviewed).
- Jorge — **Detector Training** (M3): complete. Notebook: `notebooks/02_detector_training.ipynb` (writing-reviewed).
- Alberto — **Evaluation & Failure Analysis** (M4): complete, GPU-validated Jun 29. Notebook: `notebooks/03_evaluation_failure_analysis.ipynb` (writing-reviewed). **Co-owns slides.**
- Vlad — **Small-Object + Tracking** (M5): complete locally Jun 30. Notebook: `notebooks/04_fixed.ipynb` (NOT writing-reviewed; NOT Colab-ready). Must push `PRIMARY_WEIGHTS` fix before integration.
- Felipe — **Demo + Integration/PM** (M6, M8, M9): M6 is the critical path tonight. **Co-owns slides (M7).**

## Timeline (revised — submission day)
- [x] Jun 16 — Dataset finalized
- [x] Jun 16–19 — M1+M2 Data & Annotation (Patrick)
- [x] Jun 21 — M3 Detector Training (Jorge, both scales, val results)
- [x] Jun 29 — M4 Evaluation complete, GPU-validated (Alberto, test-set results locked)
- [x] Jun 30 — M5 Tracking + SAHI complete locally (Vlad, pushed to git)
- [ ] **Jul 1 (TODAY) — M6: Assemble FINAL_drone_vs_bird.ipynb (Felipe, DUE TONIGHT)**
- [ ] Jul 1–2 — M7: Slides (Alberto + Felipe)
- [ ] Jul 2 — M8: Live demo + recorded fallback ready for session
- [ ] Jul 2 — M9: Presentation + Q&A (all 5 members)

## Milestone Tracker
[x] M0 — Define use case & real-world problem (drone-vs-bird, defense)
[x] M1 — Source dataset — Roboflow v3 downloaded, annotations verified, class balance checked, no leakage, data.yaml written. (Patrick, complete)
[x] M2 — Verify annotations + train/val/test split — 1950/555/278, valid→val renamed, class distribution saved, data.yaml written. (Patrick, complete)
[x] M3 — Transfer-learn YOLOv11s + YOLOv11n; both trained 50 epochs; best_s.pt and best_n.pt on Drive. Notebook: `notebooks/02_detector_training.ipynb`. (Jorge, complete)
[x] M4 — Test-set evaluation: YOLOv11s mAP50=0.9796, drone recall=1.0000. Failure analysis: 5 bird-as-drone, 0 drone-as-bird, 2 missed, 17 ghost detections. Notebook: `notebooks/03_evaluation_failure_analysis.ipynb`. (Alberto, complete)
[x] M5 — ByteTrack tracking on held-out clip + SAHI tiling demo + fallback video. Annotated output at `runs/track_demo/`. Notebook: `notebooks/04_fixed.ipynb`. (Vlad, complete locally — Colab-ready fix needed before integration)
[ ] M6 — FINAL_drone_vs_bird.ipynb assembled, Colab-tested, Drive link ready. **(Felipe, TONIGHT)**
[ ] M7 — PowerPoint: use case + rationale, data, model, metrics, limitations, future improvements. (Alberto + Felipe)
[ ] M8 — Live demo + recorded fallback confirmed working. Fallback video already at `runs/track_demo/...playable.mp4`.
[ ] M9 — Rehearse 12-min presentation + mock 2-min Q&A (all 5 members)
