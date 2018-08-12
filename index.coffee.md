Red Ring
--------

Abstract class

    {RedRingCore} = require './core'

    class RedRing extends RedRingCore

      # send: (msg) -> action

On a "client", the `key` is really going to be a key used to subscribe to NOTIFY events.
On a backend server, the `key` is going to be a pattern used to receive SUBSCRIBE, UPDATE, UNSUBSCRIBE messages, and the `subscribe`/`unsubscribe` methods should be translated into the "backend"'s matching operations. (But maybe this should be a totally different method altogether.)

      # source_of: (key) -> most Stream

      receive: (key,limit = most.never()) ->
        @subscribe key
        @source_of key
        .until limit
        .continueWith =>
          @unsubscribe key
          most.empty()

    most = require 'most'

    module.exports = {RedRing}
