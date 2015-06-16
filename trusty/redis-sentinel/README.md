# Redis Sentinel

Juju charm for Redis Sentinel.

Redis Sentinel provides high availability for Redis. In practical terms this means that using Sentinel you can create a Redis deployment that resists without human intervention to certain kind of failures.

Redis Sentinel also provides other collateral tasks such as monitoring, notifications and acts as a configuration provider for clients.

This is the full list of Sentinel capabilities at a macroscopical level (i.e. the big picture):

* Monitoring. Sentinel constantly checks if your master and slave instances are working as expected.
* Notification. Sentinel can notify the system administrator, another computer programs, via an API, that something is wrong with one of the monitored Redis instances.
* Automatic failover. If a master is not working as expected, Sentinel can start a failover process where a slave is promoted to master, the other additional slaves are reconfigured to use the new master, and the applications using the Redis server informed about the new address to use when connecting.
* Configuration provider. Sentinel acts as a source of authority for clients service discovery: clients connect to Sentinels in order to ask for the address of the current Redis master responsible for a given service. If a failover occurs, Sentinels will report the new address.

# Usage

```
juju deploy redis-sentinel
```

You need to connect it with a redis master in order to use it.

```
juju add-relation redis-sentinel:redis-master redis:master
```

**NOTE**: We recommend to use this [Redis Charm](https://jujucharms.com/u/juju-gui/redis/trusty/1) as the slave nodes work well, and 
also supports the trusty.


## Debugging

You can access your sentinel machine. First check which unit is running:

```
juju status
```

Then ssh to it:

```
juju ssh UNIT
```

When you are in, you will find the logs in */var/log/juju/* and also you can test
the Sentinel config with:


```
redis-cli -p 26379
sentinel master mymaster
```

It should return something like this:

```
 1) "name"
 2) "mymaster"
 3) "ip"
 4) "10.0.3.33"
 5) "port"
 6) "6379"
 7) "runid"
 8) "953ae6a589449c13ddefaee3538d356d287f509b"
 9) "flags"
10) "master"
11) "link-pending-commands"
12) "0"
13) "link-refcount"
14) "1"
15) "last-ping-sent"
16) "0"
17) "last-ok-ping-reply"
18) "735"
19) "last-ping-reply"
20) "735"
21) "down-after-milliseconds"
22) "5000"
23) "info-refresh"
24) "126"
25) "role-reported"
26) "master"
27) "role-reported-time"
28) "532439"
29) "config-epoch"
30) "1"
31) "num-slaves"
32) "0"
33) "num-other-sentinels"
34) "0"
35) "quorum"
36) "2"
37) "failover-timeout"
38) "60000"
39) "parallel-syncs"
40) "1"
```

If you don't get this output, then something is not working.
