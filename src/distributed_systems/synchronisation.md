# Synchronisation and Communication

## Distributed Algorithms

_Distributed algorithms_ are either required by or used to achieve synchronisation and coordination between nodes.
Distributed algorithms are have either asynchronous or synchronous timing.
Timing is affected by clock drift, communication delays and execution speed, but we can't control these.

Key properties every distributed algorithm should have:

- _Safety_: nothing bad should happen.
  I.e. an algorithm shouldn't have any side effects if it fails.
- _Liveness_: something good will eventually happen.
- _Properties of a distributed system_: want efficiency, high performance, scalable and reliable properties

### Synchronous Distributed Algorithms

_Synchronous distributed algorithms_ have a **bounded time variance** - minimum and maximum time for clock drift, communication delay and execution speed.
Therefore, we can use timeouts to detect failure.
However, because of the maximum time restriction, synchronous DA's are not very scalable.
The benefits and drawbacks of using synchronous distributed systems are:

| Benefits                                              | Drawbacks                                                                      |
| ----------------------------------------------------- | ------------------------------------------------------------------------------ |
| _Algorithmic complexity_: algorithms are less complex | _Thread limit_: can't spawn lots of threads (max. time restriction)            |
|                                                       | _Network restriction_: need to limit network usage (max. time restriction)     |
|                                                       | _Clocks_: drift must be very low or synchronised often (max. time restriction) |
|                                                       | _Communication complexity_: communication is more complex                      |

### Asynchronous Distributed Algorithms

_Asynchronous distributed algorithms_ have an **unbounded time variance** - can't rely on timeouts to detect failure.
The benefits and drawbacks of using asynchronous distributed systems are:

| Benefits                                                                                        | Drawbacks                                                                                     |
| ----------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| _Communication complexity_: communication is less complex                                       | _Thread limit_: can't spawn lots of threads, as there is a maximum execution time to maintain |
| _Synchronous solutions_: finding an asynchronous solutions also provides a synchronous solution | _Algorithmic complexity_: algorithms are more complex and are hard to solve                   |

### Synchronisation and Coordination

_Coordination_: doing the right operations.
Need to agree on what actions will occur by whom.
_Synchronisation_: doing operations at the right time.
Need to have a total order of events, instructions and communication.
Main issues with synchronisation and coordination:

- Time and clocks
- Maintaining a global state
- Concurrency control

## Time and Clocks

Clocks on different computers will come out of sync - hence there is no such thing as absolute time
Astronomical time and international atomic time (IAT) drift eventually
Coordinated universal time (UTC) (based on IAT) tries to calculate the time dynamically - adds leap seconds and broadcasts time over the world
Can have either physical clocks (based on an actual time source) or logical clocks

Physical clocks: Cp(t) - current UTC time on machine p
P must regularly synchronise with UTC as drift occurs
Can either synchronise internally (between computers) or externally (with a time server)
Berkley algorithm: everyone polls a node, and the node says how much to move the clock by (including the leader node itself)
Leaders can change when one goes down
Good for when you want synchronisation and not the actual time
Good for low latency networks
Cristian’s algorithm: clients query a time server (contains UTC time)
Accounts for network delays and interrupt handling
Client slows down or speeds up time until it is synchronised with the time server
Not scalable - relies on a client-server architecture
Network Time Protocol (NTP): uses a hierarchy of time servers
Can have many different ways to synchronise time
E.g. multicast, RPCs, or symmetric communication
Can be employed in WANs because there is no centralisation
