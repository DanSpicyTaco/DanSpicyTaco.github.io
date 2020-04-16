---
categories: [Distributed Systems]
excerpt: "What is the best way to get a bunch of computers to communicate with each other?"
toc: true
header:
  overlay_image: /assets/images/posts/distributed/communication_header.jpg
  caption: "Photo Credit: [technologymoon](https://technologymoon.com/wp-content/uploads/2019/12/Canva-Share-and-send-media-files-between-phones.-Paper-planes-letter-sent-from-phone-to-phone-technology-linking-people-1536x1025.jpg)"
---

Turns out communication in distributed systems is pretty difficult.
Separate computers don't have access to each other's memory; they need to pass messages instead.

> A computer communicating with another through `send()` and `receive()` functions is called _message passing_.

This post will cover the fundamentals of message passing, as well as a couple of common abstractions we can use on top of it.

## What's in a message?

Messages will contain either _data_ or _control_.
Which one you use will depend on the requirements of your system.

In _data-oriented communication_, data is requested or sent in messages.
Most messages we have use data-oriented communication.
A famous example is the _HTTP protocol_, where a client would request a file (the data) at a certain location (e.g. `/index.html`), and the server would respond with the appropriate file (or error).
Data-oriented communication is good when you have files or similar data you can pass around.
However, it isn't much help if you need to find the answer to a calculation, which is why we have control-oriented communication.

In _control-oriented communication_, control directives are sent, rather than data.
Essentially, the sender asks the receiver to compute some function and return the result.
For example, a sender can request `sum(2, 5)` and a receiver should reply with `7`.
The sender has requested the receiver to go away and calculate `sum(2,5)` and return the answer.
Obviously, the sender can calculate this itself, but using control-oriented communication comes in handy when you need to do a lot of calculations as fast as possible.
Instead of doing all the calculations yourself, you can just ask a bunch of computers to go and calculate a part of it, then come back and assemble the answer.
In fact, this is the basis of many real-world divide and conquer solutions, like [this one](https://www.programmableweb.com/api/divide-and-conquer-multiple-sequence-alignment-rpc-api/articles).

## Types of communication

There are many different types of message passing in distributed systems.
Often, we can combine these types to create really efficient ways of communicating.
Let's go through the most common types.

**Synchronous**: The sender assumes the receiver is active, and will block until a reply is received.
The receiver will wait for requests, process them and returns them.
Both parties need to be active and know about each other.

**Asynchronous**: the sender does not block, the receiver does not have to be active.
Suitable when the sender can’t do any other tasks while waiting for the receiver to reply

- _E.g._ transferring money - want to block until you get a confirmation reply
- **Loose temporal coupling** - receiver does not have to be active
- **Tight spatial coupling** - both parties need to know about each other

Can make synchronous communication from asynchronous messages by:

1. Send an asynchronous send
2. Block until a message is received

Can make asynchronous communication from synchronous messages by:

1. Spawning a new thread
2. Sending a synchronous send/receive
3. Returning to the main thread when a message has been received

_Transient communication_: message is lost if the receiver does not accept it immediately

- **Tight temporal coupling**

_Persistent communication_: message is stored in a buffer if the receiver does not accept it immediately

- **Loose temporal coupling**

_Provider-initiated communication_: server sends message when available

- _E.g._ notifications

_Consumer-initiated communication_: client requests for data

- _E.g_. HTTP requests

_Direct-addressing_: message is sent to a specific receiver

- _E.g._ HTTP requests)
- **Tight spatial coupling**

_Indirect-addressing_: message is sent to anyone

- E.g. broadcast
- **Loose spatial coupling**

{% include figure image_path="assets/images/posts/distributed/communication_types.png" alt="Communication Types" caption="Combinations of Communication Types." %}

## Coupling - assessing communication

We usually use _coupling_ as a metric to assess distributed communication.
You can think of coupling as the dependency between a sender and a receiver.
There are different types of coupling:

- _Temporal_: are the messages dependent on time? E.g. does the receiver have to send a message back immediately?
- _Spatial_: do both parties need to be aware of each other's existence?
- _Semantic_: how much structure does the message require to be understood by both parties?
- _Platform_: can we use any platform to pass messages?

