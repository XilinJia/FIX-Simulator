Overview
--------

FIX-Simulator is an example project for FIX protocol client and server implementations.
It can be used for testing your client or server and you might find a need to add new feature or change FIX-Simulator's behavior.  FIX-Simulator also can be used just as example for implementing acceptors and initiators.

** This is an updated version of https://github.com/gloryofrobots/fixsim.  It runs with Python 3.10 and comes with a requirements file.

Configuration
-------------

FIX-Simulator consists of two scripts fixsim-client.py and fixsim-server.py. Each
script uses two configuration files, one for FIX Session settings in ini format
and one for business logic in YAML. You can find example configurations with
comments in project tree. For example server and client may be started by commands:

```
python fixsim-server.py -ac fixsim-server.conf.ini -c fixsim-server.conf.yaml
```
```
python fixsim-client.py -ic fixsim-client.conf.ini -c fixsim-client.conf.yaml
```

FIX-Simulator depends on twisted but you can easily purge it from poject by replacing reactor infinite loop by differrent infinite loop in main thread and implementing something like twisted.internet.task.LoopingCall which is used for periodical sending snapshots and subscribing to instruments.

FIX-Simulator supports only FIX44 now

Workflow
--------

FIX-Simulator business logic is pretty simple. Server receives client session and stores it. Client subscribes to the one or more instrument (like EUR/USD, USD/CAD etc) and server starts sending market data snapshots to client. Client can create a new orderfor each snapshot and send it to acceptor or skip this snapshot (see skip_snapshot_chance attr in client yaml config). Order is created for one randomly selected quote from previously received snapshot. For each such order acceptor can create filled or rejected execution report(see reject rate in server yaml config)


