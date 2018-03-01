Red Ring
--------

    class RedRing # abstract base class

      # send: (msg) -> action

A NOTIFY message is formatted based on the rows of a CouchDB query.

      notify: (key,id,value,doc) ->
        assert key?
        assert is_string id

        @send {op:NOTIFY,key,id,value,doc}

      subscribe: (key) ->
        assert key?

        @send {op:SUBSCRIBE,key}

      unsubscribe: (key) ->
        assert key?

        @send {op:UNSUBSCRIBE,key}

      create: (doc) ->
        assert is_string doc._id
        assert not doc._rev?
        assert not doc._deleted

        @send {op:UPDATE,doc}

      update: (id,rev,doc,operations) ->
        assert is_string id
        assert is_string rev
        assert doc? or operations?

        @send {op: UPDATE,id,rev,doc,operations}

      delete: (id,rev) ->
        assert is_string id
        assert is_string rev

        @send {op: UPDATE,id,rev,deleted:true}

      # source_of: (key) -> most Stream

      receive: (key,limit = most.never()) ->
        @subscribe key
        @source_of key
        .until limit
        .continueWith =>
          @unsubscribe key
          most.empty()

    assert = require 'minimalistic-assert'
    is_string = (x) -> x? and typeof x is 'string' and x.length > 0
    {NOTIFY,UPDATE,SUBSCRIBE,UNSUBSCRIBE} = require './operations'
    most = require 'most'

    module.exports = {RedRing}
