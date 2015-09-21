Container Failure Resilience
============================

This section describes how to handle container failures. By container failures we refer to situations where a container process for some reason exits with a failure status (non-zero).

The same as with unix commands, containers report their process status when exiting to docker. This helps the orchestrator understand if the process failed or exited succesfully (e.g. manual stopping or oneshot processes)

For this it is important that the container handles correctly their own exit-status.

When a container is requested to be stopped, a SIGTERM signal is sent to the container process. If the process doesn't respond in 10 secs (default timeout) Docker will forcefull kill the process and provide a non-zero exit status.

## Verify a Container handles SIGTERMs

Before continuing with the rest of the process, first we need to check if a container handles signals properly and stops gracefully.

We'll use `nginx` as a container example.

```
$ docker run -d --name nginx nginx
0866d8cd462db7d0b16f3ccff26e43ab1afa6ecb2996d45198ccc2bc4eb5ef2d
$ docker stop nginx
nginx
$ docker ps -l     
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
0866d8cd462d        nginx               "nginx -g 'daemon off"   11 seconds ago      Exited (0) 1 seconds ago                       nginx
```

We can see that the STATUS is `Exited (0)` Which means the process stopped succesfuly. If non-zero response is given it is considered failure.

If container takes 10 seconds to stop, it probably means that the SIGNTERM is not sent correctly to the process.

Now let's kill the container instead of stopping it and see how it responds:

```
$ docker start nginx
nginx
$ docker kill nginx 
nginx
$ docker ps -l
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS                       PORTS               NAMES
682974ade50d        nginx               "nginx -g 'daemon off"   About a minute ago   Exited (137) 2 seconds ago                       nginx
```

The STATUS is now non-zero: `Exited (137)` which represents a failure.

To cleanup created containers do the following:

```
$ docker rm -v nginx
nginx
```

## Docker Auto-restart

In order to achieve container failure resilience, we can define the containers to auto-start on failure.

Docker has [Restart policies](https://docs.docker.com/reference/run/#restart-policies-restart) that can help in automating restarting in case of container failure.

Let's simulate a failure with restart policy:

```
$ docker run --restart=always -d --name nginx nginx
```

Stopping or killling the container using the docker CLI will not trigger the restart policy, instead we need to kill the process inside it:

```
$ docker top nginx
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                2836                726                 0                   07:59               ?                   00:00:00            nginx: master process nginx -g daemon off;
104                 2871                2836                0                   07:59               ?                   00:00:00            nginx: worker process
$ sudo kill 2836
$ docker ps -l
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                  PORTS               NAMES
2c68b08c77d9        nginx               "nginx -g 'daemon of   11 minutes ago      Up Less than a second   80/tcp, 443/tcp     nginx
```

We can now see that the container is restarted automatically (On the STATUS column).

Restart policy helps to create failure resilience and self-heal the process. In this specific scenario the same container is reused and started again.

In case of container corruption (some alteration on the Filesystem that renders the container useless) restart policy will not resolve the problem.

In order to shield us from failures of container corruption, and always following immutable services principles, our restart policy should regenerate a container using the image directly.

This can be performed with [Fleet](	https://coreos.com/using-coreos/clustering/)


`nginx-demo@.service`

```
[Unit]
Description=Nginx Demo
After=docker.service
Requires=docker.service

[Service]
EnvironmentFile=/etc/environment
ExecStartPre=-/usr/bin/docker kill nginx-demo-%i
ExecStartPre=-/usr/bin/docker rm nginx-demo-%i
ExecStart=/usr/bin/docker run --name=nginx-demo-%i -e SERVICE_80_NAME=nginx-demo -P nginx
ExecStop=/usr/bin/docker stop nginx-demo-%i
TimeoutStartSec=0
Restart=always
RestartSec=10s
```

Let's deploy it:

```
$ fleetctl submit nginx-demo@.service
$ fleetctl list-unit-files
UNIT				HASH	DSTATE		STATE		TARGET
nginx-demo@.service		cbdebdc	inactive	inactive	-
```

And start it:

```
$ fleetctl start nginx-demo@1
$ fleetctl list-units
UNIT				MACHINE			ACTIVE	SUB
nginx-demo@1.service		09375bc3.../10.0.0.101	active	running
```

Now we can try to kill it:

```
$ docker kill nginx-demo
$ sleep 10
$ docker ps
CONTAINER ID IMAGE COMMAND     CREATED       STATUS       PORTS                                         NAMES
3f4f757facf1 nginx "nginx -g ' 2 seconds ago Up 1 seconds 0.0.0.0:32836->80/tcp, 0.0.0.0:32835->443/tcp nginx-demo
````

In this case, container was regenerated (see CREATED column) and started automatically after 10 seconds (`RestartSec` option).

The service can be seen at http://nginx-demo.cluster.local/

But in case of a total machine failure this is not enough, we need to get [Machine Failure Resilience](https://github.com/harbur/colossus/tree/master/docs/machineFailureResilience) also to achieve better stability.
