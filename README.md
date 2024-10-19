# Pattern Knowledge
## REST

### What is REST
REST (Representational State Transfer) is an architectural style for designing networked applications. It relies on a stateless, client-server communication protocol, typically HTTP. RESTful applications use HTTP requests to perform CRUD (Create, Read, Update, Delete) operations on resources.

<details>
  <summary>What is a Stateless REST API?</summary>
  <br/>

   Stateless REST APIs do not establish or maintain client sessions. Clients are responsible for providing all necessary information in each request, such as authentication tokens, credentials, or context data. The server does not store client-specific session data.
  
</details>

<details>
  <summary>Core Principles of REST</summary>
  <br/>

  + Client/Server - Client are separated from servers by a well-defined interface.
  + Stateless - each request from the client to the server must contain all of the information necessary to establish and complete a request.
  + Cacheability - Responses must define themselves as cacheable or non-cacheable (`Cache-Control: max-age=3600` - the header indicates that the response can be cached for 3600)
  + Uniform Interface - The uniform interface includes using standard HTTP verbs (GET, POST, PUT, DELETE, etc.), standard HTTP error responses, and resource identification through URI.
  
</details>

<details>
  <summary>Advantages & Disadvantages of REST</summary>
  <br/>

  **Advantages of REST:**

  + **Simple and Ease of Use:** REST APIs use standard HTTP methods (GET, POST, PUT, DELETE), making them easy to understand and implement.
  + **Performance:** RESTful APIs can be cached, which can improve performance and reduce load on the server.
  + **Flexibility:** RESTful APIs REST APIs can return data in multiple formats, such as JSON, XML, or plain text.
  + **Scalability:** REST APIs are stateless, meaning each request from a client to server must contain all the information the server needs to fulfill that request. This makes it easier to scale applications horizontally.
  + **Platform independence:** RESTful APIs are platform-independent, as they can be accessed from any device or programming language.

  _Exmaple:_
  + _Scalability:_ For example, a social media platform can handle millions of users by distributing requests across multiple servers.
  + _Flexibility:_ For example, a weather app might use JSON for its mobile app and XML for its web service.
  + _Caching:_ For instance, a news website can cache the results of popular articles, reducing the load on the server.

  **Disadvantages of REST:**

  + **Security Concerns:** REST APIs rely on HTTP, which can be less secure compared to other protocols like SOAP. 
  + **Lack of Standardization:** While REST is a set of guidelines, it doesn’t enforce strict standards. This can lead to inconsistencies in how APIs

  _Exmaple:_
  _Security Concerns:_ For example, if an API does not use HTTPS, sensitive data like user credentials can be intercepted.
  _Lack of Standardization:_ For instance, one API might use PUT for updates, while another uses PATCH.
</details>

### Rules to define REST API

<details>
  <summary>HTTP Methods</summary>
  <br/>

  + **GET:** Use `GET` requests to fetch data from the server without modifying it.
  + **POST:** Use `POST` requests to create new resources.
  + **PUT:** Use `PUT` requests to update an existing resource or create a resource if it does not exist.
  + **DELETE:** Use `DELETE` requests to delete a resource.
  + **PATCH:** Use `PATCH` requests to update a piece of resource.

  _Note:_ And `GET`, `PUT`, `DELETE` should be idempotent.
</details>

<details>
  <summary>URL</summary>
  <br/>

  + **Use Nouns, Not Verbs:** URLs should use resources (nouns) rather than actions (verbs). For example, use `/users`.
  + **Use Plural Nouns:** Use plural nouns to represent collections of resources. For example, `/users`.
  + **Hierarchical Structure:** Use a hierarchical structure to represent relationships between resources. For example, `/users/{userId}/orders` to represent orders for a specific user.
  + **Query Parameters for Filtering:** Use query parameters to filter, sort, or paginate resources. For example, `/users?sort=asc&limit=10`
  + **Versioning:** Include versioning in your URLs to manage changes over time. For example, `/v1/users`.

</details>

<details>
  <summary>Path Parameter, Query Parameter</summary>
  <br/>

  **Path Parameters:** are used to identify specific resources. 
  
  _Example:_ we have user id is `123` then we should use path parameter for URL.

  **Query Parameter:** are used to filter, sort, or paginate resources. They are appended to the end of the URL and follow a question mark (`?`).
  
  _Example:_ /users?sort=age&limit=10&page=2

