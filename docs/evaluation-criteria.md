# Evaluation Criteria

Evaluation criteria are used to assess how well the Autonomy achieves the Autonomous Robotics Research for Ocean Worlds (ARROW) program's goals: 
```"The goal of the ARROW program is to develop autonomy software technologies to significantly increase the robustness and productivity of future Ocean Worlds lander missions including those to never-before-visited destinations with surface conditions and phenomena that may be largely, or completely, unknown a priori at the time of landing."```


The robotic exploration of Ocean Worlds needs to address many challenges:

* Extremely low temperatures, as well as high-radiation levels, pose significant problems for reliable and long-duration mission operations. 

* Lengthy communication delays, limited bandwidth, and the difficulty of maintaining communications links prevent the use of pervasive ground control supervision from Earth. 

* Although future orbital missions, such as Europa Clipper, may provide high-resolution remote sensing, data and knowledge of Ocean World surface environments (including topography, surface composition, etc) are extremely limited or unknown. 

* Spacecraft power will be restricted to on-board systems (batteries, etc.), which limits avionics performance and places a premium on efficient use and management of power.

In particular, the Autonomy for the Ocean Worlds should enable continued operation without ground control intervention in the presence of uncertainties such as:

- Unpredictable sub-system faults 
- Time-varying degradations 
- Unpredictable failures


The evaluation criteria must assess the ability to 

* Capability 1 (Runtime Detection of Faults and Root-cause Analysis): Analyze and monitors the target system in operation to detect `root causes` of failures, faults, or any perturbations that affect the target system’s ability to realize its intent. 
 
* Capability 2 (Runtime Adaptation): Adapts the target system to `maintain or recover` as much of its intent as possible. In particular, the Autonomy component enables adaptation to run-time/mission-time uncertainties. e.g., faults, failures, behavior execution error, degradations, or other unexpected behavior in the Europa lander arm or any other subsystem of the space lander. 

The evaluation criteria also aim to assess how efficiently the Autonomy provides each of these capabilities concerning the available resources and constraints in the mission scenario of the challenge problem.

The evaluation criteria described in this document are relevant to the `RASPBERRY-SI Autonomy`. However, the specified criteria may be relevant to other teams in the `ARROW/CLDTECH` programs.

## Runtime Detection of Faults and Root-cause Analysis

The Autonomy monitors the runtime environment of the target system and triggers an adaptation when the ecosystem is perturbed to the point it affects the target system's ability to satisfy its intent. 

The evaluation challenge will be to assess the detection and analysis:

  * Sensitivity: What changes trigger a response? How fast is the response? Is response proportionate with the magnitude of the change?

  * Stability: Do identical changes produce identical responses? Do convergent sequences of changes produce divergent responses? Do multiple concurrent target systems result in divergent responses?

  * Performance Overhead: How does runtime monitoring affect system performance?

Clearly, an adaptive system has benefits for improved resilience, but it is important to measure both the benefits and the costs to know whether those costs exceed the benefits offered. One possible cost is the time and resources required to provide the adaptive capability, including the overhead cost of runtime detection and analysis.

There should be a balance between the sensitivity of Autonomy to the need for adaptation and the cost of performing an adaptation. If adaptations are very costly, they shouldn't happen very often. These considerations will be assessed qualitatively, although the measurements of sensitivity, stability, and performance will be captured quantitatively.

### Sensitivity

The Autonomy must be able to detect faults/failures and determine whether these faults threaten the intent of the target system. Sensitivity aims to understand the degree to which changes to the environment elicit the need to adapt and if so measure the reaction time and degree of response of the system.

**Assessment**

Sensitivity will be assessed qualitatively, although the measurements of sensitivity, e.g., degree of environmental uncertainties and detection and analysis time, will be measured quantitatively. Sensitivity analysis will likely require the construction of specific test cases which control the selection of and the degree to which input parameters of an Autonomy are perturbed. 

**Example Discussion**

In the context of the example challenge problem, an analysis of sensitivity would require the construction of tests cases that not only isolate perturbations to a subset of the test parameters (e.g., Sensory noise and/or bias) which degrade the sensor capability but also prescribe a strategy for systematically choosing values increasingly further from baseline.

### Stability

Stability attempts to assess the types of adaptation plan choices generated by the Autonomy when presented with the same or similar ecosystem conditions. 

**Assessment**

