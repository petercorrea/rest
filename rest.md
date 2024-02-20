# REST

REST, or Representational State Transfer, is an architectural style for designing networked applications. It relies on a stateless, client-server, cacheable communications protocol -- in virtually all cases, the HTTP protocol. RESTful applications use HTTP requests to post data (create and/or update), read data (e.g., make queries), and delete data, thus covering the CRUD operations.

## Components of RESTful Applications

**Resources**: The key abstraction of information in REST. Resources are represented by URIs. They represent the items we're looking for. Anything that can be named can be a resource: a document or image, a temporal service (e.g., "today's weather in Los Angeles"), a collection of other resources, a non-virtual object (e.g., a person), etc.

**Methods**: REST uses a set of predefined operations (HTTP methods like GET, POST, PUT, DELETE, etc.) to manipulate the resources. Each method implied a certain semantic with it. Methods allow us to do 'something' to the resource, like reading it, or updating its value.

**Representations**: Resources can be represented in various formats such as JSON, XML, HTML, etc. The client and server negotiate to select the most appropriate format for their communication.

**Stateless Interactions**: Each request from the client contains all the information needed by the server to fulfill the request. The server does not store any state about the client session on the server between requests.

To put this all together, a client makes a request for a certain resources using a specified method. Such as a `GET` request to `apple.com/v1/iphones`. The server would then respond with data typically in a `JSON` format and a `status code` indicating some feedback about the operation.

## Core Principles of REST

**Client-Server Model**: This model describes the sequence of interaction between two entities, namely between a client and a server. A client, such as a personal computer, makes a request to a server and the server, if able to fulfill the request, will provide a response to the client. This model embraces decoupling by having two separate entities, allowing each to change independently as needed.

**Statelessness**: Each request from the client must contain all the information needed to understand and complete the request. The server does not store any client context (state) between requests.

**Cacheability**: Clients and intermediaries can cache responses. Responses must, therefore, explicitly state themselves as cacheable or not to prevent clients from reusing stale or inappropriate data in response to further requests.

**Uniform Interface**: The interface between clients and servers is uniform, simplifying and decoupling the architecture. This includes:

- **Resource-Based**: Individual resources are identified in requests using URIs as resource identifiers.

- **Manipulation of Resources Through Representations**: When a client holds a representation of a resource, including any metadata attached, it has enough information to modify or delete the resource on the server, provided it has permission to do so.

- **Self-descriptive Messages**: Each message includes enough information to describe how to process the message.

**Layered System**: A client cannot ordinarily tell whether it is connected directly to the end server, or to an intermediary along the way.

## Advantages of REST

- Scalability due to the stateless nature of the architecture.
- Separation of concerns between client and server.
- Use of standard HTTP methods provides a pre-defined set of operations, enhancing understandability and reducing the learning curve.
- Enhanced performance through caching.
- Increased flexibility due to the use of standard URIs and the ability to return various media types.

## Best Practices for RESTful API Design

- Use nouns to represent resources (e.g., /users, /books).
- Use HTTP methods (GET, POST, PUT, DELETE) to represent actions on resources.
- Be consistent with the resource names and schema.
- Use sub-resources for relations (e.g., /users/123/posts for posts of user 123).
- Provide filtering, sorting, field selection, and paging for collections.
- Version your API (e.g., /api/v1/resource).
- Secure your API using HTTPS and other security mechanisms like OAuth.
- Provide meaningful HTTP status codes to represent the outcome of operations on resources.

## Understanding HTTP Protocols in Depth

`GET`: Requests a representation of the specified resource. GET requests should only retrieve data and have no other effect.

`POST`: Used to submit an entity to the specified resource, often causing a change in state or side effects on the server.

`PUT`: Replaces all current representations of the target resource with the request payload.

`DELETE`: Removes the specified resource.

`PATCH`: Applies partial modifications to a resource.

`HEAD`: Similar to GET, but it transfers the status line and header section only.

`OPTIONS`: Describes the communication options for the target resource.

`CONNECT`: Establishes a tunnel to the server identified by the target resource.

`TRACE`: Performs a message loop-back test along the path to the target resource.

## HTTP Status Codes

### 1xx (Informational Responses)

`100 Continue`: The server has received the request headers, and the client should proceed to send the request body.

### 2xx (Successful Responses)

`200 OK`: The request has succeeded. The meaning of success varies depending on the HTTP method used.

`201 Created`: The request has been fulfilled, resulting in the creation of a new resource.

`204 No Content`: The server successfully processed the request, but is not returning any content.

### 3xx (Redirection Messages)

`301 Moved Permanently`: This and all future requests should be directed to the given URI.

`302 Found`: The resource is temporarily located at another URI.

`304 Not Modified`: Indicates that the resource has not been modified since the last request.

### 4xx (Client Error Responses)

`400 Bad Request`: The server cannot or will not process the request due to something that is perceived to be a client error.

`401 Unauthorized`: Authentication is required and has failed or has not yet been provided.

`403 Forbidden`: The server understood the request but refuses to authorize it.

`404 Not Found`: The requested resource could not be found but may be available in the future.

`405 Method Not Allowed`: A request method is not supported for the requested resource.

### 5xx (Server Error Responses)

`500 Internal Server Error`: A generic error message when the server encounters an unexpected condition.

`501 Not Implemented`: The server either does not recognize the request method, or it lacks the ability to fulfill the request.

`503 Service Unavailable`: The server is currently unavailable (due to overload or maintenance). It is a temporary state.

## Performance Optimization

**Caching Strategies**: Effective caching reduces the number of requests that reach the server, improving response times and reducing load. Understanding Cache-Control directives, ETag for conditional requests, and Expires headers allows for fine-tuned caching policies.

