### 安装



https://linuxhint.com/install_minio_ubuntu_1804/



```shell
# !/bin/bash

# add minio user group
sudo useradd --system minio-user --shell /sbin/nologin

# download minio
curl -O http://172.16.0.201:4321/minio/minio

# executable
sudo chmod +x minio

# mv minio to /usr/local/bin
sudo mv minio /usr/local/bin

# add minio permission
sudo chown minio-user:minio-user /usr/local/bin/minio


# config envirnment
echo '' > /etc/default/minio
cat <<EOT >> /etc/default/minio
# Volume to be used for MinIO server.
MINIO_VOLUMES="/data/minio/"
# Use if you want to run MinIO on a custom port.
MINIO_OPTS="-C /etc/minio --address :9000
# Access Key of the server.
MINIO_ACCESS_KEY=metapro
# Secret key of the server.
MINIO_SECRET_KEY=caaa84acca34f6fbe1be609bc4e81c43

EOT

# make minio config directory
sudo mkdir /usr/local/share/minio
sudo mkdir /etc/minio
sudo mkdir -p /data/minio

# add minio permission
sudo chown minio-user:minio-user /usr/local/share/minio
sudo chown minio-user:minio-user /etc/minio
sudo chown minio-user:minio-user /data/minio

# download minio system config
curl -O http://39.104.72.253:9000/founder/minio.service

# mv config file
sudo mv minio.service /etc/systemd/system

# reload config
sudo systemctl daemon-reload
# enable auto start when reboot
sudo systemctl enable minio

sudo systemctl start minio
sudo systemctl status minio
```



### minio集群下的写入速度比较慢

![image-20200422135458244](/Users/lttzzlll/Documents/notes/minio.assets/image-20200422135458244.png)

说明切分文件和计算纠删码还是比较耗费CPI的



### minio的数据迁移



https://www.scaleway.com/en/docs/how-to-migrate-object-storage-buckets-with-minio/