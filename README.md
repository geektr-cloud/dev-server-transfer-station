# dev-server-transfer-station

this project deployed via [geektr-cloud/deployer](https://github.com/geektr-cloud/deployer)

## Deploy

```bash
# update (init) project to local enviroment
# when first run, it will init data directory and secrets directory
deployer update geektr-cloud/dev-server-transfer-station

# edit secrets files
# vim xxxxxx

# up the services
deployer up geektr-cloud/dev-server-transfer-station
```

# Port Usage

port|desc
-|-
5700|frp port
5701|frp udp port
5750|frp admin panel
5780|http
5743|https
5722|ssh

# proxy a entry server transfer station

```
DEV_FRP=$(dig +short dev.frp.geektr.cloud | sed -n '/[0-9]/p')

iptables -A FORWARD -j ACCEPT
iptables -t nat -A POSTROUTING -j MASQUERADE
iptables -t nat -A PREROUTING -p tcp --dport 22 -j DNAT --to-destination $DEV_FRP:5722
iptables -t nat -A PREROUTING -p tcp --dport 80  -j DNAT --to-destination $DEV_FRP:5780
iptables -t nat -A PREROUTING -p tcp --dport 443 -j DNAT --to-destination $DEV_FRP:5743
```
