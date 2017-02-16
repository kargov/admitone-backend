# AdmitOne-Backend

Web application (the backend part) that models the business of a fictional competitor to TicketMaster.

## Architecture Diagram 

![](https://raw.githubusercontent.com/kargov/admitone-docs/master/admitone-backend.png)

## Technology Stack

* JAVA
* SPRING (MVC, JPA, SECURITY, AOP) 
* HIBERNATE
* JACKSON
* HSQLDB
* LOMBOK
* JUNIT
* MOCKITO

## REST API

### ADMINISTRATION

#### Login

---

Only users with role ROLE_ADMIN can use this functionality. Used to authenticate admin users.

**Endpoint** -  api/admin/login/

**Method** - GET

**Request Parameters**

| Name        | Type           
| ------------- |:-------------:
| username      | String
| password      | String        
  
**Response**

    {
    "username":bob@me.com,
    "success":true
    }

**Example**

    http://10.200.111.195:8080/admitone-backend/api/admin/login/?username=bob@me.com&password=admin

#### Tickets Overview

---

Only users with role ROLE_ADMIN can use this functionality. Lists each event, with the customers attending and the current number of uncanceled tickets.

**Endpoint** -  api/admin/tickets/overview/

**Method** - GET

**Request Parameters**

| Name        | Type           
| ------------- |:-------------:
| startEventId      | Long
| endEventId        | Long        
  
**Response**

    {
        "results": [
            {
                "eventId": 1,
                "username": "bob@me.com",
                "totalNumberOfTickets": 4
            },
            {
                "eventId": 2,
                "username": "bob@me.com",
                "totalNumberOfTickets": 4
            },
            {
                "eventId": 3,
                "username": "bob@me.com",
                "totalNumberOfTickets": 4
            }
        ],
        "success": true
    }

**Example**

    http://10.200.111.195:8080/admitone-backend/api/admin/tickets/overview/?startEventId=1&endEventId=100

### CUSTOMERS

---
 
#### Purchases

Only users with role ROLE_CUSTOMER can use this functionality. Purchases specified number of tickets for an event and a given user.

**Endpoint** -  api/customers/purchase/

**Method** - POST

**Payload**

| Name        | Type           
| ------------- |:-------------:
| username              | String
| sourceEventId         | Long
| numberOfTickets       | Long  

    {
     "username":"bob@me.com",
     "sourceEventId":1,
     "numberOfTickets":4
    }
  
**Response**

    
    {
        "user": {
            "id": 1,
            "username": "bob@me.com"
        },
        "event": {
            "id": 1,
            "name": "Event 1"
        },
        "success": true,
        "tickets": [
                {
                    "id": 1,
                    "status": "PURCHASED"
                },
                {
                    "id": 2,
                    "status": "PURCHASED"
                },
                {
                    "id": 3,
                    "status": "PURCHASED"
                },
                {
                    "id": 4,
                    "status": "PURCHASED"
                }
        ],
        "messages": null
    }

**Example**

    http://10.200.111.195:8080/admitone-backend/api/customers/purchase/

#### Cancellations

---

Only users with role ROLE_CUSTOMER can use this functionality. Cancels already purchased/exchanged tickets.

**Endpoint** -  api/customers/cancellation/

**Method** - POST

**Payload**

| Name        | Type           
| ------------- |:-------------:
| username              | String
| sourceEventId         | Long
| numberOfTickets       | Long  

    {
        "username":"bob@me.com",
        "sourceEventId":1,
        "numberOfTickets":2
    }
  
**Response**

    
    {
        "user": {
            "id": 1,
            "username": "bob@me.com"
        },
        "event": {
            "id": 1,
            "name": "Event 1"
        },
        "success": true,
        "tickets": [
            {
                "id": 1,
                "status": "CANCELLED"
            },
            {
                "id": 2,
                "status": "CANCELLED"
            }
        ],
        "messages": null
    }

**Example**

    http://10.200.111.195:8080/admitone-backend/api/customers/cancellation/

#### Exchanges

---

Only users with role ROLE_CUSTOMER can use this functionality. Exchanges already purchased tickets.

**Endpoint** -  api/customers/exchange/

**Method** - POST

**Payload**

| Name        | Type           
| ------------- |:-------------:
| username              | String
| sourceEventId         | Long
| targetEventId         | Long
| numberOfTickets       | Long

    {
        "username":"bob@me.com",
        "sourceEventId":1,
        "targetEventId":2,
        "numberOfTickets":2
    }
  
**Response**

    {
        "user": {
            "id": 1,
            "username": "bob@me.com"
        },
        "event": {
            "id": 2,
            "name": "Event 2"
        },
        "success": true,
        "tickets": [
            {
                "id": 3,
                "status": "EXCHANGED"
            },
            {
                "id": 4,
                "status": "EXCHANGED"
            }
        ],
        "messages": null
    }

**Example**

    http://10.200.111.195:8080/admitone-backend/api/customers/exchange/
    
    
## USERS

| Name                      | Password     |   Role      
| --------------------------|:-------------|:------------:
| bob@me.com                | admin        | ROLE_ADMIN
| suzie@them.com            | customer     | ROLE_CUSTOMER
| terrance12@gmail.com      | customer     | ROLE_CUSTOMER
| phil@gmail.com            | customer     | ROLE_CUSTOMER
| greg@foo.com              | customer     | ROLE_CUSTOMER