</details>

<details>
  <summary>HTTP Status Code</summary>
  <br/>

  Send the right HTTP Status Code in response.

  + **2xx: Successful Responses**: The codes indicate that the request was successfully 
    + **200 OK**: The request was successful, and the server returned the requested resource.
    + **201 Created**: The request was successful, and a new resource was created as a result.
    + **204 No Content**: The request was successful, but there is no content to return.
  + **4xx: Client Error Responses**: The codes indicate that there was an error with the request made by the client.
    + **400 Bad Request**: The server could not understand the request due to invalid syntax.
    + **403 Forbidden:** The client does not have access rights to the content.
    + **404 Not Found:** The server cannot find the requested resource.

  To handle errors effectively, we need to implement exception handling that assigns the appropriate error
codes. For example:
  + 404 Not Found for EntityNotFoundException
  + 400 Bad Request for IllegalArgumentException

</details>

<details>
  <summary>Idempotency key</summary>
  <br/>

  An idempotency key works by ensuring that a particular operation is performed only once, even if the same request is sent multiple times. Idempotency keys are particularly useful with `POST` & `PATCH` HTTP methods.

  + **Client Generates Key:** The client generates a unique idempotency key for the request. This key is usually a **UUID** or another unique identifier.
  + **Client Sends Request with Key:** The client sends the request to the server, including the idempotency key in the request headers or body.
  + **Server Checks Key:** Upon receiving the request, the server checks if the idempotency key has already been used. This involves looking up the key in a database or cache where previously processed keys are stored.
  + **Process or Retrieve Result:** If the key is new, the server processes the request as usual and stores the result along with the idempotency key. If the key exists, the server retrieves the stored result associated with that key and returns it, without reprocessing the request.
  + **Return Response:** The server returns the response to the client. If the request was processed for the first time, it returns the new result. If the request was a duplicate, it returns the previously stored result.

</details>

### HTTP methods

<details>
  <summary>What is idempotency in REST?</summary>
  <br/>

  The idempotency means we can run the operation as many times as we want the outcome will be the same.

  The HTTP methods that are idempotent include `GET`, `PUT`, `DELETE`:
  
  + **GET:** Get a resource multiple times doesn't change its state.
  + **PUT:** Updating a resource with the same data multiple times, the result is still the same state.
  + **DELETE:** Deleting a resource multiple times has the same effect.

  `POST` and `PATCH` are generally not idempotent;
  + **POST:** Used to create new resources. Multiple POST requests can create multiple resources.
  + **PATCH:** Repeating the same PATCH request can lead to different outcomes depending on the initial state of the resource.

  **Importance of Idempotency:** The idempotency is crucial because that operations can be safely retried without causing any side effects.

</details>

<details>
  <summary>Difference between PUT and POST</summary>
  <br/>

The details differences are as follows:

|                      | PUT                                                                         | POST                                                                               |
| -------------------- | --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Request Body:        | The PUT body contains the full updated data for the resource.               | POST body only includes data for the new resource.                                 |
| URI Meaning:         | PUT uses the URI to directly identify the resource to update (e.g. user 1). | POST uses the URI to specify the collection where a new resource will be created.  |
| Idempotency:         | PUT is idempotent - the same request gives the same result.                 | POST can produce different results each time.                                      |
| Existing Resources:  | PUT replaces the entire resource with the request body.                     | POST partially updates the resource. (should use PATH)                             |
| New Resources:       | Both PUT and POST can create new resources.                                 | Both PUT and POST can create new resources.                                        |

_Example:_
```
// PUT example  
PUT /users/1
{
  "id": 1,
  "name": "Ichiro",
  "age": 22
}
// This sends a request to replace user 1's record.
```
PUT is limited to creating or updating operations and exclusively acts upon the resource identified by the provided URL.
```
// POST example
POST /users  
{
  "name": "Saburo",
  "age": 18
}
// This sends a request to create a new user.
```
POST is more flexible, and capable of executing various types of processing tasks.

</details>

<details>
  <summary>HTTP method exmaples</summary>
  <br/>

```
GET 	/device-management/devices       : Get all devices
POST 	/device-management/devices       : Create a new device

GET 	/device-management/devices/{id}   : Get the device information identified by "id"
PUT 	/device-management/devices/{id}   : Update the device information identified by "id"
DELETE	/device-management/devices/{id}   : Delete device by "id"
```

