influxdb
========

Ansible role which installs and configures InfluxDB.

The configuraton of the role is done in such way that it should not be necessary
to change the role for any kind of configuration. All can be done either by
changing role parameters or by declaring completely new configuration as a
variable. That makes this role absolutely universal. See the examples below for
more details.

Please report any issues or send PR.


Examples
--------

```
---

- name Example of how to use the role with default parameters
  hosts: myhost1
  roles:
    - influxdb

- name: Example how to customize some of the role parameters
  hosts: myhost2
  vars:
    # Enable Graphite listener
    influxdb_config_graphite_list__default_enabled: "true"
  roles:
    - influxdb

- name: Example of how to white the config file from scratch
  hosts: myhost3
  vars:
    # Very minimalistic config
    influxdb_config:
      meta:
        dir: /var/lib/influxdb/meta
      data:
        dir: /var/lib/influxdb/data
        wal-dir: /var/lib/influxdb/wal
      coordinator: {}
      retention: {}
      shard-precreation: {}
      monitor: {}
      http: {}
      subscriber: {}
      graphite: []
      collectd: []
      opentsdb: []
      udp: []
      continuous_queries: {}
  roles:
    - influxdb
```


Role variables
--------------

List of variables used by the role:

```
# Yumrepo URL
influxdb_yumrepo_url: https://repos.influxdata.com/centos/$releasever/$basearch/stable

# Yumrepo GPG key URL
influxdb_yumrepo_gpgkey: https://repos.influxdata.com/influxdb.key

# Custom yumrepo params to add or override the existing one
influxdb_yumrepo_params: {}

# Package to be installed (version can be specified here)
influxdb_pkg: influxdb

# Location of the influxdb config file
influxdb_config_file: /etc/influxdb/influxdb.conf

# Name of the InfluxDB service
influxdb_service: influxdb


# Default values of options for the meta section
influxdb_config_meta__default_dir: /var/lib/influxdb/meta
influxdb_config_meta__default_retention_autocreate: "true"
influxdb_config_meta__default_logging_enabled: "true"

# Default options of the meta section
influxdb_config_meta__default:
  dir: "{{ influxdb_config_meta__default_dir }}"
  retention-autocreate: "{{ influxdb_config_meta__default_retention_autocreate }}"
  logging-enabled: "{{ influxdb_config_meta__default_logging_enabled }}"

# Custom options of the meta section
influxdb_config_meta__custom: {}

# Final meta section
influxdb_config_meta: "{{
  influxdb_config_meta__default.update(influxdb_config_meta__custom) }}{{
  influxdb_config_meta__default }}"


# Default values of options for the data section
influxdb_config_data__default_dir: /var/lib/influxdb/data
influxdb_config_data__default_wal_dir: /var/lib/influxdb/wal
influxdb_config_data__default_wal_fsync_delay: 0s
influxdb_config_data__default_index_version: inmem
influxdb_config_data__default_trace_logging_enabled: "false"
influxdb_config_data__default_query_log_enabled: "false"
influxdb_config_data__default_cache_max_memory_size: 1048576000
influxdb_config_data__default_cache_snapshot_memory_size: 26214400
influxdb_config_data__default_cache_snapshot_write_cold_duration: 10m
influxdb_config_data__default_compact_full_write_cold_duration: 4h
influxdb_config_data__default_max_concurrent_compactions: 0
influxdb_config_data__default_max_series_per_database: 1000000
influxdb_config_data__default_max_values_per_tag: 100000

# Default options of the data section
influxdb_config_data__default:
  dir: "{{ influxdb_config_data__default_dir }}"
  wal-dir: "{{ influxdb_config_data__default_wal_dir }}"
  wal-fsync-delay: "{{ influxdb_config_data__default_wal_fsync_delay }}"
  index-version: "{{ influxdb_config_data__default_index_version }}"
  trace-logging-enabled: "{{ influxdb_config_data__default_trace_logging_enabled }}"
  query-log-enabled: "{{ influxdb_config_data__default_query_log_enabled }}"
  cache-max-memory-size: "{{ influxdb_config_data__default_cache_max_memory_size }}"
  cache-snapshot-memory-size: "{{ influxdb_config_data__default_cache_snapshot_memory_size }}"
  cache-snapshot-write-cold-duration: "{{ influxdb_config_data__default_cache_snapshot_write_cold_duration }}"
  compact-full-write-cold-duration: "{{ influxdb_config_data__default_compact_full_write_cold_duration }}"
  max-concurrent-compactions: "{{ influxdb_config_data__default_max_concurrent_compactions }}"
  max-series-per-database: "{{ influxdb_config_data__default_max_series_per_database }}"
  max-values-per-tag: "{{ influxdb_config_data__default_max_values_per_tag }}"

# Custom options of the data section
influxdb_config_data__custom: {}

# Final data section
influxdb_config_data: "{{
  influxdb_config_data__default.update(influxdb_config_data__custom) }}{{
  influxdb_config_data__default }}"


# Default values of options for the coordinator section
influxdb_config_coordinator__default_write_timeout: 10s
influxdb_config_coordinator__default_max_concurrent_queries: 0
influxdb_config_coordinator__default_query_timeout: 0s
influxdb_config_coordinator__default_log_queries_after: 0s
influxdb_config_coordinator__default_max_select_point: 0
influxdb_config_coordinator__default_max_select_series: 0
influxdb_config_coordinator__default_max_select_buckets: 0

# Default options of the coordinator section
influxdb_config_coordinator__default:
  write-timeout: "{{ influxdb_config_coordinator__default_write_timeout }}"
  max-concurrent-queries: "{{ influxdb_config_coordinator__default_max_concurrent_queries }}"
  query-timeout: "{{ influxdb_config_coordinator__default_query_timeout }}"
  log-queries-after: "{{ influxdb_config_coordinator__default_log_queries_after }}"
  max-select-point: "{{ influxdb_config_coordinator__default_max_select_point }}"
  max-select-series: "{{ influxdb_config_coordinator__default_max_select_series }}"
  max-select-buckets: "{{ influxdb_config_coordinator__default_max_select_buckets }}"

# Custom options of the coordinator section
influxdb_config_coordinator__custom: {}

# Final data the coordinator section
influxdb_config_coordinator: "{{
  influxdb_config_coordinator__default.update(influxdb_config_coordinator__custom) }}{{
  influxdb_config_coordinator__default }}"


# Default values of options for the section
influxdb_config_retention__default_enabled: "true"
influxdb_config_retention__default_check_interval: 30m

# Default options of the retention section
influxdb_config_retention__default:
    enabled: "{{ influxdb_config_retention__default_enabled }}"
    check-interval: "{{ influxdb_config_retention__default_check_interval }}"

# Custom options of the retention section
influxdb_config_retention__custom: {}

# Final data the retention section
influxdb_config_retention: "{{
  influxdb_config_retention__default.update(influxdb_config_retention__custom) }}{{
  influxdb_config_retention__default }}"


# Default values of options for the section
influxdb_config_shard__default_enabled: "true"
influxdb_config_shard__default_check_interval: 10m
influxdb_config_shard__default_advance_period: 30m

# Default options of the shard-precreation: section
influxdb_config_shard__default:
  enabled: "{{ influxdb_config_shard__default_enabled }}"
  check-interval: "{{ influxdb_config_shard__default_check_interval }}"
  advance-period: "{{ influxdb_config_shard__default_advance_period }}"

# Custom options of the shard-precreation: section
influxdb_config_shard__custom: {}

# Final data the shard-precreation: section
influxdb_config_shard: "{{
  influxdb_config_shard__default.update(influxdb_config_shard__custom) }}{{
  influxdb_config_shard__default }}"


# Default values of options for the section
influxdb_config_monitor__default_store_enabled: "true"
influxdb_config_monitor__default_store_database: _internal
influxdb_config_monitor__default_store_interval: 10s

# Default options of the monitor section
influxdb_config_monitor__default:
  store-enabled: "{{ influxdb_config_monitor__default_store_enabled }}"
  store-database: "{{ influxdb_config_monitor__default_store_database }}"
  store-interval: "{{ influxdb_config_monitor__default_store_interval }}"

# Custom options of the monitor section
influxdb_config_monitor__custom: {}

# Final data the monitor section
influxdb_config_monitor: "{{
  influxdb_config_monitor__default.update(influxdb_config_monitor__custom) }}{{
  influxdb_config_monitor__default }}"


# Default values of options for the http section
influxdb_config_http__default_enabled: "true"
influxdb_config_http__default_bind_address: :8086
influxdb_config_http__default_auth_enabled: "false"
influxdb_config_http__default_realm: InfluxDB
influxdb_config_http__default_log_enabled: "false"
influxdb_config_http__default_write_tracing: "false"
influxdb_config_http__default_pprof_enabled: "false"
influxdb_config_http__default_https_enabled: "false"
influxdb_config_http__default_https_certificate: /etc/ssl/influxdb.pem
influxdb_config_http__default_https_private_key: ""
influxdb_config_http__default_shared_secret: ""
influxdb_config_http__default_max_row_limit: 0
influxdb_config_http__default_max_connection_limit: 0
influxdb_config_http__default_unix_socket_enabled: "false"
influxdb_config_http__default_bind_socket: /var/run/influxdb.sock

# Default options of the http section
influxdb_config_http__default:
  enabled: "{{ influxdb_config_http__default_enabled }}"
  bind-address: "{{ influxdb_config_http__default_bind_address }}"
  auth-enabled: "{{ influxdb_config_http__default_auth_enabled }}"
  realm: "{{ influxdb_config_http__default_realm }}"
  log-enabled: "{{ influxdb_config_http__default_log_enabled }}"
  write-tracing: "{{ influxdb_config_http__default_write_tracing }}"
  pprof-enabled: "{{ influxdb_config_http__default_pprof_enabled }}"
  https-enabled: "{{ influxdb_config_http__default_https_enabled }}"
  https-certificate: "{{ influxdb_config_http__default_https_certificate }}"
  https-private-key: "{{ influxdb_config_http__default_https_private_key }}"
  shared-secret: "{{ influxdb_config_http__default_shared_secret }}"
  max-row-limit: "{{ influxdb_config_http__default_max_row_limit }}"
  max-connection-limit: "{{ influxdb_config_http__default_max_connection_limit }}"
  unix-socket-enabled: "{{ influxdb_config_http__default_unix_socket_enabled }}"
  bind-socket: "{{ influxdb_config_http__default_bind_socket }}"

# Custom options of the http section
influxdb_config_http__custom: {}

# Final data the http section
influxdb_config_http: "{{
  influxdb_config_http__default.update(influxdb_config_http__custom) }}{{
  influxdb_config_http__default }}"


# Default values of options for the subscriber section
influxdb_config_subscriber__default_enabled: "true"
influxdb_config_subscriber__default_influxdb_config_subscriber__default_enabled: "true"
influxdb_config_subscriber__default_http_timeout: 30s
influxdb_config_subscriber__default_insecure_skip_verify: "false"
influxdb_config_subscriber__default_ca_certs: ""
influxdb_config_subscriber__default_write_concurrency: 40
influxdb_config_subscriber__default_write_buffer_size: 1000

# Default options of the subscriber section
influxdb_config_subscriber__default:
  enabled: "{{ influxdb_config_subscriber__default_enabled }}"
  http-timeout: "{{ influxdb_config_subscriber__default_http_timeout }}"
  insecure-skip-verify: "{{ influxdb_config_subscriber__default_insecure_skip_verify }}"
  ca-certs: "{{ influxdb_config_subscriber__default_ca_certs }}"
  write-concurrency: "{{ influxdb_config_subscriber__default_write_concurrency }}"
  write-buffer-size: "{{ influxdb_config_subscriber__default_write_buffer_size }}"

# Custom options of the subscriber section
influxdb_config_subscriber__custom: {}

# Final data the subscriber section
influxdb_config_subscriber: "{{
  influxdb_config_subscriber__default.update(influxdb_config_subscriber__custom) }}{{
  influxdb_config_subscriber__default }}"


# Default option values of the default list item of the graphite section
influxdb_config_graphite_list__default_enabled: "false"
influxdb_config_graphite_list__default_database: graphite
influxdb_config_graphite_list__default_retention_policy: ""
influxdb_config_graphite_list__default_bind_address: :2003
influxdb_config_graphite_list__default_protocol: tcp
influxdb_config_graphite_list__default_consistency_level: one
influxdb_config_graphite_list__default_batch_size: 5000
influxdb_config_graphite_list__default_batch_pending: 10
influxdb_config_graphite_list__default_batch_timeout: 1s
influxdb_config_graphite_list__default_udp_read_buffer: 0
influxdb_config_graphite_list__default_separator: .
influxdb_config_graphite_list__default_tags: []
influxdb_config_graphite_list__default_templates: []

# Default options of the default list item of the graphite section
influxdb_config_graphite_list__default:
  enabled: "{{ influxdb_config_graphite_list__default_enabled }}"
  database: "{{ influxdb_config_graphite_list__default_database }}"
  retention-policy: "{{ influxdb_config_graphite_list__default_retention_policy }}"
  bind-address: "{{ influxdb_config_graphite_list__default_bind_address }}"
  protocol: "{{ influxdb_config_graphite_list__default_protocol }}"
  consistency-level: "{{ influxdb_config_graphite_list__default_consistency_level }}"
  batch-size: "{{ influxdb_config_graphite_list__default_batch_size }}"
  batch-pending: "{{ influxdb_config_graphite_list__default_batch_pending }}"
  batch-timeout: "{{ influxdb_config_graphite_list__default_batch_timeout }}"
  udp-read-buffer: "{{ influxdb_config_graphite_list__default_udp_read_buffer }}"
  separator: "{{ influxdb_config_graphite_list__default_separator }}"
  tags: "{{ influxdb_config_graphite_list__default_tags }}"
  templates: "{{ influxdb_config_graphite_list__default_templates }}"

# Custom options of the default list item of the graphite section
influxdb_config_graphite_list__custom: {}

# Default list item of the graphite section
influxdb_config_graphite__default:
 - "{{ influxdb_config_graphite_list__default.update(influxdb_config_graphite_list__custom) }}{{
       influxdb_config_graphite_list__default }}"

# Custom list items of the graphite section
influxdb_config_graphite__custom: []

# Final data the graphite section
influxdb_config_graphite: "{{
  influxdb_config_graphite__default +
  influxdb_config_graphite__custom
}}"


# Default option values of the default list item of the collectd section
influxdb_config_collectd_list__default_enabled: "false"
influxdb_config_collectd_list__default_bind_address: :25826
influxdb_config_collectd_list__default_database: collectd
influxdb_config_collectd_list__default_retention_policy: ""
influxdb_config_collectd_list__default_typesdb: /usr/local/share/collectd
influxdb_config_collectd_list__default_security_level: "none"
influxdb_config_collectd_list__default_auth_file: /etc/collectd/auth_file
influxdb_config_collectd_list__default_batch_size: 5000
influxdb_config_collectd_list__default_batch_pending: 10
influxdb_config_collectd_list__default_batch_timeout: 10s
influxdb_config_collectd_list__default_read_buffer: 0

# Default options of the default list item of the graphite section
influxdb_config_collectd_list__default:
  enabled: "{{ influxdb_config_collectd_list__default_enabled }}"
  bind-address: "{{ influxdb_config_collectd_list__default_bind_address }}"
  database: "{{ influxdb_config_collectd_list__default_database }}"
  retention-policy: "{{ influxdb_config_collectd_list__default_retention_policy }}"
  typesdb: "{{ influxdb_config_collectd_list__default_typesdb }}"
  security-level: "{{ influxdb_config_collectd_list__default_security_level }}"
  auth-file: "{{ influxdb_config_collectd_list__default_auth_file }}"
  batch-size: "{{ influxdb_config_collectd_list__default_batch_size }}"
  batch-pending: "{{ influxdb_config_collectd_list__default_batch_pending }}"
  batch-timeout: "{{ influxdb_config_collectd_list__default_batch_timeout }}"
  read-buffer: "{{ influxdb_config_collectd_list__default_read_buffer }}"

# Custom options of the default list item of the collectd section
influxdb_config_collectd_list__custom: {}

# Default list item of the collectd section
influxdb_config_collectd__default:
 - "{{ influxdb_config_collectd_list__default.update(influxdb_config_collectd_list__custom) }}{{
       influxdb_config_collectd_list__default }}"

# Custom list items of the collectd section
influxdb_config_collectd__custom: []

# Final data the collectd section
influxdb_config_collectd: "{{
  influxdb_config_collectd__default +
  influxdb_config_collectd__custom
}}"


# Default option values of the default list item of the opentsdb section
influxdb_config_opentsdb_list__default_enabled: "false"
influxdb_config_opentsdb_list__default_bind_address: :4242
influxdb_config_opentsdb_list__default_database: opentsdb
influxdb_config_opentsdb_list__default_retention_policy: ""
influxdb_config_opentsdb_list__default_consistency_level: one
influxdb_config_opentsdb_list__default_tls_enabled: "false"
influxdb_config_opentsdb_list__default_certificate: /etc/ssl/influxdb.pem
influxdb_config_opentsdb_list__default_log_point_errors: "true"
influxdb_config_opentsdb_list__default_batch_size: 1000
influxdb_config_opentsdb_list__default_batch_pending: 5
influxdb_config_opentsdb_list__default_batch_timeout: 1s

# Default options of the default list item of the graphite section
influxdb_config_opentsdb_list__default:
  enabled: "{{ influxdb_config_opentsdb_list__default_enabled }}"
  bind-address: "{{ influxdb_config_opentsdb_list__default_bind_address }}"
  database: "{{ influxdb_config_opentsdb_list__default_database }}"
  retention-policy: "{{ influxdb_config_opentsdb_list__default_retention_policy }}"
  consistency-level: "{{ influxdb_config_opentsdb_list__default_consistency_level }}"
  tls-enabled: "{{ influxdb_config_opentsdb_list__default_tls_enabled }}"
  certificate: "{{ influxdb_config_opentsdb_list__default_certificate }}"
  log-point-errors: "{{ influxdb_config_opentsdb_list__default_log_point_errors }}"
  batch-size: "{{ influxdb_config_opentsdb_list__default_batch_size }}"
  batch-pending: "{{ influxdb_config_opentsdb_list__default_batch_pending }}"
  batch-timeout: "{{ influxdb_config_opentsdb_list__default_batch_timeout }}"

# Custom options of the default list item of the opentsdb section
influxdb_config_opentsdb_list__custom: {}

# Default list item of the opentsdb section
influxdb_config_opentsdb__default:
 - "{{ influxdb_config_opentsdb_list__default.update(influxdb_config_opentsdb_list__custom) }}{{
       influxdb_config_opentsdb_list__default }}"

# Custom list items of the opentsdb section
influxdb_config_opentsdb__custom: []

# Final data the opentsdb section
influxdb_config_opentsdb: "{{
  influxdb_config_opentsdb__default +
  influxdb_config_opentsdb__custom
}}"


# Default values of options for the udp section
influxdb_config_udp_list__default_enabled: "false"
influxdb_config_udp_list__default_bind_address: :8089
influxdb_config_udp_list__default_database: udp
influxdb_config_udp_list__default_retention_policy: ""
influxdb_config_udp_list__default_batch_size: 5000
influxdb_config_udp_list__default_batch_pending: 10
influxdb_config_udp_list__default_batch_timeout: 1s
influxdb_config_udp_list__default_read_buffer: 0

# Default options of the udp section
influxdb_config_udp_list__default:
  enabled: "{{ influxdb_config_udp_list__default_enabled }}"
  bind-address: "{{ influxdb_config_udp_list__default_bind_address }}"
  database: "{{ influxdb_config_udp_list__default_database }}"
  retention-policy: "{{ influxdb_config_udp_list__default_retention_policy }}"
  batch-size: "{{ influxdb_config_udp_list__default_batch_size }}"
  batch-pending: "{{ influxdb_config_udp_list__default_batch_pending }}"
  batch-timeout: "{{ influxdb_config_udp_list__default_batch_timeout }}"
  read-buffer: "{{ influxdb_config_udp_list__default_read_buffer }}"

# Custom options of the udp section
influxdb_config_udp_list__custom: {}

# Default item of the udp section list
influxdb_config_udp__default:
 - "{{ influxdb_config_udp_list__default.update(influxdb_config_udp_list__custom) }}{{
       influxdb_config_udp_list__default }}"

# Custom item of the udp section list
influxdb_config_udp__custom: []

# Final data the udp section
influxdb_config_udp: "{{
  influxdb_config_udp__default +
  influxdb_config_udp__custom
}}"


# Default values of options for the continuous-queries section
influxdb_config_continuous__default_log_enabled: "true"
influxdb_config_continuous__default_enabled: "true"
influxdb_config_continuous__default_run_interval: 1s

# Default options of the continuous-queries section
influxdb_config_continuous__default:
  log-enabled: "{{ influxdb_config_continuous__default_log_enabled }}"
  enabled: "{{ influxdb_config_continuous__default_enabled }}"
  run-interval: "{{ influxdb_config_continuous__default_run_interval }}"

# Custom options of the continuous-queries section
influxdb_config_continuous__custom: {}

# Final data the continuous-queries section
influxdb_config_continuous: "{{
  influxdb_config_continuous__default.update(influxdb_config_continuous__custom) }}{{
  influxdb_config_continuous__default }}"


# Default values of options for the root section
influxdb_config_reporting_disabled: "false"
influxdb_config_bind_host: 127.0.0.1
influxdb_config_bind_port: 8088
influxdb_config_bind_address: "{{ influxdb_config_bind_host }}:{{ influxdb_config_bind_port }}"

# Default configuration template
influxdb_config:
  reporting-disabled: "{{ influxdb_config_reporting_disabled }}"
  bind-address: "{{ influxdb_config_bind_address }}"
  meta: "{{ influxdb_config_meta }}"
  data: "{{ influxdb_config_data }}"
  coordinator: "{{ influxdb_config_coordinator }}"
  retention: "{{ influxdb_config_retention }}"
  shard-precreation: "{{ influxdb_config_shard }}"
  monitor: "{{ influxdb_config_monitor }}"
  http: "{{ influxdb_config_http }}"
  subscriber: "{{ influxdb_config_subscriber }}"
  graphite: "{{ influxdb_config_graphite }}"
  collectd: "{{ influxdb_config_collectd }}"
  opentsdb: "{{ influxdb_config_opentsdb }}"
  udp: "{{ influxdb_config_udp }}"
  continuous_queries: "{{ influxdb_config_continuous }}"
```


Dependencies
------------

- [`config_encoder_filters`](https://github.com/jtyr/ansible-config_encoder_filters)


License
-------

MIT


Author
------

Jiri Tyr
