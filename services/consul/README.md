Run Consul in CoreOS
====================

Run the Consul on your cluster:

```
harbur -f harbur/consul start consul
```

Once you run the Consul as global service to your cluster, check journal to see the progress:

```
-- Logs begin at Thu 2015-08-06 09:17:51 UTC, end at Thu 2015-08-06 09:29:07 UTC. --
Aug 06 09:19:52 t1.harbur.io systemd[1]: Starting Consul...
Aug 06 09:19:52 t1.harbur.io docker[1038]: Error response from daemon: no such id: consul
Aug 06 09:19:52 t1.harbur.io docker[1038]: time="2015-08-06T09:19:52Z" level=fatal msg="Error: failed to kill one or more containers"
Aug 06 09:19:52 t1.harbur.io docker[1044]: Error response from daemon: no such id: consul
Aug 06 09:19:52 t1.harbur.io docker[1044]: time="2015-08-06T09:19:52Z" level=fatal msg="Error: failed to remove one or more containers"
Aug 06 09:19:52 t1.harbur.io bash[1052]: rm: cannot remove '/etc/systemd/resolved.conf.d/00-consul-dns.conf': No such file or directory
Aug 06 09:19:52 t1.harbur.io docker[1056]: Pulling repository quay.io/democracyworks/consul-coreos
Aug 06 09:19:53 t1.harbur.io docker[1056]: 8c89aecc4f23: Pulling image (2015041102) from quay.io/democracyworks/consul-coreos
Aug 06 09:19:53 t1.harbur.io docker[1056]: 8c89aecc4f23: Pulling image (2015041102) from quay.io/democracyworks/consul-coreos, endpoint: https://quay.io/v1/
Aug 06 09:19:55 t1.harbur.io docker[1056]: 8c89aecc4f23: Pulling dependent layers
Aug 06 09:19:55 t1.harbur.io docker[1056]: 511136ea3c5a: Pulling metadata
Aug 06 09:19:55 t1.harbur.io docker[1056]: 511136ea3c5a: Pulling fs layer
Aug 06 09:19:55 t1.harbur.io docker[1056]: 511136ea3c5a: Download complete
Aug 06 09:19:55 t1.harbur.io docker[1056]: 46e263e5de56: Pulling metadata
Aug 06 09:19:57 t1.harbur.io docker[1056]: 46e263e5de56: Pulling fs layer
...
Aug 06 09:21:35 t1.harbur.io bash[1291]: Status: Downloaded newer image for cap10morgan/etcdctl:0.4.6
Aug 06 09:21:35 t1.harbur.io bash[1291]: /consul.io/bootstrap/machines/625acb1c670d43688e6dd165f3d5f673
Aug 06 09:21:36 t1.harbur.io bash[1291]: ==> WARNING: Expect Mode enabled, expecting 3 servers
Aug 06 09:21:36 t1.harbur.io bash[1291]: ==> Starting Consul agent...
Aug 06 09:21:36 t1.harbur.io bash[1291]: ==> Starting Consul agent RPC...
Aug 06 09:21:36 t1.harbur.io bash[1291]: ==> Joining cluster...
Aug 06 09:21:36 t1.harbur.io bash[1291]: Join completed. Synced with 1 initial agents
Aug 06 09:21:36 t1.harbur.io bash[1291]: ==> Consul agent running!
Aug 06 09:21:36 t1.harbur.io bash[1291]: Node name: 't1.harbur.io'
Aug 06 09:21:36 t1.harbur.io bash[1291]: Datacenter: 'dc1'
Aug 06 09:21:36 t1.harbur.io bash[1291]: Server: true (bootstrap: false)
Aug 06 09:21:36 t1.harbur.io bash[1291]: Client Addr: 0.0.0.0 (HTTP: 8500, HTTPS: -1, DNS: 53, RPC: 8400)
Aug 06 09:21:36 t1.harbur.io bash[1291]: Cluster Addr: 10.132.127.197 (LAN: 8301, WAN: 8302)
Aug 06 09:21:36 t1.harbur.io bash[1291]: Gossip encrypt: false, RPC-TLS: false, TLS-Incoming: false
Aug 06 09:21:36 t1.harbur.io bash[1291]: Atlas: <disabled>
Aug 06 09:21:36 t1.harbur.io bash[1291]: ==> Log data will now stream in as it occurs:
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [INFO] raft: Node at 10.132.127.197:8300 [Follower] entering Follower state
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [INFO] serf: EventMemberJoin: t1.harbur.io 10.132.127.197
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [INFO] serf: EventMemberJoin: t1.harbur.io.dc1 10.132.127.197
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [INFO] consul: adding server t1.harbur.io (Addr: 10.132.127.197:8300) (DC: dc1)
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [INFO] consul: adding server t1.harbur.io.dc1 (Addr: 10.132.127.197:8300) (DC: dc1)
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [INFO] agent: (LAN) joining: [10.132.127.193]
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [INFO] serf: EventMemberJoin: t2.harbur.io 10.132.127.193
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [INFO] agent: (LAN) joined: 1 Err: <nil>
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [ERR] agent: failed to sync remote state: No cluster leader
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [INFO] consul: adding server t2.harbur.io (Addr: 10.132.127.193:8300) (DC: dc1)
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [INFO] serf: EventMemberJoin: t3.harbur.io 10.132.129.62
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [INFO] consul: adding server t3.harbur.io (Addr: 10.132.129.62:8300) (DC: dc1)
Aug 06 09:21:36 t1.harbur.io bash[1291]: 2015/08/06 09:21:36 [INFO] consul: Attempting bootstrap with nodes: [10.132.129.62:8300 10.132.127.197:8300 10.132.127.193:8300]
Aug 06 09:21:37 t1.harbur.io bash[1291]: 2015/08/06 09:21:37 [INFO] consul: New leader elected: t2.harbur.io
Aug 06 09:21:38 t1.harbur.io bash[1291]: 2015/08/06 09:21:38 [INFO] agent: Synced service 'consul'
```

