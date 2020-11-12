```
docker system prune --force && \
docker-compose --file=$PWD/docker-compose.yml up --remove-orphans --build
```



# [How to rebuild and update a container without downtime with docker-compose?](https://stackoverflow.com/questions/42529211/how-to-rebuild-and-update-a-container-without-downtime-with-docker-compose)



29







from the manual [docker-compose restart](https://docs.docker.com/compose/reference/restart/)

> If you make changes to your docker-compose.yml configuration these changes will not be reflected after running this command.

you should be able to do

```bash
$docker-compose up -d --no-deps --build <service_name>
```

The `--no-deps` will not start linked services.



https://stackoverflow.com/questions/42529211/how-to-rebuild-and-update-a-container-without-downtime-with-docker-compose/42751272