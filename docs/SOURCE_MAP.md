# Source provenance

## Confirmed final version

- Source: `subin11111/autonomous-driving-platform`.
- Branch: `main`.
- Commit: `c28f7a6d3b95383ff9ee961d12dab2cc9e75ac3e`.
- Commit date: 2026-05-25.
- The project author confirmed this as the final version and clarified the unsuccessful project outcome.

## Collection

All **69 tracked files** were read directly from the source Git objects, preserving bytes and executable modes. Original directory structure is retained. The only path change is the root `README.md`, preserved as [README.upstream.md](../README.upstream.md) so the portfolio overview can occupy the root README.

[SOURCE_MANIFEST.csv](SOURCE_MANIFEST.csv) records source and destination paths, source commit, Git blob IDs, modes, file sizes and SHA-256 hashes. New portfolio documents do not change the original runtime behavior.

| Area | Scope |
| --- | --- |
| `src/neuro_decision` | Current behavior/steering nodes and archived CARLA implementations |
| `src/vehicle_serial_bridge` | ROS desired commands to serial/mock output |
| `src/yolopv2_ros` | Perception nodes and launch files |
| `src/autonomous_bringup` | Integrated launch compositions |
| `perception/lane_detection` | Separate training/inference scripts and dataset configuration |
| `scripts`, original `docs` | Topic checks and MCU connection documentation |

The source snapshot contains no model weights, dataset images or input videos. Original metadata and attribution are retained. License declarations are mixed or incomplete across package files, and this collection introduces no blanket license or change to third-party licensing.
