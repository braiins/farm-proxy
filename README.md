# License

Any content in this repository is released under our proprietary License. The current version of the License is available in this repository in a separate [file](LICENSE.md), and can be also accessed at the following website: https://braiins.com/farm-proxy/license.

By downloading, copying, installing, or otherwise using all or any content from this repository, you accept all the terms and conditions set out in the License, so please, read the License carefully. If you do not agree with any of the terms and conditions of the License, do not use any content from this repository and delete or destroy any such content that is already in your possession or control.

Please, keep in mind that the License automatically renews every month and from time to time it can be changed or amended, so revisit the License regularly to keep track of any changes. The date when the License was last materially changed or amended is listed in the header of the License for convenience.

If you have any other questions, please contact us at info@braiins.com.

# Changelog

Changelog can be found on the Github release page [here](https://github.com/braiins/farm-proxy/releases).

# Installation manual

## Prerequisites
### docker & docker-compose
#### Linux
- Follow [docker installation instructions](https://docs.docker.com/engine/install/ubuntu/)
- Follow [optional docker post installation steps](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)
- Follow [docker-compose installation](https://docs.docker.com/compose/install/)

#### RPi
- Follow one of the many manuals - e.g. https://jfrog.com/connect/post/install-docker-compose-on-raspberry-pi/

### Install git
```
sudo apt update
sudo apt install git
```

### Verify prerequisite installed
```
docker version
docker-compose --version
git --version
```

## Configure farm-proxy
### Clone farm-proxy
```
git clone https://github.com/braiins/farm-proxy.git
```
1. Prepare your farm-proxy configuration based on examples in config directory
   1. Make sure you specify "userName.workerName" so that hashrate is routed to correct pool account.     
3. Tweak docker-compose.yml file according to your needs
   1. Specify which configuration file shall be used for farm proxy
   2. Change section farm-proxy.volumes to following in order for farm-proxy to use config.minimal.toml as its configuration  "./config/minimal.toml:/conf/farm_proxy.yml"
4. Start farm-proxy & out-of-the-box monitoring
   ```
   docker-compose up -d
   ```
4. Verify that farm-proxy is running
   ```
   docker ps
   ```
5. Connect miner ```stratum+tcp://<your_host:port>```
6. See out-of-the-box monitoring that everything is fine ```http://<your_host>:3000/```
   1. Default credentials admin:admin (specified in [docker-compose.yml](/docker-compose.yml))

## Operations
### Changing configuration
Change configuration file according to your needs
```
docker-compose restart farm-proxy
```
### Updating farm-proxy
```
git pull origin master
docker-compose up -d
```
