### nginx限流

```
location ^~ /videos/ {
...
limit_rate_after 1m;
limit_rate 150k;
...
}

```



https://www.scalescale.com/tips/nginx/how-to-limit-nginx-download-speed/

