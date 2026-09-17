# Collection validation

This record covers offline source and documentation checks. The project outcome is documented in [EXPERIMENTS.md](EXPERIMENTS.md).

## Completed checks

| Check | Result |
| --- | --- |
| Original source files | All 69 sizes, Git blob IDs and SHA-256 hashes match the confirmed source commit |
| Python syntax | 42 files parsed successfully |
| ROS package XML | Four manifests parsed successfully |
| JSON | One configuration file parsed successfully |
| YAML | MCU bridge configuration parsed; dataset configuration failed parsing as described below |
| New documentation links | 39 local file and directory targets verified |

`perception/lane_detection/configs/dataset.yaml` includes shell heredoc wrapper lines (`cat > ...` and `EOF`) inside the tracked file. It is therefore not valid standalone YAML. The original bytes are preserved, and the issue is recorded rather than counted as a passing check. PyYAML 6.0.3 was used for this inspection.

Original executable modes are carried into Git. Whitespace checks apply to the newly authored documents; existing source formatting is retained.

## Runtime scope

The collection environment is Windows with Python 3.12. ROS 2 builds, CARLA execution, inference, serial connections, and physical vehicle tests were not run during collection. Existing ROS ament lint tests are preserved and are not reported as passing runtime tests.

Syntax parsing does not resolve imports, verify model assets, validate a ROS graph, or establish control performance. Known environment/source limitations are listed in [PORTFOLIO_SETUP.md](PORTFOLIO_SETUP.md).
