---
title: MySQL Integration
tags: [integrations list]
permalink: mysql.html
summary: Learn about the Wavefront MySQL Integration.
---
## MySQL Integration

MySQL is an open source relational database management system. This integration installs and configures Telegraf to send MySQL metrics into Wavefront. Telegraf is a light-weight server process capable of collecting, processing, aggregating, and sending metrics to a [Wavefront proxy](https://docs.wavefront.com/proxies.html).

In addition to setting up the metrics flow, this integration also installs a dashboard. For example, here's screenshot of dashboard with statistics collected from MySQL server.
{% include image.md src="images/mysql-metrics.png" width="80" %}



To see a list of the metrics for this integration, select the integration from <https://github.com/influxdata/telegraf/tree/master/plugins/inputs>.
## MySQL Setup



**Note:** If you use vRealize Operations, the application proxy agent sets up the integration for you. See the [setup instructions](http://YOUR_CLUSTER.wavefront.com/integration/vrops/setup). Otherwise, follow the setup steps on this page.

### Step 1. Install the Telegraf Agent

This integration uses the MySQL input plugin for Telegraf. If you've already installed Telegraf on your server(s), you can skip to Step 2.

Log in to your Wavefront instance and follow the instructions in the **Setup** tab to install Telegraf and a Wavefront proxy in your environment. If a proxy is already running in your environment, you can select that proxy and the Telegraf install command connects with that proxy. Sign up for a [free trial](http://wavefront.com/sign-up/?utm_source=docs.vmware.com&utm_medium=referral&utm_campaign=docs-front-page){:target="_blank" rel="noopenner noreferrer"} to check it out!

### Step 2. Configure MySQL Input Plugin

1. Create a file called `mysql.conf` in `/etc/telegraf/telegraf.d` and enter the following snippet:
{% raw %}
    ```
    # Read metrics from one or many mysql servers
    [[inputs.mysql]]
      ## specify servers via a url matching:
      ##  [username[:password]@][protocol[(address)]]/[?tls=[true|false|skip-verify]]
      ##  see https://github.com/go-sql-driver/mysql#dsn-data-source-name
      ##  e.g.
      ##    servers = ["user:passwd@tcp(127.0.0.1:3306)/?tls=false"]
      ##    servers = ["user@tcp(127.0.0.1:3306)/?tls=false"]
      #
      ## CHANGE THE SERVERS FIELD HERE
      servers                                   = ["mysql_user:password@tcp(127.0.0.1:3306)/"]
      #
      ## the limits for metrics form perf_events_statements
      perf_events_statements_digest_text_limit  = 120
      perf_events_statements_limit              = 250
      perf_events_statements_time_limit         = 86400
      #
      ## if the list is empty, then metrics are gathered from all database tables
      table_schema_databases                    = []
      #
      ## gather metrics from INFORMATION_SCHEMA.TABLES for databases provided above list
      gather_table_schema                       = false
      #
      ## gather thread state counts from INFORMATION_SCHEMA.PROCESSLIST
      gather_process_list                       = true
      #
      ## gather thread state counts from INFORMATION_SCHEMA.USER_STATISTICS
      gather_user_statistics                    = true
      #
      ## gather auto_increment columns and max values from information schema
      gather_info_schema_auto_inc               = true
      #
      ## gather metrics from INFORMATION_SCHEMA.INNODB_METRICS
      gather_innodb_metrics                     = true
      #
      ## gather metrics from SHOW SLAVE STATUS command output
      gather_slave_status                       = true
      #
      ## gather metrics from SHOW BINARY LOGS command output
      gather_binary_logs                        = false
      #
      ## gather metrics from PERFORMANCE_SCHEMA.TABLE_IO_WAITS_SUMMARY_BY_TABLE
      gather_table_io_waits                     = false
      #
      ## gather metrics from PERFORMANCE_SCHEMA.TABLE_LOCK_WAITS
      gather_table_lock_waits                   = false
      #
      ## gather metrics from PERFORMANCE_SCHEMA.TABLE_IO_WAITS_SUMMARY_BY_INDEX_USAGE
      gather_index_io_waits                     = false
      #
      ## gather metrics from PERFORMANCE_SCHEMA.EVENT_WAITS
      gather_event_waits                        = false
      #
      ## gather metrics from PERFORMANCE_SCHEMA.FILE_SUMMARY_BY_EVENT_NAME
      gather_file_events_stats                  = false
      #
      ## gather metrics from PERFORMANCE_SCHEMA.EVENTS_STATEMENTS_SUMMARY_BY_DIGEST
      gather_perf_events_statements             = false
      #
      ## Some queries we may want to run less often (such as SHOW GLOBAL VARIABLES)
      interval_slow                             = "30m"
    ```

1. Replace the `servers` value with your MySQL server URL. Specify your servers with URL matching.
    
    Format:
    ```
    servers = [username[:password]@][protocol[(address)]]/[?tls=[true|false|skip-verify]]
    ```
    Example:
    ```
    servers = ["user:password@tcp(your_mysql_server:3306)/"]
    ```
{% endraw %}

### Step 3. Restart Telegraf

Run `sudo service telegraf restart` to restart your agent.
