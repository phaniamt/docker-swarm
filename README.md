### docker-swarm deployment with zero downtime ###
   version: '3.7'

   networks:
     nginx:
       external: false

   services:

   # --- NGINX ---
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
