# Distributed File Systems

## Introduction

- Idea is that you have a bunch of files that can be accessed by multiple computers
- Can communicate and coordinate with a DFS paradigm
- Shared data is persistent --> communication is persistent
- General model --> clients and servers
- Challenges - transparency, flexibility, dependability (consistency), performance and scalability
  - When things break, it's not pretty - things can lock and you need a locking service

### Client's Perspective: File Services

- Ideally, client sees remote and local files as the same
- Standard interface is a file - uninterpreted sequence of bytes
- _upload/download model_: download the file, make changes, then upload it to the server
- _remote access model_: read/write request over the network
- _Unix semantics_: too strong because we need caching for performance and unix semantics requires central control of the data
- _Session semantics_: run into problems with concurrent writes
- _Immutable semantics_: run into problems with concurrent creates
- _Atomic semantics_: expensive to implement

### Server's Perspective: Implementation

- Need to first decide on what semantics you need to use
  - Need to figure out what your system is going to be used for
- Lots of different reasons a DFS is used today
- _Stateless server_: does not have open/close calls - client needs to do this
  - No need to keep track of open files via a table
  - Client has to provide all the context again - filename, offset, etc. --> leads to bigger messages, external servers to deal with locking, stateful clients
- _Stateful server_: better performance because it knows the context, but file locking is possible

### Caching and Replication

- Can cache in server memory, client disk or main memory of the client
- Consistency problem with caching, but there are standard ways to deal with it
- Lose unix semantics if caching is implemented

-_Explicit replication_: client manages files via naming different copies, etc.

- Standard solutions for replication can be implemented

## NFS

- Fits nicely into Unix but does not have Unix semantics
- Creates a problem because people assume its Unix but it doesn't behave like it (e.g. write propagation is slow)
- Stateless (old) and stateful (new)
- Client and server live in the OS

## AFS & Coda

- Intended to be used at uni --> had to scale well
- Wanted to create a global namespace
- Write-on-close semantics for global space, unix semantics for local
- AFS doesn't support replication
- Coda does support replication + offline operations
- Don't want to reintegrate immediately - too much to handle
- Conflicts may happen if reintegration is too slow

## GFS

- File system for clusters and large distributed systems
- High latency, but high bandwidth to transfer large amount of data
- Have chunk servers - chunks are 64Mb
- If the client wants to read a file, GFS chunk master gives the chunk handle, client contacts chunk server and sends back chunk
- GFS master is a single point of failure and gets overloaded

- _Cells_: different namespace - different master responsible for each cell

### Chubby

- Locking service implemented as a simple file system/name service