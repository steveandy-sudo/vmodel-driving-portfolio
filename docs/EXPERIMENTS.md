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

## Source-based diagnostic guide

The following separates visible implementation from questions that would need a new, controlled experiment. It does not reconstruct missing historical results.

| Source behavior | Concrete inspection point | What a new experiment would need to resolve |
| --- | --- | --- |
| Pure Pursuit uses target x/y and wheelbase | `compute_steering_angle_rad` in the [steering node](../src/neuro_decision/neuro_decision/steering_command_node.py) | Target frame, sign, and expected turn direction for fixed left/right targets |
| Steering is smoothed and change-limited | EMA and `delta_per_cycle` in `publish_steering_commands` | Raw versus filtered command over time, with the update period held fixed |
| Degrees and normalized commands are both published | `update_degree_output_from_filtered_normalized` | Scale agreement between the producer and each consumer |
| Stale or unknown input states produce zero steering | Timeout and state branches in the steering node | Whether input age or state transitions coincide with tracking interruptions |
| Desired commands are serialized separately | [MCU bridge](../src/vehicle_serial_bridge/vehicle_serial_bridge/mcu_serial_bridge.py) | Requested command versus transmitted packet and measured actuator response |

## Available evidence

The project author confirmed that historical footage and logs are unavailable. The public record therefore consists of the preserved source, the development account, and the outcome above. No trajectory plot, before/after result, or success video is reconstructed. A future reimplementation would be a new experiment and should be labeled with its own date and source version.
