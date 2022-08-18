# Intent Specification

In this document, we list all intents, verdict expression, and verdict evaluation for the RASPBERI-SI test cases in one place.


## Arm Movement Intents

### Intent Element 1. Location Accuracy

* *Description*: Lander arm successfully navigates to a target location (specified in the mission specification) with an appropriate pose for sample collection.

* *Verdict Expression*: Using the `final_pose` data reported via `geometry msgs/Pose` to calculate the euclidean distance `d_loc` from the target pose location specified in the mission specification.

$$d_{loc} = \sqrt { ( {x_{final-pose} - x_{target-pose}} )^2 + ( {y_{final-pose} - y_{target-pose}})^2 + ( {z_{final-pose} - z_{target-pose}} )^2}$$

* *Verdict Evaluation*: `PASS` if $d_{loc} < 2$, `DEGRADED` if  $2 < d_{pose} < 10$, otherwise `FAIL`.

### Intent Element 2. Pose Accuracy

* *Verdict Expression*: Using the `final_quat` data reported via `geometry msgs/Pose` to calculate the euclidean distance d from the target `target_quat` location for sample collection at a sample collection point.

$d_{pose} = d_{angular}(final-quat, target-quat)$

* *Verdict Evaluation*: `PASS` if $d_{pose} < 2$, `DEGRADED` if  $2 < d_{pose} < 10$, otherwise `FAIL`.

### Intent Element 3. Force Accuracy

*Verdict Expression*: Using the `max_force` data reported via `TASK_PSP` to calculate the difference between force generated for excavating a particular location compared with the optimal threshold calculated based on math and the arm dynamics and physics.

`$d_{force} = (force-generated - force-optimal) / force-optimal$`

*Verdict Evaluation*: `PASS` if $d_{force} < 0.2$, `DEGRADED` if $0.8 > d_{force} >= 0.2$, otherwise `FAIL`.

### Intent Element 4. Force Accuracy

*Verdict Expression*: Using the `max_force` data reported via `TASK_PSP` to calculate the difference between force generated for excavating a particular location compared with the optimal threshold calculated based on math and the arm dynamics and physics.

`$d_{force} = (force-generated - force-optimal) / force-optimal$`

*Verdict Evaluation*: `PASS` if $d_{force} < 0.2$, `DEGRADED` if $0.8 > d_{force} >= 0.2$, otherwise `FAIL`.