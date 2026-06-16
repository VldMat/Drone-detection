# Drone vs Bird Detection — Defense Use Case

Computer Vision group project (IE University MBDS 2025 · Term 3).

Detecting drones from aerial imagery and video, and distinguishing them from birds —
a well-known hard problem in counter-UAV defense: birds are the classic false-positive source,
and drones are small, fast targets easily confused with birds at distance.

---

## Use Case

Unauthorized drones pose a growing security threat to sensitive airspace. Existing radar and
acoustic systems struggle at short range and in cluttered environments. A vision-based detector
that reliably separates **drone** from **bird** provides a fast, cost-effective first layer of
detection for defense and perimeter-security scenarios.

---

## Approach

| Component | Method |
|---|---|
| **Object detection** (core) | Transfer learning on YOLOv11 (pretrained on COCO) |
| **Classes** | `drone`, `bird` |
| **Small-object handling** | SAHI tiling |
| **Object tracking** (bonus) | ByteTrack / BoT-SORT paired with the detector |
| **Dataset** | Roboflow Universe — drone-vs-bird (still images, YOLO format) |

---

## Repository Structure

```
Drone-detection/
├── configs/                  # YOLO training config + hyperparameters
├── data/                     # gitignored — stored on Google Drive
│   ├── raw/                  # original Roboflow export
│   ├── processed/            # train / val / test split
│   └── video/                # held-out clip for tracking + demo
├── demo/                     # live demo instructions + Drive asset links
├── docs/                     # assignment brief (GroupProject.txt / .pdf)
├── models/                   # gitignored — weights on Google Drive
├── results/                  # plots, confusion matrix, metrics CSVs
├── slides/                   # PowerPoint presentation
├── PROJECT_STATUS.md         # milestone tracker + team roles + timeline
└── README.md
```

> Data and model weights are stored on a shared Google Drive folder (link in `demo/README.md`).

---

## Team & Roles

| Member | Pillar | Core responsibility |
|---|---|---|
| Patrick | Data & Annotation | Roboflow export, annotation verification, class balance, train/val/test split, held-out video clip |
| Jorge | Detector Training | Transfer-learn YOLO, log all hyperparameters, training run |
| Alberto | Evaluation & Failure Analysis | Test-set mAP50/mAP50-95/P/R, confusion matrix, failure-case gallery |
| Vlad | Small-Object + Tracking | SAHI tiling, ByteTrack/BoT-SORT tracker on video |
| Felipe | Demo + Integration | Inference script, live demo + recorded fallback, notebook integration |

All 5 members author the shared Colab notebook (one section each) and present on July 2nd.

---

## Deliverables

- **Google Colab notebook** — submitted as link + file by July 1st (midnight)
- **PowerPoint** — use case, data, model, metrics, limitations, future work
- **Live demo** — real-time drone-vs-bird inference with a recorded fallback

---

## Key Dates

| Date | Milestone |
|---|---|
| Jun 16 | Dataset confirmed with team |
| Jun 19 | Data & annotation ready |
| Jun 21 | Rough detector checkpoint |
| Jun 24 | Training complete |
| Jun 26 | Evaluation + tracking done |
| Jun 27 | Technical feature-freeze |
| Jun 28–29 | Notebook integrated + Colab-tested |
| Jun 26–30 | Slides built |
| Jun 29 | Live demo + fallback ready |
| Jun 30–Jul 1 | Full rehearsal + mock Q&A |
| **Jul 1 (midnight)** | **Notebook submitted** |
| **Jul 2** | **12-min presentation + 2-min Q&A** |

---

## Setup

```bash
git clone https://github.com/VldMat/Drone-detection.git
cd Drone-detection
pip install -r requirements.txt   # coming soon
```

For Colab: open `notebooks/FINAL_drone_vs_bird.ipynb`, mount Google Drive,
and follow the setup cell at the top.

---

## Course

IE University · Master in Big Data & Business Analytics (MBDS) 2025
Term 3 — Computer Vision
