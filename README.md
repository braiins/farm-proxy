# Installation manual

## Prerequisites
### docker & docker-compose
#### Linux
- Follow [docker installation instructions](https://docs.docker.com/engine/install/ubuntu/)
- Follow [Optional docker post installation steps](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)
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
1. Prepare your farm_proxy configuration based on examples in config directory
2. Tweak docker-compose.yml file according to your configuration
   1. Specify which configuration file shall be used for farm proxy
   2. Change section farm-proxy.volumes to following in order for farm-proxy to use config.minimal.toml as its configuration  "./config/minimal.toml:/conf/farm_proxy.yml"
   Map server ports
   3. Section farm-proxy.ports must contain all ports utilized in farm-proxy servers. For example add “- 3337:3337” to enable port 3337. Note that spacing must be folllowed in .yml files
3. Start farm-proxy & out-of-the-box monitoring
   ```
   Run “docker-compose up -d”
   ```
4. Verify that farm-proxy
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
docker-compose restart -d farm-proxy
```
### Updating farm-proxy
```
git pull origin master
docker-compose up -d
```

