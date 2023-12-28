# order-saga-api
An example of distributed transaction Saga pattern support
An entry point when saga distributed transaction starts
when the user calls the API endpoint to create an order

To start need to:
# Start Mongo in Docker
```
docker run --name mongodb_container -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=pass -p 27017:27017 -d mongodb/mongodb-community-server:latest
```
# Start 2 Postgresql instances in Docker
```
For the same machine diferent port number can be used.
For example default for postgresql is 5432. 
For the second instance 5433 can be used.
This docker-compose can be used ...  will add later
```
# Start Kafka in Docker
```
This docker-compose can be used ...  will add later
```

# Start all the microservices
```
... will add later
```

# Create one or more products by sending the request to products Get endpoint
```
...  will add later
```

# Send request to order endpoint
```
... will add later
```

# To check the products in the mongoDB send request to products endpoint
```
... will add later
```

# To check the topic messages this link can be used
```
http://localhost:9021/clusters/MkU3OEVBNTcwNTJENDM2Qg/management/topics
or 
using http://localhost:9021/clusters link by clicking on the cluster node on the UI
because the cluster id would be different for each case.
```

# Tips
### @KafkaListener

```
When developring or testing with JSON type using microservices architecture.
Properties configuration is needed when different microservices uses JSON ser/deserialization type
with different DTO path that microservice uses. Properties configuration define the dto path that this
service uses to deserialize the json object to the object to use

@KafkaListener(
 groupId = "inventory-consumer",
 topics = {"order-saga-topic"}
 properties = {"spring.json.value.default.type=org.saga.inventory.dto.saga.order.event.OrderCreateEvent"}
)

+

config.put(JsonDeserializer.USE_TYPE_INFO_HEADERS, false);

This will ensure that when deserealizing object listener will not look at the
HEADER of the message where the DTO path is present. This path is set by producer
side and the value is the object path the producer microserice uses.


```

### @RepeatableTopic with @DltHandler
```
@RepeatableTopic  automaticaly creates 2 retry topics and 1 dlt topic and send it and received it
after to retry the method execution in the method. 

Method annotated with @DltHandler handles message from DLT after riching out the limit of retires.
But dot work without @RepeatableTopic.
If need to use DLT we can send when needed and another method or separate class 
can be used with @KafkaListener using DLT topic name.
```