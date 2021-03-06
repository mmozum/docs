---
title: Oracle RDBMS Integration
tags: [integrations list]
permalink: oracle.html
summary: Learn about the Wavefront Oracle RDBMS Integration.
---
## Oracle RDBMS

Oracle is an Industry-leading enterprise Relational Database Management System.

This integration installs and configures Telegraf and a custom Python script to send Oracle metrics into Wavefront. Telegraf is a light-weight server process capable of collecting, processing, aggregating, and sending metrics to a Wavefront proxy. The custom script uses the Dynamic Performance views that Oracle provides to gather metrics.

In addition to setting up the metrics flow, this integration also sets up a dashboard.
{% include image.md src="images/Oracle-DB-metrics.png" width="80" %}

To see a list of the metrics for this integration, select the integration from <https://github.com/influxdata/telegraf/tree/master/plugins/inputs>.
## Oracle RDBMS



### Step 1. Install the Telegraf Agent

If you don't have the Telegraf agent installed, follow the steps below. Otherwise, continue to step 2.

Log in to your Wavefront instance and follow the instructions in the **Setup** tab to install Telegraf and a Wavefront proxy in your environment. If a proxy is already running in your environment, you can select that proxy and the Telegraf install command connects with that proxy. Sign up for a [free trial](http://wavefront.com/sign-up/?utm_source=docs.vmware.com&utm_medium=referral&utm_campaign=docs-front-page){:target="_blank" rel="noopenner noreferrer"} to check it out!

### Step 2. Create wavefront User in Oracle
{% raw %}
```
  CREATE USER wavefront IDENTIFIED BY <yourpassword>;
  GRANT select_catalog_role TO wavefront;
  GRANT CREATE SESSION TO wavefront;
```

### Step 3. Install Python

1. Make sure python 3.6 or higher is installed on the Telegraf agent server.
2. Install python package cx_Oracle. Use the following snippet.
  ```
    python -m pip install cx_Oracle --upgrade
  ```

### Step 4. Create a Script to Gather Oracle RDBMS Metrics

1. Download [wavefront_oracle_metrics.py](https://raw.githubusercontent.com/wavefrontHQ/oracle-metrics-collector/master/wavefront_oracle_metrics.py) onto your Telegraf agent server.
2. Test the script execution using this command:
    ```
    python wavefront_oracle_metrics.py
    ```
    You should get a response similar to this:
    ```
    usage: wavefront_oracle_metrics.py [-h] -u USER -p PASSWD -s SID
    wavefront_oracle_metrics.py: error: the following arguments are required: -u/--user, -p/--passwd, -s/--sid
    ```
    If the script is not executing, adjust the file permission and the Python path.
3. Download [exec_oracle_python.sh](https://raw.githubusercontent.com/wavefrontHQ/oracle-metrics-collector/master/exec_oracle_python.sh) onto your Telegraf agent server.
4. Edit the script to change the environment variables, and  python execution path for your Telegraf agent server. 
5. Change the ``wavefront password`` & ``sid`` parameters in exec_oracle_python.sh file. 
```
   # Example.
   /usr/bin/python "/home/oracle/Documents/wavefront_oracle_metrics.py" -u "wavefront" -p "wavefront123" -s "orcl"
```
6. Note down the full paths for files downloaded and saved from steps 1 & 3 above.

### Step 5. Configure Telegraf Exec Input Plugin

For Linux Telegraf agent server.

Create a file called `oracle.conf` in `/etc/telegraf/telegraf.d` and enter the following snippet:

```
[[inputs.exec]]
   commands = ["/home/oracle/Documents/exec_oracle_python.sh"]
   timeout = "5s"
   data_format = "influx"
```
**NOTE:** use the path of the exec_oracle_python.sh.

For Windows Telegraf agent server.

Edit the `telegraf.conf` file located at `Program Files\Telegraf` and enter the following snippet:

```
[[inputs.exec]]
   commands = [
         'python "C:\Wavefront\wavefront_oracle_metrics.py" -u "wavefront" -p "<password>" -s "<sid>"'
			]
   timeout = "5s"
   data_format = "influx"
```
Change the ``password`` and ``sid`` in the code snippet.

**NOTE:** use the path of the wavefront_oracle_metrics.py.

### Step 6. Restart Telegraf

For Linux

Run `sudo service telegraf restart` to restart your Telegraf agent.

For Windows

Restart the Telegraf service using the Windows Services Management Console or from the command prompt:

```
  net stop telegraf
  net start telegraf
```
{% endraw %}
