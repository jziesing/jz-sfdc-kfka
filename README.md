# Salesforce Multi Org Syn Using Kafka & Postgres


### IN PROGRESS

needs: https://github.com/amiel/heroku-buildpack-apt#feature/support-adding-keys
  and  heroku/jvm

This proof of concept is intended to demonstrate the use of Kafka Connect to sync the data from Heroku Postgres to Heroku Kafka and f
## Usage

**Create Kafka Connect Connectors**

Source Connector 

```
curl -X POST https://<HEROKU_APP_URL>/connectors -H "Content-Type: application/json" --data @- << EOF
{
    "name": "postgres-source",
    "config": {
      "connector.class": "io.confluent.connect.jdbc.JdbcSourceConnector",
      "tasks.max": "1",      
      "connection.url": "jdbc:postgresql://<Postgres-Host>:5432/<Database-Name>?user=<Database-UserName>&password=<DatabasePassword>",
      "mode": "incrementing",
      "incrementing.column.name": "id",
      "topic.prefix": "connect_"      
    }
}
EOF
```

Sink Connector 

```
curl -X POST https://<HEROKU_APP_URL>/connectors -H "Content-Type: application/json" --data @- << EOF
{
    "name": "redshift-sink",
    "config": {
      "connector.class": "io.confluent.connect.jdbc.JdbcSinkConnector",
      "tasks.max": "1",
      "topics": "connect_demo",
      "connection.url": "jdbc:redshift://<Redshift-Host>:5439/<Redshift-Database>?user=<Redshift-UserName>&password=<Redshift-Password>",
      "auto.create": "false"
    }
}
EOF
```

**Delete Kafka Connect Connectors**

```
curl -X DELETE https://<HEROKU_APP_URL>/connectors/<CONNECTOR_NAME> 
```

**Insert some data into Heroku Postgres**


**JDBC Connector Info**
- https://www.confluent.io/blog/kafka-connect-deep-dive-jdbc-source-connector
- https://docs.confluent.io/current/connect/kafka-connect-jdbc/source-connector/index.html
- https://docs.confluent.io/current/connect/kafka-connect-jdbc/sink-connector/index.html
- https://docs.confluent.io/current/connect/managing/connectors.html#connect-bundled-connectors
- https://docs.confluent.io/current/connect/kafka-connect-jdbc/source-connector/source_config_options.html
- https://docs.confluent.io/current/connect/kafka-connect-jdbc/sink-connector/sink_config_options.html






