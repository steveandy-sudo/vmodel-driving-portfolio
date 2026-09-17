# Experiment and outcome record

## Confirmed outcome

The project author confirmed the linked `main` branch as the final version and clarified that the project failed: the final code did not lead to an actual vehicle run. Stable autonomous driving was not achieved. The March–June 2026 period describes the Dream Semester project; its end date does not indicate a successful driving demonstration.

## Development record

| Stage | Work recorded | Outcome scope |
| --- | --- | --- |
| ROS 2 / CARLA | Decision logic, waypoint tracking, Pure Pursuit and longitudinal control integration | Closed-loop simulation work exposed unstable tracking |
| Tracking investigation | Steering direction, lookahead, smoothing, rate limits and command bounds | Adjustments were investigated; a single root cause and successful final correction were not established |
| Later MCU adaptation | Desired speed/steering interfaces, serial command formatting and mock bringup | Source implementation exists; it did not lead to an actual vehicle run |

The later bridge and launch files should not be interpreted as a successful hardware test. Existing source comments describing deployment intent or expected behavior are implementation notes, not measured outcomes.

## Lessons for subsequent experiments

- Record target coordinates, steering sign and output units alongside the observed trajectory.
- Keep the input sequence and operating condition fixed when comparing lookahead, smoothing or rate-limit settings.
- Track perception message timing and state transitions together with control outputs.
- Evaluate each interface and the closed-loop behavior separately, then record which version was used in the integrated test.

These are lessons and proposed improvements to the experimental process. No new success rate, tracking-error measurement, or root-cause diagnosis is added here.

## Evidence to add if available

A CARLA recording or plot showing the unstable tracking would help explain the debugging work. Useful accompanying details are the route, command/trajectory traces, parameter values and observed change. The repository currently presents code and the author's outcome record without a placeholder for an unavailable success video.