Now check fleet to see the instances running:

```
core@t1 ~ $ fleetctl list-units
UNIT		MACHINE				ACTIVE	SUB
consul.service	625acb1c.../10.132.127.193	active	running
consul.service	9d6b5b0d.../10.132.127.197	active	running
consul.service	ac908cfa.../10.132.129.62	active	running
```

On etcd you'll see the machines registered (Etcd is used to publish consul server instances):

```
core@t1 ~ $ etcdctl ls  --recursive
/consul.io
/consul.io/bootstrap
/consul.io/bootstrap/machines
/consul.io/bootstrap/machines/625acb1c670d43688e6dd165f3d5f673
/consul.io/bootstrap/machines/9d6b5b0d0d7b4f5fb5a74c34a69cdaeb
/consul.io/bootstrap/machines/ac908cfa57cd4edfb9e07a43594d9335
```

Point with the browser to any host with consul server running at port 8500: http://servername:8500/
You'll be able to see the consul UI.

To run consul CLI you can use the existing running container:

```
core@t1 ~ $ docker exec -it consul consul --help
usage: consul [--version] [--help] <command> [<args>]

Available commands are:
    agent          Runs a Consul agent
    event          Fire a new event
    exec           Executes a command on Consul nodes
    force-leave    Forces a member of the cluster to enter the "left" state
    info           Provides debugging information for operators
    join           Tell Consul agent to join cluster
    keygen         Generates a new encryption key
    keyring        Manages gossip layer encryption keys
    leave          Gracefully leaves the Consul cluster and shuts down
    lock           Execute a command holding a lock
    maint          Controls node or service maintenance mode
    members        Lists the members of a Consul cluster
    monitor        Stream logs from a Consul agent
    reload         Triggers the agent to reload configuration files
    version        Prints the Consul version
    watch          Watch for changes in Consul
```

