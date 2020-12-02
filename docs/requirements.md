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

## User-facing Requirements

1. The intersection has 2 intersecting roads and supports 1 *or more*
   lanes of vehicular traffic, as well as pedestrian traffic, in all 4
   directions.  <!-- *Also supports left-turn lanes* -->

1. The intersection controller shall have multiple operating modes to
   accomodate different traffic situations, including
   1. **FSM: failsafe mode** (4-way flashing red, at 0.5Hz with a 75%
      duty cycle)
   2. **SCM: static cycle mode** (fixed cycle times, does not respond to
      pedestrians, ERV, or BRT)

1. Rule: If any traffic light is not Red, the cross traffic light
   shall be Red, and its walk light shall be in the DON'T WALK state.
   
1. The intersection controller shall have a mode **ATM: Accelerated
   Test Mode**.  ATM mode allows an observer to visually verify
   correct operation.  It allows the system to be sped up by a given
   factor, for instance 10x, or 100x.  The point is to make
   the sequencing faster so the observer can watch it for seconds
   instead of minutes.
   
1. The software shall support the following commands and behaviour:
   1. **help**: display a list of valid commands
   
   1. **mode fsm**: switch to Failsafe mode
   
   1. **mode scm**: switch to Static Cycle mode
   
   1. **atm x**: enter accelerated test mode with multiplication
      factor *x*.  This multiplies the speed of the intersection by
      *x*, where *x* is between 1 and 100.  An *x* of 1 means run in
      normal time, just like the real intersection.  Values of *x*
      outside the range shall result in an invalid command message.

   1. Any invalid command shall result in a useful error message.
      `Invalid command: ...` where the colon is followed by a verbatim
      copy of the typed invalid command.
   
   1. Blank lines shall be ignored, and the system will emit another
      prompt on the following line.
   
   1. Leading and trailing whitespace shall be ignored
   
   2. The system shall be case-sensitive

1. The sample timing diagram of the Gordon Road/ Lockwood Road should
   be implemented as given.  The document is
   [here](./intersection-timing.pdf)
1. The Walk light shall have three states, with the following indications:
   1. When in Walk mode, the Walk light is on
   1. When in Walk Warning mode, the Walk light blinks at 1 Hz, with a
      50% duty cycle.
   2. When in Don't Walk mode, the Walk light is off.
   
1. When the system starts up it enters FSM.

## Developer Requirements
1. Three tasks should be implemented
   1. **cliTask**
   1. **statusUpdateTask**
   1. **stateControllerTask**
1. Two queues should be implemented
   1. a queue allowing **cliTask** to send commands to
      **stateControllerTask**.
   1. a queue allowing **stateControllerTask** to send state
      information to **statusUpdateTask**.

<!-- ## Use Cases -->

<!-- ### UC1: Handle System Error -->
<!-- If any unrecoverable error occurs, enter failsafe mode.  This mode is -->
<!-- exited by initiating a system reset. -->

<!-- ### UC2: Handle BRT -->
<!-- Triggered by Operator. -->

<!-- 1. A BRT is detected from one of the 4 directions -->
<!-- 2. If through lane is green, extend the green time by `BRT_EGT` -->
<!--    (extend green time) -->
<!--    seconds, otherwise reduce the cross-traffic green time by `BRT_RGT` -->
<!--    (reduce green time). -->


<!--     - Q: treat it as if it were red, and follow the same action.  In -->
<!--       the case of a left-turn-enabled intersection, skip the left-hand -->
<!--       turn and do a multi-directional left turn phase at the end -->
<!--       (after BRT leaves). -->
<!-- 2. Other considerations -->
<!--    1. What if an ERV happens during this use case? (overrides BRT, probably) -->
<!--    2. What if there are two BRT vehicles approaching in the same lane? -->
<!--       In opposite lanes? -->
<!--    3. What if there are two BRT vehicles approaching: one in the -->
<!--       through lane and one in the cross lane? -->
<!-- 1. System logs all vehicle event(s) (event type, time, etc.) -->

<!-- ### UC3: Handle ERV -->
<!-- Triggered by Operator -->

<!-- 1. An ERV is detected from one of the 4 directions -->
<!-- 2. If through lane is green, it shall stay green until ERV has cleared -->
<!--    the intersection, otherwise if cross-traffic light is green, -->
<!--    immediately cycle that light to red, and keep the through light -->
<!--    green until ERV has cleared the intersection.   -->
<!--     - This behaviour holds even if **Handle BRT** is in progress -->


<!-- 3. Other considerations: -->
<!--    2. What if there are two ERV vehicles approaching in the same lane? -->
<!--       In opposite lanes? -->
<!--    3. What if there are two ERV vehicles approaching: one in the -->
<!--       through lane and one in the cross lane? -->
      
<!-- 1. System logs all vehicle event(s) (event type, time, etc.) -->


<!-- ### UC4: Handle Regular Vehicle -->
<!-- Triggered by Operator. -->

<!-- 1. A regular vehicle is detected -->
<!-- 1. If the system is in **On-Demand Secondary mode** and the vehicle is -->
<!--    on the secondary rode, give the vehicle a green cycle.  Otherwise -->
<!--    no special action is needed. -->
<!-- 1. System logs all vehicle event(s) (event type, time, etc.) -->
