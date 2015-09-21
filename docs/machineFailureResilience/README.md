## Machine Failure Resilience

This section describes how to handle machine failures. By machine failures we refer to situations where the entire Operating System for some reason crashes or reboots.

Continuing on the example of [Container Failure Resilience](https://github.com/harbur/colossus/tree/master/docs/containerFailureResilience) now we test how it performs when the OS is restarted.

First, let's use a control center outside the Cluster itself. The best way is to install [Fleetctl client](http://docs.harbur.io/en/beta/installation/coreos-fleetctl/index.html) locally and configure it to go through the [VIP](https://github.com/harbur/colossus/tree/master/docs/VIP).

Fleet is 

## Next Chapter

But if the service fails we'll get a downtime until it is restored. To avoid downtimes go to [High Availability](https://github.com/harbur/colossus/tree/master/docs/highavailability) chapter