```
@RestController
@RequestMapping("/gold/v1")
public class GoldController {

  @GetMapping
  public Page<GoldRequest> getAllGold(
@PageableDefault(page = 0, size = 10, sort = "name", direction = Sort.Direction.DESC) Pageable pageable) {
    return goldService.getAllGold(pageable);
  }

  @GetMapping("/{id}")
  public ResponseEntity<GoldResponse> getGoldById(@PathVariable Long id) {
    GoldResponse goldResponse = goldService.getGoldById(id);
    if (goldResponse != null) {
      return ResponseEntity.ok(goldResponse);
    } else {
      return ResponseEntity.notFound().build();
    }
  }

  @PostMapping
  public ResponseEntity<GoldResponse> createGold(@RequestBody GoldRequest goldRequest) {
    GoldResponse goldResponse = goldService.createGold(goldRequest);
    URI location = ServletUriComponentsBuilder.fromCurrentRequest().path("/{id}")
        .buildAndExpand(goldResponse.getId()).toUri();
    return ResponseEntity.created(location).body(goldResponse);
  }

  @PutMapping("/{id}")
  public ResponseEntity<GoldResponse> updateGold(@PathVariable Long id, @RequestBody GoldRequest goldRequest) {
    GoldResponse goldResponse = goldService.updateGold(id, goldRequest);
    if (goldResponse != null) {
      return ResponseEntity.ok(goldResponse);
    } else {
      return ResponseEntity.notFound().build();
    }
  }

  @DeleteMapping("/{id}")
  public ResponseEntity<Void> deleteGold(@PathVariable Long id) {
    goldService.deleteGold(id);
    return ResponseEntity.ok().build();
  }
}
```

</details>

### Additional Methods
<details>
  <summary>Additional HTTP Method</summary>
  <br/>

| HTTP Method           | Description          |
| --------------------- | -------------------- |
| PATCH                 | Updates a part of an existing resource. Not idempotent.                 |
| HEAD.                 | Similar to GET, but only returns the header information, not the body.  |
| OPTIONS               | Used to determine the supported methods and options for a resource.     |

</details>

### Additional tips when creating APIs

<details>
  <summary>Read file with Resource</summary>
  <br/>

  The `Resource` class is a high-level abstraction provided by Spring. Using a Resource over an InputStream has the following benefits:
  + `Resource` provides additional metadata about the resource.
  + It’s possible to use a `Resource` in conjunction with other Spring abstractions.
  + It’s easier to mock.

  _Example:_
  ```
    @GetMapping("/{id}")
    public ResponseEntity<InputStreamResource> getFile(@PathVariable Long id) {
        InputStreamResource fileStream = fileService.getFileAsStream(id);

        if (fileStream != null) {
            Optional<FileEntity> fileEntityOptional = fileService.getFile(id);
            FileEntity fileEntity = fileEntityOptional.get();
            return ResponseEntity.ok()
                    .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + fileEntity.getFileName() + "\"")
                    .body(fileStream);
        } else {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
    }
  ```
</details>

## Microservice Architecture
### Fundamental concept
<details>
  <summary>Microservice architecture</summary>
  <br/>

  Microservice architecture is an architectural style that structures an application as a collection of services. Each service is created independently, and each one runs a unique process and usually manages its own database. 

  **Core principle:** Breaking down a large application into smaller, independent services.

  _Example:_
Imagine an e-commerce application. In a monolithic architecture, all functionalities like user management, product catalog, order processing, and payment would be part of a single codebase. In a microservice architecture, these functionalities would be split into separate services: _User Service_, _Product Service_, _Order Service_, _Payment Service_
+ User Service: Manages user accounts and authentication.
+ Product Service: Handles product catalog and inventory.
+ Order Service: Manages order processing and tracking.
+ Payment Service: Handles payment processing.

Each service can be developed, deployed, and scaled independently. 

</details>

<details>
  <summary>Pros and Cons</summary>
  <br/>

  _Pros:_
  + **Scalability:** Microservices allow for independent scaling of individual services based on demand.
  + **Resilience:** The isolation of services reduces the impact of failures, as a failure in one service.
  + **Flexibility:** Microservices enable the use of different technologies and frameworks for each service.
  + **Maintainability:** Smaller, focused services are generally easier to understand, develop, and maintain.
  + **Continuous delivery:** Microservices can be deployed and updated independently.
  
  _Cons:_
  + **Complexity:** Managing multiple services can be more complex.
  + **Distributed systems challenges:** Communication between services can introduce latency and complexity.
  + **Testing and debugging:** Testing and debugging distributed systems can be challenging.
  + **Data consistency:** Ensuring data consistency across multiple services can be difficult.
  
