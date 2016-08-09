# swarm-bootstrap-virtualbox

Simple script to set up a Docker 1.12 swarm based on virtualbox nodes created by `docker-machine`.

## Prerequisites

* Docker v1.12
* Docker Machine v0.8.0

For Mac and Windows, use Docker for Mac and Docker for Windows respectively. For Ubuntu, see:

https://docs.docker.com/engine/installation/linux/ubuntulinux/
https://github.com/docker/machine/releases/

My [gist](https://gist.github.com/subfuzion/fd9e6977717e17938f1019bb761abcd1) may help, but you need to update for the correct version of ubuntu you're running (ex, `ubuntu-wily`) and update the docker-machine version from `v0.7.0` to v0.8.0`.

## bootstrap

Update the script to set the number of managers and workers you want to use. For managers, you always want to use 1, 3, or 5. For virtualbox on localhost, 3 should is enough if you're evaluating HA, otherwise just use 1.

## Using the swarm

In this example, the `bootstrap` script was configured with `MANAGERS=1` AND `WORKERS=2`.

List nodes

    $ docker $(m config m1) node ls

Start a sample service

    $ docker $(docker-machine config m1) service create --replicas 3 --name pinger alpine ping docker.com
    5lxnahrd0895hxj181e9oxdsn

List the service

    $ docker $(docker-machine config m1) service ls
    ID            NAME    REPLICAS  IMAGE   COMMAND
    5lxnahrd0895  pinger  3/3       alpine  ping docker.com

List the service tasks (corresponds to containers)

    $ docker $(docker-machine config m1) service ps pinger
    ID                         NAME      IMAGE   NODE  DESIRED STATE  CURRENT STATE           ERROR
    3g0qenvnnu6szwbhbvwevt9n4  pinger.1  alpine  m1    Running        Running 51 seconds ago
    1y3t534iszzc0ln691k7pax22  pinger.2  alpine  w1    Running        Running 51 seconds ago
    d3tc9i8h83ws5qwo4y3rprhx4  pinger.3  alpine  w2    Running        Running 51 seconds ago
