# Distributed Shared Memory

## DSM

_Distributed Shared Memory_ is a paradigm for shared memory on a multicomputer.
DSM involves two components:

- _Shared memory_: each node has the space for all of shared memory locally.
- _Replication and consistency_: memory objects (usually pages) may or may not be stored locally.
  If the memory object is stored on another node, there must be _transparent remote access_ to it.

A naive implementation of DSM uses a central manager to control replication and consistency of memory objects.

Distributed Shared Memory: paradigm for shared memory on a multicomputer
Each node contains all pages in shared address space
Pages may or may not be contained locally on the node
Replication and consistency is managed by DSM
Can improve performance issue through replication



Benefits and drawbacks of DSM:

| Benefits | Drawbacks |
| :--------: | :-----------: |
|            |               |

Advantages of DSM:
Easy to program with and port existing code into
Pointer handling doesn’t change
No marshalling: page isn’t packed when sent through the network
Disadvantages of DSM:
False sharing - different data requested on the same page results in lots of overhead for no benefit
Page size - smaller means less false sharing but more overhead
Everything is treated like local memory - don’t know how expensive a memory lookup will be

Implementation can be on hardware, OS with hardware support, OS and virtual memory or middleware
Usually implemented in user space
Requires fault handling, VM page mapping and protection and a message passing layer

DSM models:
Shared page - prone to false sharing
Shared region - prevents false sharing but loses access transparency
Shared variable - complex for programmer, but more fine grained
Shared structure - better for consistency, but not shared memory model
Tuple Space: read and write data with tuples (tag, data in the same message)
Writers and readers act on a single tuple space → easy access of memory
Can abstract data into objects and match objects by tag
Underlying middleware is responsible for tuple consistency and updating
Does DSM hold up to the requirements of a distributed system?
Good transparency - communication, location, migration, replication and concurrency is abstracted away → i.e. is consistent
Somewhat reliable, depending on the availability of data
Good performance as there is comparatively lower overhead
Geographically scalable (used in WANs) and computationally scalable
TreadMarks: shared page DSM library
Lots of issues with its design
Used lazy release consistency
Similar to what we implement in Assignment 1, with multiple writers
Lazy release consistency: when a node writes to a page, a twin is created and written to. When a node reads the page, a diff of call the twins are collected and turned into the new page
Allowed for multiple writers
Slowed system down, as there would be a lot of read requests → implemented lazy diffs (only make the diff when requested)
Good at reducing false sharing
