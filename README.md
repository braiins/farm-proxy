![farm-proxy-logo](images/farm-proxy-logo_960x320.png)

# License

Any content in this repository is released under our proprietary License. The current version of the License is available in this repository in a separate [file](LICENSE.md), and can be also accessed at the following website: https://braiins.com/farm-proxy/license.

By downloading, copying, installing, or otherwise using all or any content from this repository, you accept all the terms and conditions set out in the License, so please, read the License carefully. If you do not agree with any of the terms and conditions of the License, do not use any content from this repository and delete or destroy any such content that is already in your possession or control.

Please, keep in mind that the License automatically renews every month and from time to time it can be changed or amended, so revisit the License regularly to keep track of any changes. The date when the License was last materially changed or amended is listed in the header of the License for convenience.

If you have any other questions, please contact us at info@braiins.com.

## Introduction
Braiins is providing to the mining world a free hashrate aggregation proxy **Braiins Farm Proxy**. Braiins Farm Proxy encompasses four primary components:
* Farm Proxy
* Farm Monitor
* Prometheus
* Grafana

Go to [Braiins Academy](https://academy.braiins.com/en/farm-proxy/about) for the full documentation.

![client-dashboard](images/farm-proxy.png)

# Quick Start
1. Clone the git repository git clone https://github.com/braiins/farm-proxy.git
2. Go to the farm-proxy repository `cd farm-proxy`
3. Fill in your preferred proxy settings in the config file `vim ./config/sample.toml` (see below the example of the config file)
   1. Fill in your mining pool in the table `target`
   2. Make sure you specify `userName.workerName` so that hashrate is routed to correct pool account.
4. [optional] If you ran also Farm Monitor, define IP address ranges for the device discovery in the config file `./monitoring/metrics_exporter/metrics-exporter.toml`
5. Run monitoring with the command `docker-compose up -d`
6. Verify that farm-proxy is running `docker ps`
7. Open URL http://localhost:3000 to see the Client Dashboard in Grafana
8. Connect miners to the Braiins Farm Proxy (fill in the proxy URL `stratum+tcp://<your_host:port>` to the pool settings of the miners)

## Farm Proxy Distribution
Farm Monitor can be currently run on Linux OS as a multiplatform software:
* AMD 64bit
* ARM 64bit
* ARMv7

## Prerequisites
At the beginning it is required to install a couple of prerequisites:

* Docker
* Docker Compose
* Git

# Configuration
Braiins Farm Proxy config file has to be present in the directory `./config` and it has be TOML file.
Braiins Farm Monitor has to be configured in the config file `./monitoring/metrics_exporter/metrics-exporter.toml`.

## Example Configuration
Examples of configuration files for Braiins Farm Proxy and Braiins Farm Monitor.

### Braiins Farm Proxy Configuration
Configuration for a farm, which is mining on the Braiins Pool with a pool usernam name braiins.

```
[[server]]
name = "S1"
port = 3336

[[target]]
name = "SP"
url = "stratum+tcp://stratum.slushpool.com:3333"
user_identity = "braiins.fp"

[[routing]]
name = "RD"
from = ["S1"]

[[routing.goal]]
name = "Primary Goal"

[[routing.goal.level]]
targets = ["SP"]
```

### Braiins Farm Monitor Configutation
Configuration for a farm, where miners have IP addresses in the IP ranges `1.2.0.*`, `1.2.*.2`, `1.3.0.*` and `1.3.*.2`.

```
# scraping internal for stock FW devices, scraping interval for BOS+ devices is defined in the monitoring/promethus/prometheus.yml (default is also 5s)
[scraping]
stock_fw_scrape_interval = "1m"

# ranges needs to be defined by user, name of the ranges is then used as Prometheus label
# naming is arbitrary
# number of ranges is also arbitrary
[ranges]
Building_A = ["1.2.0.*","1.2.*.2"]
Building_B =["1.3.0.*","1.3.*.2"]

# credentials needed only for http API, cgminer API doesn't require credentials
# default credentials are root/root, if the user is using another credentials, it has to be edited and uncommented here
#[credentials]
#username = "root"
#password = "root"

[advanced]
# interval for scanning period
scan_interval = "5m"
# timeout interval for scanning
scan_timeout = "15s"
# maximal number of parallel scan requests
scan_max_concurrency = 1000
# timeout interval for scraping stock FW devices
stock_fw_scrape_timeout = "5s"
# maximal number of parallel scrape requests for stock FW devices
stock_fw_scrape_max_concurrency = 40000
```
### Braiins Farm Proxy Configutation
Since Braiins Farm Proxy version 23.01, telemetry data are collected in order to be able to track technical issues, errors and future improvements of the application. In case the user doesn't want to send telemetry data to Braiins, he can opt out. Just add to the proxy configuration file following rows:

```
[telemetry]
enable_farm_metrics_telemetry = false
```
