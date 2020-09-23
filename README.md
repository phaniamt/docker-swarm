### docker-swarm deployment with zero downtime ###

#### Create a docker-compose file for nginx stack and deploy it ####
    
    version: '3.7'
    networks:
      nginx:
        external: false
    services:
      nginx:
        image: nginx:1.18
        ports:
          - '8088:80'
        deploy:
          replicas: 4
          update_config:
            parallelism: 2
            order: start-first
            failure_action: rollback
            delay: 10s
          rollback_config:
            parallelism: 0
            order: stop-first
          restart_policy:
            condition: any
            delay: 5s
            max_attempts: 3
            window: 120s
        healthcheck:
          test: ["CMD", "service", "nginx", "status"]
        networks:
          - nginx
#### Deploy the docker stack ####
    docker stack deploy -c docker-compose.yml nginx
#### List the docker stacks ####
    docker stack ls
#### List the docker services ####
    docker service ls
#### List the running docker containers ####
    docker ps
#### Update the docker image tag in the docker-compose file ####
    version: '3.7'
    networks:
      nginx:
        external: false
    services:
      nginx:
        image: nginx:1.19
        ports:
          - '8088:80'
        deploy:
          replicas: 4
          update_config:
            parallelism: 2
            order: start-first
            failure_action: rollback
            delay: 10s
          rollback_config:
            parallelism: 0
            order: stop-first
          restart_policy:
            condition: any
            delay: 5s
            max_attempts: 3
            window: 120s
        healthcheck:
          test: ["CMD", "service", "nginx", "status"]
        networks:
          - nginx
#### Update  the docker stack ####
    docker stack deploy -c docker-compose.yml nginx
#### List the docker stacks ####
    docker stack ls
#### List the docker services ####
    docker service ls
#### List the running docker containers ####
    docker ps
#### Ref Links ####
https://medium.com/better-programming/zero-downtime-deployment-with-docker-swarm-d84d8d9d9a14
https://docs.docker.com/compose/compose-file/#update_config
