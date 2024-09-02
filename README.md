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

### Read file or image with REST

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

## Microservice
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


### Database in Microservice architecture

<details>
  <summary>Key Principles for designing databases in Microservice architecture</summary>
  <br/>

  **Database per Service:** Each microservice has its own dedicated database

</details>

<details>
  <summary>Patterns for designing databases in Microservice architecture</summary>
  <br/>


</details>

### Microservice Communication Patterns
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

## Event Sourcing
### Fundamental concept
<details>
  <summary>Event Sourcing pattern</summary>
  <br/>

</details>
