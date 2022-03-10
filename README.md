# RedHat Klaassen OCP

This repo contains cluster specific config files for my ocp installation relating to this repo: https://github.com/RedHat-EMEA-SSA-Team/hetzner-ocp4/tree/master

## Structure

* `config/` contains default resources for initial cluster setup
* `server.config` basic host configuration
* `cluster-example.yaml` basic cluster config without any information which should be protected

## CentOS 8: Change mirrors to vault.centos.org

In order to avoid https://github.com/RedHat-EMEA-SSA-Team/hetzner-ocp4/issues/205 I decided to switch to archived mirror as long there are problems with  CentOS 8 Stream.

`cd /etc/yum.repos.d/`

`sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*`

`sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*`

`yum update -y`
