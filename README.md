### Name: Mirfak Naufal Pratama Putra
### Class: Advanced Programming B
### Student Number: 2306244961

<details>
<summary>Reflection</summary>

### 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
Unary RPC involves a single request from the client and a single response from the server, suitable for straightforward tasks like payment processing. Server streaming RPC allows the client to send one request and receive a stream of responses, ideal for scenarios like retrieving transaction histories. Bi-directional streaming enables both client and server to exchange streams of messages simultaneously, making it perfect for real-time applications such as chat systems or live data feeds. Each method serves different communication patterns based on the interaction complexity and data flow needs.
### 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
When building a gRPC service in Rust, key security considerations include authentication (verifying client identity using TLS certificates or tokens like JWT), authorization (enforcing access control via role checks or interceptors), and data encryption (securing communication with TLS). The tonic library supports TLS via rustls or openssl, and interceptors can be used to validate requests and enforce permissions. Additional precautions include input validation, rate limiting, secure logging, and safe certificate/key management to prevent unauthorized access or data leaks.
### 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
Handling bidirectional streaming in Rust gRPC, especially for chat apps, can be challenging due to the need for concurrent message sending and receiving, which must align with Rust's strict ownership model. Developers must manage flow control to avoid overwhelming the stream, detect disconnections gracefully, and maintain shared state (e.g., user sessions or chat rooms) using thread-safe structures like Arc<Mutex<>>. Additionally, robust error handling and scalability are crucial, as managing many simultaneous streams can strain system resources if not efficiently designed.
### 4. What are the advantages and disadvantages of using `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?
Advantages:
- Easy Integration: It wraps a `tokio::mpsc::Receiver` into a stream, making it simple to send async data from within your service logic.
- Concurrency-Friendly: Decouples message production from the gRPC response stream, allowing background tasks or events to push messages independently.
- Flexible Control: You can buffer, filter, or manipulate the data before sending, offering fine-grained control over the stream contents.

Disadvantages:
- Manual Backpressure Handling: `ReceiverStream` doesn’t inherently handle backpressure well, so if the consumer is slow, messages may accumulate in the channel.
- Memory Overhead: Unbounded or poorly managed channels can lead to excessive memory usage if the sender outpaces the receiver.
- Extra Complexity: Introducing channels adds indirection and may complicate error handling, especially in large or dynamic systems.

### 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
To promote modularity, maintainability, and reusability in your Rust gRPC code, structure it by separating logic into distinct modules for each service (e.g., payment, transaction, chat), with shared protobuf code stored in a central module. Abstract service logic into reusable structs, encapsulating core functionality independently from gRPC-specific code. Centralize error handling, logging, and common asynchronous utilities for tasks like streaming and message handling. Reuse client connection logic by creating functions or structs for connection management, and manage configuration with environment variables or config files. This approach ensures clear separation of concerns, easy testing, and the ability to extend or modify each service independently without tight coupling.
### 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
To handle more complex payment processing, we can consider adding transaction management to ensure atomicity, integrating third-party payment gateways for real-time processing, and implementing robust error handling.
### 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
Adopting gRPC in distributed systems enhances performance with efficient, low-latency communication and strong cross-platform compatibility due to its use of Protocol Buffers. It supports multiple languages, enabling easy integration with diverse technologies. gRPC's bidirectional streaming and asynchronous capabilities are ideal for real-time applications, but it can introduce complexity in environments typically using REST, requiring support for secure connections, load balancing and service discovery.
### 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
Advantages:
- Multiplexing: HTTP/2 allows multiple requests and responses to be sent simultaneously over a single connection, reducing latency and improving throughput.
- Lower Latency: HTTP/2 reduces latency with features like header compression and prioritization, allowing more efficient use of the network.
- Binary Protocol: HTTP/2 is a binary protocol, which is more efficient than the text-based HTTP/1.1, reducing parsing overhead and providing better performance.
- Streamed Responses: HTTP/2 natively supports bidirectional streaming, a feature that gRPC leverages for real-time applications.
- Better Resource Utilization: HTTP/2's multiplexing reduces the need for multiple connections, which can improve server resource utilization and scalability, particularly for microservices and high-volume APIs.

Disadvantages:
- Compatibility and Adoption: HTTP/2 requires both client and server to support the protocol.
- Complexity: HTTP/2 introduces additional complexity in connection management, such as managing streams, flow control, and prioritization.
- WebSocket Overhead: For real-time applications, HTTP/2's bidirectional streaming is efficient, but in cases where WebSocket is already well-supported, WebSocket may be a more straightforward solution, particularly for long-lived connections in chat applications or event-driven systems.
- 
### 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
The REST API request-response model is unidirectional and synchronous, where the client waits for a server response after each request, making it less suited for real-time communication without workaround like long polling or WebSockets. In contrast, gRPC's bidirectional streaming allows continuous, asynchronous message exchange between client and server over a single connection, making it ideal for real-time, low-latency communication.
### 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
gRPC with Protocol Buffers (Protobuf) uses a schema-based approach, ensuring data consistency, type safety, and better performance with smaller payloads and faster serialization. However, it requires predefined schemas, which can complicate versioning and synchronization. In contrast, JSON in REST APIs is schema-less, offering flexibility and easier evolution of data structures without breaking changes, but it comes at the cost of slower performance, larger payloads, and potential inconsistencies. While gRPC is more efficient and structured, REST with JSON is more adaptable and easier to integrate across systems.
</details>