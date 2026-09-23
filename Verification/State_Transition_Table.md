# SV&V Lab Task 4 — State Transition Table

## Valid Transitions

| Transition ID | Current State | Event/Condition | Next State | Required action or guard |
|---|---|---|---|---|
| T1 | IDLE | Delivery Request Received | NAVIGATING | Store the destination and begin navigation. |
| T2 | NAVIGATING | Obstacle Detected | AVOIDING_OBSTACLE | Pause normal navigation and handle the obstacle. |
| T3 | AVOIDING_OBSTACLE | Obstacle Avoided | NAVIGATING | Resume navigation toward the stored destination. |
| T4 | NAVIGATING | Destination Reached | DELIVERING | Enter delivery only when no obstacle is actively being handled. |
| T5 | DELIVERING | Delivery Successful | RETURNING | Begin the journey back to the warehouse. |
| T6 | NAVIGATING | Critical Battery | RETURNING | Stop the current delivery journey and return to the warehouse. |
| T7 | AVOIDING_OBSTACLE | Critical Battery | RETURNING | Stop obstacle handling as safely as possible and return to the warehouse. |
| T8 | RETURNING | Warehouse Reached | IDLE | Become ready for another delivery request. |

## Transition Matrix

A dash (—) means that no valid transition is defined for that event in the state.

| Current state \ Event | Delivery Request | Obstacle Detected | Obstacle Avoided | Destination Reached | Delivery Successful | Critical Battery | Warehouse Reached |
|---|---|---|---|---|---|---|---|
| IDLE | NAVIGATING | — | — | — | — | — | — |
| NAVIGATING | — | AVOIDING_OBSTACLE | — | DELIVERING | — | RETURNING | — |
| AVOIDING_OBSTACLE | — | — | NAVIGATING | — | — | RETURNING | — |
| DELIVERING | — | — | — | — | RETURNING | — | — |
| RETURNING | — | — | — | — | — | — | IDLE |

## Prohibited Direct Transitions

| Invalid transition | Why it is invalid |
|---|---|
| IDLE → DELIVERING | Violates R2 and R3: a delivery request and navigation to the destination are required first. |
| AVOIDING_OBSTACLE → DELIVERING | Violates R7: the robot must finish obstacle handling and return to NAVIGATING before delivery can begin. |
| IDLE → RETURNING | No delivery journey or critical-battery navigation event has started. |
| DELIVERING → IDLE | The robot must return to the warehouse after successful delivery. |

## Assumptions

- The robot keeps the assigned destination while it moves between NAVIGATING and AVOIDING_OBSTACLE.
- Critical battery has priority over normal navigation and delivery progress.
- A destination-reached event is accepted only from NAVIGATING.

## Verification Activity

### Check 1 — Invalid Transition: IDLE → DELIVERING

**Result: Rejected.** This transition is not present in the valid-transition table. Allowing it would violate **R2** and **R3**, because the robot would deliver without first receiving a delivery request and navigating to the destination. The expected path is:


a\
IDLE → NAVIGATING → DELIVERING

### Check 2 — Missing Transition: AVOIDING_OBSTACLE → NAVIGATING

**Result: Defect identified if missing.** Without the **Obstacle Avoided** transition, the robot remains in **AVOIDING_OBSTACLE** and cannot continue toward the destination. It cannot legitimately reach the delivery process because **R6** requires the robot to return to **NAVIGATING** after the obstacle is cleared. The table includes T3 to prevent this deadlock.

### Check 3 — Obstacle During Delivery

**Result: Direct transition rejected.** The table does not allow **AVOIDING_OBSTACLE → DELIVERING**. The robot must first complete obstacle handling, return to **NAVIGATING**, and then reach the destination before **DELIVERING** is allowed. This enforces **R7** and prevents delivery from starting while obstacle handling is active.

## Verification Summary

| Verification check | Expected outcome | Status |
|---|---|---|
| IDLE → DELIVERING | Must be rejected | PASS |
| AVOIDING_OBSTACLE without return transition | Must expose a navigation deadlock | PASS — T3 is present |
| AVOIDING_OBSTACLE → DELIVERING | Must be rejected | PASS |

The transition table is consistent with the ten requirements and explicitly records the important invalid behaviors.
