# SV&V Lab Task 4 — Delivery Robot State and Event Model

## Task 2: System States

| State ID | State | Description |
|---|---|---|
| S1 | **IDLE** | The robot is at the warehouse and waits for a delivery request. |
| S2 | **NAVIGATING** | The robot travels toward the assigned delivery destination while monitoring its environment and battery. |
| S3 | **AVOIDING_OBSTACLE** | The robot has paused normal navigation and is handling an obstacle in its path. |
| S4 | **DELIVERING** | The robot is performing the package delivery at the destination. |
| S5 | **RETURNING** | The robot is travelling back to the warehouse after delivery or because of critically low battery. |

## Task 3: Events and Conditions

| Event/Condition ID | Event or condition | Meaning |
|---|---|---|
| E1 | **Robot Switched On** | The system starts and initializes the robot in IDLE. |
| E2 | **Delivery Request Received** | A valid request containing a destination is received while the robot is IDLE. |
| E3 | **Obstacle Detected** | The navigation sensor detects an obstacle on the current route. |
| E4 | **Obstacle Avoided** | The obstacle has been safely cleared and navigation can resume. |
| E5 | **Destination Reached** | The robot arrives at the assigned delivery destination. |
| E6 | **Delivery Successful** | The package has been delivered successfully. |
| E7 | **Critical Battery** | The battery level reaches the defined critical threshold during navigation. |
| E8 | **Warehouse Reached** | The robot arrives back at the warehouse. |

## Model Constraints

1. **IDLE** must not transition directly to **DELIVERING**.
2. **AVOIDING_OBSTACLE** must not transition directly to **DELIVERING**; obstacle handling must finish first.
3. A robot with a critical battery condition must return to the warehouse rather than continue the current delivery journey.