</details>

<details>
  <summary>When to use REST and when to use Event-Driven in Microservice Architecture</summary>
  <br/>

  **When to Use REST:**
  + **Simple CRUD Operations:** If your microservices need to perform basic Create, Read, Update, and Delete operations.
  + **Synchronous Communication:** When you need immediate responses from your services.
  + **Ease of Implementation:** REST is easier to implement and understand, making it a good choice for simpler applications

  **When to Use REST:**
  + **Asynchronous Processing:** If your services need to process tasks asynchronously, allowing them to continue working without waiting for a response.
  + **High Scalability:** When you need to handle a large volume of events and scale your services independently.
  + **Decoupling Services:** If you want to decouple your services to reduce dependencies and improve fault tolerance.
  + **Complex Workflows:** When your application requires complex workflows that involve multiple services

</details>

<details>
  <summary>Type of Communication Patterns</summary>
  <br/>

  **Synchronous Communication:** In synchronous communication, the client sends a request and waits for a response before continuing its process.

  Pros: 
  + Easier to implement and understand.
  Cons:
  + Services are more dependent on each other.
  + The client has to wait for the response, which can slow down the system.
  
  _Example:_ REST
    
  **Asynchronous Communication:** In asynchronous communication, the client sends a request and continues its process without waiting for a response. The response is handled separately.

  Pros:
  + Services are less dependent on each other.
  + Better suited for handling high loads and distributed systems.  

  Cons:
  + More challenging to implement and debug.
  + The system may not be immediately consistent, which can be a drawback for certain applications.

  + _Example:_ Event-Driven Architecture
   
  ![communication_patterns](/images/communication_patterns.png)

</details>

<details>
  <summary>Reliable communication between microservices</summary>
  <br/>

  Reliable communication between microservices ensures that messages are delivered accurately and timely, even in the face of failures or network issues. To archive **Reliable communication** we can use _messaging system_ like RabbitMQ, Apache Kafka. They aslo support **Idempotency**.
   
  
</details>

### Command Query Responsibility Segregation (CQRS) Pattern

<details>
  <summary>What is CQRS</summary>
  <br/>

  CQRS stands for Command Query Responsibility Segregation. It’s a design pattern separates the responsibility for handling write operations (_command_) and read operations (_query_). CQRS separates reads and writes into _different databases_, **Commands** performs update data, **Queries** performs read data.

  ![](/images/cqrs.png)
  
</details>

<details>
  <summary>How to Sync Databases with CQRS ?</summary>
  <br/>

  When we separate read and write databases in 2 different database, the main consideration is sync these two database in a proper way. So we should sync these 2 databases and keep sync always. There are several ways: 

  _Event-Driven Architecture_
  **Concept:** Use a messaging system like Apache Kafka or RabbitMQ to propagate changes.
  **Synchronization:** When a write operation occurs, an event is published to a message broker. The read database subscribes to these events and updates itself accordingly. Tools like Debezium can monitor the write database for changes and apply these changes to the read database in real-time.

  _Event Sourcing_
  + **Core Principle:** Instead of directly updating the database, CQRS systems using event sourcing store a sequence of events that represent changes to the system's state.
  + **Synchronization:**
    + The system maintains an event stream that records all events that occur.
    + When an event occurs, it is broadcast to event handlers that update the appropriate read models (views) in the read database.

</details>

### Event-Driven Architecture

<details>
  <summary>What is Event-Driven Architecture</summary>
  <br/>

  Event-Driven Architecture is a software architectural pattern where components or services communicate primarily by producing and consuming events.
  
</details>
<details>
  <summary>Key concepts of EDA</summary>
  <br/>

  + **Event:** A representation of a specific occurrence or action.
  + **Producer:** A component that generates events.
  + **Consumer:** A component that processes events.
  + **Message Broker:** A middleware system that manage communication between producers and consumers by handling event routing, storage, and delivery.
  
</details>

### Event Sourcing Pattern
<details>
  <summary>What is Event Sourcing pattern</summary>
  <br/>

