

### 如何配置haproxy

https://linuxacademy.com/guide/14569-configuration-of-haproxy/



### reload config

```
sudo haproxy -p /var/run/haproxy.pid -sf $(cat /var/run/haproxy.pid)
```