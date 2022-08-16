# Example Challenge Problem Description

The document defines an example challenge problem description, following the structure of the [challenge problem template](./cp_template.md).


## Overview

This challenge problem requires an autonomous robotic platform navigate through an environment in order to deliver essential supplies to a defined target location. On the way the platform may experience degradation to one or more of its primary navigational aiding sensors, denoted SensorA and SensorB. By continuously monitoring the state of all sensors the system must realize when readings from one or more of its sensors threaten its ability to localize within its environment. Under these conditions, the system will attempt to use the data from complementary sensors to re-calibrate the primary aiding sensor(s).

## Test Data

None

## Test Parameters

| Name           | Value                        | Description  |
| ----------- | ---------------- | ----------------------------------- | 
| StartLocation | Integer (x,y) where x and y are defined for range(0, 99) | Start location on a quantized Cartesian grid with 15 cm spacing. |
| TargetLocation | Integer (x,y) where x, y are defined for range(0, 99) | Target location on a quantized Cartesian grid with 15 cm spacing. |
| SensorNoiseA | Float range(0, 0.58) | Additive noise to navigation SensorA. |
| SensorBiasA | Float range(-0.02, 0.3) | Bias for navigation SensorA. |
| SensorNoiseB | Float range(0, 0.58) | Additive noise to navigation SensorB. |
| SensorBiasB | Float range(-0.02, 0.3) | Bias for navigation SensorB. |

## Test Procedure

1. Initialize system with mission start and target locations.
2. At any time during a test degrade one or more sensors by applying added noise and/or sensor bias.

The platform should sense when one or more sensors are in a degraded state and adapt at runtime to assure localization in support of the mission related navigation challenge. 

*Note that there are many more details to define in this section supportive of the integration of the platform with the test harness. This definition is kept as short as possible to provide a simple example.*

## Intent Specification and Evaluation Metrics

**Intent Element 1. Accuracy**

*Description*: Robot successfully navigates to the target location.

*Verdict Expression*: Using the last position reported from the robotic platform $(x_{robot}, y_{robot})$ to calculate the distance d from the target location.

\begin{equation*}
\label{eq:distance}
d = \sqrt{ ( {x_{robot}-x_{target}} )^2 + ( {y_{robot}-y_{target}} )^2 } 
\end{equation*}

PASS if d < 25 cm, DEGRADED if d < 35 cm, otherwise FAIL.