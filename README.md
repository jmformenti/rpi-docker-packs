# Raspberry Docker packs

Some docker packs using docker-compose to easily add new services to your Raspberry Pi.

Tested on Raspberry Pi 4 (4GB).

## Requirements

Before starting you need a 64bits OS with docker installed on your Raspberry Pi.

If not, you can follow this [few steps](INSTALL_OS.md) to install and configure the Raspbian 64bits OS on your Pi.

## Packs

These are current available packs:

1. [Nextcloud](nextcloud/README.md): Nextcloud with php-fpm, mariadb, https (using Let's Encrypt) and full-text search activated and working.
2. [Plex](plex/README.md): Plex server installed alongside transmission and flexget connecting both.
