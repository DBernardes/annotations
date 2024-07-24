# Zero MQ

## Useful links

[Docs](https://zguide.zeromq.org/docs/chapter1/)

[Github](https://github.com/imatix/zguide)

[zmq_socket](https://libzmq.readthedocs.io/en/latest/zmq_socket.html)


## Chap 1
- REQ-REP: The client issues zmq_send() and then zmq_recv(), in a loop (or once if that’s all it needs). Doing any other sequence (e.g., sending two messages in a row) will result in a return code of -1 from the send or recv call.
- ZeroMQ uses C as its reference language.
- Making a TCP connection involves to and from handshaking that takes several milliseconds depending on your network and the number of hops between peers. In that time, ZeroMQ can send many messages. For sake of argument assume it takes 5 msecs to establish a connection, and that same link can handle 1M messages per second. During the 5 msecs that the subscriber is connecting to the publisher, it takes the publisher only 1 msec to send out those 1K messages.
- You should create and use exactly one context in your process. Technically, the context is the container for all sockets in a single process, and acts as the transport for inproc sockets, which are the fastest way to connect threads in one process.
- always clean-up when you finish the job. If you leave any sockets open, the zmq_ctx_destroy() function will hang forever. And even if you close all sockets, zmq_ctx_destroy() will by default wait forever if there are pending connects or sends.
- If you are opening and closing a lot of sockets, that’s probably a sign that you need to redesign your application. In some cases socket handles won’t be freed until you destroy the context. When you exit the program, close your sockets and then call zmq_ctx_destroy(). This destroys the context.
- ? Finally, destroy the context. This will cause any blocking receives or polls or sends in attached threads (i.e., which share the same context) to return with an error. Catch that error, and then set linger on, and close sockets in that thread, and exit. Do not destroy the same context twice. The zmq_ctx_destroy in the main thread will block until all sockets it knows about are safely closed.

## Chap 2
- sockets are always void pointers
- To create a connection between two nodes, you use zmq_bind() in one node and zmq_connect() in the other. As a general rule of thumb, the node that does zmq_bind() is a “server”, sitting on a well-known network address, and the node which does zmq_connect() is a “client”, with unknown or arbitrary network addresses. Thus we say that we “bind a socket to an endpoint” and “connect a socket to an endpoint”, the endpoint being that well-known network address
- The network connection itself happens in the background, and ZeroMQ will automatically reconnect if the network connection is broken (e.g., if the peer disappears and then comes back).
- Now, imagine we start the client before we start the server. In traditional networking, we get a big red Fail flag. But ZeroMQ lets us start and stop pieces arbitrarily. As soon as the client node does zmq_connect(), the connection exists and that node can start to write messages to the socket.
- A server node can bind to many endpoints (that is, a combination of protocol and address) and it can do this using a single socket.
-  The upshot is that you should usually think in terms of “servers” as static parts of your topology that bind to more or less fixed endpoints, and “clients” as dynamic parts that come and go and connect to these endpoints. Then, design your application around this model.
- Sockets have types. The socket type defines the semantics of the socket, its policies for routing messages inwards and outwards, queuing, etc. You can connect certain types of socket together, e.g., a publisher socket and a subscriber socket.
- with ZeroMQ you define your network architecture by plugging pieces together like a child’s construction toy.
- The zmq_send() method does not actually send the message to the socket connection(s). It queues the message so that the I/O thread can send it asynchronously. It does not block except in some exception cases. So the message is not necessarily sent when zmq_send() returns to your application.
- For most common cases, use tcp, which is a disconnected TCP transport. It is elastic, portable, and fast enough for most cases. We call this disconnected because ZeroMQ’s tcp transport doesn’t require that the endpoint exists before you connect to it. Clients and servers can connect and bind at any time, can go and come back, and it remains transparent to applications.
- It automatically reconnects to peers as they come and go. It queues messages at both sender and receiver, as needed. It limits these queues to guard processes against running out of memory. It handles socket errors. It does all I/O in background threads. It uses lock-free techniques for talking between nodes, so there are never locks, waits, semaphores, or deadlocks.
- ZeroMQ patterns are implemented by pairs of sockets with matching types.

## zmq_socket
- void *zmq_socket (void '*context', int 'type')
- The 'type' argument specifies the socket type, which determines the semantics of communication over the socket.
- The newly created socket is initially unbound, and not associated with any endpoints. In order to establish a message flow a socket must first be connected to at least one endpoint with zmq_connect, or at least one endpoint must be created for accepting incoming connections with zmq_bind
- 0MQ sockets being asynchronous means that the timings of the physical connection setup and tear down, reconnect and effective delivery are transparent to the user and organized by 0MQ itself. Further, messages may be queued in the event that a peer is unavailable to receive them.

### Socket types

#### Client-server pattern
- The client-server pattern is used to allow a single 'ZMQ_SERVER' server talk to one or more 'ZMQ_CLIENT' clients.
- ZMQ_CLIENT - ZMQ_SERVER: 
    - A 'ZMQ_CLIENT' socket talks to a 'ZMQ_SERVER' socket. 
    - If the 'ZMQ_CLIENT' socket has established a connection, zmq_send will accept messages, queue them, and send them as rapidly as the network allows. 
    - The 'ZMQ_CLIENT' socket will not drop messages.
    - A 'ZMQ_SERVER' socket can only reply to an incoming message: the 'ZMQ_CLIENT' peer must always initiate a conversation.
    - Each received message has a 'routing_id' that is a 32-bit unsigned integer. 


#### Radio-dish pattern
- The radio-dish pattern is used for one-to-many distribution of data from a single publisher to multiple subscribers in a fan out fashion.
- Radio-dish is using groups (vs Pub-sub topics), Dish sockets can join a group and each message sent by Radio sockets belong to a group.

#### Publish-subscribe pattern
- The publish-subscribe pattern is used for one-to-many distribution of data from a single publisher to multiple subscribers in a fan out fashion.

#### Pipeline pattern
- The pipeline pattern is used for distributing data to nodes arranged in a pipeline. Data always flows down the pipeline, and each stage of the pipeline is connected to at least one node. When a pipeline stage is connected to multiple nodes data is round-robined among all connected nodes.
- ZMQ_PUSH
    - A socket of type 'ZMQ_PUSH' is used by a pipeline node to send messages to downstream pipeline nodes. Messages are round-robined to all connected downstream nodes. The zmq_recv() function is not implemented for this socket type.
    - Unidirectional
- ZMQ_PULL
    - A socket of type 'ZMQ_PULL' is used by a pipeline node to receive messages from upstream pipeline nodes. Messages are fair-queued from among all connected upstream nodes. The zmq_send() function is not implemented for this socket type.

#### Exclusive pair pattern
- The exclusive pair pattern is used to connect a peer to precisely one other peer. This pattern is used for inter-thread communication across the inproc transport.

#### Native Pattern
- The native pattern is used for communicating with TCP peers and allows asynchronous requests and replies in either direction.

### Request-reply pattern
- The request-reply pattern is used for sending requests from a ZMQ_REQ client to one or more ZMQ_REP services, and receiving subsequent replies to each request sent.
- ZMQ_REQ
    - A socket of type 'ZMQ_REQ' is used by a client to send requests to and receive replies from a service. This socket type allows only an alternating sequence of zmq_send(request) and subsequent zmq_recv(reply) calls. Each request sent is round-robined among all services, and each reply received is matched with the last issued request.
    - The REQ socket shall not discard messages.
- ZMQ_REP
    - If the original requester does not exist any more the reply is silently discarded.
    - Acho que garante o recebimento
- ZMQ_DEALER
    - A socket of type 'ZMQ_DEALER' is an advanced pattern used for extending request/reply sockets. Each message sent is round-robined among all connected peers, and each message received is fair-queued from all connected peers.
    - Compatible peer sockets:'ZMQ_ROUTER', 'ZMQ_REP', 'ZMQ_DEALER'
    - Send/receive pattern: Unrestricted
- ZMQ_ROUTER
    - A socket of type 'ZMQ_ROUTER' is an advanced socket type used for extending request/reply sockets. When receiving messages a 'ZMQ_ROUTER' socket shall prepend a message part containing the routing id of the originating peer to the message before passing it to the application. 
    - When sending messages a 'ZMQ_ROUTER' socket shall remove the first part of the message and use it to determine the _routing id _ of the peer the message shall be routed to. If the peer does not exist anymore, or has never existed, the message shall be silently discarded. 
    