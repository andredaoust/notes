# Docker 

Just a small list of commands I've had to lookup or use more than once.  




## Kill all containers
```
 docker container stop $(docker container list -q)
```

## Stop all containers in swarm  
You can scale down the pods to 0 with:
```
docker service scale serviceName=0
```
where serviceName is the service you'd like to scale down.  

## End docker swarm
```
docker swarm leave [OPTIONS]
```

There is only one option, `--force` which will ignore any warnings when attempting to leave the swarm.