# piarmy-elasticsearch-server

#### This is currently in progress. This notice will be removed when ready for deployment and documentation is updated. Assume unusable nonsense below.

## Notes
This is part of a tutorial series, documentation and post links coming soon.

## ON HOST, prior to deployment

#### Important: On host:
`sudo nano /etc/sysctl.conf`

Add or set:

`vm.max_map_count=262144`

# Check with:
sudo sysctl vm.max_map_count

#### Volume
In this example, we're mounting a host volume for the elasticsearch server: /elasticsearch-cluster-data:/var/lib/elasticsearch

This /elasticsearch-cluster-data directory *must* exist on the target host prior to deploying. This will store elasticsearch data on the host, persisting between container instances. Kibana stores it's data in elastic search, so Kibana will also persist: visualizations, dashboards, etc.


## Build and run:
```
cd /home/pi/images/piarmy-elasticsearch-cluster/server && \
  docker build -t mattwiater/piarmy-elasticsearch-cluster . && \
  docker run -it --rm --name piarmy-elasticsearch-cluster -p 9200:9200 -p 9300:9300 mattwiater/piarmy-elasticsearch-cluster /bin/bash
```

# Push:
```
cd /home/pi/images/piarmy-elasticsearch-cluster/server && \
  docker build -t mattwiater/piarmy-elasticsearch-cluster . && \
  docker push mattwiater/piarmy-elasticsearch-cluster
```

# Stack
```
docker stack rm piarmy && \
docker stack deploy -c /home/pi/projects/piarmy-scripts/swarm/piarmy-elasticsearch-cluster.yml piarmy
```

## Service takes awhile t start, check logs with:
docker exec -it $(docker ps | grep piarmy-elasticsearch-cluster | awk '{print $1}') tail /var/log/elasticsearch/piarmy.log

## Enter running container
docker exec -it $(docker ps | grep piarmy-elasticsearch-cluster | awk '{print $1}') /bin/bash

## Cluster endpoints:
http://piarmy02:9200/_cluster/state?pretty
http://piarmy02:9200/_cluster/health?pretty
http://piarmy02:9200/_cluster/stats?human&pretty
http://piarmy02:9200/_cluster/pending_tasks
http://piarmy02:9200/_nodes
http://piarmy02:9200/_nodes/process?pretty

## To get full command run by elasticsearch:

`ps auxwww`

#=>

```
/usr/bin/java -Xms256m -Xmx265m -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly -XX:+AlwaysPreTouch -server -Xss1m -Djava.awt.headless=true -Dfile.encoding=UTF-8 -Djna.nosys=true -Djdk.io.permissionsUseCanonicalPath=true -Dio.netty.noUnsafe=true -Dio.netty.noKeySetOptimization=true -Dio.netty.recycler.maxCapacityPerThread=0 -Dlog4j.shutdownHookEnabled=false -Dlog4j2.disable.jmx=true -Dlog4j.skipJansi=true -XX:+HeapDumpOnOutOfMemoryError -Des.path.home=/usr/share/elasticsearch -cp /usr/share/elasticsearch/lib/* org.elasticsearch.bootstrap.Elasticsearch -d -p /var/run/elasticsearch/elasticsearch.pid -Edefault.path.logs=/var/log/elasticsearch -Edefault.path.data=/var/lib/elasticsearch -Edefault.path.conf=/etc/elasticsearch

```