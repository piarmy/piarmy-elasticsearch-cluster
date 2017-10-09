# piarmy-elasticsearch-kibana

#### This is currently in progress. This notice will be removed when ready for deployment and documentation is updated. Assume unusable nonsense below.

## Notes
This is part of a tutorial series, documentation and post links coming soon.

# Build and run:
```
cd /home/pi/images/piarmy-elasticsearch-cluster/kibana && \
  docker build -t mattwiater/piarmy-elasticsearch-kibana-cluster .  && \
  docker run -it --rm -p 5601:5601 piarmy-elasticsearch-kibana-cluster /bin/bash
```

# Push:
```
cd /home/pi/images/piarmy-elasticsearch-cluster/kibana && \
  docker build -t mattwiater/piarmy-elasticsearch-kibana-cluster . && \
  docker push mattwiater/piarmy-elasticsearch-kibana-cluster
```

## Enter running container
docker exec -it $(docker ps | grep piarmy-elasticsearch-kibana-cluster | awk '{print $1}') /bin/bash

