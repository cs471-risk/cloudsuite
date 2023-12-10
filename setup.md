## Data-serving

### Server
Set up server container
```
docker run --rm --name cassandra-server --net host ds-server --listen-ip=192.168.100.2
```

### Client
1. Set up client container
```
docker run -it --rm --name cassandra-client --net host ds-client bash
```

2. Warmup server (10 GB data, 1 thread)
```
./warmup.sh 192.168.100.2 10000000 1
```

3. Run benchmark
```
./load.sh 192.168.100.2 10000000 1000 10
```

## Data-caching
### Server
Set up server container
```
docker run --name dc-server --net host -d cloudsuite/data-caching:server -t 4 -m 10240 -n 550
```

### Client
Run benchmark (will run indefinitely)
```
docker exec -it dc-client /bin/bash /entrypoint.sh --m="RPS" --S=28 --g=0.8 --c=1 --w=1 --T=1 --r=21000
```

Alternatively, set timeout
```
docker exec -it dc-client timeout 10 /bin/bash /entrypoint.sh --m="RPS" --S=28 --g=0.8 --c=200 --w=8 --T=1 --r=21000 
```
**Adjust --r argument to meet 99th latency < 1 ms**

## Media-streaming
### Dataset
Populate videos and logs
```
docker run --name streaming_dataset cloudsuite/media-streaming:dataset 1 1
```

### Server
Set up server container
```
docker run -d --name streaming_server --volumes-from streaming_dataset --net host cloudsuite/media-streaming:server 50
```

### Client
Run benchmark
```
docker run -t --name=streaming_client -v /home/cloudsuite/media-streaming-logs:/videos/logs -v /home/cloudsuite/media-streaming-result:/output --net host cloudsuite/media-streaming:client 192.168.100.2 1 100
```