# Readme

[![Build Status](https://jenkins.confdroid.com/buildStatus/icon?job=puppet_collection)](https://jenkins.confdroid.com/job/puppet_collection/)
[![Quality Gate Status](https://sonarqube.confdroid.com/api/project_badges/measure?project=puppet_collection&metric=alert_status&token=sqb_912f5ceda77ac9c70271a41b0f039fad50499074)](https://sonarqube.confdroid.com/dashboard?id=puppet_collection)

- [Readme](#readme)
- [Summary](#summary)
- [Overview](#overview)
  - [**puppet\_cd**](#puppet_cd)
  - [**confdroid\_prometheus**](#confdroid_prometheus)
  - [**confdroid\_postgresql**](#confdroid_postgresql)

# Summary

I have been writing Puppet modules since 2010, starting with Puppet 3 at the time. 15 years later we have Puppet 8 and I built about 75 Puppet modules for various use cases as they come along with my [lab cloud project](https://confdroid.com/portfolio/), where I am building and hosting services both standalone and in Kubernetes. The hosts running the show are managed with Puppet.

Below is a collection of the Puppet modules I have written and published over time. Right now I am overhauling the modules for public use and will publish them one by one when ready, so feel fee to follow and watch that list grow.

All modules are specifically designed for  use with External Node Classifier (ENC), i.e. Foreman, and written for Rocky 9 and Puppet 8.  Any other OS using yum / dnf and systemd should work as well, but I have not tested it nor will I for the lack of resources.

The modules themselves are free to use as per license, you might need to adjust to your own needs. Feedback and feature requests can be given at my [feedback portal](https://feedback.confdroid.com/).


[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/grizzly_coda)


# Overview

## [**puppet_cd**](https://gitea.confdroid.com/confdroid/puppet_cd)

A Puppet module to configure a puppet environment:
  * Puppet master ready to work with Foreman as ENC (Foreman not installed by module)
  * Puppet agents
  * configures firewall ports, files and directories including selinux contexts.
  * Optionally: PuppetDB, r10k and a webhook listener

## [**confdroid_prometheus**](https://gitea.confdroid.com/confdroid/confdroid_prometheus)

Configures Prometheus, a Time Series Collection and Processing server
  * installs and configures Prometheus
  * optionally the Node exporter
  * optionally adds remote writing to a Postgresql database via postgresql Adapter ( not part of this module)
  * Optionally allows pruning of the local TSDB

## [**confdroid_postgresql**](https://gitea.confdroid.com/confdroid/confdroid_postgresql)

Automate installation, configuration and management of all aspects of PostgreSQL(standalone)
  * installs and configures Postgres server and clients
  * manage single line entries in pg_hba via define
  * manage roles and databases via define (set `$pl_manage_content` to true)
  * manage extensions (set `pl_manage_extensions` to true)
  * install and manage pg_bouncer (set `pl_use_pg_bouncer` to true)
  * enable SL / TLS manage TLS certificates (set `pl_ssl_enabled` to true and populate content externally through variables)
