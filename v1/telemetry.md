## Telemetry

The word ‘Telemetry’ is derived from its Greek etymological roots, **tele** - remote and
**metron** - measure. In today’s world, *Telemetry* is a term used for technologies that
automatically record and measure statistical data from real-world use and forward it to systems for further analysis and study.

Usage is captured in form of events. ‘Events’ are broad, human-readable actions. Events
are used to categorize telemetry data. They are the basic unit for analytics and help
identify user navigation or flow.

> ### The concept of telemetry events is to identify: Who did what, on what, and where, using what, in relation to what?

QuML implementations should capture all user interactions (explicit and implicit),
generate telemetry events and send them to the QuML repository. Analysis of telemetry
offers insights into behaviour and usage patterns, and thereby drive decisions,
recommendations, and outcomes.

![Telemtry conceptual diagram](https://github.com/sunbird-specs/inQuiry/blob/master/v1/images/Telemetry_concept.png)

```
Figure: Telemetry generation and processing
```

QuML recommends usage of Sunbird platform’s (​http://www.sunbird.org/​) telemetry
specification as the standard for telemetry events. QuML implementations should
generate the telemetry events defined in the sunbird telemetry specification during the
usage of questions and question sets.

For example, during a question session, the student goes through a series of steps. Each
step produces one or more telemetry events. The journey starts with the student
attempting a question. Student provides a response to the question, response is
evaluated and student sees the result. Other alternate paths during the flow include
when the student's response is incorrect and student sees the solution. Similarly the
student may abandon the journey at any point (end the question session). Telemetry
generated during this journey and the metrics computed using those events are
indicated in figure below.

![Telemtry sample flow of events](https://github.com/sunbird-specs/inQuiry/blob/master/v1/images/Telemetry_sample_flow.png)

```
Figure: Sample telemetry events and metrics in a question attempt
```

An individual event in isolation is of little value. However, when we see across a series
of events that represent a user journey, the insights become more powerful. Processing
of the telemetry events over a journey provides insights and equip systems using the
QuML questions to provide a wide range of other services in addition to question
storage and delivery:

#### ➢ Track hard spots for a student and enable systems to provide personalised learning

#### ➢ Provide feedback to question authors on how to improve or change the question

#### ➢ Identify concepts that need focus at an individual, class or a school level

#### ➢ Set usage metadata for questions and question sets

#### ➢ Update other metadata of questions and question sets based on usage
