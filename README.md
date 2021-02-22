# Kafka Mirror Maker PoC

This is a tiny project to help learn more about Kafka Mirror Maker (v1 and v2).

## Containers

Basically this project has compose and configuration files for Active/Passive
Broker Replication.

```text
Broker with v1.1.1 (with log format version set to 0.8.2.2) -> Broker with latest version
Broker with latest version -> Broker with latest version
```

**Project Structure**:
```text
.
├── compose
│   ├── mmv1
│   │   ├── 1.1.1-latest.yaml
│   │   └── latest-latest.yaml
│   └── mmv2
│       ├── 1.1.1-latest.yaml
│       └── latest-latest.yaml
└── config
    ├── mmv1
    │   ├── consumer.properties
    │   └── producer.properties
    └── mmv2
        └── config.properties
```

### Starting containers

You can start the compose files with
`docker-compose -f compose/<mirror-maker-version>/<source-broker-version>-<target-broker-version>.yaml`.

**For instance**:
```
# Mirror Maker v1
♪ docker-compose -f compose/mmv1/1.1.1-latest.yaml up -d
♪ docker-compose -f compose/mmv1/latest-latest.yaml up -d

# Mirror Maker v2
♪ docker-compose -f compose/mmv2/1.1.1-latest.yaml up -d
♪ docker-compose -f compose/mmv2/latest-latest.yaml up -d
```

### Checking containers

After the container `source-broker-setup` has been run, you can check the source
and target broker with the following commands:

```bash
♪ export TOPIC_NAME='<topic-name>'

# Checking source broker
♪ docker exec kfk-source \
    kafka-topics --zookeeper zk-source:2181 --list
♪ docker exec kfk-source \
    kafka-console-consumer --bootstrap-server kfk-source:9092 --topic "${TOPIC_NAME}" --from-beginning

# Checking target broker
♪ docker exec kfk-target \
    kafka-topics --zookeeper zk-target:2181 --list
♪ docker exec kfk-target \
    kafka-console-consumer --bootstrap-server kfk-target:9092 --topic "${TOPIC_NAME}" --from-beginning
```
