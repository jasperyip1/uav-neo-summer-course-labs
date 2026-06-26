# Week 2 · Module 2 — OpenCV (Thresholding & Morphology)

Core image-processing operations applied to the drone's live downward camera.

## What you'll learn

- Grayscale + binary thresholding (`cv2.threshold`)
- Cleaning a mask with erosion/dilation (morphological opening)
- Blurring and Sobel edge detection

## How to run

```bash
drone open_sim                          # launch the sim once
drone sim course/week2_vision/module2_opencv/main.py            # all steps, your code
drone sim course/week2_vision/module2_opencv/main_solution.py   # reference flight
```

Press **Enter** in the simulator window to start.

## Steps

1. **`step1_threshold.py`** — threshold the camera image and measure the bright fraction
2. **`step2_morphology.py`** — erode then dilate to remove speckle noise
3. **`step3_blur_edges.py`** — average-blur then a Sobel edge-magnitude image

## What to expect

The drone arms, climbs, and hovers while each step processes a frame and prints a measurement. It does not fly around.

---

Fill in the blanks in `tasks/`; completed references are in `solutions/` (try it yourself first!).
