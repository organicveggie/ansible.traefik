# Ansible Role: traefik


[![Build Status](https://img.shields.io/travis/arillso/ansible.traefik.svg?branch=master&style=popout-square)](https://travis-ci.org/arillso/ansible.traefik)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-traefik-blue.svg?style=popout-square)](https://galaxy.ansible.com/arillso/traefik)
<!-- [![Ansible Role](https://img.shields.io/ansible/role/d/0.svg?style=popout-square)](https://galaxy.ansible.com/arillso/traefik) -->

## Description
[Traefik](https://docs.traefik.io/v2.0) is a reverse proxy written in Go.
It can be used in multiple situations with many providers (Kubernetes, Swarm,
...). Version 2 is also capable of TCP routing.

This role sets up traefik on a host as reverse proxy and load balancer. This
allows you, to use one server as a host for multiple dockerized applications.

> **Note:** This role allows you to use one (1) server as a host for many
> applications. Depending on your usecase, this might not be what you are
> looking for. For services that need to be highly-available, consider using
> Kubernetes or other systems and setup traefik there.

## Installation
```
ansible-galaxy install arillso.traefik
```

## Requirements
* Docker

## Role Variables
Traefik v2.0 onwards supports yaml configuration. This role uses this to generate
the configuration directly from the given ansible variables.
There are certain quick-setup variables, which allow you to setup a simple
instance, but there is also the option to fully configure every key yourself.
The quick-setup allows you to:
* Setup a lets-encrypt based certificate resolver
* Setup standard entrypoints
* Setup standard Docker provider

The quick-setup variables are prefixed with `traefik_qs_`.

| Name                              | Default                      | Description                            |
|:--------------------------------- |:---------------------------- |:-------------------------------------- |
| `traefik_dir`                     | `/etc/traefik`               | where to store traefik data            |
| `traefik_hostname`                | `"{{ inventory_hostname }}"` | the hostname of this instance          |
| `traefik_network`                 | `traefik_proxy`              | the name of the generated network      |
| `traefik_qs_send_anonymous_usage` | `false`                      | wether to send anonymous usage         |
| `traefik_qs_https`                | `false`                      | wether to setup a https endpoint       |
| `traefik_qs_https_redirect`       | `false`                      | wether to setup http to https redirect |
| `traefik_qs_log_level`            | `ERROR`                      | the loglevel to apply                  |
| `traefik_qs_api`                  | `false`                      | wether to setup api access             |

The default names of the generated configs are:
* Entrypoints:
  * `web_http`
  * `web_https`
* Providers:
  * `docker`


### In-Depth Configuration
As stated before, this role also allows you to configure traefik very in-depth by
using the traefik yaml config. The following variables can be used:

| Name                                    | Default | Description                                                                    |
|:--------------------------------------- |:------- | ------------------------------------------------------------------------------ |
| `traefik_confkey_global`                | `{}`    | [see Docs 📑](https://docs.traefik.io/reference/static-configuration/file/)    |
| `traefik_confkey_serversTransport`      | `{}`    | [see Docs 📑](https://docs.traefik.io/reference/static-configuration/cli-ref/) |
| `traefik_confkey_entryPoints`           | `{}`    | [see Docs 📑](https://docs.traefik.io/routing/entrypoints/#entrypoints)        |
| `traefik_confkey_providers`             | `{}`    | [see Docs 📑](https://docs.traefik.io/routing/providers/docker/)               |
| `traefik_confkey_metrics`               | `{}`    | [see Docs 📑](https://docs.traefik.io/observability/metrics/overview/)         |
| `traefik_confkey_ping`                  | `{}`    | [see Docs 📑](https://docs.traefik.io/operations/ping/)                        |
| `traefik_confkey_log`                   | `{}`    | [see Docs 📑](https://docs.traefik.io/observability/logs/)                     |
| `traefik_confkey_accessLog`             | `{}`    | [see Docs 📑](https://docs.traefik.io/observability/access-logs/)              |
| `traefik_confkey_tracing`               | `{}`    | [see Docs 📑](https://docs.traefik.io/observability/tracing/overview/)         |
| `traefik_confkey_hostResolver`          | `{}`    | [see Docs 📑](https://docs.traefik.io/reference/static-configuration/file/)    |
| `traefik_confkey_certificatesResolvers` | `{}`    | [see Docs 📑](https://docs.traefik.io/https/acme/#certificate-resolvers)       |

These keys are merged into the configuration **after** the quick-setup config using
the [`combine()`](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html#combining-hashes-dictionaries)
filter in non recursive mode. This allows you to add configuration options as
you need them. If you want to overwrite the quick-setup items, use their key
(as specified above).
