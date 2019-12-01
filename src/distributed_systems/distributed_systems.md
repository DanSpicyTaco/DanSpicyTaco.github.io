# Distributed Systems

## Distributed Systems - What and Why

**Distributed system: a group of independent computers acting as one interface to its user**

Examples of distributed systems include:

- Systems that provide a single service or task - DNS, Netflix
- Interconnected distributed systems - Internet, IoT

_Advantages_:

- _Cost_: cheaper to horizontally scale (linear) than vertically scale (exponential)
- _Performance_: more absolute performance compared to a single computer
- _Scalability_: naturally grows as the demand grows
- _Reliability_: lots of redundant components, so the impact of faults can be reduced
- _Inherent distribution_: good for applications that are naturally distributed

_Disadvantages_:

- _Network component_: performance limit and unreliable - new points of failure
- _Complexity_: more complex system - greater chance of introducing errors
- _Partial failure_: can’t tell if a computer has crashed - won’t screw up the entire system but makes every node unreliable
- _Security_: more components to secure, therefore easier to compromise


Goals:
1. _Transparency_
2. _Dependability_: availability, reliability and maintenance
3. _Scalability_ 
4. _Performance_
5. _Flexibility_: easy to add or remove components?

