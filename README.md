# Readme

[![Build Status](https://jenkins.confdroid.com/buildStatus/icon?job=puppet_collection)](https://jenkins.confdroid.com/job/puppet_collection/)
[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/grizzlycoda/puppet_collection)

- [Readme](#readme)
  - [Summary](#summary)
  - [Overview](#overview)
    - [confdroid\_puppet](#confdroid_puppet)
    - [confdroid\_prometheus](#confdroid_prometheus)
    - [confdroid\_postgresql](#confdroid_postgresql)
    - [confdroid\_apache](#confdroid_apache)
    - [confdroid\_gitea](#confdroid_gitea)
    - [confdroid\_php](#confdroid_php)
    - [confdroid\_nrpe](#confdroid_nrpe)
    - [confdroid\_nagios](#confdroid_nagios)
    - [confdroid\_fail2ban](#confdroid_fail2ban)
    - [control repo template](#control-repo-template)
    - [confdroid\_automatic](#confdroid_automatic)
    - [confdroid\_selinux](#confdroid_selinux)
    - [confdroid\_ssh](#confdroid_ssh)
    - [confdroid\_java](#confdroid_java)
    - [confdroid\_alloy](#confdroid_alloy)
    - [confdroid\_haproxy](#confdroid_haproxy)
    - [confdroid\_clamav](#confdroid_clamav)
    - [confdroid\_ntp](#confdroid_ntp)
  - [FAQ](#faq)

## Summary

I have been writing Puppet modules since 2010, starting with Puppet 3 at the time. 15 years later we have Puppet 8 and I built about 75 Puppet modules for various use cases as they come along with my [lab cloud project](https://confdroid.com/portfolio/), where I am building and hosting services both standalone and in Kubernetes. The hosts running the show are managed with Puppet.

Below is a collection of the Puppet modules I have written and published over time. Right now I am overhauling the modules for public use and will publish them one by one when ready, so feel fee to follow and watch that list grow.

All modules are specifically designed for  use with External Node Classifier (ENC), i.e. Foreman, and written for Rocky 9 and Puppet 8.  Any other OS using yum / dnf and systemd should work as well, but I have not tested it nor will I for the lack of resources.

The modules themselves are free to use as per license, you might need to adjust to your own needs. Feedback and feature requests can be given at my [feedback portal](https://feedback.confdroid.com/).

[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/grizzly_coda)

## Overview

### [confdroid_puppet](https://3for.me/fcjai)

A Puppet module to configure a puppet environment:

- Puppet master ready to work with Foreman as ENC (Foreman not installed by module)
- Puppet agents
- configures firewall ports, files and directories including selinux contexts.
- Optionally:
  - PuppetDB
  - r10k deployment service
  - webhook listener to trigger r10k

### [confdroid_prometheus](httpshttps://3for.me/vl9de)

Configures Prometheus, a Time Series Collection and Processing server

- installs and configures Prometheus
- optionally the Node exporter
- optionally adds remote writing to a Postgresql database via postgresql Adapter ( not part of this module)
- Optionally allows pruning of the local TSDB

### [confdroid_postgresql](https://3for.me/oja4z)

Automate installation, configuration and management of all aspects of PostgreSQL(standalone)

- installs and configures Postgres server and clients
- manage single line entries in pg_hba via define
- manage roles and databases via define (set `$pl_manage_content` to true)
- manage extensions (set `pl_manage_extensions` to true)
- install and manage pg_bouncer (set `pl_use_pg_bouncer` to true)
- enable SL / TLS manage TLS certificates (set `pl_ssl_enabled` to true and populate content externally through variables)

### [confdroid_apache](https://3for.me/kdo6j)

Install and configure a standalone empty Apache (httpd) server. The module is mainly to be used by other modules to add websites or services on top, i.e. Nagios, Wordpress etd.

- install the packages
- manage main files and directories
- ensure the service is up and running
- open the firewall

### [confdroid_gitea](https://3for.me/s0tka)

Install and configure a gitea standalone instance from non-packaged binaries.

- install binary
- manage main files and directories
- manage service file and status
- manage the firewall port

### [confdroid_php](https://3for.me/b5doc)

Install and configure PHP to allow multiple applications with different settings to run in parallel. Custom php.ini settings should be configured with the application in question.

- install php
- manage required directories including selinux contexts and permissions
- manage required files including selinux contexts and permissions

### [confdroid_nrpe](https://3for.me/77w07)

Install NRPE as client for Nagios to query the status of hosts and their services.

- manage NRPE service user properties
- manage directory structure (file system permissions, selinux context) through parameters
- manage configuration files through parameters:
  - nrpe.conf
  - nrpe.cfg
- manage sudo role for nagios user on NRPE clients
- manage dynamic NRPE check command definitions
- manage iptables (optional)
- manage selinux rule exceptions (optional)
- manage NRPE service

### [confdroid_nagios](https://3for.me/bh1d2)

Install, configure and fully populate Nagios via Exports in PuppetDB

- manage configuration files through parameters:
  - nagios.cfg
  - cfi.cfg
- manage iptables (optional)
- manage service
- export client targets (hosts, services)
- import resources in nagios (hosts, services, commands, contacts etc.)

### [confdroid_fail2ban](https://3for.me/fqqjy)

Install, configure and manage fail2ban to protect hosts and services against brute force attacks.

- manage directory structure  (file system permissions, selinux context)
- manage configuration files   (file system permissions, selinux context, content based on parameters)
  - fail2ban.conf
  - fail2ban.local (overrides default settings in fail2ban.conf)
  - jail.conf
  - jail.local (overrides default settings in jail.conf)
- manage service

### [control repo template](https://3for.me/nq00s)

A template for starting a r10k control repo in a new puppet infrastructure

### [confdroid_automatic](https://3for.me/wglig)

Install, configure and manage automatic OS system updates on RHEL-9 like operating systems using DNF.

- install required binaries,
- manage files and directories with proper selinux context
- manage main configurations files via parameters
- manage service

### [confdroid_selinux](https://3for.me/z2tsi)

- Install binaries required for selinux and related tools
- manage /etc/sysconfig/selinux file (file system permissions, selinux context, content)
- manage current selinux status (permissive,enforcing)

### [confdroid_ssh](https://3for.me/jhjim)

Install, configure and manage SSHD and ssh settings

- install required binaries
- manage local custom configuration based on parameters, overriding the defaults
- manage selinux rules
- manage service
- (optional) manage firewall

### [confdroid_java](https://3for.me/erwc5)

Install java binaries for usage with other modules, i.e. confdroid_jenkins, as specified in parameters.

### [confdroid_alloy](https://3for.me/dcckx)

- install alloy binaries
- install node exporter (optional, set `ay_manage_ne`to `true`)
- manage alloy directories and files including templates
- manage the main alloy pipeline for loki and prometheus
- manage log rotation for rsyslog to allow adm membership
- manage alloy service

### [confdroid_haproxy](https://3for.me/rovxr)

install, configure and manage haproxy service configure proxies via define access

- install rpm binaries
- manage haproxy user
- manage directories (file system permissions, selinux context)
- manage files (file system permissions, content, selinux context)
- manage proxy instances through a define
- front-end options
- back-end options
- ACL options
- adds haproxy logs to rsyslog for remote logging where used
- manage fail2ban integration (optional, requires confdroid_fail2ban module)
- manage haproxy service and stats service

### [confdroid_clamav](https://3for.me/xkozd)

install, configure and manage clamav / clamd

- install all required binaries including s-nail as mail program to send alerts when infected files are found
- manage required config files
- manage cronjob settings via parameters
- run cron job to scan the file system (by default starting at root, configurable) via clamdscan
- set ConcurrentDatabaseReload option `yes`/`no` to optimize RAM consumption. defaults to `no`
- manage `freshclam` and `clamd` services

### [confdroid_ntp](https://3for.me/nbrws)

install, configure and manage chrony as ntp server or client

- install all required binaries
- manage required config files and directories
- manage optional monitoring through nagios (service)
- manage optional remote logging through alloy/loki
- manage optional nts authentication client side
- manage `chronyd` service

---

## FAQ

- Q: "Why are the names of the modules using underscore instead of hyphens?"
  A: The modules are best deployed through the [R10k](https://github.com/puppetlabs/r10k) service using a Puppetfile. The deployment process using Puppetfile would convert the name of say "confdroid-postgresql" into a module called "confdroid" locally on the puppet server, cutting off everything after the hyphen. It also would then not deploy more than one module, because they all would be called "confdroid"
- Q: "why is always only Rocky 9 listed as supported OS"?
  A: In fact **any** RHEL-9 based OS should work. But  all modules are thoroughly being tested under Rocky 9 as OS of choice.