</details>

### Saga Pattern
<details>
  <summary>What is Saga Pattern</summary>
  <br/>

  The Saga pattern is a design pattern used to manage data consistency across microservices in distributed transaction scenarios. The Saga architecture pattern provides transaction management using a sequence of local transactions.

  **Types of Saga Pattern**

  There are two types of Saga Pattern:

  + Saga Choreography Pattern
  + Saga Orchestration Pattern
  
</details>
<details>
  <summary>Saga terminology</summary>
  <br/>

  + **Sequence of Local Transactions:** A saga is incluled of multiple local transactions. Each transaction updates the database and triggers the next transaction through a message or event.
  + **Compensating Transactions:** If a transaction fails, the saga executes compensating transactions to undo the changes made by previous transactions, ensuring data consistency.
  + **Choreography vs. Orchestration:** There are two main approaches to implementing the Saga pattern.
  
</details>
<details>
  <summary>Saga Choreography Pattern</summary>
  <br/>

  The Choreography Saga pattern is a way to manage distributed transactions across multiple microservices **without** a central coordinator.

  ![communication_patterns](/images/saga-choreography.png)

  + The order service runs a local transaction, T1, which atomically updates the database and publishes an Order placed message to the message broker.
  + The inventory service subscribes to the order service messages and receives the message that an order has been created.
  + The inventory service runs a local transaction, T2, which atomically updates the database and publishes an Inventory updated message to the message broker.
  + The payment service subscribes to the messages from the inventory service and receives the message that the inventory has been updated.
  + The payment service runs a local transaction, T3, which atomically updates the database with payment details and publishes a Payment processed message to the message broker.

  _Compensating transaction:_ 
  + If the payment fails, the payment service runs a compensatory transaction, C1, which atomically reverts the payment in the database and publishes a Payment failed message to the message broker.
  + The compensatory transactions C2 and C3 are run to restore data consistency.

</details>
<details>
  <summary>Saga Orchestration Pattern</summary>
  <br/>

  The Saga orchestration pattern is a design pattern used to manage data consistency across microservices in distributed transaction scenarios. A single orchestrator is responsible for managing the overall transaction status.

  + **Orchestrator:** A central coordinator that manages the sequence of transactions.
  + **Local Transactions:** Each service performs its own transaction and then triggers the next step.
  + **Compensating Transactions:** If a step fails, the orchestrator triggers compensating transactions to undo the changes made by previous steps.

  ![communication_patterns](/images/saga-orchestration.png)
  
</details>
<details>
  <summary>Two-Phase Commit vs Saga Pattern</summary>
  <br/>

  2PC is suitable for scenarios requiring strict consistency, while Saga is better for systems where availability and resilience are more critical.

  **Two-Phase Commit:**

  + Strong Consistency: Ensures all participating nodes either commit or roll back a transaction, maintaining a consistent state across the system.
  + Synchronous: All participants must be available and agree to commit or roll back, which can lead to blocking if any participant is slow or fails

  **Saga Pattern:**

  **Eventual Consistency:** Ensures that the system will eventually reach a consistent state, but not necessarily immediately.
  **Asynchronous:** Transactions are processed asynchronously, which can improve system availability and reduce blocking.
</details>
<details>
  <summary>Eventual Consistency vs Strong Consistency</summary>
  <br/>

</details>

### Outbox pattern
<details>
  <summary>What is Outbox pattern</summary>
  <br/>
  The Outbox Pattern is a design pattern commonly used in distributed systems to ensure reliable event delivery and to maintain data consistency between the application database and external message brokers (e.g., Kafka, RabbitMQ) or other services.

  **How it works:**

  The Outbox Pattern introduces an outbox table into the database. When an application makes a state change (like creating a new order), it stores the event in this outbox table instead of immediately sending the event to a message broker. This outbox table guarantees that the event is captured in the same transaction with the state change operation.

  + When the application performs a business operation (e.g., creating an order), it writes both the application data (e.g., order record) and the event to the outbox table within a single database transaction.
  + Since both the business operation and the outbox write happen in the same transaction, both are rolled back if there's a failure.
  + There is a separate background process responsible for polling or scanning the outbox table to read new events.
  + It reads these events and publishes them to an external system, such as a Kafka topic. Then background process marks the event as “processed” to avoid reprocessing.
  + In case, external system (e.g., Kafka) is temporarily unavailable, the event remains in the outbox table until it is successfully delivered.

