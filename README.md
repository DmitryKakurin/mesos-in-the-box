mesos-in-the-box
==========

Setup of HolidayCheck AG "dust project" cluster based on docker images.

This setup was made as a PoC to see if rollout of mesos+marathon+docker infrastructure is possible via dockers only.

Also it can serve as a temporary mesos cluster for experimental and development purposes.

Setup
=====

This setup has:
 - 1 instance of zookeeper ver 3.4.6
 - 1 instance of mesos-master ver 0.26.0
 - 1 instance of mesos-slave  ver 0.26.0
 - 1 instance of marathon ver 0.15.2
 - 1 instance of chronos ver 2.4.0
 - 1 instance of bamboo ver 0.2.15  with haproxy ver 1.5.15


Usage
=====

To run this cluster you need to have **Linux**, **docker 1.8.0** and **docker-compose 1.2.0** installed (Mac users running boot2docker please adjust address setting in docker-compose.yml from localhost to proper IP).
Checkout this repository and run **docker-compose up -d** inside.

Sometimes due to random order of pulling docker images bamboo enters locked state while not able to find zookeeper. If experienced try to run **docker-compose restart**

Example deploy
==============

To deploy example container (Ghost blogging platform) enter following command:

curl -X POST -H "Content-Type: application/json" http://localhost:8080/v2/apps -d@ghost.json

Afterwards navigate to http://localhost:8000 and set desired mapping ACL for haproxy (for example "path_beg -i /").

If you now navigate to http://localhost you should be able to see the app.

Notice
======

This setup uses net:host docker parameter. It means, docker configures a bridge between container network interface and network interdace of your host.
Therefore ensure these ports are available:
 - 2181, 2888, 3888 (zookeeper)
 - 5050, 5051 (mesos master and slave)
 - 8080 (marathon)
 - 4400 (chronos)
 - 80, 8000 (haproxy, bamboo)

Links
=====
- Marathon: **http://localhost:8080**
- Chronos: **http://localhost:4400**
- Mesos: **http://localhost:5050**
- Bamboo: **http://localhost:8000**
- Haproxy: **http://admin:admin@localhost/haproxy_stats**

Todo
====
Write a HOWTO for clustering this configuration.

Credits
=======

Used repositiories and docker images:
 - https://hub.docker.com/r/mesosphere/mesos-master/
 - https://hub.docker.com/r/mesosphere/mesos-slave-dind/
 - https://hub.docker.com/r/mesoscloud/chronos/
 - https://hub.docker.com/r/mesoscloud/zookeeper/
 - https://hub.docker.com/r/mesosphere/marathon/
 - https://github.com/QubitProducts/bamboo

