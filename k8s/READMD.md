# kubernetes上部署ss client statck

## 部署

在k8s上，将所有配置放在了config-map中，用户应该先自行修改其中的内容然后按
顺序将资源部署到k8s上。例如按顺序执行如下kubectl命令:

```bash
kubectl apply -f ./k8s/10config.yml
kubectl apply -f ./k8s/20kcp.yml
kubectl apply -f ./k8s/30ss-client.yml
kubectl apply -f ./k8s/40privoxy.yml
```

如果你证书是自签的或者遇到如下类似问题
>Unable to connect to the server: x509: certificate signed by unknown authority

记得加上`--insecure-skip-tls-verify=true`参数

## 集群外访问

如果想将服务开放给k8s外的服务使用，可以将`50privoxy-nodeport.yml`也部署
到k8s上，之后可以使用`32080`来访问

## 测试连接

如果在集群内部，并且你的k8s部署了dns服务(一般默认都会部署)。那么在任何
一个pod中通过域名`privoxy.ss-stack`即可访问到服务,所以可以在任意pod中
执行以下命令来进行测试

```bash
#test socks5 proxy 
curl --socks5-hostname ss-client.ss-stack:1080 www.google.com
#test http proxy 
curl -x privoxy.ss-stack:2080 www.google.com
```

如果要通过集群外访问，由于使用的nodeport，在任一k8s的node上通过`32080`
都可以访问到privoxy服务，测试原理同上，这里就不赘述了。