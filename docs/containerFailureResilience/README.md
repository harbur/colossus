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
$ docker rm -v nginx
nginx
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

## Docker Auto-restart

In order to achieve container failure resilience, we can define the containers to auto-start on failure.
