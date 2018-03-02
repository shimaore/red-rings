    (require 'chai').should()

    describe 'The module', ->

      it 'should load', -> require '..'

      it 'should export RedRings', ->
        m = require '..'
        m.should.have.property 'RedRing'

      it 'should notify', (done) ->
        {RedRing} = require '..'
        {NOTIFY} = require '../operations'
        v = new RedRing()
        v.send = (msg) ->
          msg.should.have.property 'op', NOTIFY
          done()

        v.notify 'key:', 'id:'

      it 'should update', (done) ->
        {RedRing} = require '..'
        {UPDATE} = require '../operations'
        v = new RedRing()
        v.send = (msg) ->
          msg.should.have.property 'op', UPDATE
          done()

        v.update 'id:', '2-A', {}

      it 'should subscribe', (done) ->
        {RedRing} = require '..'
        {SUBSCRIBE} = require '../operations'
        v = new RedRing()
        v.send = (msg) ->
          msg.should.have.property 'op', SUBSCRIBE
          done()

        v.subscribe 'key:'

      it 'should unsubscribe', (done) ->
        {RedRing} = require '..'
        {UNSUBSCRIBE} = require '../operations'
        v = new RedRing()
        v.send = (msg) ->
          msg.should.have.property 'op', UNSUBSCRIBE
          done()

        v.unsubscribe 'key:'

      it 'should create', (done) ->
        {RedRing} = require '..'
        {UPDATE} = require '../operations'
        v = new RedRing()
        v.send = (msg) ->
          msg.should.have.property 'op', UPDATE
          msg.should.have.property 'id', 'id:'
          done()

        v.create _id:'id:'

      it 'should delete', (done) ->
        {RedRing} = require '..'
        {UPDATE} = require '../operations'
        v = new RedRing()
        v.send = (msg) ->
          msg.should.have.property 'op', UPDATE
          done()

        v.delete 'id:', '2-A'

      it 'should receive', (done) ->
        {RedRing} = require '..'
        {UNSUBSCRIBE} = require '../operations'
        v = new RedRing()
        v.send = (msg) ->
          if msg.op is UNSUBSCRIBE
            done()
        v.source_of = (key) -> (require 'most').empty()

        v.receive('key:').drain()
        return

      ###
      it 'should create from stream', (done) ->
        {RedRingFromStream} = require '..'
        {UNSUBSCRIBE} = require '../operations'
        v = new RedRingFromStream (require 'most').just key:0
        v.send = (msg) ->
          if msg.op is UNSUBSCRIBE
            done()

        v.receive('key:').drain()
        return

      it 'should create from events', (done) ->
        {RedRingFromEvents} = require '..'
        {UNSUBSCRIBE} = require '../operations'
        ev = new (require 'events')
        v = new RedRingFromEvents ev
        v.send = (msg) ->
          if msg.op is UNSUBSCRIBE
            done()

        limit = (require 'most').fromEvent('end', ev).take(1)
        v
        .receive 'key:', limit
        .drain()
        ev.emit 'key:', {}
        ev.emit 'end', 'yo'
        return
      ###
