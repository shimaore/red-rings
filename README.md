CCNQ4: events handlers
----------------------

This module is the base for a series of modules that provide handlers or transports with the semantics of the `abrasive-ducks` datastream routing engine.

The base module ensures the messages are (minimally) formatted properly.

A dependant module (e.g. red-rings-redis, red-rings-socket.io) might contain:

- `backend` — a backend plugin for abrasive-ducks; the module is a function that builds a callback compatible with abrasive-ducks' `backend_join` functionality;
- `index` — a backend remote (using the same transport as the matching `backend`), which is used inside an application (e.g. huge-play, ccnq4-opensips, etc.) to communicate with an abrasive-ducks `backend`;

- `frontend` — a frontend plugin for abrasive-ducks; the module is a function that builds a callback compatible with abrasive-ducks' `frontend_join` functionality;
- `client` — a frontend remote, which is typically used in an external application such as a UI.

In each case, only the "remote" modules (`index` or `client`) inherit from RedRing; the `backend` and `frontend` modules follow their own specifications, typically:
- `frontend` will accept a transport, a `build_client_policy` function which is used to build a proper client policy stream at connection time based on transport-provided authentication, and the `frontend_join` function;
- `backend` will accept a transport and return a function that takes an input stream and returns an output stream.

Note that the distinction between backend and fronted is mostly one of API and trust boundaries. On that last point,
- frontends are authenticated connection which are authorized inbound and outbound;
- backends are assumed to be the holders of "ultimate truth"; inside abrasive-ducks they are trusted by default.

Related projects
----------------

- [abrasive-ducks](https://github.com/shimaore/abrasive-ducks)
