# RedHat Klaassen OCP

This repo contains cluster specific config files for my ocp installation relating to this repo: https://github.com/RedHat-EMEA-SSA-Team/hetzner-ocp4/tree/master

## Structure

* `initial-config/` contains default resources for initial cluster setup
* `gitops/` stuff to deploy after initial cluster setup
* `server.config` basic host configuration
* `cluster-example.yaml` basic cluster config without any information which should be protected

## CentOS 7: Change mirrors to vault.centos.org

In order to avoid https://github.com/RedHat-EMEA-SSA-Team/hetzner-ocp4/issues/205 I decided to switch to archived mirror as long there are problems with CentOS 8 Stream.

```
sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
```

```
sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
```

```
yum update -y
```

## SOPS Secret Handling

List Your Keys
```
gpg --list-keys
```

encrypt ocp.yaml
```
sops --encrypt --in-place --encrypted-regex 'public_domain|letsencrypt_account_email|hetzner_account_api_token|hetzner_zone|auth_htpasswd|image_pull_secret' --pgp <your-pub> ocp.yaml
```

decrypt ocp.yaml
```
sops --decrypt ocp.yaml
```

encrypt initial-config/github-secret.yaml
```
sops --encrypt --in-place --encrypted-regex 'data' --pgp <your-pub> initial-config/github-secret.yaml
```

decrypt initial-config/github-secret.yaml
```
sops --decrypt initial-config/github-secret.yaml
```

## How to find install logs

If you like to follow the installation run 'tail -f  /root/hetzner-ocp4/ansible/../ocp4/.openshift_install.log' in a second terminal. For more details, connect to the bootstrap node: ssh -l core <ip-address>.
