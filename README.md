# udm-utilities [![Slack](https://img.shields.io/badge/slack-boostchicken-blue.svg?logo=slack "Boostchicken Slack")](https://join.slack.com/t/boostchicken/shared_invite/zt-fcjszaw4-2ZuNFxIQnrpjxixnm17LXQ)

A fork of the [great boostchicken/udm-utilities](https://github.com/boostchicken/udm-utilities) which is a collection of things to enhance the capabilities of your Unifi Dream Machine or Dream Machine Pro. This fork has the notes and sample files updated to what I actually use.

## Contributing

This fork is primarily a learning experiment in hands on github use. Open to comments and questions in the form of issues or pull requests, but adding to do new cool stuff to your UDM/P should go the original, not this fork.

## Core: on-boot-script
**Do this first!** This is what enables complete customization of your UDM/P and survives through reboots and upgrades.

Enables init.d style scripts to run on every boot of your UDM.
**Fills the gap that config.gateway.json left behind.**

Includes examples to run on startup:
* wpa-supplicant/eap-proxy
* ntop-ng
* Follow this [readme](https://github.com/scpotter/udm-utilities/blob/master/on-boot-script/README.md).

## General Tools

### suricata (not used)
Run an updated version of suricata and apply custom rules

### python (not needed)

If you need python3 on your UDM, generally not recommended, can always use it in unifi-os container

## VPN Servers / Clients

### wireguard-go (not used)

Run a Wireguard client/server on your UDM/P.  Utilizes wireguard-go, not linux kernel modules.  The performance will take a hit due to that.

## DNS Providers
Install a DNS server that functions as a network-wide ad and tracker blocker, and which can also securely proxy encrypted DNS requests to an upstream DNS provider. Begin by following the instructions to setup [on-boot-script](https://github.com/scpotter/udm-utilities/tree/master/on-boot-script) and [dns-common](https://github.com/scpotter/udm-utilities/tree/master/dns-common/on_boot.d). Then, follow the guides below to setup either Pi-Hole, NextDNS, or AdGuard Home.

### dns-common 
Base configuration for DNS server containers, both IPv4 and IPv6.  Utilizes MacVLAN CNI plugins to completely isolate the network stack.

### run-pihole (not used)

Run pihole on your UDM with podman.
[Read

### nextdns (not used)

Run NextDNS on your UDM with podman.

### AdguardHome

Run AdguardHome on your UDM with podman.

## Cool projects you can use with this

### multicast-relay (not used)

<https://hub.docker.com/r/scyto/multicast-relay>

This is a docker container that implements <https://github.com/alsmith/multicast-relay> to provide mDNS and SSDP on a unifi dream machine. It will likely work on any multi homed host.

### ntopng

<https://github.com/tusc/ntopng-udm>

Much better network stats for your UDM/P!  Install this docker container and create an on_boot script to make sure it's always running.

### LetsEncrypt SSL Certs

<https://github.com/kchristensen/udm-le>

Provision and renew LetsEncrypt SSL certs from your UDM/P

### OpenConnect VPN
<https://github.com/shuguet/openconnect-udm>
OpenConnect VPN Client for the UniFi Dream Machine Pro (Unofficial)
