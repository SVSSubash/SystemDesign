This part of the document answers questions on what are the steps you should follow to get or generate all the use cases for any design.

How to design a software system?
A. First Steps
    0. Understand the domain.
        eg. Your an engineer building a house, architecuture domain.
    1. Get the problem statement.
        eg. User wants an office building.
    2. Gather the requirements (Requirements gathering)
        a. Can be functional and non functional.
            eg. Office rooms location, conference room location, etc
        b. Can be explicit and implicit. (implicit requirements are requirements that are understood by context and are usually not required to be addressed in a design interview.)
            eg. The entrance to a room etc.
B. Deep Dive
   1. Use Case stateemtnt should answer the question, What do you want to achieve? (This is obtained from the requirements gathering step above. Some achievements to complete are implicit. They should follow CAM principle: Clear=specific, Actionable=doable, Measurable=quantifiable )
   2. Goals and Business Goals should answer the quesiton, Why do you want to achieve this? (Business value motivation)
   3. Goals and Functional Goals should answer the quesiton, why do you want to achieve this? (functionality perspective)
   4. Actors should answer the question, Who is trying to achieve it and who do you interact with to achieve this? (Gets actors)
   5. Main flow should answer the questions, What steps are needed to achieve this? (Gets Main flow)
      There are 2 methodologies to get the steps needed to achieve this.
      1. User journey method: Follow the users path through the system.
      2. Input process output mehtod: Define what goes in, what happens and what comes out.
      Should also answer the question How will you know you succeeded 
      (Gets success criteria, 
      expected result, 
      failure criteria, controlled failure or uncontrolled failure
      not expected result.)
   6. Alternate flows should answer the question, Is there another way of doing the same thing? (Gets Alternate flow)
      Follows same methodology as step 5.
   7. Exceptional flows should answer the question Can something exceptional happen? (Eg. Like an emergency fire)
      Follows same methodology as step 5.