---
documentclass: article
classoption: 
title: Lessons Learned in Integrating Autonomy with a Physical Testbed
institute: UofSC, NASA
date: \today
colorlinks: true
linkcolor: blue
citecolor: green
urlcolor: cyan

...

\tableofcontents

\clearpage

# Overview


# Methodology

We have followed the following steps


# Lessons Learned from Integration Tests

# Lessons Learned from Functional Tests

# Lessons Learned from Infrastructure Tests



# Recommnedations to Autonomy Providers



# Recommendations to Testbed Providers


Technology that NASA could use

- Results
- Compared with the state of the art

Process and lessons learned.
- How things could have been done better
- What recommendation to the teams and the NASA participants
- What could have been done to make it more productive


Audience:
Jeremy
Larry
Ryan 
Carolyn

JPL
Issa
Pat 
Program office people 


Emulate for Venus mission


The gap with those systems in simulation

Keeping general 
Applicable across Mission
Resource-constrained

Autonomy testbed and ocean world


Shared repository between JPL and Ames

Unified interface

Evolve over time

Diverge 

Testbeds are different, and differences between the two.

Tests in the wild

Philosophical 

Owlet-sim is meant not to be used for the performer.


COVID had a significant impact

Two levels of adaptations: What levels are you adapting to

Send commands at a high level.


Interfacing same 

Survey of 50-60 autonomy testbeds
Generic autonomy testbed

Never works
Something is different from virtual to physical testbeds.

When we use a physical testbed, never use ROS.
Only when integrating with other systems do we use ROS.

When we open source, everything.

Fully that you want to do
The requirement that you want to assess
Use as a goal for what you want to achieve
Monitor how you are progressing
Making progress toward what you want to achieve

Change the plan in the middle of the work.
A lot of there it did not seems and making crisp progress towards

Too ambitious and baseline thing that is not too hard, and 50% of those \

Unknow things that come along

The minimum that should be something easy to do and stick with do
Backup and you know you are going to do it, and something that you say
Risky stuff
Betting every 

Having a real environment for testing

Testing as early as possible and as often as possible is very beneficial.

Risk reduction stages via owlet-sim 

It takes a lot of time to get things to work.


Test reduction tests

Plan execution work

Target location to narrow down some scenarios

OCEAN world is great


Mike:

More meetings of these
16 teams 
RASPBERRY-SI involvement, GitHub issues, 
Testbed providers 

Unification Autonomy 

Hiro Onno

Involvement of program managers, Carolyn

Chuck fry
Jeremy Frank

Specific goals, 


Safety of the testbed



Autonomy should provide mechanisms to provide system-level verification. For example, queries from the Autonomy evaluator, to verify whether the plan generation was aware of the fault injection parameters before test execution. In particular, the autonomy may provide verifications using a combination of static verification (via code analysis) and online dynamic verification (via runtime introspection). 


Safety of test with the physical testbed


Git process for the dev, test, evaluation



Scenarios that involve more interesting disturbances

The surface looks different after failing on initial commands and partially. 



Baseline 

Physical perturbation in testing physical testbed



Enough understanding about the testbed and behaviors.

Defining the adaptation space

Phase 0: Behavior testing, and checking the behavior and the data from telemetry and validating the data, and the semantics of each telemetry

The interface and capability of the testbeds will evolve, and the autonomy needs to be designed at the same time.
The different capabilities between sim and physical testbed.
Rigorous testing remotely and in interaction with testbed providers.
The interaction would be beneficial for testbed providers and is suitable for testbed providers debugging their testbed system.

Phase 1: Integration testing with reactive autonomy

Phase 2: Integration testing with autonomy that generates a plan at runtime based on runtime info

Phase 3: Integration with Autonomy framework

OWLAT-sim was very useful

Science meeting with Jianhai and Abir

Known Unknowns

Imaging and optimizing the imaging

What happened to the sample collected
What instrument to go to

Mass spectrum


Testbed develop more behavior that hopes lander is operating
The target body we are going to be using
Decadal survey rethink the whole process
Rethinking more testbed wide variety of Ocean Worlds


A demo that involves interaction with the testbed, and random perturbation of the environment and the system

Injecting behavioral faults, so we use the inputs for the commands that we know it is going to trigger a fault.


The framework for autonomy should be all docker based, no killing python script, etc. 
The framework should have mechanisms to kill the connection between Autonomy <-> Testbed.


Miscalibration issue: sending the arm to a location, but it goes to a slightly different place. 

We added a 1 cm offset for the safety

Extending the interface Testbed provides

E.g., we wanted to have the ability to auto-calibrate sensors. 





-------

Having a telemetry dashboard and visualization on the Autonomy machine to diagnose issues with the autonomy and testbed. 


The spikes in force threshold


A sequence of actions in PLEXIL could not raise a fault when the threshold was set to 5 pounds, while the ARM_MOV_CRTESIAN command on testbed NUC raised the fault. 


PLEXIL feature: PLEXIL-ADAPTER -> the list of the issued command after adapting them for the target system

We are running ROS on your side, and we are running ROS on our side, Erica said.

The input for the command specified in the PLEXIL plans sometimes is received with different input on the testbed side; this could be due to PLEXIL adapter, ROS bug in reordering messages, or network issues we they operate on two other compute nodes connected via a VPN connection. 

Losing the connection between Autonomy and Testbed machines
Heartbeat message
Automated recovery of the connection

Resource business


Curiosity

SUTL: Super tactical planning process for 


In some rooms in the lab, we could not go inside because the person sitting there was involved in flight missions 
Whenever we wanted to go somewhere, we had to be escorted, including to the restroom!

Incomplete information


Test case


--------
