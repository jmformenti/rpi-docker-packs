# Docker nginx-proxy for arm64

Based on [jwilder-nginx-proxy-arm](https://github.com/Budry/jwilder-nginx-proxy-arm)

``docker-gen`` was generated on a ``Raspberry Pi 4`` with ``Raspbian OS 64 bits`` using ``pi`` user:
```
sudo apt install golang
mkdir -p /home/pi/go/src/github.com/jwilder
cd /home/pi/go/src/github.com/jwilder
git clone https://github.com/jwilder/docker-gen.git
cd docker-gen
go get github.com/robfig/glock
export PATH=$PATH:/home/pi/go/bin
glock install github.com/jwilder/docker-gen
make get-deps
make
```
