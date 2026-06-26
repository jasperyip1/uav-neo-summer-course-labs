# Week 2 · Module 4 — Downward Camera (Gate Detection)

Find a gate beneath the drone with contour analysis and hover directly over it.

## What you'll learn

- Building a mask of the glowing gate edges
- `cv2.findContours` and picking the largest
- Contour centroid + area
- Visual-servoing with pitch/roll to center on a target

## How to run

```bash
drone open_sim                          # launch the sim once
drone sim course/week2_vision/module4_downward/main.py            # all steps, your code
drone sim course/week2_vision/module4_downward/main_solution.py   # reference flight
```

Press **Enter** in the simulator window to start.

## Steps

1. **`step1_find_contours.py`** — count the glowing-edge contours below the drone
2. **`step2_largest_object.py`** — locate the largest gate and report its center & area
3. **`step3_track_object.py`** — fly pitch/roll to center the drone over the gate

## What to expect

The drone arms, climbs, finds the gate frame below it, then nudges itself until the gate is centered in the downward image, then lands.

---

Fill in the blanks in `tasks/`; completed references are in `solutions/` (try it yourself first!).