</details>

<details>
  <summary>When use the Outbox Pattern</summary>
  <br/>
  
  **When to Use the Outbox Pattern**

 + You need to maintain consistency between your database state and messages published to a message broker.
 + Your system requires high reliability and cannot lose events or process duplicates.

</details>

<details>
  <summary>Duplication issues in consumer</summary>
  <br/>

  + Order Service inserts the order into the database.
  + The order service stores an event related to the order in an "outbox" table.
  + A Message Publisher reads the event from the outbox table and sends it to a message broker (like Kafka).
  + The message is consumed by another microservice to process order.
  + **If** Message Publisher sends the event to Kafka but crashes or encounters a network issue after sending, and before it can mark that event as "sent" in the outbox table.
  + When restart, the publisher reads the event from the outbox table again (because it still shows as unsent) and republishes the event.
  + So the consumer can consume the event twice and processes the order twice.

  **Solution:**

  _Store Processed Events/Transaction IDs:_
  
  + When processing events, store a unique identifier (like a UUID) for each event.
  + Before processing a new event, check if it has already been processed.
  + If it has, you ignore the message.

</details>

<details>
  <summary>Example Use Case</summary>
  <br/>

  In an e-commerce platform, you have a microservice-based architecture where orders are placed by customers and we need to ensure that these orders are processed correctly.

  + The order data must be saved in the database, and the corresponding event (e.g., "Order Created") must be published to Kafka exactly once, without losing or duplicating messages.
  + In case of system crashes or failures, the system must ensure no order data is lost, or order event is missed or sent multiple times.

</details>

### Change Data Capture (CDC)

<details>
  <summary>About Debezium</summary>
  <br/>
</details>

## Serverless

## Resilience

<details>
  <summary>What is Resilience</summary>
  <br/>

  Resilience in microservices refers to the ability of an application to withstand failures and continue to operate smoothly.
</details>

### Circuit Breaker pattern
<details>
  <summary>What is Circuit Breakers</summary>
  <br/>

  A circuit breaker is a design pattern used in microservices architecture to prevent cascading failures and provide fallback mechanisms.

  For example: We have a payment system that processes millions of payments every day, and every time a payment fails it sends an email and tries again. If it weren't use the circuit breaker. It would overload the mail server. Bring down the whole system.

  Circuit breakers can provide the best of _both_ sides. The circuit breaker is usually configured on the **client side** (_caller side_).

</details>

<details>
  <summary>How does it work?</summary>
  <br/>

  + **Closed State:** Initially, the circuit breaker is in a closed state. Requests are allowed to pass through to the service.
  + **Open State:** If the service fails a certain number of times within a specified time window, the circuit breaker will transit to an open state. This means that next requests are immediately rejected without trying to contact the service.
  + **Half-Open State:** After a predefined timeout, the circuit breaker moves to a half-open state. A single request is allowed to pass through. If the request is successful, the circuit breaker returns to the closed state. If the request fails, the circuit breaker remains open for another timeout period.

</details>

### Retry pattern
<details>
  <summary>What is Retry pattern?</summary>
  <br/>

  Retry pattern is a strategy in software development that involves reattempting a failed operation after a certain delay. This pattern is often used to handle transient errors or temporary network issues.

  For example, if a network connection is temporarily disrupted, a retry pattern can be used to automatically reattempt the operation after a short delay, increasing the chances of success.

</details>
<details>
  <summary>How does it work?</summary>
  <br/>

  **Initial Request:** When a microservice makes a request to another service, it may encounter an error due to issues like network interruptions.
  **Retry Logic:** If the initial request fails, the Retry Pattern triggers a retry mechanism. This involves automatically resending the request a specified number of times.
  **Backoff Strategy:** To prevent overwhelming the system, the retry pattern uses _backoff strategy_. Backoff strategy manage the timing of retry attempts after a failure.

  _Exmaple:_ 

  + The microservice attempts to fetch data from the external API.
  + The API request fails due to a temporary network issue.
  + _First Retry:_ The microservice waits for a short delay (e.g., 1 second) and retries the fetch.
  + _Second Retry:_ If the second attempt fails, the microservice waits for a longer delay (e.g., 4 seconds) before retrying again.
  + _Subsequent Retries:_ The delay between retries continues to increase (e.g., 16 seconds, 64 seconds, etc.) until a maximum number of retries is reached. (_Exponential Backoff_)

