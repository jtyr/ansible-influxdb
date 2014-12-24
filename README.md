influxdb
========

Role which installs and configures InfluxDB. This role doesn't download the
influxdb package. In order to install it, it needs to be put into a repo.


Example
-------

```
---

# Example of how to use the role
- hosts: myhost
  roles:
    - influxdb
```


Role variables
--------------

List of variables used by the role:

```
# Default configuration
influxdb_config:
  '':
    bind-address: 0.0.0.0
    reporting-disabled: 'true'
  logging:
    level: info
    file: /opt/influxdb/shared/log.txt
  admin:
    port: 8083
  api:
    port: 8086
    read-timeout: 5s
  input_plugins:
    input_plugins.graphite:
      enabled: 'false'
    input_plugins.collectd:
      enabled: 'false'
    input_plugins.udp:
      enabled: 'false'
    input_plugins.udp_servers:
      - enabled: 'false'
  raft:
    port: 8090
    dir: /opt/influxdb/shared/data/raft
    debug: 'false'
  storage:
    dir: /opt/influxdb/shared/data/db
    write-buffer-size: 10000
    default-engine: rocksdb
    max-open-shards: 0
    point-batch-size: 100
    write-batch-size: 5000000
    retention-sweep-period: 10m
  storage.engines.leveldb:
    max-open-files: 1000
    lru-cache-size: 200m
  storage.engines.rocksdb:
    max-open-files: 1000
    lru-cache-size: 200m
  storage.engines.hyperleveldb:
    max-open-files: 1000
    lru-cache-size: 200m
  storage.engines.lmdb:
    map-size: 100g
  cluster:
    protobuf_port: 8099
    protobuf_timeout: 2s
    protobuf_heartbeat: 200ms
    protobuf_min_backoff: 1s
    protobuf_max_backoff: 10s
    write-buffer-size: 1000
    max-response-buffer-size: 100
    concurrent-shard-query-limit: 10
  wal:
    dir: /opt/influxdb/shared/data/wal
    flush-after: 1000
    bookmark-after: 1000
    index-after: 1000
    requests-per-logfile: 10000
```


License
-------

MIT


Author
------

Jiri Tyr
