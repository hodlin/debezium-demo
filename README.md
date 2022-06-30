# Debezium MySQL tutorial

To connect to mysql through remote cli use:

```
docker exec -it mysqlterm sh -c 'mysql --user="$MYSQL_REMOTE_USER" --password="$MYSQL_REMOTE_PASSWORD" --host="$MYSQL_REMOTE_HOST" --port="$MYSQL_REMOTE_PORT"'
```

To check Kafka Connect service use:

```
curl -H "Accept:application/json" localhost:8083/
```
```
curl -H "Accept:application/json" localhost:8083/connectors/
```

To register a connector to monitor sample inventory database:

1. Review connector configuration

```
{
  "name": "inventory-connector",  
  "config": {  
    "connector.class": "io.debezium.connector.mysql.MySqlConnector",
    "tasks.max": "1",  
    "database.hostname": "mysql",  
    "database.port": "3306",
    "database.user": "debezium",
    "database.password": "dbz",
    "database.server.id": "184054",  
    "database.server.name": "dbserver1",  
    "database.include.list": "inventory",  
    "database.history.kafka.bootstrap.servers": "kafka:9092",  
    "database.history.kafka.topic": "schema-changes.inventory"  
  }
}
```

2. Register Debezium MySQL connector through Kafka Connector REST API
```
curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" localhost:8083/connectors/ -d "{ \"name\": \"inventory-connector\", \"config\": { \"connector.class\": \"io.debezium.connector.mysql.MySqlConnector\", \"tasks.max\": \"1\", \"database.hostname\": \"mysql\", \"database.port\": \"3306\", \"database.user\": \"debezium\", \"database.password\": \"dbz\", \"database.server.id\": \"184054\", \"database.server.name\": \"dbserver1\", \"database.include.list\": \"inventory\", \"database.history.kafka.bootstrap.servers\": \"kafka:9092\", \"database.history.kafka.topic\": \"dbhistory.inventory\" } }"
```

3. Verify connector exists
```
curl -H "Accept:application/json" localhost:8083/connectors/
```

4. Review connector's tasks
```
curl -i -X GET -H "Accept:application/json" localhost:8083/connectors/inventory-connector
```
