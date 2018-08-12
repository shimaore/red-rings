Red Ring
--------

    class RedRingCore # abstract base class

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

        @send {op:UPDATE,id:doc._id,doc}

      update: (id,rev,doc,operations) ->
        assert is_string id
        assert is_string rev
        assert doc? or operations?

        msg = {op:UPDATE,id,rev}
        msg.doc = doc if doc?
        msg.operations = operations if operations?

        @send msg

      delete: (id,rev) ->
        assert is_string id
        assert is_string rev

        @send {op: UPDATE,id,rev,deleted:true}

    assert = require 'minimalistic-assert'
    is_string = (x) -> x? and typeof x is 'string' and x.length > 0
    {NOTIFY,UPDATE,SUBSCRIBE,UNSUBSCRIBE} = require './operations'

    module.exports = {RedRingCore}
