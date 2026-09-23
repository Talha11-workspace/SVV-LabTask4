# SV&V Lab Task 4 — Autonomous Delivery Robot Requirements

## Task 1: Functional and Behavioral Requirements

| Req. ID | Requirement |
|---|---|
| R1 | When the robot is switched on, the system shall place it in the **IDLE** state. |
| R2 | While in **IDLE**, the robot shall wait for a valid delivery request and shall not begin delivery or navigation without one. |
| R3 | When a delivery request is received in **IDLE**, the system shall assign the destination and transition the robot to **NAVIGATING**. |
| R4 | While **NAVIGATING**, the robot shall continuously monitor its surroundings for obstacles and its battery level. |
| R5 | When an obstacle is detected during **NAVIGATING**, the system shall pause normal navigation and transition the robot to **AVOIDING_OBSTACLE**. |
| R6 | When the obstacle has been successfully avoided, the system shall transition the robot from **AVOIDING_OBSTACLE** back to **NAVIGATING** and resume travel toward the assigned destination. |
| R7 | The system shall transition to **DELIVERING** only when the robot has reached the assigned destination from **NAVIGATING** and is not actively handling an obstacle; **IDLE** and **AVOIDING_OBSTACLE** shall not transition directly to **DELIVERING**. |
| R8 | When the delivery process completes successfully, the system shall transition the robot from **DELIVERING** to **RETURNING**. |
| R9 | If the battery becomes critically low during **NAVIGATING** or **AVOIDING_OBSTACLE**, the robot shall stop the current delivery journey and transition to **RETURNING**. |
| R10 | When the robot reaches the warehouse while **RETURNING**, the system shall transition it to **IDLE** so it can wait for the next delivery request. |

## Testability Notes

- Each requirement identifies an observable state, event, condition, or prohibited transition.
- The prohibited transitions in R2 and R7 are safety and behavioral constraints that must be checked during verification.
