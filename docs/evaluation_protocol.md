# RASPBERRY-SI Test and Evaluation Protocol

version: 1.0


This document explains the test and evaluation protocol `RASPBERRY-SI` partially followed before and during the visit to JPL in August 2022, and will fully follow after the visit for evaluating the Autonomy when interfaced with NASA JPL OWLAT (physical testbed) and NASA Ames OceanWATERS (virtual testbed). The challenges that the `RASPBERRY-SI` team faced for testing and evaluating Autonomy with the physical testbed during the visit to JPL in August 2022 provided the required info for following rigorous integration tests and defining test specifications prior to evaluation.


## Overview

[RASPBERRY-SI](https://nasa-raspberry-si.github.io/raspberry-si/) is a NASA-funded project to develop `Autonomy` for `Ocean World Lander` (target system) that provides the following capabilities:

 * Capability 1 (Root-cause Analysis): Analyze and monitors the target system in operation to detect `root causes` of failures, faults, or any perturbations that affect the target system’s ability to realize its intent. 
 
 * Capability 2 (Runtime Adaptation): Adapts the target system to `maintain or recover` as much of its intent as possible. In particular, the Autonomy component enables adaptation to run-time/mission-time uncertainties. e.g., faults, failures, behavior execution error, degradations, or other unexpected behavior in the Europa lander arm or any other subsystem of the space lander.

 * Capability 3 (Verifiability)[^1]: Autonomy should provide mechanisms to provide system-level verification. For example, queries from the Autonomy evaluator, to verify whether the plan generation was aware of the fault injection parameters before test execution. In particular, the autonomy may provide verifications using a combination of static verification (via code analysis) and online dynamic verification (via runtime introspection). 

[^1]: This capability was not promised in the proposal, but we identified it as a critical capability for autonomy in mission-critical target systems.  


## RASPBERRY-SI's Autonomy

In this section, we only describe the implementation view of the Autonomy component and we leave out the technical details of the methods implemented for each component.

The `RASPBERRY-SI-Autonomy` (`Autonomy` from hereafter) implements a reference framework in autonomous and adaptive systems, called `MAPE-K`, originally developed by IBM and has been extended by several members of `RASPBERRY-SI` team extensively over the past decade. The components of  our Autonomy include: 

* `M: <autonomy-monitor>`: The monitoring component obtains the telemetry data and stores the data in `K: <autonomy-knowledge`>` with the appropriate format.
* `A: <autonomy-analyze>`: The analysis component detects any anomaly in the target system and perturbations in its environment that require adaptation to the target system. The analysis component does so using the live and historical monitoring data.
* `P: <autonomy-plan>`: The planner component synthesizes/generates a plan to adapt the target system to maintain or recover the specified intent.
* `E: <autonomy-execute>`: The executor component translates the adaptation plans, specified in a domain-specific language (`RASPBERRY-SI Autonomy` uses [PRISM](http://www.prismmodelchecker.org/)--probabilistic model-checking toolchain for plan synthesis) to the target system's execution language (`RASPBERRY-SI Autonomy` uses `PLEXIL` for executing all behaviors and adaptations required to be executed on the target system).  
* `K: <autonomy-knowledge>`: The knowledge components stores all data, including the telemetry data, configuration parameters, the specified intent, and all information that is required by the Autonomy components.

## Test Phases

RASPBERRY-SI follows an iterative test, integration, and evaluation strategy to reduce the risks and increase the efficiency in test execution.

In this project, there are three major separate components:
* Testbeds: 
  * [Ocean World Lander Autonomy Testbed (OWLAT)](https://www-robotics.jpl.nasa.gov/how-we-do-it/systems/ocean-world-lander-autonomy-testbed-owlat/):  
    * OWLAT Hardware Platform (`owlat-physical`): The testbed comprises a base, an instrument arm, and perception sensors (camera). It provides the hardware interfaces and low-level software infrastructure to allow various autonomy solutions to command typical lander operations and receive telemetry and performance feedback through a ROS-based software interface in a plug-and-play manner [2]. 
    * OWLAT Control Software Simulation (`owlat-sim`): This software provides a partial simulation of the OWLAT hardware platform via Dynamics and Real-Time Simulation (DARTS). The simulation currently only includes limited aspects of the system. In particular, a subset of the robotic arm commands via ROS actions. 
  * [Ocean Worlds Autonomy Testbed for Exploration Research & Simulation (`OceanWATERS`)](https://github.com/nasa/ow_simulator) [3]: This software-based simulator emulates surface environmental conditions (e.g., lighting and surface material properties), robotic manipulator operation, and high-level lander systems. The simulator provides system introspection capabilities. 

## Test Process

* Unit Testing: Testing individual components inside Autonomy. The usual unit tests for MAPE-K. 

* Integration Testing: Testing interfacing/integration of Autonomy with the Testbeds. 

* Test Case Execution for Evaluation: Evaluating the performance of Autonomy

We use the following tools/data: `rostest`, `unittest` (python), `gtest` (c++), `Logs`, `RQT`, `ROSBag`, `RViz`, `SymPy`

### Unit Testing

* Unit testing of individual components inside Autonomy. The usual unit testing for MAPE-K. 

* Fault Injection unit tests:
  * This stage tests the interfacing of Fault Injection with the testbeds.

### Infrastructure Testing

* Testing the connection between the Autonomy node and the testbed computer.  
* Testing launch files: 
  * launch file for OceanWATERS plan execution (`ow_exec.launch`)
  * launch file for OWLAT plan execution (`owlat_exec.launch`)
  * launch file for Autonomy plan execution (`autonomy_exec.launch`)
* Testing the interface between `autonomy-node` <-> ROS <-> `testbed-node`

We use `rostest` and embed tests within the `roslaunch` files to verify that they are functioning properly. In particular, we use `<test>` tags (specifies test nodes) within the launch file.

### Integration Testing

The integration testing involves testing the integration of `Autonomy` and `OWLAT` (software-hardware interface) and `OceanWATERS` (software-software interface) testbeds. Testing complex distributed systems that involve hardware components requires many considerations as it involves several risk factors: (i) the safety of the robots and humans; (ii) it is a time-consuming task. To test Autonomy on the physical testbed, a physical setup that includes rebuilding every time that we run tests. For example, running each test with the `OWLAT` testbed requires manual tasks, including putting the testbed in a ready/safe state. In addition, when a test involves executing behaviors during which the arm touches the ground, such as scooping or digging, a human operating the physical testbed, requires cleaning the robot arm (with a vacuum cleaner).

Before running the evaluation tests, the integration test stage is to demonstrate that Autonomy and Testbeds can interface and communicate with each other. The integration testing of the Autonomy and the testbeds involves: 

Autonomy and Testbeds should be interfaced via ROS:
  * Autonomy <-> <`ROS`: [node: `ow_exec`, actions: [`oceanwaters_msgs/ACTION_NAME_1`, ..., `oceanwaters_msgs/ACTION_NAME_N`], topic: [`oceanwaters_msgs/TOPIC_NAME_1`, ..., `oceanwaters_msgs/TOPIC_NAME_N`]> <-> Virtual Testbed (`OceanWATERS`)
  * Autonomy <-> <`ROS`: [node: `owlat_exec`, actions: [`owlat_msgs/ACTION_NAME_1`, ..., `owlat_msgs/ACTION_NAME_N`], topic: [`owlat_msgs/TOPIC_NAME_1`, ..., `owlat_msgs/TOPIC_NAME_N`]> <-> Physical Testbed (`owlat-physical`)
  * Autonomy <-> <`ROS`: [node: `owlat_exec`, actions: [`owlat_sim_msgs/ACTION_NAME_1`, ..., `owlat_sim_msgs/ACTION_NAME_N`], topic: [`owlat_sim_msgs/TOPIC_NAME_1`, ..., `owlat_sim_msgs/TOPIC_NAME_N`]> <-> Simulation Testbed (`owlat-sim`)


The integration tests include testing two touch points between Autonomy and the Testbeds: (i) information flow from the testbed to the autonomy via ROS topics provided in the API document; (ii) information flow and the flow of commands from Autonomy to the Testbed via ROS actions provided in the API document:

  * Autonomy <- Testbed: `M: <autonomy-monitor>` <- `telemetry:` [`owlat_sim_msgs.TOPICs`, `owlat_msgs.TOPICs`, `oceanwaters_msgs.TOPICs`] <-> Testbed
  
  * Autonomy -> Testbed: `E: <autonomy-execute>` -> `ros_action:` [`owlat_exec owlat_sim_msgs/ACTIONs`, `owlat_exec owlat_msgs/ACTIONs`, `ow_exec oceanwaters_msgs/ACTIONs`] <-> Testbed

Here, `ow_exec` and `owlat_exec` are ROS nodes that embody `PLEXIL Executive` for executing `PLEXIL plans` as well as adapters, `ow-plexil-adapter` and `owlat-plexil-adapter`, for the virtual and physical testbeds respectively. 



### Functional Testing

The functional testing is involved exercising the execution of all provided testbed behaviors through the end-to-end execution pipelines: 
* Functional testing on virtual testbed: `plexil-plan` <-> `plexil-executive` <-> `ow-plexil-adapter` <-> `ros-action-server` <-> `OceanWATERS`
* Functional testing on physical testbed simulator: `plexil-plan` <-> `plexil-executive` <-> `owlat-plexil-adapter` <-> `ros-action-server` <->`owlat-sim`
* Functional testing on physical testbed: `plexil-plan`<-> `plexil-executive` <-> `owlat-plexil-adapter` <-> `ros-action-server` <->`owlat-physical`


In the functional testing, we test different types of behaviors provided by the testbeds:
* Basic behaviors: `ARMUNSTOW`, `ARMSTOW`, etc.
* Composite behaviors: `TASK_SCOOP`: `ARM_UNSTOW`+ `ARM_MOVE_CARTESIAN` + `TASK_SCOOP`

The testbed provides the API via the testbed's user guide.


## Autonomy Evaluation

### Test Case Design
We evaluate the performance of Autonomy in maintaining the intents by running test cases, specified with the following structure:

- Test Case #1: A name or sentence in English
- A `mission-specification` (`mission-spec.PLEXIL`)
- Intent elements:
    - `intent-element-1`: A `intent-description` in English about the expected behavior from the target system and an `intent-formula` that determines the extent to which the target system maintained the expected behavior, by precisely measuring `deviations from the specified intent` (`intent-element-1-sympy-formula.txt`). 
    - `intent-element-2`: A `description` in English and a `formula` that determines the extent to which the target system is successfully maintaining intent as defined in the description of `intent-element-2` (`intent-element-2-sympy-formula.txt`). 
    - `intent-element-3`: A `description` in English and a `formula` that determines the extent to which the target system is successfully maintaining intent as defined in the description of `intent-element-1` (`intent-element-3-sympy-formula.txt`). 
- Test configuration: `test-config`:[`any-other-test-level-configuration-options`]: The list of configuration parameters as information that may be required to run the test. For example, the time and the frequency of the faults injected into the target system. 

We will use the [autonomy evaluation criteria](./evaluation_criteria.md) and the task-specific evaluation criteria provided by NASA [1] to formulate the intent expression in terms of symbolic formula.

We collect the information about test case execution in the following format.

- `test-id` (`test-UID.yaml`)
  - `test-trial-id:` 
  - `test-case-number:`
  - `test-execution-stage: ["A", "B", "C.A", "C.B", "C.C"]`
  - `mission-id:` 
  - `fault-injection-configuration-id:`
  - `experiment-data:`
    - `ros-bag-id`
    - `any-other-raw-data-collected-at-test-execution-time`
  - `results:`
    - `intent-element-1: <float> [0 - 1]`
    - `intent-element-2: <float> [0 - 1]`
    - `any-other-post-processed-data-item-related-to-performance-of-the-autonomy-during-the-runtime-execution:`

The information stored in the test case execution files is used by evaluation scripts to determine the outcome of the evaluation. In particular, we use the following evaluation procedure for each testbed:

* `OceanWATERS`: the evaluation is done by an automated procedure. The evaluation procedure includes:  
  * `instantiation of the experimental environment:` The Autonomy and a simulator are automatically instantiated and deployed in their deployment nodes. 
  * `perturbations:` The fault injection component is instantiated and faults are injected using an automated procedure using configuration files and scripts. 
  * `score:` the human evaluator runs scripts to calculate `verdict-expression` over the `trials`.


* `owlat-sim`: similar to `OceanWATERS`

* `owlat-physical`: the evaluations are done primarily manually with humans deriving the test execution. The evaluation procedure includes: 
  * `instantiation of the experimental environment (semi-automated):` The Autonomy and the Physical Testbed are instantiated and deployed in their deployment nodes. 
  * `perturbations (manual to have control, but hidden from Autonomy):` The fault injection component is instantiated and faults are injected with a manual or automated procedure using configuration files and scripts. 
  * `score:` the human evaluator runs scripts to calculate `verdict-expression` over `trials`.



## Test Execution 

We perform the following steps for each test case execution:

* Pre-test execution: We instantiate the executors required for running the test and use `PLEXIL` to initialize the test infrastructure including the testbed, autonomy, fault injection, and other test-related nodes.

* During test execution: We execute a test and collect data via `PLEXIL` that interacts with the target system to collect telemetry data for test evaluation. `telemetry:` [`owlat_sim_msgs.TOPICs`, `owlat_msgs.TOPICs`, `oceanwaters_msgs.TOPICs`]

* Post-test execution: We use `SymPy` for evaluating symbolic formulas (`intent-formula`, `verdict-expression`). 

# Test Stages

The high-level timeline consists of sequential stages of tests. A testing stage is an execution of the test using either the unadaptive or adaptive version of the target system running in the context of either the unperturbed (original) or a perturbed ecosystem.

A test comprises some or all of the test stages identified in the following table and described in more detail below.

|                           | **Unadaptive Target System** | **Adaptive Target System** |
|-------------------------: | :--------------------------- | :------------------------- |
| **Unperturbed Ecosystem** | Baseline A Stage             | N/A                        |
| **Perturbed Ecosystem**   | Baseline B Stage             | Challenge Stage   

**Baseline A Stage**

The first stage represents the unadaptive target system operating in an unperturbed ecosystem. The expectation for this stage is that the test result is consistent with intent satisfaction, i.e., the verdict is `PASS` for all applicable intent elements. If this was not the case, the test must be rejected. It is impossible to know whether Autonomy has maintained or recovered the intended functionality of the target system on test inputs if the target system itself did not provide a clear indication of what its intended behavior actually was. If there is no interfacing issue, recurring failures in the Baseline A Stage could indicate that the intent is not accurately encoded in the verdict expression.

*Possible deviation from running Baseline A Stage as part of a testing timeline is if intent satisfaction cannot be falsified in the unperturbed ecosystem.*

**Baseline B Stage**

The second stage represents the unadaptive target system operating in a perturbed ecosystem (the perturbation may have been present at the start of the test or it may have been applied during test execution). This stage is used to confirm that the perturbation to the ecosystem threatens the target system's ability to achieve its intent. The expectation for this stage is that the test result is consistent with a failure to fully satisfy one or more applicable intent elements (a verdict of `FAIL` or `DEGRADED`). If not, it is impossible to know whether Autonomy has made any difference in the adaptability of the target system if it could have succeeded by simply doing nothing. Note that a perturbation for a particular test may not affect the satisfaction of every intent element. 

*All that is required of this stage is that some reduction in intended functionality was observed as a result of the perturbation. Recurring intent satisfactions in the Baseline B Stage could indicate that the range of perturbations allowed by the challenge problem definition is too narrow.*


**Challenge Stage**

The final stage represents an adaptive target system operating in a perturbed ecosystem (the perturbation may have been present at the start of the test or it may have been applied during test execution). This stage is required to maintain or recover the intended functionality of the target system despite a perturbation to its ecosystem.

* Stage C.A (Challenge Stage - Design time): In this stage, the Autonomy is very basic, meaning that the possible faults that the system is going to be faced are fixed and predefined. The autonomy also has access to a set of fixes for the predefined list of faults, so whenever a fault appears, it is going to select the appropriate fix which was provided at design time.

* Stage C.B (Challenge Stage - Runtime): A synthesized adaptation plan starts a PLEXIL plan, randomly injects the fault, the autonomy detects the fault, and the prism synthesizes an optimal plan at runtime concerning objectives of the mission (energy consumption, time)  that is going to fix the fault, translate the prism plan to PLEXIL plan and execute the PLEXIL plan.

* Stage C.C (Challenge Stage - Causality + Runtime): synthesized adaptation plan by querying the causal model
start a PLEXIL plan, randomly inject the fault, the autonomy detects the fault, the prism synthesizes an optimal plan at runtime concerning objectives of the mission (energy consumption, time)  that is going to fix the fault, translate the prism plan to PLEXIL plan and execute the PLEXIL plan.

*It is expected that the intended functionality or a degraded functionality is preserved. The efficiency of planning at these different stages is different. For the same fault, the time that it takes to generate a plan is expected to _be ordered as `C.A < C.C < C.b`.*


## Verdict and Test Outcome

The verdict represents the determination of whether the target system achieves an element of its intent in the context of a specific configuration and execution of the target system. That is, a verdict is calculated for each intent element after each test stage.

While the verdict is expressed in the language of intent achievement, the test outcome is focused on the test execution process itself. As each challenge problem may have more than one intent element which is independently being evaluated as part of the test stages, the test outcome provides a score representing the outcome of evaluating the system across all possible test stages.

The following sections describe the possible verdicts and test outcomes along with a description of each.

## Verdict

**PASS**

The test result showed that the target system (whether enhanced by the Autonomy or not) successfully achieved an element of its intent when the target system was run in a particular ecosystem (whether perturbed or not). In principle, the unadaptive target system should achieve its intent when run in its unperturbed ecosystem.

**DEGRADED**

The test result showed that the target system (whether enhanced by the Autonomy or not) maintained or recovered some but not all of an element of its intent. A `DEGRADED` verdict expresses an impaired but usable level of functionality.

**FAIL**

The test result showed that the target system (whether enhanced by the Autonomy or not) failed to maintain or recover an element of its intent. A `FAIL` verdict expresses a failure of the target system to provide an expected effect in its mission context.

**INCONCLUSIVE**

The test result was completed, but the information provided by the test is too uncertain to be used as the basis for a `PASS`, `DEGRADED`, or `FAIL` determination for an intent element.

Rules for revising the verdict of other test stages:

  * If the verdict expression for a test stage yields `INCONCLUSIVE` for an applicable intent element during any test stage, that intent element will be recorded as `INCONCLUSIVE` in any test outcome with a score of `COMPLETE`, regardless of the result of any other stage.

  * If the verdict expression for Baseline B Stage does not yield a `FAIL` or `DEGRADED` for an applicable intent element, that intent element will be recorded as `INCONCLUSIVE` in any test outcome with a score of `COMPLETE`, regardless of the result of any other stage.

**INAPPLICABLE**

The test result is not relevant to an intent element. Tests are required to be relevant to at least one intent element, but not necessarily all.

**ERROR**

The test could not be completed due to some runtime fault in the test framework. A declaration of error may be the result of the SUT directly conveying an error status to the test harness or it might be at the behest of the test harness itself.

The verdict for all intent elements will be recorded as ERROR and the associated test outcome will be recorded as ERROR, regardless of the result of any other stage.

## Test Outcome

**COMPLETE**

The test is considered `COMPLETE` if all relevant test stages are complete and the verdict calculation for those test stages did not imply a test outcome of `INVALID` or `ERROR`.

**INVALID**

The test is considered `INVALID` if it is not capable of producing information about the ability of the SUT to maintain or recover the intended functionality of the target system. This situation usually arises due to the following reasons.

  * If the Baseline A Stage of a test does not yield a `PASS` verdict for all applicable intent elements.

  * If the Baseline B Stage of a test does not yield a `DEGRADED` or `FAIL` verdict for at least one intent element.

**INCOMPLETE**

The test is considered INCOMPLETE if it began to execute, but failed to finish due to lack of time and/or resources or a stage of a test necessary for computing the fidelity of the SUT is not executed.

**ERROR**

Some conditions occurred during test execution that precluded continuation of the test (usually accompanied by the Autonomy communicating an error status to the test harness), or precluded interpretation of the test results due to missing or malformed data or logs.

If any stage of a test yields an error condition, the test outcome is `ERROR`. All test results for other stages will be ignored.

For more specific examples, please refer to the [demo test cases](./demo_test_cases.md) that involve test specifications for each test stage.

# References

[1] Mike Dalal, Hari Nayar, 'Evaluation Criteria for Ocean World Lander Autonomy,' (Oct 11, 2021).

[2] NASA JPL, Ocean World Lander Autonomy Testbed (OWLAT), 
[URL](https://www-robotics.jpl.nasa.gov/how-we-do-it/systems/ocean-world-lander-autonomy-testbed-owlat/) 

[3] NASA Ames, Ocean Worlds Autonomy Testbed for Exploration Research & Simulation (OceanWATERS), [URL](https://github.com/nasa/ow_simulator). 

[4] [RASPBERRY-SI project website](https://nasa-raspberry-si.github.io/raspberry-si/)

# Appendix 1: Terminology

**Challenge Problem (CP)**

> A mission scenario and associated application software defined by the performer to test the adaptability of their platform.

**Autonomy**

> The system that is being evaluated is the Autonomy component.  
This deserves to be stated since many tests of fidelity will actually interact with the testbeds in either unadaptive (testbed without using autonomy) or adaptive (testbed + autonomy), appearing superficially to be a test of that system. Nevertheless, the goal of all testing will be to gather information about the effectiveness of the Autonomy.

**Target System (Testbed)**

> [OWLAT](https://www-robotics.jpl.nasa.gov/how-we-do-it/systems/ocean-world-lander-autonomy-testbed-owlat/) 
> [OceanWATERS](https://github.com/nasa/ow_simulator)

**Ecosystem: Autonomy + Testbed + Environment**

> We call the target system operating in an uncertain environment with the Autonomy providing the capability to adapt the system. 

**Intent**

> The functions and effects that the target system has been deployed to provide in the context of a specified mission. Some functions and effects of the target system are not intent-related, but are incidental to the engineering choices made in the design, implementation, and configuration of the system. 

**Intent Element**

> A discrete set of target system measurements and associated method for determining the extent to which the system is successfully maintaining intent as defined in the challenge problems. 

**Test**

> A Test is an instantiation of a Test Case with configuration parameter values. A single Test Case specification may be used to construct many randomized variations of a Test. In other words, a test includes several configuraton parameters and each is assiciated with a range (the variability space for the test execution). The test harness automatically assigns value to the parameters and automatically runs instantiated test cases and collect the experimental results.

**Test Case**

> A Test Case is a parameterized specification for an experiment in which test inputs are provided to the Autonomy, the target system, and/or fault injection, and the responses observed and measured. 

This includes the mission specification, and test configuration, including the frequency, type, and time info for the automated fault injection. 

**Test Harness**

> A collection of software components to facilitate the automated running of test cases and collecting results during the evaluation. 
For evaluation of Autonomy with the physical testbed, human must be in the loop for the safety purposes. 

**Test Outcome**

> What happened as a result of performing a test. While the verdict is expressed in the language of intent achievement, the test outcome is focused on the test execution process itself. Since tests are composed of multiple stages the completion and result of each stage will be used to determine the overall test outcome. 

**Test Result**

> The expected behavior of the target system when executed in a specified configuration and with a specified interaction with the target system's interface. Since the Autonomy may not be able to maintaion or recover from a perturbation, we assume that the behavior of the autonomous system is probabilistic and the test result is obtained by running many trials and computing a statistic over the set of trial results.

**Test Stage**

> A test stage is an execution of the test using either the unadaptive or adaptive version of the target system running in the context of either the unperturbed or a perturbed ecosystem.

**Trial**

> An individual run of a test. Each trial of a test uses the same configuration, test parameters, and interaction with the target system.

**Trial Result**

> The observable behavior of the target system when executed in a specified configuration and with a specified interaction with target system's interface.

**Verdict**

> A determination of whether the target system achieves an element of its intent in the context of a specific configuration and execution of the target system.

**Verdict Expression**

> A procedure, expression, or program that maps the test result of one or more stages to a verdict for an intent element. While the test result describes what happened in a stage, the verdict expression assigns a meaning with respect to intent satisfaction. A verdict expression may involve a comparison with the test result from prior stages.