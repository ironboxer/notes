### 使用ssh建立代理

```shell
ssh -N -f -D 3128 root@39.104.139.66
```



### 查看当前所有tcp端口

```
netstat -ntlp  
```



### 显示路径存储大小

```shell
du -sh *
```



### curl download file

```
curl -O http://example.com/download/myfile.zip
```

```
curl -o localname.zip http://example.com/download/myfile.zip
```



### pstree

```
pstree -p 602
```

