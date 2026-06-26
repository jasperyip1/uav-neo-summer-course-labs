# Week 2 · Module 5 — Color Segmentation (Seek a Gate)

Segment the cyan gates from the blue background, then search for, center, and approach one.

## What you'll learn

- HSV color masking (`cv2.inRange`) for cyan vs. the blue wall
- Bounding boxes (`cv2.boundingRect`)
- Filtering to gate-shaped (square) contours
- Yaw-to-center then approach control

## How to run

```bash
drone open_sim                          # launch the sim once
drone sim course/week2_vision/module5_color_segmentation/main.py            # all steps, your code
drone sim course/week2_vision/module5_color_segmentation/main_solution.py   # reference flight
```

Press **Enter** in the simulator window to start.

## Steps

1. **`step1_hsv_mask.py`** — measure how much of the image is cyan gate
2. **`step2_bounding_box.py`** — find the largest cyan gate's bounding box
3. **`step3_seek_gate.py`** — spin to find a gate, center it with yaw, fly toward it

## What to expect

The drone hovers and reports, then turns until a gate is centered and flies forward until the gate fills enough of the view ('Reached the gate').

---

Fill in the blanks in `tasks/`; completed references are in `solutions/` (try it yourself first!).
