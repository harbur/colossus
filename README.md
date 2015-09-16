Colossus Platform
=================

Colossus is platform to run distributed services using containers.

Components
----------

The Colossus is comprised of the following components:

* CoreOS - The Operating System
	* Etcd - The Distributed key-value store
	* Fleet - The Distributed init System
* Consul - The Service Discovery
	* Registrator - The Docker Bridge
	* HAProxy - The Proxy & Load Balancer
* Docker - The Container Runtime

![image](./imgs/architecture.png)

For more details about each component check [architecture](https://github.com/harbur/colossus/tree/master/docs/architecture)

Installation
------------

To Install Colossus follow these Steps:

* [Create a Coreos Cluster](https://coreos.com/os/docs/latest/booting-on-ec2.html)
* [Install Harbur CLI](http://docs.harbur.io/en/latest/installation/harbur-cli/index.html)
* [Configure Virtual IP](https://github.com/harbur/colossus/tree/master/docs/VIP)
* [Configure Wildcard DNS](https://github.com/harbur/colossus/tree/master/docs/DNS)
* [Launch Consul in CoreOS Cluster](https://cloud.harbur.io/unitfiles/harbur/consul)
* [Launch Registrator in CoreOS Cluster](https://cloud.harbur.io/unitfiles/harbur/registrator-consul)
* [Launch HAProxy in CoreOS Cluster](https://cloud.harbur.io/unitfiles/harbur/haproxy-consul)
* [Configure Docker DNS Search](https://github.com/harbur/colossus/tree/master/docs/dockerDNS)

## Using Colossus

## Run Redis Server

Connect to a node with SSH and run:

```shell
$ docker run --name redis_server -d -p 6379:6379 redis
```

This will start a redis server in the node. Registrator will pick-up the container creation event and register it to Consul Backend. You can verify that at the Consul UI (http://cluster.local:8500/)

Since Consul is your DNS server on the cluster, all services are now discoverable by DNS. To verify that go to another node and run:

```shell
$ ping redis.service.consul
PING redis.service.consul (10.0.0.100) 56(84) bytes of data.
64 bytes from coreos1.node.dc1.consul (10.0.0.100): icmp_seq=1 ttl=64 time=0.294 ms
```

For more information about the Consul DNS discovery review [Consul DNS Interface](https://www.consul.io/docs/agent/dns.html)

This also works inside a container as expected:

```shell
$ docker run --rm -it redis ping redis.service.consul
PING redis.service.consul (10.0.0.100): 48 data bytes
56 bytes from 10.0.0.10: icmp_seq=0 ttl=64 time=0.151 ms
```

Since we configured the [Docker DNS Search](https://github.com/harbur/colossus/tree/master/docs/dockerDNS) The service is also discoverable using just the service name:

```shell
$ docker run --rm -it redis ping redis
PING redis.service.consul (10.0.0.100): 48 data bytes
56 bytes from 10.0.0.100: icmp_seq=0 ttl=64 time=0.187 ms
```

## Run an Nginx Service

```shell
$ docker run -d -p 80 -e SERVICE_NAME=example nginx
```

Open http://example.cluster.local to see the nginx server.

License
-------

Copyright © 2015 Harbur Cloud Solutions LC.

Licensed under the Apache License, Version 2.0 (the "License").

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.