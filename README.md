CCNQ4: events for the backends
------------------------------

This module will wrap whatever is the method-du-jour for inbound & outbound events _inside_ an application.

For now it's basically a most.js wrapper around ioredis, since the interface between the different processes and the forwarding process is the local Redis server.

The module ensures the messages are formatted properly.
