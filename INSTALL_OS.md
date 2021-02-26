# Install Raspbian OS 64 bits

Minimal installation for Raspbian OS 64bits with docker:

1. Download latest Raspbian 64bits image from [here](https://downloads.raspberrypi.org/raspios_arm64/images/).
2. Write the image to an SD card. Recommended: use [Raspberry Pi Imager](https://www.raspberrypi.org/software/) with `Use Custom` option.
3. Enable ssh by default, adding an empty file named ``ssh`` to the SD card on `boot` partition.
4. Connect the Pi with cable and turn on.
5. Discover IP from your router and access via ssh. Default user ``pi`` with password ``raspberry``.
6. Change ``pi`` user password.
```
passwd
```
7. Configure timezone (for example, Europe/Andorra).
```
timedatectl set-timezone Europe/Andorra
```
8. Configure static IP (for example, asigning 192.168.1.2 as ip in 192.168.1.* network):
```
interface eth0
static ip_address=192.168.1.2/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1
```
9. Reboot and access via ssh using the new IP.
```
reboot
```
10. Update the system.
```
sudo apt update
sudo apt upgrade
```
11. Install ``docker`` and ``docker-compose``.
```
sudo apt install docker docker-compose
```
12. Add ``pi`` user to docker group.
```
sudo usermod -aG docker pi
```
13. Enable and start docker service.
```
sudo systemctl enable --now docker.service
```

Then you can [mount an USB disk](https://www.raspberrypi.org/documentation/configuration/external-storage.md) (or anything else) where store all your stuff.

**Tip**: To extend the life of your SD it is recommended to use a directory of your external storage for the temporary docker directory. Add this line to ``/etc/defaul/docker`` file using your dir:
```
export DOCKER_TMPDIR=/path/to/docker/tmp/dir
```
