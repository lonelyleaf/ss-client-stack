# ss-client-stack
Shadowsocks client stack integrated kcp and privoxy for linux server,using docker-compose.

## Usage

Clone this repo in your server
```bash
git clone https://github.com/lonelyleaf/ss-client-stack.git
```

Install docker and docker-compose,if you have installed,skip this step.see [install docker-compose](https://docs.docker.com/compose/install/)
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

All the config files are in `configs` folder.You need to **edit them by your owm**.

For `shadowsocks`,edit`sslocal-config.json`.If you use shadowsocks with `kcp`,you need to 
edit both `sslocal-kcp-config.json` and `kcp-config.json`,and the `server` in `sslocal-kcp-config.json`
**must** be `kcptun`!!
```json
//sslocal-config.json,edit server、port、password
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

//sslocal-kcp-config.json,"server" must be "kcptun"
{
    "server": "kcptun",
    "server_port": 8989,
    .......
}
```

Then start service by docker-compose:
```bash
docker-compose up -d
```

If need change configs,after configs files are changed,:
```bash
#stop your service
docker-compose down
#start service
docker-compose up -d
```

## Test whether shadowsocks works
you can use curl,it's easy:
```bash
#test socks5 proxy 
curl --socks5-hostname localhost:1080 www.google.com
#test http proxy 
curl -x localhost:2080 www.google.com
```