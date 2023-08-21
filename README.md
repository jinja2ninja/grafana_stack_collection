# Ansible Collection - fahcsim.grafana_stack
[![CI](https://github.com/fahcsim/grafana_stack_collection/actions/workflows/prometheus.yml/badge.svg)](https://github.com/fahcsim/grafana_stack_collection/actions/workflows/prometheus.yml)
[![Release](https://github.com/fahcsim/grafana_stack_collection/actions/workflows/release.yml/badge.svg)](https://github.com/fahcsim/grafana_stack_collection/actions/workflows/release.yml)
### Version 1.4.1
This should be enough to set up centralized logging and monitoring fairly easily. Prometheus will be configured to monitor every host in the inventory (see roles/prometheus/templates/prometheus.yml.j2). Loki agents will point to the loki server(configured as `loki_server` variable). All of these services are installed as binaries with systemd.
### This collection has a role for each of the following:
- Prometheus
- Prometheus Alertmanager
- Prometheus Node Exporter
- Loki
- Promtail
## Requirements
For the prometheus config to work correctly, the ansible inventory hostname of each server must be a DNS name that your prometheus server can resolve.
Current supported distributions are Debian 10/11 and Ubuntu 20/22. It may work on other distros, but it has not been tested.
## Example Playbook
```
- name: bootstrap monitoring server
  hosts: monitoring
  become: True
  vars:
    loki_server: loki_server.local
  roles:
    - fahcsim.grafana_stack.prometheus
    - fahcsim.grafana_stack.loki
    - fahcsim.grafana_stack.prometheus_node_exporter
    - fahcsim.grafana_stack.prometheus_alert_manager
    - fahcsim.grafana_stack.promtail
    - fahcsim.grafana_stack.grafana
```
### Updating
Each service can be updated by changing the `<service>_version` variable, as long as a corresponding version exists in the project's github repo.
#### Current Defaults
##### Prometheus
`prometheus_version: 2.42.0`
`prometheus_retention_time: "15d"`
`prometheus_external_url: ""`
`extra_prometheus_config: <additional prometheus config goes here` - If you want to add custom prometheus configuration, put it in this variable.
##### Prometheus Alert Manager
`alert_manager_version: 0.25.0`
`pushover: true` - This variables defines whether or not to include the Pushover config section.
`pushover_token: <pushover token>`
`pushover_key: <pushover user key>`
`alertmanager_external_url: ""`
`extra_alertmanager_config: <additional alertmanager config goes here` - If you're not using pushover, or have additional config, put it in this variable.
##### Loki
`loki_version: 2.7.3`
##### Prometheus Node Exporter
`node_exporter_version: 1.5.0`
##### Promtail
`promtail_version: 2.7.3`
`loki_server: localhost` - change this to whatever host you want to use as the loki server

### Node Exporter Offline Installation
set the variable `node_exporter_binary_local_dir` to the path of the node exporter binary you copied locally

### Customization

#### Prometheus

```
- name: bootstrap monitoring server
  hosts: monitoring
  become: True
  vars:
    alertmanager_external_url: "http://example.org:9093"
    extra_prometheus_config: |
      rule_files:
        - /etc/prometheus/rules.d/alert.rules.yml
  roles:
    - fahcsim.grafana_stack.prometheus
```

#### Promtail
Several variables can be used to customize the config file for promtail:
- `custom_server_config` - If defined this will replace the "server" section of the config with whatever is defined in the variable.
- `custom_scrape_configs` - If defined this will replace the "scrape_configs" section of the config with whatever is defined in the variable.
- `loki_server` - If defined this will replace the loki server's address (if using this, you must also define the port as shown below)
- `loki_port` - If defined this will replace the loki server's port (if using this, you must also define the address as shown above)
