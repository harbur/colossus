Configure DNS
=============

[DNS](https://en.wikipedia.org/wiki/Domain_Name_System) is the Domain Name System, a network system used to translate names into IP addresses.

Now that we have a [Virtual IP](https://github.com/harbur/colossus/tree/master/docs/VIP) we want to associate it with an easy to remember name. We'll call this name **Cluster Domain** and will represent the entire cluster.

We'll also want to configure a [wilcard DNS record](https://en.wikipedia.org/wiki/Wildcard_DNS_record) so that all subdomains of the cluster domain also redirect to the same Virtual IP.

To do that add the following on your DNS configuration. In our example the cluster domain is `cluster.local`, we have 3 nodes on the cluster with IPs 10.0.0.101-103 and our Virtual IP is 10.0.0.200.

| Name                  | Record Type   | IP Address |
| --------------------- | ------------- | ---------- |
| coreos1.cluster.local | A             | 10.0.0.101 |
| coreos2.cluster.local | A             | 10.0.0.102 |
| coreos3.cluster.local | A             | 10.0.0.103 |
| cluster.local         | A             | 10.0.0.200 |
| *.cluster.local       | A             | 10.0.0.200 |
