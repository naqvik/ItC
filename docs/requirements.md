# ItC: Actors, Requirements, and Use Cases

## Actors
1. **Operator**: The person operating the console, this actor
   - manually initiates vehicle arrival
   - manually initiates other system events
   - configures the operating parameters of each mode
   - can set the system to any of its operating modes
   - can view the logs
1. **Vehicle**: a vehicle represents an actor because it's in
   principle an independent entity that gains something in its
   interaction with the system: it can proceed safely through the
   intersection towards its destination.  However, in our system,
   vehicle arrival is (probably) always triggered by the Operator, so
   I'm not sure of its status.
1. **QA analyst** Runs the system, and runs tests, verifies the system
   meets its requirements.
1. **Timed events?** Another possible part of the system could
   generate a stream of events.  We might consider this an Actor, if
   it's outside the system we're designing.

## Requirements
The _italicized_ items are there for completeness, but are beyond the
project scope.


1. Each Intersection is controlled by a separate intersection
   controller, that can be configured manually from the attached
   console *or remotely via the network*.

1. The intersection has 2 intersecting roads and supports 1 *or more*
   lanes of vehicular traffic, as well as pedestrian traffic, in all 4
   directions.  *Also supports left-turn lanes*

1. General detection of vehicles arriving and waiting at the intersection is
   supported.  (This is normally done with passive induction loops
   buried beneath the pavement.)

1. The intersection controller shall have multiple operating modes to
   accomodate different traffic situations, including
   1. **failsafe** mode (4-way flashing red)
   2. **static cycle mode** (fixed cycle times, responds to pedestrians,
      ERV, and BRT)
   3. **On-Demand Secondary mode** (secondary road only cycles when a
      vehicle or pedestrian is detected)
   4. **BRT mode** (handles a Bus Rapid Transit vehicle)
   5. **ERV mode** (handles an Emergency Response vehicle)
   6. *Adaptive mode responds to changes in traffic statitics by
      altering timings*
1. Known events include

    - Regular vehicle detected, with extensions 
        - BRT detected 
        - ERV detected

    - Pedestrian detected
    - any more?  time-based events?
    
1. Rule: If any traffic light is not red, the cross traffic light
   shall be red, and its walk light will be in DON'T WALK state.
1. Rule: If any traffic light cannot communicate with the ItC, the ItC
   goes into failsafe mode.
1. Accelerated test mode, for visually verifying correct operation,
   the system can be sped up by 10x, or 100x
2. Collapsed-time mode.  The operation of the system is reduced to a
   series of events, with no time lag.  This might be used for
   verifying correct state transitions, avoiding any time lag.

## Use Cases
### UC1: Handle System Error
If any unrecoverable error occurs, enter failsafe mode.  This mode is
exited by initiating a system reset.

### UC2: Handle BRT
Triggered by Operator.

1. A BRT is detected from one of the 4 directions
2. If through lane is green, extend the green time by `BRT_EGT`
   (extend green time)
   seconds, otherwise reduce the cross-traffic green time by `BRT_RGT`
   (reduce green time).


    - Q: treat it as if it were red, and follow the same action.  In
      the case of a left-turn-enabled intersection, skip the left-hand
      turn and do a multi-directional left turn phase at the end
      (after BRT leaves).
2. Other considerations
   1. What if an ERV happens during this use case? (overrides BRT, probably)
   2. What if there are two BRT vehicles approaching in the same lane?
      In opposite lanes?
   3. What if there are two BRT vehicles approaching: one in the
      through lane and one in the cross lane?
1. System logs all vehicle event(s) (event type, time, etc.)

### UC3: Handle ERV
Triggered by Operator

1. An ERV is detected from one of the 4 directions
2. If through lane is green, it shall stay green until ERV has cleared
   the intersection, otherwise if cross-traffic light is green,
   immediately cycle that light to red, and keep the through light
   green until ERV has cleared the intersection.  
    - This behaviour holds even if **Handle BRT** is in progress


3. Other considerations:
   2. What if there are two ERV vehicles approaching in the same lane?
      In opposite lanes?
   3. What if there are two ERV vehicles approaching: one in the
      through lane and one in the cross lane?
      
1. System logs all vehicle event(s) (event type, time, etc.)


### UC4: Handle Regular Vehicle
Triggered by Operator.

1. A regular vehicle is detected
1. If the system is in **On-Demand Secondary mode** and the vehicle is
   on the secondary rode, give the vehicle a green cycle.  Otherwise
   no special action is needed.
1. System logs all vehicle event(s) (event type, time, etc.)
