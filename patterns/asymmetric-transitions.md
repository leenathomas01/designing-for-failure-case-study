# Asymmetric Transitions

## Problem

Systems that use identical thresholds for entering and exiting states oscillate under perturbation. A single anomaly triggers state change, then system returns immediately when anomaly clears. This flapping consumes resources, corrupts state, and delays actual recovery.

## Core Rule

Entry to crisis state is immediate upon critical signal. Exit from crisis state requires sustained multi-signal evidence over a defined threshold duration. The entry and exit paths have asymmetric timing.

## Structural Mechanism

Asymmetry prevents oscillation through hysteresis. State transitions use different thresholds for entry vs. exit:

- **Entry threshold:** Single critical signal triggers RED state immediately
- **Exit threshold:** Multiple signals must sustain for duration T before exiting

The asymmetry creates hysteresis. Once entered, the state cannot be exited without stronger evidence than was required for entry. This decouples noise sensitivity (entry) from recovery confidence (exit).

![Asymmetric Transitions Diagram](../diagrams/asymmetric-transitions.png)

## Example Instantiation

A communication system under geomagnetic storm:
- Entry to RED: One critical signal (HF propagation collapses) → immediate
- Exit from RED to ORANGE: Three independent signals all nominal for 6 hours → exit allowed
- Exit from ORANGE to GREEN: Same three signals sustained for additional validation period → resume

During the storm, brief signal improvements do not trigger state changes. Only sustained, multi-source confirmation allows progression back to normal.

## Pseudocode

```python
state = GREEN
recovery_timer = 0
sustained_duration = 6_hours

def evaluate_state():
    global state, recovery_timer
    
    # Fast entry
    if critical_signal_detected():
        state = RED
        recovery_timer = 0
        return state
    
    # Slow exit
    if state == RED:
        if all_signals_nominal():
            recovery_timer += sampling_interval
        else:
            recovery_timer = 0
        
        if recovery_timer >= sustained_duration:
            state = ORANGE
    
    return state
```

## Domain Adaptation Notes

**Infrastructure:** Power grid blackout recovery. Entry: frequency deviation triggers load shedding. Exit: 1+ hour of stable frequency before restoring load.

**Finance:** Bank-run circuit breaker. Entry: liquidity ratio drops below threshold → halt trading immediately. Exit: liquidity restored and stable for 4+ hours → resume.

**Autonomous Systems:** Agent containment. Entry: anomaly detected → read-only mode immediately. Exit: supervisor approval + 10 verification steps before autonomous operation.

**Distributed Networks:** Network partition detection. Entry: communication loss detected → switch to isolated mode. Exit: rejoining partition requires full state sync + consensus validation.

---

