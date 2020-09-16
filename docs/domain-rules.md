# ItC: Domain Rules
Here we'll try to capture the essential business rules (or domain
rules as I'll call them).  These are basically detailed requirements
which pertain to the domain of the project.  These rules might very
well be checked at runtime (or at compile time if possible) because a
violation of a rule could lead to a fatality.

For example there's a rule that if one direction (primary or
secondary) is NOT red then the other direction SHALL be red.

Another rule might be related to the SCM (Static Cycle Mode) of the
intersection, and would consist of all the independently defined times
(PGT, PYT, PRDT, PWT, PWWT, SGT, SYT, SRDT), and all the times which
must be derived from the independent times (PRT, SRT, etc).  There are
many constraints on these times, and we'll discuss them and refine
them as our understanding increases.  For example, PGT cannot be
negative, and in fact must not be less than some minimum positive
value like 15 s.

Domain rules exist in a specification document, but they also exist in
the code you flash to the micro.  So they are part of the software,
and the principles of software engineering apply to them.  For
example:


- low coupling and high cohesion: the business rules shall be isolated
  from both interface details, and control details.  A change in a
  rule shall require a change to a single file. (and possibly a recompile)
- DRY: a business rule (in the code) shall be expressed in exactly one place.
