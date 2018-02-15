CCNQ4: events for the backends
------------------------------

This module will wrap whatever is the method-du-jour for inbound & outbound events _inside_ an application.

The module ensures the messages are formatted properly.

A submodule (e.g. red-rings-redis, red-rings-socket.io) might contain:

- `index` — a backend client
- `backend` — a backend plugin for abrasive-ducks
- `client` — a frontend client (e.g. UI application)
- `frontend` — a frontend plugin for abrasive-ducks

Note that the distinction between backend and fronted is mostly one of convenience; backend indicates (in abrasive-ducks) terms a trusted connection that receives all messages from all frontends; while frontend indicates (in abrasive-ducks) an authenticated connection that receives only messages from the backends it subscribed to. This model most probably still needs to be refined.

Related projects
----------------

- [abrasive-ducks](https://github.com/shimaore/abrasive-ducks)
