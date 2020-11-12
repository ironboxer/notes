### 环境变量放在哪?



[https://askubuntu.com/questions/164586/environment-variables-where-are-they-stored-by-linux-how-do-i-change-them-and#:~:text=The%20Global%20environment%20variables%20of,made%20here%20to%20take%20effect.](https://askubuntu.com/questions/164586/environment-variables-where-are-they-stored-by-linux-how-do-i-change-them-and#:~:text=The Global environment variables of,made here to take effect.)





The **Global** environment variables of your system are stored in `/etc/environment`.
Any changes here will get reflected throughout the system and will affect all users of the system. Also, you need a Reboot, for any changes made here to take effect.

**User** level Environment variables are mostly stored in `.bashrc` and `.profile` files in your Home folder. Changes here only affect that particular user. Just close and open the terminal for configuration changes to take place.

*Edit* : If you don't want to Reboot or restart your terminal, you can make use of the source command.
Eg. `source /etc/environment` or `source .bashrc`