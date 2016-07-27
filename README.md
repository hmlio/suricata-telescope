# dockerized suricata as a network telescope

[suricata](http://suricata-ids.org/) is a Network IDS, IPS and Network Security Monitoring engine.

The logging mechanism in Suricata is ideal for implementing the concept of a network telescope a.k.a. internet background noise monitor a.k.a. darknet a.k.a. greynet::
set up in an unused ip address space, Suricata can record access to the network and log the packet details in it's JSON EVE log for further analysis.

In this particular setup the following specifics and restrictions apply:
* Suricata will monitor only UDP traffic and collect the payload thrown at it (BPF in supervisord script)
* all config settings for TCP based services have been deactivated in the config

This project is based on the dockerized version of Suricata as used in the **[T-Pot community honeypot](http://dtag-dev-sec.github.io/)** of Deutsche Telekom AG.

In other words: this configurastion provided here does not work for "generic" setups but ruther requires to be run in the T-Pot environment.

The `Dockerfile` contains the blueprint for the dockerized suricata and will be used to setup the docker image, however it has been stripped of the not used parts like the p0f part.

The `suricata.yaml` has been modified to contain the relevant part for payload logging and is further tailored to fit the T-Pot environment.

The `supervisord.conf` is used to start suricata under supervision of supervisord.

Using upstart, copy the `upstart/suricata.conf` to `/etc/init/suricata.conf` and start using

    service suricata start

This will make sure that the docker container is started with the appropriate rights and port mappings. Further, it autostarts during boot.

By default all data will be stored in `/data/suricata/` until the suricata service will be restarted which is by default every 24 hours. If you want to keep data persistently simply rename `/data/persistence.off` to `/data/persistence.on`. Be advised to establish some sort of log management if you wish to do so.

# Suricata Telescope Dashboard

![Suricata Dashboard](https://raw.githubusercontent.com/dtag-dev-sec/suricata/master/doc/dashboard.png)
