Colossus Architecture
=====================

Here is desribed the components used for the Colossus platform.

CoreOS
------

[CoreOS](https://coreos.com/) provides the Operating System layer. It comes with [Etcd](https://github.com/coreos/etcd) service, a distributed highly-available key-value storage, and [Fleet](https://github.com/coreos/fleet) a Distributed init System.

Docker
------

[Docker](https://www.docker.com/) provides the abstraction of packaging and running apps as lightweight linux-based containers.

Consul
------

We use [Consul](https://www.consul.io/) as a Service Discovery process.

Consul makes it simple for services to register themselves and to discover other services via a DNS or HTTP interface. Register external services such as SaaS providers as well.

Pairing service discovery with health checking prevents routing requests to unhealthy hosts and enables services to easily provide circuit breakers.

Registrator
-----------

[Registrator](http://gliderlabs.com/registrator/latest/) automatically registers and deregisters services for any Docker container by inspecting containers as they come online.

HAProxy
-------

We use [HAProxy](http://www.haproxy.org/) as reverse proxy and load balancing between instances of a service. The port 80 is listened by the HAProxy that monitors Consul service for existing services and re-configures itself to route traffic to the services using subdomains. For example for service `stable` the URL http://stable.coreos1.acme.local can be used if `acme.local` is the domain of the cluster and `coreos1` is the name of a cluster node.
