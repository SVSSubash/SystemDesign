This design template ensures that all design decisions are traceable back to real user needs and system behaviors

# Use Case Design Template

## Core Use Case Definition
- **Use case name**
- **Actors**
- **Goal**
- **Boundaries**

## Functional Requirements
*(Covers system architecture level, code level design, and UI Level)*

### **Data Plane Operations** *(Core business value delivery)*
**Definition**: Operations that directly deliver the primary business value **for the specific actor**
**Test**: If this fails, **this actor** cannot accomplish their main goal
- **Success scenarios**
  - Expected results/Not Expected results
- **Alternative flows**
  - Expected results/Not Expected results
- **Error Handling**
- **Pre conditions and post conditions**

### **Control Plane Operations** *(Enabling and managing the core value)*
**Definition**: Operations that provision, configure, monitor, or coordinate data plane operations **for the specific actor**
**Test**: If this fails, it impacts operations but doesn't directly block **this actor's** core business value
- **Success scenarios**
  - Expected results/Not Expected results
- **Alternative flows**
  - Expected results/Not Expected results
- **Error Handling**
- **Pre conditions and post conditions**

## Database Design
- **Data modeling**
    - Conceptual - Business point of view (entities, relationships)
    - Logical - Database engineer point of view (schema design, normalization, constraints)
    - Physical - Implementation point of view (specific technology, storage, indexes, query optimization)
- **Data migration/evolution**

## Non-Functional Requirements

### Code Level
- **Code Quality**
- **Maintainability**
- **Testability**
- **Design patterns**

### Architecture Level
- **Performance requirements**
- **Scalability Constraints**
- **Reliability**
- **Operability**
- **Integration Points**
- **Availability/Uptime**

### Security
- **Authentication**
- **Authorization**
- **Data Privacy**
- **Encryption requirements**
- **Audit logging**

### Network
- **Network failure Path**
- **Retry logic**
- **Bandwidth Considerations**
- **Latency requirements**
- **Protocol specifications**

### User Experience (UX/UI)
- **Usability requirements**
- **Accessibility standards**
- **Response time expectations**
- **Mobile/responsive design**

## Deployment Design Level
- **Deployment Environment Setup**
- **Configuration Management**
- **Monitoring**
- **CI/CD pipeline requirements**
- **Rollback strategies**

## Design Constraints

### Technical
- **Platform limitations**
- **Integration requirements**
- **Technology stack constraints**
- **Legacy system dependencies**

### Business
- **Compliance**
- **Budget**
- **Timeline**
- **Resource constraints**

### Regulatory
- **Data governance**
- **Privacy laws**
- **Industry standards**
- **Audit requirements** 

## Quality Assurance
- **Testing strategy**
- **Acceptance criteria**
- **Performance benchmarks**
- **Risk assessment**

This part of the document answers questions on what are the steps you should follow to get or generate all the use cases for any design.

1. What do you want to achieve? (Gets use case) (CAM Clear, Actionable and Measureable)
2. Who is trying to acheive it? (Gets actors)
3. What steps are needed to achieve it? (Gets main flow)
3a. How will you know youve succeeded. (success criteria- direct/ Expected Result - descriptive)
4. What can go wrong? (Gets exceptional flows (failure criteria, abort steps or specific action by context))
   4a. Controlled Failures: (failure criteria + specific corrective action)
       - Business validation failures → Retry with corrected data
       - Permission denied → Request elevated access
       - Resource not found → Create resource or redirect
   
   4b. Uncontrolled Failures: (failure criteria + abort/escalation steps)
       - Network timeouts → Circuit breaker, fallback service
       - 500 server errors → Log, alert, abort with cleanup
       - Resource exhaustion → Scale up, queue request, abort
5. Are there alternative flows to achieve it? (Gets alternate flows)
6. Why do you want to achieve this (Business value motivation)
7. What constraints must you work within. (Boundries)