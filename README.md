# Ansible Collection - fahcsim.grafana_stack
[![CI](https://github.com/fahcsim/grafana_stack_collection/actions/workflows/prometheus.yml/badge.svg)](https://github.com/fahcsim/grafana_stack_collection/actions/workflows/prometheus.yml)
### This collection has a role for each of the following:
- Prometheus
- Loki
- Prometheus Node Exporter
- Promtail

This should be enough to set up centralized logging and monitoring fairly easily. Prometheus will be configured to monitor every host in the inventory (see roles/prometheus/templates/prometheus.yml.j2). Loki agents will
Current supported distributions are Debian 10/11 and Ubuntu 20/22.

### Vars
##### Grafana

##### Prometheus
- `version: 2.35.0`
##### Loki
- `version: 2.5.0`
##### Prometheus Node Exporter
- `version: 1.3.1`
##### Promtail
- `version: 2.5.0`
- `loki_server: localhost` - change this to whatever host you want to use as the loki server
