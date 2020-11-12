### supervisorctl reload



It doesn't kill the supervisord process, it just stops all processes, reload the configuration file, and restart processes again.

If you just want to apply the new configurations use `reread` command. It'd just reload the configuration without stopping, and respawning processes.

And running `update` will restart the processes (groups) that have changed.



```shell
supervisorctl reread
supervisorctl update
```





https://stackoverflow.com/questions/3792081/will-reloading-supervisord-cause-the-process-under-its-to-stop