Stability will be assessed qualitatively, although the measurements of stability will be measured quantitatively. The stability of the Autonomy can be measured by identifying sequences of test cases that can be chained together. Some simple approaches could include (where E# and T# represent a specific state of the environment and target system, respectively):

  * Short cycle: Perturb E0/T0 to E1/T1, then back to E0/T0’, Compare T0 and T0’

  * Multi-path: Perturb E0/T0 → E1a/T1a → E2/T2a, then E0/T0 → E1b/T1b → E2/T2b, Compare T2a and T2b.

Ideally, there will exist a way to quantitatively measure the similarity of the adapted target system. Instead of this, a comparison of the degree to which intent is preserved with each adaptation may apply.

**Example Discussion**

In the context of the example challenge problem, an analysis of stability would require the construction of test cases that present the consistency of input states representing the types of cycles described above. The implementation of this could represent the test interaction of a single test or multiple tests where the starting points of future tests are defined by the final adapted state of the system from a previous test.

### Performance Overhead

Performance overhead attempts to assess the non-intent related performance impact associated with the runtime monitoring, detection and analysis components of the Autonomy.

**Assessment**

Performance overhead will be assessed qualitatively, although the measurements of system resource usage will be measured quantitatively. The Autonomy should provide queryable performance monitors, provide periodic performance logging, or both.

**Example Discussion**

The qualitative assessment of non-intent-related performance impact (e.g. memory) will take into account knowledge of the underlying processing platforms within a given domain in addition to the descriptions provided in the challenge problem descriptions. For example, an Autonomy that utilizes many gigabytes of system memory or takes a long time to generate an adaptation would likely not be an ideal fit for the Ocean Worlds missions.

## Runtime Adaptation

The goal of adaptation is to maintain or recover the intent of the target system, in the face of environmental changes. 

The evaluation challenge will be to assess the adaptation:

  * Fidelity: How much of the target system intent is retained after the adaptation? How much of the target system intent that is lost by a change is recovered by the adaptation?

  * Target System Performance: What is the change in system resource usage after an adaption, relative to the original system usage? Relative to the degraded system usage?

  * Autonomy Performance: What system resources are consumed during adaptation?

The final two items in the list above are related to the costs of performing an adaptation. As with the costs of runtime adaptation, if these costs are excessive relative to the frequency of adaptation, the Autonomy may render the system unusable. Performance of both the target system and the Autonomy will be measured quantitatively, but interpreted qualitatively, in conjunction with dialogue with each testbed provider (JPL/Ames teams).

### Fidelity

Fidelity is a measure of how well the adaptive version of the target system maintains or recovers the intent of the original system despite a perturbation to its environmental resources. Fidelity is evaluated separately for each intent element. There is no generic overall measure of fidelity that incorporates multiple intent elements, although individual challenge problems could define one if applicable.

The desired end state of a test is a verdict for each intent element that indicates whether the adaptive version of the target system successfully maintained or recovered its intent despite a perturbation that would have disrupted the original target system. Several conditions may be encountered that prevent this end state from being realized for one or more intent elements, that prevent the test from being completed. For example, if any stage of a test yields an error condition, the test outcome is ERROR.

*See the Verdict and Test Outcome section of this document to understand how tests are scored.*

**Assessment**

The Autonomy's effectiveness is demonstrated by executing tests. Intent satisfaction is assessed by evaluating a verdict expression defined for each intent element over the test result. The following equation is used to calculate the percentage of tests that demonstrate the ability of the adaptive system to maintain the intent concerning each defined intent element.

The fidelity of the SUT concerning test outcome $t$ and intent element $e$ is defined to be:


$$ fidelity (e) = \frac
{| \{ t\ \colon\ t\ \textnormal{is COMPLETE} \cap verdict (e, t) \in \{ \textnormal{PASS}, \textnormal{DEGRADED} \} \} |}
{| \{ t\ \colon t\ \textnormal{is COMPLETE} \cap verdict (e, t) \in \{ \textnormal{PASS}, \textnormal{DEGRADED}, \textnormal{FAIL} \} \} |}$$

Note that this definition excludes from the assessment any tests with an outcome of ERROR, INVALID, or INCOMPLETE, and for intent element $e$, any tests that result in a verdict of INAPPLICABLE or INCONCLUSIVE.

**Example Discussion**

For discussion, assume that we ran a total of 10 tests for the example challenge problem. Test results for each are represented in the following table.

| Test Outcome | Challenge Stage Intent Element 1 Verdict |
| ----------- | ------------------- |
| ERROR             | ERROR                             |
| INVALID           | INCONCLUSIVE               |
| INCOMPLETE    | PASS                               |
| INCOMPLETE    | FAIL                               |
| COMPLETE       | PASS                               |
| COMPLETE       | PASS                               |
| COMPLETE       | PASS                               |
| COMPLETE       | DEGRADED                     |
| COMPLETE       | FAIL                               |
| COMPLETE       | FAIL                               |


$$Fidelity (e_{1}) = \frac{4}{6} = 0.66$$

Based on the observed tests the platform was capable of satisfying the mission requirement of navigating to the target location approximately 2/3rds of the time. For challenge problems with more than one intent element, this metric can be used to analyze potential trade-offs the system is making amongst the set.

### Target System Performance

Target system performance focuses on measuring the non-intent related resource usage of the adapted target system, relative to the original system usage. It is expected that for some systems, an adaptation of the target system can impact the required resource utilization such as CPU and memory.

**Assessment**

Performance of the target system will be measured quantitatively, but interpreted qualitatively, in conjunction with dialogue with the testbed providers (JPL/Ames teams).

**Example Discussion**

The example challenge problem described is focused on tuning sensor parameters, which, based on the description is unlikely to drastically require more (or less) resource utilization as the system adapts to its ecosystem. However, other systems may trade off system resources to adapt to the degradation or loss of platform functionality. For example, a system capable of emulating legacy architectures may be more resilient to changes in the underlying hardware. However, software emulation is likely to increase the need for both system memory and CPU cycles. 

### Autonomy Performance

Autonomy performance relates to the measured amount of system resources and time needed to complete an adaptation. The adaptation process may take a considerable amount of system resources to perform. The resources consumed and the time to complete will constrain the types of operational environments for which an approach is useful.

**Assessment**

Performance of Autonomy will be measured quantitatively, but interpreted qualitatively, in conjunction with dialogue with the testbed providers (JPL/Ames teams).

*This evaluation criterion is not applicable for approaches that perform adaptation offline or for adaptations that are essentially continuously performed and have no discrete start and end.*

**Example Discussion**

Though not explicitly stated in the test procedure of the example challenge problem there should be a means to measure the time necessary to adapt at runtime to unexpected runtime uncertainties in the environment in which the target system is deployed or the system itself. For this discussion, let's define adaptation time as the elapsed time between the time a fault is detected by the Autonomy and the time a new plan was generated and submitted to the action server.

Autonomy solutions that employ complex search at runtime would be ideal candidates for measuring additional resource utilization related to these processes.