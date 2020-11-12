### linux中如何新建一个root用户



```bash
sudo useradd tom
```



```bash
sudo usermod -aG sudo <username>
```



```bash
sudo -l
```



```bash
grep '^sudo' /etc/group
```



https://build5nines.com/add-new-root-user-ubuntu-server-using-bash



https://www.liquidweb.com/kb/how-to-add-a-user-and-grant-root-privileges-on-ubuntu-16-04/