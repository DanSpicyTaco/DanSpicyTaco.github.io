# Distributed File Systems

## Introduction

_DFS_ is a paradigm in distributed systems that outlines how to share files with multiple computers.
The central idea is that files can be accessed by many clients through a file server.
The files on a file server is persistent, so it is possible to have long lasting asynchronous communication between clients.
This is different to DSM or the shared object paradigm, who do not have persistent file storage.
For example, in DSM, files only exist for as long as the master lives.

The basic model in DFS is client-server.
Clients access files and directories, while servers store and provide the files to clients.
Different clients may be served different views of the same file, meaning client state is important in this paradigm.

Challenges and desired properties when implementing DFS includes:

- _Transparency_: different levels of transparency are desired.
  Clients should not be aware of a file's _location_, number of _replicas_ and when it _migrates_.
  Clients should be aware when a file is being accessed _concurrently_ by multiple clients.
- _Flexibility_: servers can be added, removed and replaced with ease.
Furthermore, servers should be able to serve from any underlying OS, such as Windows, OSX or Linux.
- _Reliability_: file servers must deal with _consistency_ issues (replication and concurrent access), _security_ (file access rights) and _fault tolerance_ (crashing servers and availability of files).
- _Performance_: might need to distribute client requests to prevent overloading a single file server.
  Multiple file servers may be required to store a large file system.
- _Scalability_: need to avoid centralised components, including naming service, locking service and file stores.
  Should also deal with geographical and administrative scalability.
  For example, servers should be separated geographically.

### Client's Perspective: File Services

The standard interface for DFS supports a file as an **uninterpreted sequence of bytes**.
Furthermore, the interface stores file metadata - access times, ownership, creation data and permissions.
Clients have four main operations available to them:

- Read (from a file)
- Write (to a file)
- Add (a file)
- Remove (a file)

From the client's perspective, there are two main models a DFS can follow:

- _Upload/download model_: a client downloads the file, makes changes and uploads the file back to the server.
Network traffic is reduced in this model, as the client does not have to send read and write operations to the server.
Furthermore, the client can have remote access to the file when it goes offline.
However, concurrency 

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