Actors - TypesOfActors
1. User By Action Performed
2. User By Role
3. System
4. Primary and Secondary Actor.

**Data Plane Operations** *(Core business value delivery)*
**Definition**: Operations that directly deliver the primary business value **for the specific actor**
**Test**: If this fails, **this actor** cannot accomplish their main goal

**Control Plane Operations** *(Enabling and managing the core value)*
**Definition**: Operations that provision, configure, monitor, or coordinate data plane operations **for the specific actor**
**Test**: If this fails, it impacts operations but doesn't directly block **this actor's** core business value

Controlled failures Failures that the actor or system can handle/control
UnControlled failures Failures outside the control of the current actor/system