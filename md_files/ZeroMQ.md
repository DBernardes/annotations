# Zero MQ

[Docs](https://zguide.zeromq.org/docs/chapter1/)
[Github](https://github.com/imatix/zguide)


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
- 
