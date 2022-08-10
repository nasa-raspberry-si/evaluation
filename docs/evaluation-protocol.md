**Baseline A Stage**

The first stage represents the unadaptive target system operating in an unperturbed ecosystem. The expectation for this stage is that the test result is consistent with intent satisfaction, i.e., the verdict is PASS for all applicable intent elements. If this was not the case, the test must be rejected. It is impossible to know whether the SUT has maintained or recovered the intended functionality of the target system on test inputs, if the target system itself did not provide a clear indication of what its intended behavior actually was. Recurring failures in the Baseline A Stage could indicate that the intent is not accurately encoded in the verdict expression.

*Possible deviation from running Baseline A Stage as part of a testing timeline is if intent satisfaction cannot be falsified in the unperturbed ecosystem.*

**Baseline B Stage**

The second stage represents the unadaptive target system operating in a perturbed ecosystem (the perturbation may have been present at the start of the test or it may be have been applied during test execution). This stage is used to confirm that the perturbation to the ecosystem actually threatens the target system's ability to achieve its intent. The expectation for this stage is that the test result is consistent with a failure to fully satisfy one or more applicable intent elements (a verdict of FAIL or DEGRADED). If not, it is impossible to know whether the SUT has made any difference in the adaptability of the target system if it could have succeeded by simply doing nothing. Note that a perturbation for a particular test may not affect the satisfaction of every intent element. All that is required of this stage is that some reduction in intended functionality was observed as a result of the perturbation. Recurring intent satisfactions in the Baseline B Stage could indicate that the range of perturbations allowed by the challenge problem definition is too narrow.

**Challenge Stage**

The final stage represents an adaptive target system operating in a perturbed ecosystem (the perturbation may have been present at the start of the test or it may be have been applied during test execution). This stage is required to maintain or recover the intended functionality of the target system despite a perturbation to its ecosystem.

# Verdict and Test Outcome

The verdict represents the determination of whether the target system achieves an element of its intent in the context of a specific configuration and execution of the target system. That is, a verdict is calculated for each intent element at the conclusion of each test stage.

While the verdict is expressed in the language of intent achievement, the test outcome is focused on the test execution process itself. As each challenge problem may have more than one intent element which is independently being evaluated as part of the test stages, the test outcome provides a score representing the outcome of evaluating the system across all possible test stages.

The following sections describe the possible verdicts and test outcomes along with a description of each.

## Verdict

**PASS**

The test result showed that the target system (whether enhanced by the DAS or not) successfully achieved an element of its intent when the target system was run in a particular ecosystem (whether perturbed or not). In principle, the unadaptive target system should achieve its intent when run in its unperturbed ecosystem.

**DEGRADED**

The test result showed that the target system (whether enhanced by the DAS or not) maintained or recovered some but not all of an element of its intent. A DEGRADED verdict expresses an impaired but usable level of functionality.

**FAIL**

The test result showed that the target system (whether enhanced by the DAS or not) failed to maintain or recover an element of its intent. A FAIL verdict expresses a failure of the target system to provide an expected effect in its mission context.

**INCONCLUSIVE**

The test result completed, but the information provided by the test is too uncertain to be used as the basis for a PASS, DEGRADED, or FAIL determination for an intent element.

Rules for revising the verdict of other test stages:

  * If the verdict expression for a test stage yields INCONCLUSIVE for an applicable intent element during any test stage, that intent element will be recorded as INCONCLUSIVE in any test outcome with a score of COMPLETE, regardless of the result of any other stage.

  * If the verdict expression for Baseline B Stage does not yield a FAIL or DEGRADED for an applicable intent element, that intent element will be recorded as INCONCLUSIVE in any test outcome with a score of COMPLETE, regardless of the result of any other stage.

**INAPPLICABLE**

The test result is not relevant to an intent element. Tests are required to be relevant to at least one intent element, but not necessarily all.

**ERROR**

The test could not be completed due to some runtime fault in the test framework. A declaration of error may be the result of the SUT directly conveying an error status to the test harness or it might be at the behest of the test harness itself.

The verdict for all intent elements will be recorded as ERROR and the associated test outcome will be recorded as ERROR, regardless of the result of any other stage.

## Test Outcome

**COMPLETE**

The test is considered COMPLETE if all relevant test stages are complete and the verdict calculation for those test stages did not imply a test outcome of INVALID or ERROR.

**INVALID**

The test is considered INVALID if it is not capable of producing information about the ability of the SUT to maintain or recover the intended functionality of the target system. This situation usually arises due to the following reasons.

  * If the Baseline A Stage of a test does not yield a PASS verdict for all applicable intent elements.
  * If the Baseline B Stage of a test does not yield a DEGRADED or FAIL verdict for at least one intent element.

**INCOMPLETE**

The test is considered INCOMPLETE if it began to execute, but failed to finish due to lack of time and/or resources or a stage of a test necessary for computing the fidelity of the SUT is not executed.

**ERROR**

Some condition occurred during test execution that precluded continuation of the test (usually accompanied by the SUT communicating an error status to the test harness), or precluded interpretation of the test results due to missing or malformed data or logs.

If any stage of a test yields an error condition, the test outcome is ERROR. All test results for other stages will be ignored.


# Evaluation Terminology

**Challenge Problem (CP)**

> A mission scenario and associated application software defined by the performer to test the adaptability of their platform.

**System Under Test (SUT)**

> The system that is being evaluated is Autonomy component that are being developed by the RASPBERI-SI team. The Autonomy component enables adaptation to run-time/mission-time uncertainties. e.g., Europa space arm failure. 
This deserves to be stated since many tests of fidelity will actually interact with the target system in either its unadaptive (original) or adaptive version, appearing superficially to be a test of that system. Nevertheless, the goal of all testing will be to gather information about the effectiveness of the Autonomy.

**Target System**

> The [OWLAT](https://www-robotics.jpl.nasa.gov/how-we-do-it/systems/ocean-world-lander-autonomy-testbed-owlat/) and [OceanWATERS](https://github.com/nasa/ow_simulator) testbeds. The integration of the [Autonomy](https://github.com/nasa-raspberry-si) component by RASPBERI-SI will make the Target System Autonomous (more specifically self-adaptive).


**Intent**

> The functions and effects that the target system has been deployed to provide in the context of a defined mission, network, and platform. Some functions and effects of the target system are not intent-related, but are incidental to the engineering choices made in the design, implementation, and configuration of the system. It is recognized that this definition is incomplete, imprecise, and even circular on some interpretation of has been deployed to provide.

**Intent Element**

> A discrete set of target system measurements and associated method for determining the extent to which each system is successfully maintaining intent as defined by each team in regards to each of their challenge problems.

**Test**

> A Test is an instantiation of a Test Case with configuration parameter values. A single Test Case specification may be used to construct many randomized variations of a Test.

**Test Case**

> A Test Case is a parameterized specification for an experiment in which test inputs are provided to the system under test, target system, and/or platform, and the responses observed and measured.

**Test Harness**

> A collection of software components to facilitate the automated running of test cases and collecting results during the evaluation.

**Test Outcome**

> What happened as a result of performing a test. While the verdict is expressed in the language of intent achievement, the test outcome is focused on the test execution process itself. Since tests are composed of multiple stages the completion and result of each stage will be used to determine the overall test outcome. 

**Test Result**

> The expected behavior of the target system when executed in a specified configuration and with a specified interaction with the target system's interface. In many cases, a test result can be obtained by running a single trial. In other cases, the behavior of the target system is probabilistic and the test result is obtained by running many trials and computing a statistic over the set of trial results.

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


