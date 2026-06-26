# Week 2 · Module 3 — Linear Regression (Edge Following)

Fit a straight line to detected pixels with least squares, then use it to steer. The gates glow, so we follow a bright gate edge from the downward camera.

## What you'll learn

- Thresholding bright pixels and `np.argwhere`
- Least-squares line fit (`np.polyfit`)
- Turning a pixel offset into a steering (roll) command

## How to run

```bash
drone open_sim                          # launch the sim once
drone sim course/week2_vision/module3_linear_regression/main.py            # all steps, your code
drone sim course/week2_vision/module3_linear_regression/main_solution.py   # reference flight
```

Press **Enter** in the simulator window to start.

## Steps

1. **`step1_detect_line.py`** — find and count the bright edge pixels
2. **`step2_fit_line.py`** — fit y = m·x + b to those pixels
3. **`step3_follow_line.py`** — fly forward while rolling to keep the edge centered

## What to expect

Steps 1-2 hover and report; Step 3 flies forward, steering to stay over a bright edge for a fixed time.

---

Fill in the blanks in `tasks/`; completed references are in `solutions/` (try it yourself first!).