</details>

### Rate limiter

<details>
  <summary>What is Rate limiter?</summary>
  <br/>

  A rate limiter is a mechanism that controls the rate at which requests are processed. A rate limiter is a mechanism that controls the rate at which requests are processed. It's designed to prevent excessive load on a system.
  
</details>
<details>
  <summary>How does it work?</summary>
  <br/>

  A rate limiter sets a limit on the number of requests that can be processed within a specific time window. When the rate limit is reached, subsequent requests are rejected

  _Example:_ A popular e-commerce website receives a huge of traffic during a sale. The API gateway enforces a rate limit on the number of requests per user or per IP address. This prevents a small number of users from overwhelming the backend services.
  
</details>

## API Gateway Pattern

<details>
  <summary>What is API Gateway</summary>
  <br/>

  
  
</details>

### Kong API Gateway

<details>
  <summary>About Kong API Gateway</summary>
  <br/>

  Kong is an open-source API gateway that is **built on top of Nginx**. Its primary purpose is to  manage, secure, and extend APIs and microservices.

  Features of Kong API Gateway:
  + **High Performance:** Leveraging Nginx and OpenResty, Kong can handle high traffic with low latency.
  + **Security:** Provides security features such as rate limiting, IP restriction, OAuth2 authentication, and more.
  + **API Management:** Tools for managing the entire API lifecycle, including versioning, monitoring, and documentation.
  + **Extensibility:** Supports numerous plugins for authentication, security, traffic control, and analytics.
  
</details>

## Common experience design
<details>
  <summary>Design upload API</summary>
  <br/>

  **Step 1: Upload File**

  + **Upload to a Temporary Location:** When the user uploads a file, store it in a temporary location in S3 (_file storage_). You can use a specific folder or prefix to differentiate these temporary files.
  + **Generate a Unique Identifier:** Assign a unique identifier to each upload request. This will help in tracking and managing the files.

  **Step 2: Confirm/Cancel the Upload Request**

  + **Confirm Upload:** If the user confirms the upload, move the file from the temporary location to the final destination in S3 (_file storage_). This can be done using the `copyObject` method in S3 and then deleting the original file from the temporary location.
  + **Cancel Upload:** If the user cancels the upload, simply delete the file from the temporary location in S3 using the `deleteObject` method.

</details>
<details>
  <summary>How to upload a large file in API Gateway?</summary>
  <br/>

  The AWS API Gateway has a payload size limit of 10 MB. If the payload exceeds 10MB, API Gateway may reject the request and return an error (_“Error 413 Request entity too large”_). However, there are effective strategies to handle this:
  
  **Lambda Function with Presigned URLs**

  ![](/images/presigned-url-s3-gateway-limit-upload-file.png)

  We can use lambda function to generate the **presigned URLs** to upload file directly to S3.

  Presigned URLs allow you to grant temporary access to objects in your Amazon S3 bucket without exposing your AWS credentials. These URLs are time-limited and can be used to upload or download files securely.

  1. The client requests a presigned URL from your API.
  2. The request is routed to the API Gateway, which triggers the Lambda function.
  3. The Lambda function generates a presigned URL using the AWS SDK.
  4. The presigned URL is returned to the client.
  5. The client uses the presigned URL to upload or download the file.

  _Note:_ We also use **S3 accelerator** + **multipart file upload** to optimize uploading performance.

  **Upload zipped file as the payload**
  
</details>

<details>
  <summary>Optimize a slow API</summary>
  <br/>

  There are several strategies to optimze slow API;

  If you’re using **AWS API Gateway**, there are several specific optimizations you can implement to improve the performance of your API:
  + **Enable Caching:** Use API Gateway’s built-in caching to store responses for a specified TTL (Time to Live).
  + **Enable Compression:** Enable payload compression in API Gateway to reduce the size of the data transferred. Help Faster data transfer and reduced bandwidth usage.

  And in the application
  + **Caching:** Implement caching mechanisms to store frequently requested data. This reduces the need to repeatedly fetch the same data from the server.
  + **Asynchronous Processing:** Use `CompletableFuture` to handle multiple tasks.
  + **Database Optimization:** Use indexing, avoid unnecessary joins, avoid N + 1 query problems.
  
</details>
