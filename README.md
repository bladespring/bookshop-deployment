# Deployment for **Bookshop**

**Bookshop** is a cloud native microservices system that sells books online. It features two categories of users:

* *Customers* - can browse books in the catalog, buy some and check out their orders. 
    
* *Employers* - can manage books, update existing ones, and add new items to the catalog
---
## Setup Instructions
* Install [docker](https://docs.docker.com/engine/install/)

* Clone this repository
```bash
git clone https://github.com/bladespring/bookshop-deployment
```
* Navigate to `docker` directory
```bash
cd bookshop-deployment/docker
```
* Run application using [docker-compose](https://docs.docker.com/compose/)
```bash
docker-compose up 
```
---
## Architecture of **Bookshop** System 

```mermaid
flowchart TD
    Customer["Customer
    [Person]

    A customer of the Bookshop Application"]

    Employee["Employee
    [Person]

    An employee of the Bookshop Application"]

    EDGE["Edge Service
    [Container: Spring Boot]
    
    Provides an API gateway and cross-cutting concerns"]

    REDIS[("Session Store
    [Container: Redis]

    Stores web session data and cache")]

    SPA["SPA
    [Container: Angular]

    Provides Bookshop functionality to users"]   

    CAT["Catalog Service
    [Container: Spring Boot]

    Provides functionality for managing the books in the catalog"]

    ORDER["Order Service
    [Container: Spring Boot]

    Provides functionality for purchasing books"]

    CDB[("Catalog Database
    [Container: PostgreSQL]

    Stores book data")]

    ODB[("Order Database
    [Container: PostgreSQL]

    Stores order data")]

    DISP["Dispatcher Service
    [Container: Spring Boot]

    Provides functionality for dispatching orders"]

    BROKER["Event Broker
    [Container: RabbitMQ]

    Provides event processing functionality"]

    CONF["Config Service
    [Container: Spring Boot]

    Provides centralized configuration"]


    CONFREPO[("Config Repo
    [Container: Git]

    Stores configuration data")] 

    EDGE--"Delivers to the user's web browser"-->SPA

    Customer--"Uses
    [REST/HTTP]"-->EDGE

    ORDER--"Makes API calls to\n[REST/HTTP]"-->CAT


    Employee--"Uses
    [REST/HTTP]"-->EDGE

    CAT--"Reads from and writes to\n[JDBC]"-->CDB

    CAT--"Gets configuration from
    [REST/HTTP]"-->CONF

    ORDER--"Gets configuration from
    [REST/HTTP]"-->CONF

    CONF--"Reads config data from
    [REST/HTTP]"-->CONFREPO

    DISP--"Gets configuration from
    [REST/HTTP]"-->CONF

    ORDER--"Reads from and writes to\n[AMQP]"-->BROKER

    DISP--"Reads from and writes to\n[AMQP]"-->BROKER

    ORDER--"Reads from and writes to \n[R2DBC]"-->ODB

    EDGE--"Reads from and writes to\n[RESP]"-->REDIS

    EDGE--"Makes API calls to
    [REST/HTTP]"-->CAT

    EDGE--"Makes API calls to
    [REST/HTTP]"-->ORDER

    classDef person fill:lightgray,color:black
    class Customer person
    class Employee person

    classDef focusSystem fill:lightgreen,stroke:black,color:black
    class CAT focusSystem
    class ORDER focusSystem
    class DISP focusSystem
    class CONF focusSystem
    class EDGE focusSystem

    classDef frontend fill:beige,color:brown
    class SPA frontend

    classDef broker fill:white,stroke:green,color:green
    class BROKER broker

    classDef redis stroke:brown,fill:white,color:brown
    class REDIS redis
```