## Registrator

This can be combined with the [harbur/registrator-consul](https://cloud.harbur.io/unitfiles/harbur/registrator-consul) that is a registrator service that monitors docker instances and registers them using consul as backend.

```
harbur -f harbur/registrator-consul start registrator-consul
```

Now you can run a container exposing the port to a random number:

```
docker run -dP redis
```

You can verify that redis was registered on Consul using the web UI (at port 8500), doing a REST request:

```
core@t2 ~ $ curl  -w '\n' http://localhost:8500/v1/catalog/service/redis
[{"Node":"t2.harbur.io","Address":"10.132.127.193","ServiceID":"t2.harbur.io:silly_wright:6379","ServiceName":"redis","ServiceTags":null,"ServiceAddress":"","ServicePort":32782}]
```

or using the consul client:

```
core@t2 ~ $ docker exec -it consul consul watch -service=redis -type=service
[
    {
        "Node": {
            "Node": "t2.harbur.io",
            "Address": "10.132.127.193"
        },
        "Service": {
            "ID": "t2.harbur.io:silly_wright:6379",
            "Service": "redis",
            "Tags": null,
            "Port": 32782,
            "Address": ""
        },
        "Checks": [
            {
                "Node": "t2.harbur.io",
                "CheckID": "serfHealth",
                "Name": "Serf Health Status",
                "Status": "passing",
                "Notes": "",
                "Output": "Agent alive and reachable",
                "ServiceID": "",
                "ServiceName": ""
            }
        ]
    }
]
```

## Internal Consumption

In order to consume the service internally you can now use DNS requests to decouple the service endpoints from the instances themselves:

```
core@t2 ~ $ ping redis.service.consul
PING redis.service.consul (10.132.127.193) 56(84) bytes of data.
64 bytes from t2.harbur.io.node.dc1.consul (10.132.127.193): icmp_seq=1 ttl=64 time=0.054 ms
64 bytes from t2.harbur.io.node.dc1.consul (10.132.127.193): icmp_seq=2 ttl=64 time=0.036 ms
64 bytes from t2.harbur.io.node.dc1.consul (10.132.127.193): icmp_seq=3 ttl=64 time=0.064 ms
64 bytes from t2.harbur.io.node.dc1.consul (10.132.127.193): icmp_seq=4 ttl=64 time=0.058 ms
64 bytes from t2.harbur.io.node.dc1.consul (10.132.127.193): icmp_seq=5 ttl=64 time=0.078 ms
```

Consul is configured as our DNS resolver and it responds with the IP of the server that contains the redis instance. If you run another instance on another machine of your cluster then the ping request will load-balance between the two instances, and when a service is knocked-out Consul will be updated and the DNS resolution will go to the live instance only. This way you can for example migrate a service from one machine to another without having to restart other containers that depend on it in order to update the configuration, instead it is done dynamically on DNS side.

## External Consumption

In order to consume Consul externally the [harbur/haproxy-consul](https://cloud.harbur.io/unitfiles/harbur/registrator-consul) can be used. It is basically an haproxy that is configured dynamically by Consul backend.

To install and run it do:

```
harbur -f harbur/haproxy-consul start haproxy-consul
```

Now to test it start an nginx container:

```
docker run -dP nginx
```

Now point with your browser at: http://nginx-80.t2.harbur.io/

(change t2.harbur.io to your own cluster server name)

It will load balance between all nginx instances at port 80. Since the haproxy-consul is global fleet unit, it is launched on all servers and this means that you can point to any server and you'll get the same response:

* http://nginx-80.t1.harbur.io/
* http://nginx-80.t2.harbur.io/
* http://nginx-80.t3.harbur.io/

## References

* Based on [democracyworks/consul-coreos](https://github.com/democracyworks/consul-coreos)