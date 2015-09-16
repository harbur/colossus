Configure Virtual IP
====================

Virtual IP is an IP that is not assigned to a specific machine but can roam between various instances. The objective of the Virtual IP is that it is decoupled from a specific server instance, so that if the assigned server fails, another can take-over the IP and avoid downtime.

It is often called as floating IP or elastic IP.

Virtual IP at EC2
-----------------

To associate a Virtual IP (often mentioned as Elastic IP) in EC2 follow the guide [here](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html#using-instance-addressing-eips-associating)

Virtual IP on-Premise
---------------------

In case you're installing the Cluster on-Premise ask your IT Administrator to provide you with a Virtual IP.
