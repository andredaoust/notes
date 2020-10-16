# Docker 

Just a small list of commands I've had to lookup or use more than once.  




## Kill all containers
```
 docker container stop $(docker container list -q)
```

## End docker swarm
```
docker swarm leave [OPTIONS]
```

There is only one option, `--force` which will ignore any warnings when attempting to leave the swarm.