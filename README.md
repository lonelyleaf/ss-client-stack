# ss-client-stack
Shadowsocks client stack for linux server,using docker-compose.

## Usage

Clone this repo in your server
```bash
git clone https://github.com/lonelyleaf/ss-client-stack.git
```

Install docker and docker-compose,see [install docker-compose](https://docs.docker.com/compose/install/)
and [install docker](https://docs.docker.com/install/)

In centos,you may use:
```bash
#install docker
sudo curl -fsSL get.docker.com -o get-docker.sh
sudo sh get-docker.sh
#start docker
sudo systemctl start docker
sudo systemctl enable docker
#install docker-compose
sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

Write you own `sslocal.json` file in `ss-client` folder,like:
```json
{
    "server": "1.2.3.4",
    "server_port": 8989,
    "method": "aes-128-cfb",
    "password": "123456",
    "fast_open": false,
    "local_address": "127.0.0.1",
    "local_port": 1080,
    "workers": 2
}
```

Then run by docker-compose:
```bash
docker-compose up -d
```

If need update the sslocal config:
```bash
#stop your service
docker-compose down
#rebuild your image
docker-compose build
#start service
docker-compose up -d
```

## test sslocal with http proxy
you can use curl,it's easy:
```bash
curl -x localhost:2080 www.google.com
```