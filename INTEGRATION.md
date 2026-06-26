# Simulator Integration Notes

Notes from validating these labs against the real **UAVSim_Linux** build. Read this
before running on the simulator or tuning the control labs.

## How to run a lab against the sim

```bash
# 1. Launch the simulator (native Linux build)
~/projects/bwsi/uav-neo-installer/UAVNeo-Simulator/UAVSim_Linux_v0.0.1/UAVSim_Linux_v0.0.1.x86_64 &

# 2. Run a lab. The course labs are symlinked into the drone tool at labs/course,
#    so once the venv .pth points at the library you can use:
drone sim course/week3_controls/module2_feedback_control/main.py

#    ...or run directly with the venv python:
PYTHONPATH=.../drone-student/library \
  .../drone-venv/bin/python .../module2_feedback_control/main.py -s

# 3. In the UAVSim window: press ENTER to connect + enter user-program mode.
```

The Python script is the server; the simulator connects to it over UDP
(127.0.0.1:5064/5065). Pressing **Enter** in the sim window is required to start the
program — that step is manual.

## Bug found & fixed (committed to uav-neo-installer @ course-labs-integration)

`drone_core_sim.__resolve_sim_ip()` used the default-route **gateway**
unconditionally (a comment said "WSL2" but there was no WSL2 check). On native
Linux this resolved the sim to the LAN router (e.g. `192.168.1.1`) and the lab
hung at *"awaiting connection."* Fixed by guarding the gateway lookup behind an
actual `/proc/version` WSL2 check, falling back to `127.0.0.1`. You can also force
it with `DRONE_SIM_IP=127.0.0.1`.

## Sim flight model (measured)

These facts drive `labs/neo_lab.py` and the control-lab gains:

| Observation | Value / behavior |
|-------------|------------------|
| Spawn altitude (`get_altitude()`) | **~7 m**, not 0 — there is a ground offset |
| `takeoff()` alone | arms the motors but does **not** auto-climb |
| `send_pcmd(0,0,0,throttle)` | throttle is a **vertical velocity** command (~12 m/s per unit) |
| `stop()` (`throttle=0`) | engages hover-hold; altitude stays rock-steady |
| Throttle deadband | ~0.05 — commands below this are absorbed by hover-hold |

Consequences encoded in the labs:
- **Always launch via `neo_lab.Launcher`**: arm with `takeoff()`, then climb with a
  small-gain throttle controller to a height **above the measured ground**.
- **Altitude targets are heights above ground** (`neo_lab.height(drone)`), never
  absolute — the spawn offset varies and the drone keeps its altitude between runs.
- **Keep gains small** (`KP≈0.2`, clamp throttle). The deadband means a pure-P
  controller settles ~0.2–0.4 m short of target (a real proportional-control droop),
  so tolerances are widened and the PID lab's integral term closes the gap.

## Validation status

- ✅ Connection / handshake (after the IP fix)
- ✅ `neo_lab.Launcher` arm + climb to a height above ground
- ✅ **Module 2** (proportional altitude hold) — full launch + hold 5 m + setpoint
  sequence 3→6→2 m, flown in the sim
- ⏳ Not yet flown in-sim: Module 3 (PID, position, visual-servo) and the Week 2
  vision labs. They reuse the same validated launcher; the vision labs additionally
  depend on the scene actually containing the expected props (colored gates, a
  ground line, a downward target), which still needs to be confirmed.
