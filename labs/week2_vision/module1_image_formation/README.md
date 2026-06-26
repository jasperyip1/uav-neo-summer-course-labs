# Week 2 · Module 1 — Image Formation

The pinhole camera model: how a 3-D point in the world becomes a pixel. This is pure math — no simulator needed.

## What you'll learn

- Perspective projection (x = f·X/Z)
- Converting image-plane meters to pixels
- Building the camera intrinsic matrix K
- Projecting a world point given the camera pose
- Modeling radial (lens) distortion

## How to run

```bash
python3 tasks/image_formation.py        # your work (prints PASS/FAIL)
python3 solutions/image_formation.py    # reference
```

## What to expect

Each function you complete is checked automatically; the script prints `PASS`/`FAIL` per question and a final score.

---

Fill in the blanks in `tasks/`; completed references are in `solutions/` (try it yourself first!).
