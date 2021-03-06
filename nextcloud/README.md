# Nextcloud over Docker on Raspberry

Build your own [nextcloud](https://nextcloud.com/) server with php-fpm, https (using [Let's Encrypt](https://letsencrypt.org/)) and [full-text search](https://apps.nextcloud.com/apps/fulltextsearch) working on your Raspberry from scratch.

Tested on Raspberry Pi 4 (4GB).

## Requirements

Before starting you need a 64bits OS with docker installed on your Raspberry Pi.

If not, you can follow this [few steps](../INSTALL_OS.md) to install and configure the Raspbian 64bits OS on your Pi:
 
## Install

To install plex, transmission and flexget run this steps:
1. Clone this repo and enter in ``rpi-docker-packs/nextcloud`` directory.
```
git clone https://github.com/jmformenti/rpi-docker-packs.git
cd rpi-docker-packs/nextcloud
```
2. Configure parameters inside `.env` file.
	* **NEXTCLOUD_VOLUME**. Root directory where nextcloud and related dockers write config and data.
	* **NEXTCLOUD_ADMIN_USER**. Username for admin user.
	* **NEXTCLOUD_ADMIN_PASSWORD**. Password for admin user.
	* **MYSQL_ROOT_PASSWORD**. Root password for mariadb.
	* **WEB_VIRTUAL_HOST_INTRA**. Your raspberry local network IP/name (for example, 192.168.1.2).
	* **WEB_VIRTUAL_HOST_EXTRA**. Your internet domain to use for nextcloud (for example, my-personal-domain.duckdns.org).
	* **WEB_LETSENCRYPT_EMAIL**. Your email to use with Let's Encrypt.
3. Configure parameters inside `db.env` file.
	* **MYSQL_USER**. User for mariadb nextcloud database.
	* **MYSQL_PASSWORD**. Password for mariadb nextcloud user.
	* **MYSQL_DATABASE**. Database name for nextcloud.

## Executing

Once prepared, just run this command (inside ``rpi-docker-packs/nextcloud`` dir) to start all containers:
```
docker-compose up -d
```
**NOTE:** The first time it will take a while to get started and to be available the web.

To stop all containers run:
```
docker-compose stop
```
If you want remove all containers run:
```
docker-compose down
```

## Accessing

### From internet

Accessing from internet:

https://host
where `host` is your external dns name, i.e. `WEB_VIRTUAL_HOST_EXTRA` parameter.

### From local network

Accessing from inside your network:

http://host
where `host` is your raspberry IP, i.e. `WEB_VIRTUAL_HOST_INTRA` parameter.
 
## Configuring

Once you have logged in nextcloud with the admin user, you can configure full-text search plugin:

1. Activate full-text search plugin.
	* Go to `Apps` and install: `Full text search`, `Full text search - Elasticsearch platform` and `Full text search - Files`.
	* Go to `Settings` -> `Administration` -> `Full text search`.
	* Select `Elasticsearch` from `Search Platform` drop-down.
	* Fill in `Elastic Search` section:
		* `Address of the Servlet`: `http://elasticsearch:9200`
		* `Index`: a name (for example, my_index)
	* That's all! to check the plugin, run:
	```
	docker exec -u www-data -it nextcloud_cron_1 ./occ fulltextsearch:check
	```
	*Note*: a cron is already configured to index existing and new files.

## Troubleshooting

### Reset password

If at any time you lose the admin password, you can perform a reset by running:
```
docker exec -it -u www-data nextcloud_app_1 ./occ user:resetpassword <username>
```

### Use Collabora Online app

You can use `Collabora Online` app from your own server using `Collabora Online - Built-in CODE server (ARM64)` but currently if you activated it, it is very likely that your nextcloud will become extremely slow.

To avoid that we need to wait that [this issue](https://github.com/nextcloud/richdocuments/issues/1282) be resolved.
