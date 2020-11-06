#Axion Framework Gift card example

## Install
To run this example you must use Axion Framework and Postgres DB


Axion Server
```
docker run -d --name axonserver -p 8024:8024 -p 8124:8124 axoniq/axonserver
```

Postgress install
```
docker run -it --rm --name postgres -p 5432:5432 -e POSTGRES_USER=giftcard -e POSTGRES_PASSWORD=secret postgres:9.6
```

## Run
This application has 3 spring profiles emulating a microservice architecture

- command: process and validate each command for apply into the Aggregate
- query: handle every Event to persit and find from DB
- client: Simulate the application flow sending multiple commands and reading queries.

launch `com.example.giftcard.GiftcardApplication` with these profiles and open `http://localhost:8024/#overview`
