# ss-client-stack
使用docker-compose集成了kcp和privoxy的shadowsocks客户端，主要方便服务器科学上网。

[English](README_en.md)

## 如何使用

下载这个仓库到你的主机
```bash
git clone https://github.com/lonelyleaf/ss-client-stack.git
```

安装docker和docker-compose，如果已经安装可以跳过此步。具体如何安装可以参考 [安装docker-compose](https://docs.docker.com/compose/install/)
和 [安装docker](https://docs.docker.com/install/)

在centos上，可以使用下面的命令:
```bash
#安装docker
sudo curl -fsSL get.docker.com -o get-docker.sh
sudo sh get-docker.sh
#启动docker
sudo systemctl start docker
sudo systemctl enable docker
#安装docker-compose
sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

所有的配置文件都在`configs`文件夹中，你需要**自己修改**配置文件。

如果只使用`shadowsocks`,修改`sslocal-config.json`就可以了。如果还需要用`kcp`,就需要修改
`sslocal-kcp-config.json` 和 `kcp-config.json`两个文件，同时`sslocal-kcp-config.json`中的
`server`参数**必须**是`kcptun`

```json
//sslocal-config.json,修改server、port、password
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

//sslocal-kcp-config.json,"server"参数必须是"kcptun"
{
    "server": "kcptun",
    "server_port": 8989,
    .......
}
```

用docker-compose启动服务:
```bash
docker-compose up -d
```

当你需要改变配置时:
```bash
#stop your service
docker-compose down
#start service
docker-compose up -d
```

## 测试shadowsocks是否正常工作
使用curl，非常简单:
```bash
#test socks5 proxy 
curl --socks5-hostname localhost:1080 www.google.com
#test http proxy 
curl -x localhost:2080 www.google.com
```