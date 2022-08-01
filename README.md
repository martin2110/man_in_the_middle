## create cert for stunnel
```
openssl req -batch -new -x509 -days 365 -nodes -out mitm.pem -keyout mitm.pem
```
## create config
```
cat <<EOF >> stunnel.conf
foreground = yes
debug = info
output = stunnel.log
[server]
client = no
cert= ./mitm.pem
accept = source:443
connect = 127.0.0.1:31337

[client]
client = yes
accept = 127.0.0.1:31337
connect = <TARGET>:443
EOF
```
## run stunnel 
```
yum install stunnel
stunnel stunnel.conf
```
## capture traffic
```
tcpdump -i lo port 31337 -s 0 -w /tmp/mitm.pcap
```
```
# this might be useful as well
docker run andboson/http-cli-echo-logger
```