**Rate Limiting**: Implementing rate limiting protects your API from overuse and abuse, ensuring availability for all users. Techniques vary from simple count-based limits to more sophisticated token bucket algorithms.

**Connection Keep-Alive**: Utilizing keep-alive connections reduces the overhead of TCP handshake processes for multiple requests, enhancing the performance of HTTP communication.

## Versioning Strategies

Versioning an API is a critical aspect of its design, ensuring that changes and enhancements to the API do not disrupt existing clients. There are several strategies for versioning, with URI versioning, header versioning, and parameter versioning being among the most common. Each strategy has its own benefits and trade-offs.

### URI Versioning

Involves including the version number directly in the API's URI path. This approach is straightforward and easy for developers to understand at a glance. It makes the version an explicit part of the API endpoint.

Example: <https://api.example.com/v1/resource>

**Pros:**

- Simplicity: It's easy to implement and use.
- Visibility: The version is immediately visible in the URL, making it clear which version of the API is being accessed.
- Cache-friendly: Different versions of the API can be cached separately by HTTP caches.

**Cons:**

- URL Pollution: Adding version numbers to the URI can lead to URL pollution, especially if there are many versions.
- Resource identification: It can imply that different versions are different resources, which might not always be conceptually accurate.

### Header Versioning

Header versioning involves sending the version information in the HTTP headers of the request. The server then reads this header to determine which version of the API to use for processing the request. This method keeps the API URL clean and separates versioning from the resource identification.

Example: A custom header such as API-Version: v1 or using the Accept header with a versioned media type, e.g., Accept: application/vnd.example.v1+json.

**Pros:**

- Clean URLs: Keeps the URLs unchanged even when the API version changes, avoiding URL pollution.
- Decoupling: Versioning is decoupled from the resource location.

**Cons:**

- Less visibility: The version is not visible in the URL, which can make it harder to debug and understand for developers.
- Complexity: Requires more effort to parse and handle on the server side.

### Parameter Versioning

Parameter versioning uses query parameters to specify the API version. The version information is provided as part of the query string in the URL.

Example: <https://api.example.com/resource?version=v1>

**Pros:**

- Easy to implement: Like URI versioning, it is straightforward to add version parameters to a request.
- Flexibility: It's easy to test different versions by changing the query parameter.

**Cons:**

- Cache issues: Caching can be more challenging because HTTP caches typically consider the full URL, including query parameters, which might lead to caching separate versions of the same resource.
- URL Pollution: Adds complexity to the URL, although less so than with URI versioning.

## REST in Microservices

Microservices architecture breaks down an application into a collection of loosely coupled services, each implementing a specific business functionality. This architectural style promotes scalability, agility, and the ability to develop and deploy services independently. When designing REST APIs for microservices, consider the following practices:

- Service Autonomy and Domain-Driven Design: Design each microservice around a specific business domain, ensuring it is autonomous and self-contained. This approach simplifies understanding the system and promotes cohesion within the service.
- API Modularity: Design APIs to be modular, mirroring the microservices architecture. Each microservice should have its own API, allowing teams to work independently on different services.
- Consistent Communication Protocols: Use REST as a uniform communication protocol across services. This consistency simplifies interaction between services and with external clients.
- Service Discovery: Implement dynamic service discovery mechanisms to allow services to find and communicate with each other. This is crucial due to the dynamic nature of microservice deployments, where instances may change due to scaling operations or deployments.
- Security and Isolation: Design each API with security in mind, implementing authentication, authorization, and encryption as needed. Using API gateways can help centralize security policies.
- Versioning: Use a clear versioning strategy for your APIs to manage changes without breaking existing clients or services. This is particularly important in a microservices architecture where different services may evolve at different rates.
- Documentation and Contracts: Maintain up-to-date API documentation and contracts (using tools like Swagger/OpenAPI). This documentation is vital for developers working on or interacting with the microservice.

### API Gateway

The API Gateway pattern provides a unified entry point for all client requests to a system's microservices. The gateway acts as an intermediary, routing requests to the appropriate microservice, aggregating results from multiple services, and implementing cross-cutting concerns such as authentication, logging, and error handling. Key considerations include:

- Routing: The API Gateway routes incoming requests to the appropriate microservices based on the request path, method, and possibly other attributes. This routing logic is central to the gateway's functionality.
- Aggregation: For operations that require data from multiple services, the gateway can aggregate responses from these services and return a unified response to the client, reducing the number of round trips needed.
- Authentication and Authorization: Implement security mechanisms at the gateway level, providing a single point of authentication and authorization. This simplifies security management across services.
- Rate Limiting and Quotas: Enforce rate limiting and quotas at the gateway to protect backend services from being overwhelmed by too many requests.
- Caching: Implement response caching at the gateway to improve response times and reduce the load on backend services.
- Resiliency and Fault Tolerance: Incorporate patterns like Circuit Breaker and Bulkhead at the gateway to manage service failures gracefully, ensuring the system remains responsive even when individual services are down.

## Security Considerations

Authentication and Authorization: Methods like OAuth 2.0, JWT (JSON Web Tokens), and basic authentication are essential for controlling access to resources.

Data Encryption: Using HTTPS for data in transit, understanding encryption for data at rest, and considering API gateway or service mesh implementations for secure microservices communication.

Cross-Origin Resource Sharing (CORS): Handling cross-domain requests securely.

## REST Alternatives & Complementary Technologies

**GraphQL**: A data query language developed by Facebook, offering clients the power to request exactly the data they need, potentially reducing the amount of data transferred over the network.

**gRPC**: Developed by Google, gRPC is a high-performance, open-source framework that leverages HTTP/2 and Protocol Buffers. It's particularly suited for tightly coupled systems and microservices communication.
