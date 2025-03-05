# Software development

## Create application from scratch

In development world two possibilites exists :
- Application maintenance
- Application creation

Application Maintenance is kind of hard cause we are dealing with technical debt from so called legacy applications.
But when the possibility of developing an app from scratch emerges we will try to develop it with all best practices that we know.

Let's talk about the second option.

So business analysts think that a new app should be created, they start writing requirements. Those include functional and non-functional ones.
Specially for non-functional requirements they will require the guidance of experienced developers or software architects.
After that a software architecture blueprint is obtained.

After requirements and software architecture blueprint are given, they start writing specifications.
Normally developers are contacted in order to seek advice or to give technical information about the existing architecture.

Not bad, but, shouldn't they participate in specification also ?
But they talk different languages, business analysts talk about business needs and developers talk about technical implementations.

So an intermediary language is necessary. For that, Behavior Driven Development seems to be a good option.
The intermediary language is Gherkin language. It allows us to give a structure for specifications and define different scenarios for each specification (feature).
From there an ubiquitous language is obtained as business analysts and developers will use the terms specified in those files.

If we start writing specifications in Gherkin, means that business needs are mostly clear.

Assuming specifications are not clear enough. Maybe a better understanding of business is required ? An opportunity to use Domain Driven Design (Strategic Design) ?
By better understanding business means that business processes are clear enough. If not, DDD and BPM are helpful options.

Let's talk about Strategic Design in DDD, one helpful concept in Strategic Design is Bounded Context.
Bounded Context helps dividing large models into coherent ones, each one containing elements that are strong coherent around a business concept.

We can complement DDD by using BPM in the identification of processes. We could find a correspondance between process and boundend context.

Assuming business needs are clear, let's get back to that intermediary language between business analysts and developers, Gherkin.

Those specifications expressed in gherkin language will turn into acceptance tests and they also can be used for automated testing.

## System design
Process of defining architecture, components, interfaces and other chracteristics of a system to satisfy requirements.
This means transforming requirements into a blueprint or plan that describes how the software wille be structured, implemented and maintained.
This involve two main steps: 
### High-level design
- Design APIs
- Design diagram (blueprint)
- Data flow
- Data model schema
### Low-level design
- Algorithms
- Data structures
- Technology used
