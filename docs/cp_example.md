# Example Challenge Problem Description

The document section defines an example challenge problem description, following the structure of the challenge problem template, which is used to illustrate the application of the evaluation criteria.


## Overview

This challenge problem requires the lander arm to collect samples from a location and deliver the sample to a defined target location. On the way, the target system may experience degradation to one or more of its primary move aiding sensors, denoted SensorA and SensorB. By continuously monitoring the state of all sensors the system must realize when readings from one or more of its sensors threaten its ability to localize within its environment. Under these conditions, the system will attempt to use the data from complementary sensors to re-calibrate the primary aiding sensor(s).

## Test Data

None

## Test Parameters

| Name           | Value                        | Description  |
| ----------- | ---------------- | ----------------------------------- | 
| StartLocation | Float (x,y,z) where x, y, z are defined for range(0, 99) | Start location on a quantized Cartesian grid with 5 cm spacing. |
| TargetLocation | Float (x,y,z) where x, y, z are defined for range(0, 99) | Target location on a quantized Cartesian grid with 5 cm spacing. |
| ForceNoiseA | Float range(0, 0.58) | Additive noise to lander arm force sensor. |
| ForceBiasA | Float range(-0.02, 0.3) | Bias for lander arm force sensor. |
| TorqueNoiseB | Float range(0, 0.58) | Additive noise to lander arm torque sensor. |
| TorqueBiasB | Float range(-0.02, 0.3) | Bias for lander arm torque sensor. |

## Test Procedure

1. Initialize the lander testbed with mission start and target locations.
2. Initialize the Autonomy
3. Initialize the Fault Injection
4. At any time during a test degrade the lander arm force and torque sensors by applying added noise and/or sensor bias.
5. Tear down the infrastructure

The platform should sense when one or more sensors are in a degraded state and adapt at runtime to assure localization in support of the specified mission. 

*Note that there are many more details to define in this section supportive of the integration of the platform with the test harness. This definition is kept as short as possible to provide a simple example.*

## Intent Specification and Evaluation Metrics

