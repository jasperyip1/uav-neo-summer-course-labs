# Week 3 · Module 2 — Proportional Control (Altitude Hold)

Your first closed-loop controller: proportional control. Warm up on a 1-D math model, then hold the real drone at a target height.

## What you'll learn

- The proportional law: command = Kp · error
- Why a P-only controller leaves a small steady-state error
- Driving the drone's throttle from an altitude error

## How to run

```bash
python3 tasks/p_control.py        # your work (prints PASS/FAIL)
python3 solutions/p_control.py    # reference
```

Then on the simulator:
```bash
drone open_sim                          # launch the sim once
drone sim course/week3_controls/module2_feedback_control/main.py            # all steps, your code
drone sim course/week3_controls/module2_feedback_control/main_solution.py   # reference flight
```

Press **Enter** in the simulator window to start.

## Steps

1. **`step1_altitude_hold.py`** — proportional throttle to hold a target height above ground
2. **`step2_altitude_steps.py`** — chase a sequence of target heights (a step response)

## What to expect

`p_control.py` self-checks with `python3`. The sim steps arm, climb, and hold each target height (settling a little short — that's the P-control lesson).

---

Fill in the blanks in `tasks/`; completed references are in `solutions/` (try it yourself first!).
