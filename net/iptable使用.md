### iptable



- 设置入口流量

```bash
iptables -I INPUT -p tcp --dport 9222 -m state --state NEW -j ACCEPT
```