# Communication Abstractions

_Communication abstractions_: implementations on top of message passing that make communication easier for the programmer.

## Message-orientated Communication

_Message-orientated communication_: communication that abstracts basic send() and receive().

- Doesn’t abstract the fact the communication is required, just the complexity of it
- Commonly used in middleware

On top of send() and receive(), it provides:

- Persistent communication (only for message queuing services)
- _Marshalling_
- Abstraction layer

_E.g._ Message passing interface (MPI) - transient communication library

- IBM MQSeries - persistent communication library
- Similar to email, but more general

|      Message Queuing Systems      |
| :-------------------------------: |
| ![mqs](img/communication/mqs.png) |

## Request-reply Communication

_Request-reply communication_: control-orientated communication with defined a message format and protocol

- **Tight semantic coupling**

### RPC

_RPC_ - execute a function on a remote node and send the reply back.

- Communication is abstracted from the application - looks like a local call
- Synchronous
- When client calls an RPC, it is passed into a stub, which sends it to the remote node stub, passing it up into the server function
- _Marshalling_ - stubs need to pack/unpack the data & flaten (serialise) pointers
- Server registers their functions to a binding service. When an RPC is called, it does a lookup in the binding service and forwards the function appropriately
- XML-RPC is a common example - encodes RPC calls in XML so it can be transferred over HTTP

|                RPC                |
| :-------------------------------: |
| ![rpc](img/communication/rpc.png) |

RPC stub for client:

```erlang
% Client code using RPC stub
client (Server) ->
    register(server, Server),
    Result = inc (10),
    io:format ("Result: ~w~n", [Result]).

% RPC stub for the increment server
inc (Value) ->
    server ! {self (), inc, Value},
    receive
        {From, inc, Reply} ->
            Reply
    end.
```

RPC stub for server:

```erlang
% increment implementation
inc (Value) ->
    Value + 1.

% RPC Server dispatch loop
server () ->
    receive
        {From, inc, Value} ->
            From ! {self(), inc, inc(Value)}
    end,
    server().
```

_Asynchronous RPC_: client blocks until server receives the RPC (i.e. sends an ACK)

- Boost performance, but becomes less transparent
- _E.g._ RPC doesn’t return - client should just wait until server sends an ACK and continue

_Deferred synchronous RPC_: client blocks until ACK, server interrupts client when message is ready

- Can think of it as two asynchronous RPCs
- _E.g._ client needs important network information, but not immediately - defer RPC call will interrupt client program once the server replies

### RMI

_RMI_ - invoke methods on remote objects

- Bind host information to object - no need for lookups through a binding service
- Can pass objects by reference - removes pointer problem in RPCs
- Encapsulate error handling and resource management in an object
- Less transparent than RPCs
- Transparency provided by RPCs and RMIs can be dangerous
- Need to handle errors specific to remote failures
- Arbitrary latency
- User interrupts do not carry over to a remote host
- Concurrency is difficult to track (e.g. errno)

## Group-based Communication

_Group-based communication_: send a message to a group of machines, instead of one

- Issues include message ordering and message reliability

_If one machine doesn’t receive a message, should it be resent to everyone?_

_Broadcast_: send a message to everyone on the network

_Multicast_: send a message to a specific group of machines

- _E.g._ gossip-based communication - good way of quickly spreading data
  - Node A tells asks all neighbours if it’s heard of message P
  - If neighbour hasn’t heard of it, store message P and spread message to other neighbours
  - If it has heard of it, stop broadcasting message

## Event-based Communication

_Event-based communication_: abstract senders and receivers into events

- Senders publish events, receivers subscribe to events
- Loose spatial and temporal coupling
- _E.g._ firefighter services subscribe to a fire event, when something is on fire and publishes a fire event, all firefighter services will be notified

## Isochronous Communication

_Isochronous communication_: communication that has a minimum and maximum receive time (e.g. streams - video and audio channels)

- Data itself has tight spatial and temporal coupled

_Token bucket model_: token is added every specified second, data is sent with a token

- If no token is available, data can’t be sent out; if no data is available, tokens aren’t generated
