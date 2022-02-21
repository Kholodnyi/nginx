# CDN

Example of basic handmade CDN using nginx that consists of 5 containers - bind server, load balancer, three nodes. Try different balancing approaches and implement caching. 

### Requirements:
 - [docker](https://docs.docker.com/get-docker/) >20.10.7
 - [docker-compose](https://docs.docker.com/compose/install/) >2.1.0
 
### Setup

Firstly, need to clone the git repository, navigate to docker-compose file and execute next commands with sudo rights:  

```
docker-compose build
docker network create --gateway 172.16.1.1 --subnet 172.16.1.0/24 cdn_subnet
```

### Testing CDN
```
sudo docker exec -it tester ping cdn.picture.com

PING cdn.picture.com (172.16.1.30) 56(84) bytes of data.
64 bytes from load_balancer_world.cdn_subnet (172.16.1.30): icmp_seq=1 ttl=64 time=0.087 ms
64 bytes from load_balancer_world.cdn_subnet (172.16.1.30): icmp_seq=2 ttl=64 time=0.093 ms
64 bytes from load_balancer_world.cdn_subnet (172.16.1.30): icmp_seq=3 ttl=64 time=0.093 ms
64 bytes from load_balancer_world.cdn_subnet (172.16.1.30): icmp_seq=4 ttl=64 time=0.096 ms
```

### Testing Load Balancers
```
docker-compose run --rm siege -c25 -t10s -b "http://cdn.picture.com/django-logo.png"
```

#### Cache on
```
Transactions:                 118196 hits
Availability:                 100.00 %
Elapsed time:                   9.01 secs
Data transferred:            2492.38 MB
Response time:                  0.00 secs
Transaction rate:           13118.31 trans/sec
Throughput:                   276.62 MB/sec
Concurrency:                   24.57
Successful transactions:      118197
Failed transactions:               0
Longest transaction:            0.50
Shortest transaction:           0.00
```

#### Cache off, balancing model: Round Robin
```
Transactions:                  73256 hits 
Availability:                 100.00 %    
Elapsed time:                   9.58 secs 
Data transferred:            1572.98 MB   
Response time:                  0.01 secs 
Transaction rate:            7646.76 trans/sec
Throughput:                   164.19 MB/sec   
Concurrency:                   24.65
Successful transactions:       73256
Failed transactions:               0
Longest transaction:            0.62
Shortest transaction:           0.00
```

#### Cache off, balancing model: least connection
```
Transactions:                  81739 hits      
Availability:                 100.00 %         
Elapsed time:                  10.24 secs      
Data transferred:            1891.12 MB        
Response time:                  0.01 secs      
Transaction rate:            7982.32 trans/sec 
Throughput:                   184.68 MB/sec    
Concurrency:                   24.58           
Successful transactions:       81739           
Failed transactions:               0           
Longest transaction:            0.86          
Shortest transaction:           0.00           
```
