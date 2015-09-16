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

**Usage Example**

Run an nginx server:

```
docker run -d -p 80 -e SERVICE_NAME=example nginx
```

Open http://example.cluster.local to see the nginx server.

Getting Started
---------------

To Get Started follow these Steps:

* [Create a Coreos Cluster](https://coreos.com/os/docs/latest/booting-on-ec2.html)
* [Install Harbur CLI](http://docs.harbur.io/en/latest/installation/harbur-cli/index.html)
* [Configure Virtual IP](https://github.com/harbur/colossus/tree/master/docs/VIP)
* [Configure Wildcard DNS](https://github.com/harbur/colossus/tree/master/docs/DNS)
* [Launch Consul in CoreOS Cluster](https://cloud.harbur.io/unitfiles/harbur/consul)
* [Launch Registrator in CoreOS Cluster](https://cloud.harbur.io/unitfiles/harbur/registrator-consul)
* [Launch HAProxy in CoreOS Cluster](https://cloud.harbur.io/unitfiles/harbur/haproxy-consul)

License
-------

Copyright © 2015 Harbur Cloud Solutions LC.

Licensed under the Apache License, Version 2.0 (the "License").

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.