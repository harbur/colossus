Docker DNS Search
=================

In order for docker to connect with Consul DNS in a more transparent way you can configure DNS search to by default look at service.consul (The domain name for DNS services).

This will make it possible to search services simply by using their [SERVICE_NAME](http://gliderlabs.com/registrator/latest/user/services/#service-name) which defaults to the image name (if only one port is exposed)

To configure this in CoreOS run the following command **on each node**:

```
coreos-cloudinit --from-url=https://raw.githubusercontent.com/harbur/colossus/master/docs/dockerDNS/consul-dns-search.config
```

*IMPORTANT*: This will restart your docker container server, stopping all running containers.
