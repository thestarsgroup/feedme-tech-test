Technology used.

RabbitMQ for sending json messages to the queue so they can be consumed by another service.
MongoDB for storage (I haven't used MongoDB before so the usage may not be ideal as it would require more time to investigate)

json-adapter microservice to consume the feed of messages, create json message and send to the rabbitMQ.
packet-publisher microsevice consumes messages from the queue, applies filters and stores into MongoDB when required.

Both services use spring boot 2 with Java 11. Unit tests in Junit and Mockito, test converage not ideal but enough to present approach to testing left some code unstested due to the time spend to get it all in the working state. No integration or performance tests.

json-adapter has a docker compose file that starts the rabbit, mongo and provided inside docker, almost got running those two services in docker to but due to the time spend on this test decided not to full finish it as I had some issue. docker files can been seen in the project directory.

How to run:
1. In json-adapter project directory run docker-compose up to start up rabbitmq, mongodb and feed provider.
2. In an new terminal in each projects directory(json-adapter & packet-publisher) run: mvn clean install
   This will build the jar required to run and execute all the unit tests.
3. Inside json-adapter project directory run: java -jar target/json-adapter-1.0.jar to start the service.
4. Inside packet-publisher directory run: java -jar target/packet-publisher-1.0.jar to start the service.
5. Send a curl command to start consuming data feed from json adapter: curl http://localhost:8080/packet/retrieve/all
6. Go to the rabbitmq to check relevant exchanges and queues for the data flow: http://localhost:15672/#/
