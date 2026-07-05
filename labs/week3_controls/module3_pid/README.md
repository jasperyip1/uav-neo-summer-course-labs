# Week 3 · Module 3 — PID Control

Full PID control, and a capstone that combines Week 2 vision with Week 3 control.

In Module 2 you held altitude with a proportional controller and watched it settle a
little short of the target — a permanent gap that never closed. This module fixes that
gap and adds damping, so the drone reaches a setpoint quickly *and* sits on it exactly.
The same three-term controller (PID) is the workhorse behind nearly every altitude,
position, and heading loop on a real drone.

## What you'll learn

- **The PID law (P + I + D)** — how three simple terms combine into one controller that is
  fast, accurate, and stable.
- **Why the integral term removes steady-state error** — the reason a P-only controller
  droops, and how the I term erases the gap.
- **What the derivative term does** — how reacting to the *rate* of error damps overshoot.
- **Anti-windup** — why the integral has to be clamped, and what goes wrong if it isn't.
- **Dead reckoning** — estimating how far you have travelled by integrating velocity when
  there is no position sensor.
- **Visual servoing** — closing a PID loop on a *camera pixel* error instead of a physical
  distance, so the drone steers using what it sees.

## How PID control works

A feedback controller has one job: look at the **error** (how far you are from the target)
and decide what command to send. PID builds that command from three terms, each answering
a different question about the error.

**Proportional (P) — "how far off am I right now?"**
The P term is just `Kp · error`: a big error gives a big correction, and the push shrinks
as you close in. On its own, P has a flaw. To counteract a constant pull — gravity acting
on the throttle, for instance — it needs to output a non-zero command, but it only
*produces* one when there is a non-zero error. So it parks a little short of the target
forever. That leftover gap is the **steady-state error**.

**Integral (I) — "how long have I been off?"**
The I term sums the error over time: `Ki · Σ(error · dt)`. Even a tiny, constant error
keeps accumulating, so the integral grows and pushes harder until the error is driven to
*zero* — erasing the gap P leaves behind. The hazard: if the drone stays far from target
for a while, the integral can balloon and then violently overshoot once it catches up.
**Anti-windup** clamps the accumulated integral to a fixed range (`INT_CLAMP`) so it can
never wind that far up.

**Derivative (D) — "how fast is the error changing?"**
The D term is `Kd · d(error)/dt`. If the error is collapsing quickly you are about to
overshoot, so D pushes back to slow the approach — a shock absorber that trades a little
speed for much less overshoot and oscillation. D amplifies noise, so it is usually the
smallest gain.

Together: `output = Kp·error + Ki·∫error dt + Kd·d(error)/dt`. Tuning is the art of
balancing them: raise `Kp` for a faster response, add `Ki` to kill the steady-state gap,
add `Kd` to tame the overshoot that more `Kp` and `Ki` create.

**Dead reckoning (Step 2).** The sim reports velocity but not forward position, so you
estimate position by *integrating* velocity — `position += velocity · dt` every frame,
the same idea as the integral term. It works, but small velocity errors pile up, so the
estimate slowly drifts. That drift is a real limitation of navigating without a position
fix, and it is why real drones fuse in GPS or a camera.

**Visual servoing (Step 3).** The error need not be a distance in meters. Here it is a
*pixel* offset: how far the gate's center sits from the middle of the image. Feed that
pixel error through the same PID to command yaw, and the drone turns until the gate is
centered. This is the bridge between Week 2 (finding things in images) and Week 3
(controlling the drone): vision produces the error, the controller acts on it.

## Key terms

- **PID control** — a controller that sums three terms: `output = Kp·error + Ki·(integral of error) + Kd·(rate of change of error)`.
- **Integral term (I)** — adds up error over time. A small constant error builds the integral until the controller pushes hard enough to erase it — this is what removes the steady-state error P alone leaves.
- **Derivative term (D)** — responds to how fast the error is changing; it acts as a brake to reduce overshoot and oscillation.
- **Anti-windup** — clamping the accumulated integral so it can't grow huge while the drone is far from target (which would cause a big overshoot later). Here `INT_CLAMP` bounds it.
- **Dead reckoning** — estimating position by integrating velocity over time (`position += velocity · dt`) when you have no direct position sensor.
- **Visual servoing** — closing a control loop on a camera pixel error (here, yaw until the gate's column equals the image center).
- **Normalized error** — a pixel error divided by half the image width, so it lands in roughly −1…+1 regardless of resolution.

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

## You're done when

- Step 1: the drone settles to `TARGET_HEIGHT` with **no** lasting gap (tighter than the P-only Module 2) and holds for `HOLD_TIME`.
- Step 2: the drone flies forward about `TARGET_DIST` meters and brakes to a stop instead of overshooting.
- Step 3: the drone turns until a gate is centered (within `CENTER_TOL`), holds, then lands.

## If it doesn't work

| Symptom | Fix |
|---------|-----|
| `NameError: name 'image' is not defined` (Step 3) | Capture the frame: `image = drone.camera.get_color_image()`. |
| Altitude oscillates and grows | Integral windup — make sure you clamp `_err_int` to `±INT_CLAMP`, and add some `Kd`. |
| Step 2 overshoots the distance | Use velocity as the derivative term (`err_dot = -velocity[2]`) so it brakes early; raise `KD`. |
| Step 3 jumps between gates and never locks | Track one gate: store `_target_col` and use `gate_nearest_to`, not `gate_nearest_center`, after the first frame. |
| Step never finishes | Check your "settled" timer logic — it must require staying within tolerance for the full hold time. |

## Going further (optional)

- Tune Step 1 for the fastest settle with **no** overshoot. Record the `Kp, Ki, Kd` you land on.
- Step 2 dead-reckons distance from velocity, which drifts. How far off is it after 4 m? Could the downward camera correct the drift?
- Combine Steps 2 and 3: visual-servo the yaw *while* flying forward, so the drone both aims at and approaches the gate.

---

Fill in the blanks in `tasks/`; completed references are in `solutions/` (try it yourself first!).
