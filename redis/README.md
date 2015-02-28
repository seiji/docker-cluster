# Create Redis cluster

* $ compose up
* $ docker exec -it redis01 bash
* # /tmp/redis-trib.rb create $(hostname -i):6379 redis02:6379 redis03:6379 
