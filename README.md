# swarmstack/influxdb

Docker compose file for InfluxDB OSS version, also useful for Prometheus long-term storage

## DEPLOY INFLUXDB AS A STACK

```
INFLUXDB_ADMIN_USER='admin' \
INFLUXDB_ADMIN_PASSWORD='admin' \
INFLUXDB_USER='prometheus' \
INFLUXDB_USER_PASSWORD='prompass' \
docker stack deploy -c docker-compose.yml influxdb
```

Or you can take some or all of the defaults above:

    docker stack deploy -c docker-compose.yml influxdb

[swarmstack](https://github.com/swarmstack/swarmstack) users should use docker-compose-swarmstack.yml instead.


## PROMETHEUS REMOTE READ/WRITE DATABASE (Optional)

Add remote-write and remote-read stanzas to your Prometheus configuration in order to use InfluxDB to store Prometheus metrics longer-term. [swarmstack](https://github.com/swarmstack/swarmstack) users can add the below to localswarmstack/prometheus/conf/prometheus.yml, otherwise just substitute http://influxdb with your Influx address:

```
alerting:
  alertmanagers:
  - static_configs:
    - targets: [ 'alertmanager:9093', 'alertmanagerB:9093' ]

remote_write:
  - url: "http://influxdb:8086/api/v1/prom/write?db=prometheus&u=prometheus&p=prompass"

remote_read:
  - url: "http://influxdb:8086/api/v1/prom/read?db=prometheus&u=prometheus&p=prompass"
```

## GRAFANA DASHBOARD

A [dashboard](https://grafana.com/grafana/dashboards/11032) which nicely visualizes all 'internal' Grafana OSS metrics documented at [InfluxData.com](https://docs.influxdata.com/platform/monitoring/influxdata-platform/tools/measurements-internal/) and exported via [influxdb_stats_exporter](https://github.com/carlpett/influxdb_stats_exporter) into Prometheus.
