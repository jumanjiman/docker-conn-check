## Overview

Docker-based application container to provide
[conn-check](http://conn-check.org/) via
[PIP](https://pypi.python.org/pypi/conn-check).

[![Download size](https://images.microbadger.com/badges/image/jumanjiman/conn-check.svg)](http://microbadger.com/images/jumanjiman/conn-check "View on microbadger.com")&nbsp;
[![Version](https://images.microbadger.com/badges/version/jumanjiman/conn-check.svg)](http://microbadger.com/images/jumanjiman/conn-check "View on microbadger.com")&nbsp;
[![Docker Registry](https://img.shields.io/docker/pulls/jumanjiman/conn-check.svg)](https://registry.hub.docker.com/u/jumanjiman/conn-check)&nbsp;
[![Circle CI](https://circleci.com/gh/jumanjihouse/docker-conn-check.png?circle-token=9b167f5d055084e8422996ec63678e42859ec5d0)](https://circleci.com/gh/jumanjihouse/docker-conn-check/tree/master 'View CI builds')

Project URL: [https://github.com/jumanjihouse/docker-conn-check](https://github.com/jumanjihouse/docker-conn-check)
<br />
Docker hub: [https://registry.hub.docker.com/u/jumanjiman/conn-check/](https://registry.hub.docker.com/u/jumanjiman/conn-check/)
<br />
Image metadata: http://microbadger.com/images/jumanjiman/conn-check


### The Problem

http://1stvamp.org/verifying-post-deploy-connections-with-conn-check

Deployments of a service have a number of different network
dependencies that require verification:

* Connections between services (e.g. app <-> postgresql, are
  ports unblocked at the firewall(s)? If talking to a data centre
  instance do we have a route?)

* External services (e.g. webservices such as S3)

* Verification that the services on the other end are real
  (you're actually talking to MongoDB or rabbitmq via AMQP, not
  just another TCP service running on those ports)

* Verification of authentication

Although many of these can be solved with smoke tests, it's not
always immediately obvious that there is a problem, or what the
problem is.

conn-check takes a [simple YAML config](http://bazaar.launchpad.net/~ubuntuone-hackers/conn-check/trunk/view/head:/demo.yaml)
defining a list of checks to perform (udp, tcp, tls,
http, amqp, postgres, mongodb, redis, memcache), and
performs those checks asynchronously using Twisted's thread
pool, and outputs the results in a [Nagios check standard
output](https://nagios-plugins.org/doc/guidelines.html#AEN33), so
conn-check can be run regulary as a Nagios check to continually
verify network status between services (and alert on change,
e.g. out of band firewall changes).


### Usage

Run the docker container as if it were an application:

    docker run --rm -it jumanjiman/conn-check -h

Details about the options and its YAML config are at
http://bazaar.launchpad.net/~ubuntuone-hackers/conn-check/trunk/view/head:/README.rst


### Build integrity

An unattended test harness runs on CircleCI to build and test the image.
If all tests pass on master branch in the unattended test harness,
CircleCI pushes the built image to the Docker hub.

Available tags:

* Optimistic: `jumanjiman/conn-check:latest`
* Pessimistic: `jumanjiman/conn-check:<VERSION>-<date>T<time>-git-<hash>`

![workflow](assets/docker_hub_workflow.png)

A simple test harness uses [BATS](https://github.com/sstephenson/bats).
Output resembles:

    1..2
    ok 1 --help works
    ok 2 successfully checks TLS to google.com


### License

GNU GPLv3
