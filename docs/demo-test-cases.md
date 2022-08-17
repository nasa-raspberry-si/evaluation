1. No fault, No autonomy: Reach an excavation location to excavate.
	Mission specification:
		ARM_UNSTOW
		ARM_MOVE_CARTESIAN
		ARM_MOVE_CARTESIAN_GUARDED
		ARM_MOVE_CARTESIAN_GUARDED (retracting=True)
	Expected behavior: The arm should successfully reach the location defined in the mission specification	


2. Fault, No autonomy:
	Mission specification:
		ARM_UNSTOW
		ARM_MOVE_CARTESIAN
		ARM_MOVE_CARTESIAN_GUARDED
		ARM_MOVE_CARTESIAN_GUARDED (retracting=True)
	Fault injection: A fault (ARM_GOAL_ERROR) is injected randomly while the performing the mission through axclient
	Expected behavior: Mission should be a failure, the arm should not continue with the rest of the actions the moment when a fault is injected.	

3. Fault, Autonomy (Design time solution)
	Mission specification:
		ARM_UNSTOW
		ARM_MOVE_CARTESIAN
		ARM_MOVE_CARTESIAN_GUARDED
		ARM_MOVE_CARTESIAN_GUARDED (retracting=True)
	Fault injection: A fault (ARM_GOAL_ERROR) is injected randomly while the performing the mission through axclient
	Expected behavior: Mission should be a success. After a fault is injected the autonomy should detect the fault and replan from the design time fix (cleaning the injected fault) and finish the mission.	
	
4. Fault, Autonomy (Runtime solution with PRISM)
	Mission specification:
		ARM_UNSTOW
		ARM_MOVE_CARTESIAN
		ARM_MOVE_CARTESIAN_GUARDED
		ARM_MOVE_CARTESIAN_GUARDED (retracting=True)
	Fault injection: A fault (ARM_GOAL_ERROR) is injected randomly while the performing the mission through axclient
	Expected behavior: Mission should be a success. After a fault is injected the autonomy should detect the fault and stop the arm, then it queries the PRISM planner and the new plan is generated at runtime and finally executes the new plan.
	
-------- Work in Progress -------
 5.  Behavior fault, Autonomy (Runtime-> finding the correct “z” value and “force threshold”)
	Mission specification:
		ARM_UNSTOW
		ARM_MOVE_CARTESIAN (intentionally setting a wrong "z" value that will trigger a fault, surface not reachable)
		ARM_MOVE_CARTESIAN_GUARDED
		ARM_MOVE_CARTESIAN_GUARDED (retracting=True)
	Fault injection: A intentionally setting a wrong "z" value in the mission description that will trigger a fault, surface not reachable
	Expected behavior: Mission should be a success. After a fault is injected the autonomy should detect the fault and stop the arm, then it queries the PRISM 	planner and the new plan is generated at runtime and finally executes the new plan till it finds the correct "z" value or "force_threshold"