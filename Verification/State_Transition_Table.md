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
