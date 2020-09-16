# Glossary for Intersection Controller
This is a collection of terms and concepts used in this project.  I'm
trying to build up a "Ubiquitous Language" as described by Eric Evans
in his seminal text "Domain-Driven Design", 2004, Addison-Wesley.

The following lists might eventually be replaced by tables

##Concepts:


- Primary Road : the busier road of the two intersecting roads
- Secondary Road : the less busy road of the two intersecting roads
- Priority Vehicle : a vehicle, usually a public-transit vehicle, such
  as a bus which is to be granted priority at the intersection.  Also
  called RBT (rapid bus transit)
- Emergency Vehicle : a vehicle (such as police, fire, or ambulance)
  which is to be granted top priority at the intersection.  Also
  referred to as ERV (emergency response vehicle).
- SCM : Static Cycling Mode : a mode in which the lights in all
  directions cycle through a fixed (static) pattern, regardless of
  other inputs
- Vehicle Detector: VD : detects a vehicle approaching the
  intersection, or stopped waiting at a red light.  There are four VDs
  in an intersection: one for each of the four directions.
- PRIVD : Priority Vehicle detector: detects an RBT approaching the
  intersection and alters the lights to give priority to the vehicle.
  The PRIVD will detect the vehicle much further away than the VD.
- ERVD: Emergency Response Vehicle Detector: detects an ERV approaching the
  intersection and alters the lights to give priority to the vehicle.
  The ERVD will detect the vehicle much further away than the VD.
- Walk Button: the button pressed by a pedestrian wishing to cross the
  street.  An intersection will have four Walk Buttons.
- Failsafe: this mode is entered in case of a system failure.  All
  four directions will have flashing red lights

Time parameters:


- PGT : Primary Green Time: the length of time the primary green light
  is on.
- PYT : Primary Yellow Time: the length of time the primary yellow
  light is on
- PRDT : Primary Red Delay Time: the length of time from the end of
  the Primary red light turning on and the Secondary green light
  turning on.  Also called the "all red" time.  (May not be the best
  name, because it's actually the /Secondary/ red light time which is
  extended by PRDT seconds.)
- IIT: Intersection Initialization Time: after system boot, all lights
  at the intersection remain red for this time.  After the IIT is
  over, the controller moves into one of the available modes.
- PWT : Primary walk time : the length of time the primary walk light
  is on.  During this time, pedestrians should be able to initiate
  a crossing.
- PWWT : Primary Walk Warning Time: the length of time the primary
  walk warning light is on.  The actual indicator can have many forms
  - a flashing DON'T WALK
  - a countdown timer giving the number of seconds remaining before RED
  For our purposes we will flash the blue WALK light on our solderless
  breadboard 
