### Deployment for **Bookshop System**
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
```

