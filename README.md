# swarmstack/influxdb

Docker compose file for InfluxDB

## USAGE:

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

You'll also need to add remote-write and remote-read stanzas to your localswarmstack/prometheus/conf/prometheus.yml:

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
