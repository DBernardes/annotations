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