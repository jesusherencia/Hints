# Create application from scratch

In development world two possibilites exists :
- Application maintenance
- Application creation

Application Maintenance is kind of hard cause we are dealing with technical debt from so called legacy applications.
But when the possibility of developing an app from scratch emerges we will try to develop it with all best practices that we know.

Let's talk about the second option.

So business analysts think that a new app should be created, they start writing specifications.
Normally developers are contacted in order to seek advice or to give technical information about the existing architecture.

Not bad, but, shouldn't they participate in specification also ?
But they talk different languages, business analysts talk about business needs and developers talk about technical implementations.

So an intermediary language is necessary. For that, Behavior Driven Development seems to be a good option.
The intermediary language is Gherkin language. It allows us to give a structure for specifications and define differente scenarios for each specification.

If we start writing specifications in Gherkin means that business needs are mostly clear.
Assuming specifications are not clear enough. Maybe a better understanding of business is required ? An opportunity to use Domain Driven Design (Strategic Design) ?
By better understanding business means that business processes are clear enough. If not DDD and BPM are helpful options.

Let's talk about Strategic Design in DDD, one helpful concept in Strategic Design is Bounded Context.
Bounded Context helps dividing large models into coherent ones, each one containing elements that are strong coherent around a business concept.

We can complement DDD by using BPM in the identification of processes. We could find a correspondance between process and boundend context.

Assuming business needs are clear, let's get back to that intermediary language between business analysts and developers, Gherkin.


