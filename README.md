## Getting Started

### Ensure Zookeeper is running and has registered brokers for both clusters

```bash
docker-compose exec zookeeper1 zookeeper-shell localhost:2181
```

Then run:

```bash
get /controller
get /cluster/id
ls /brokers/ids
```

Do the same for Zookeeper 2 to make sure everything is okay:

```bash
docker-compose exec zookeeper2 zookeeper-shell localhost:2182
```

Doing this should give you two `cluster/id`s:

Cluster 1: `CXO-syrOSP2STbhNBQVU5g`
Cluster 2: `-Eo-O5OxSFmXE3Kz6uhR1Q`

### Ensure Schema Registry is functional on both sides

For Schema Registry 1:

```bash
curl --silent -X GET http://localhost:8081/subjects/ | jq
curl --silent -X GET http://localhost:8081/config | jq
curl -s -XGET http://localhost:8081/schemas/types | jq
```

For Schema Registry 2:

```bash
curl --silent -X GET http://localhost:8083/subjects/ | jq
curl --silent -X GET http://localhost:8083/config | jq
curl -s -XGET http://localhost:8083/schemas/types | jq
```

### Ensure ReST Proxy is working as expected

Finally, we will make sure both ReST Proxy instances are running on both clusters; for ReST Proxy 1:

```bash
curl --silent -X GET "http://localhost:8082/topics" | jq
```

And for ReST Proxy 2:

```bash
curl --silent -X GET "http://localhost:8084/topics" | jq
```

### Set up Schema Linking

Now let's set up Schema Exporters:

TODO - complete this...

```
docker-compose exec schema-registry1 bash -c 'schema-exporter --create --schema.registry.url http://localhost:8081 --name schema-link --config-file'
```

To get a full list of commands, run:

```bash
docker-compose exec schema-registry1 bash -c 'schema-exporter --help'
```