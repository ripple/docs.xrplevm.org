# Monitoring the Node

## Introduction

Monitoring is an essential aspect of maintaining a healthy and efficient XRPL EVM sidechain node. By continuously tracking the node's performance and behavior, you can detect potential issues early, ensure optimal resource utilization, and maintain network stability.

This guide explains how to enable monitoring for the XRPL EVM sidechain node using Grafana and Prometheus. We assume that you already have Grafana and Prometheus set up either through an on-premise deployment or via Grafana Cloud. If you do not have these tools configured yet, please refer to the following guides:

- [Get started with Grafana Cloud](https://grafana.com/docs/grafana-cloud/get-started/)
- [Introduction to monitoring with Prometheus & Grafana](https://medium.com/@dineshmurali/introduction-to-monitoring-with-prometheus-grafana-ea338d93b2d9)
- [How to Install Prometheus and Grafana on Ubuntu 22.04 LTS](https://ibrahims.medium.com/how-to-install-prometheus-and-grafana-on-ubuntu-22-04-lts-configure-grafana-dashboard-5d11e3cb3cfd)
- [How to setup Monitoring using Prometheus and Grafana](https://medium.com/backstagewitharchitects/how-to-setup-monitoring-using-prometheus-and-grafana-9ba7bf8cbda9)

## Enabling Monitoring in `config.yaml`

To enable Prometheus metrics collection for your XRPL EVM sidechain node, you need to update the `config.yaml` file with the following values:

```yaml
#######################################################
###       Instrumentation Configuration Options     ###
#######################################################
[instrumentation]

# When true, Prometheus metrics are served under /metrics on
# PrometheusListenAddr.
# Check out the documentation for the list of available metrics.
prometheus = true

# Address to listen for Prometheus collector(s) connections
prometheus_listen_addr = ":26660"

# Maximum number of simultaneous connections.
# If you want to accept a larger number than the default, make sure
# you increase your OS limits.
# 0 - unlimited.
max_open_connections = 3

# Instrumentation namespace
namespace = "cometbft"
```

Once these changes are applied, restart the node to activate monitoring.

## Exporting Metrics to Grafana

After configuring Prometheus to collect metrics from your node, you need to integrate these metrics into Grafana. To do this:

1. Ensure that Prometheus is scraping the node's metrics by adding the following job to your Prometheus configuration (`prometheus.yml`):

   ```yaml
   scrape_configs:
     - job_name: "xrpl-evm-node"
       static_configs:
         - targets: ["<node_ip>:26660"]
   ```

2. Restart Prometheus to apply the changes.
3. Open Grafana and add Prometheus as a data source.
4. Import a preconfigured dashboard or create a custom dashboard to visualize node performance metrics.

## Recommended Dashboard

For an optimized monitoring experience, you can use the following preconfigured dashboard:

- [XRPL EVM Node Monitoring Dashboard](https://grafana.com/grafana/dashboards/22701)

This dashboard provides detailed insights into node performance, including CPU usage, memory consumption, network activity, and transaction processing.

By following this guide, you can ensure effective monitoring of your XRPL EVM sidechain node, allowing for proactive maintenance and optimal performance.
