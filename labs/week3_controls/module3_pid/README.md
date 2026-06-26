# Week 3 · Module 3 — PID Control

Full PID control, and a capstone that combines Week 2 vision with Week 3 control.

## What you'll learn

- The PID law (P + I + D) and anti-windup
- Why the integral term removes steady-state error
- Estimating distance by integrating velocity (dead reckoning)
- Visual-servoing: closing a PID loop on a camera pixel error

## How to run

```bash
drone open_sim                          # launch the sim once
drone sim course/week3_controls/module3_pid/main.py            # all steps, your code
drone sim course/week3_controls/module3_pid/main_solution.py   # reference flight
```

Press **Enter** in the simulator window to start.

## Steps

1. **`step1_pid_altitude.py`** — hold a target height with a full PID controller
2. **`step2_position_hold.py`** — fly a set distance forward (PID on integrated position)
3. **`step3_visual_servo.py`** — yaw with a PID loop to lock onto a glowing gate

## What to expect

Runs the three steps in order: hold 5 m, fly forward, then turn to center and lock onto a gate, then land.

---

Fill in the blanks in `tasks/`; completed references are in `solutions/` (try it yourself first!).
