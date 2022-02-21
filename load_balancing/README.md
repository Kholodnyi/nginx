# Load balancing 

Load balancing based on geo data can be easily setup with nginx

### Requirements:
 - [docker](https://docs.docker.com/get-docker/) >20.10.7
 - [docker-compose](https://docs.docker.com/compose/install/) >2.1.0

### Setup

Firstly, need to clone the git repository, navigate to docker-compose file and run next command:  

```shell
sudo docker-compose up -d
```

Then http://localhost/ in your web browser to test load balancer (you can use VPN to imitate access from different regions).

Based on you IP, http://localhost/ should return one of responses listed below (represent a separate nginx server):
```
US-1 server
US-2 server
UK server
Other server
Backup server
```

Logs can be found in load-balancer container at `/var/log/nginx/access.log`
