# Pattern Knowledge
## REST

### What is REST
REST (representational state transfer) is a software architectural style that was created to guide the design and development 

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
  + Cacheability - 
  + Layered System -
  + Uniform Interface - The uniform interface includes using standard HTTP verbs (GET, POST, PUT, DELETE, etc.), standard HTTP error responses, and resource identification through URI.
  + Code on Demand (optional) - 
  
</details>

### Difference between PUT and POST

<details>
  <summary>Difference between boths</summary>
  <br/>

The details differences are as follows:

|| PUT           | POST          |
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

  Microservice architecture is an architectural style that structures an application as a collection of small, independent services. Each service is created independently, and each one runs a unique process and usually manages its own database. 

  **Core principle:** Breaking down a large application into smaller, independent services.

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
  <summary>Type of Communication Patterns</summary>
  <br/>

  + Asyn non-blocking
    + Share data
    + Event driven
    + Request response
  + Sync blocking
    + Request response
   
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

  The AWS API Gateway has a payload size limit of 10 MB. If the payload exceeds 10MB, API Gateway may reject the request and return an error (“Error 413 Request entity too large”). However, there are effective strategies to handle this:

  **Upload zipped file as the payload**

  **Using S3 Presigned URLs**
  
  **Direct Integration with S3**

</details>
