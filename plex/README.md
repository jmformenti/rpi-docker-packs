# Plex over Docker on Raspberry

Build your own [plex](https://www.plex.tv/) server with [transmission](https://transmissionbt.com/) and [flexget](https://flexget.com/) on your Raspberry from scratch.

Transmission media files downloads are automatically published on plex using flexget.

Based on peladonerd [video](https://www.youtube.com/watch?v=TqVoHWjz_tI) and his [repo](https://github.com/pablokbs/plex-rpi).

Tested on Raspberry Pi 4 (4GB).

## Requirements

Before starting you need a 64bits OS with docker installed on your Raspberry Pi.

If not, you can follow this [few steps](../INSTALL_OS.md) to install and configure the Raspbian 64bits OS on your Pi:
 
## Install

To install plex, transmission and flexget run this steps:
1. Clone this repo and enter in ``rpi-docker-packs/plex`` directory.
```
git clone https://github.com/jmformenti/rpi-docker-packs.git
cd rpi-docker-packs/plex
```
2. Configure main parameters inside `.env` file.
	* **UID**. User ID who runs the dockers (typically `pi` user, run `id -u pi`).
	* **GID**. Group ID who runs the dockers (typically `pi` group, run `id -g pi`).
	* **MEDIA**. Root directory where films, series, etc are saved.
	* **STORAGE**. Root directory for torrents and temporal files.
	* **NETWORK_SUBNET**. Configure your subnet (for example, 192.168.1.0/24).
	* **NETWORK_GATEWAY**. Configure your gateway (for example, 192.168.1.1).
	* **NETWORK_PLEX_IP**. Configure the IP to user for plex server (for example, 192.168.1.3).
	* **FLEXGET_PWD**. Password for flexget.
3. Configure transmission password.
	* Sets transmission password, `rpc-password` parameter in `transmission/settings.json` file.
	* Sets transmission password in flexget, `transmission.pwd` parameter in `flexget/variables.yml` file.
4. Configure flexget in `flexget/config.yml` as you need. Recommended:
	* Add your series in `templates.tv.series.tv`.
	* Add your torrent feeds in `tasks`.

## Executing

Once installed, just run this command (inside ``rpi-docker-packs/plex`` dir) to start all containers:
```
docker-compose up -d
```
To stop all containers run:
```
docker-compose stop
```
If you want remove all containers run:
```
docker-compose down
```

## Accessing

### Plex
http://host:32400/web/index.html 
where `host` is your `NETWORK_PLEX_IP` parameter.

First time you access you'll have to complete the configuration with the setup wizard.

### Transmission
http://host:9091/transmission/web/ 
where `host` is your Pi ip.

### Flexget
http://host:5050 
where `host` is your Pi ip.

## Troubleshooting

### Flexget is not accessible
Check logs to see the problem.
```
docker logs plex_flexget_1
```
If you see this error "Password 'XXXXX' is not strong enough. Suggestions: Add another word or two. Uncommon words are better." try to put a more complex password.

### Avoid online transcode video

By default, plex try to transcode video to save network bandwith but this can put our Pi in troubles. To avoid this you have to configure **all the clients** (web, android, ...) to use max resolution. 

For example in plex web:
1. Go to `Settings`.
2. Go to `Quality` section.
    * Turn off `Automatically Adjust Quality` check.
    * Put `Internet Streaming` to max value.
    * Put `Home Streaming` to max value.
