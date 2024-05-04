# API Gateway Microservice

The API Gateway Microservice is a component responsible for routing and forwarding client requests to the appropriate downstream microservices. It acts as a single entry point for clients to access various services in the system.

## Prerequisites
Before running the API Gateway Microservice, ensure the following prerequisites are met:

* Java Development Kit (JDK) installed on your machine.
* Apache Maven for building and managing dependencies.
* Netflix Eureka server running for service registration and discovery.

## Installation
1. Clone the repository:
```bash
git clone https://github.com/ChijiokeOkorji/api-gateway.git
```
2. Navigate to the project directory:
```bash
cd api-gateway
```
3. Build the project using Maven:
```bash
mvn clean install
```
4. Set the required environment variables (e.g., PORT, EUREKA_SERVICE_URL) or provide them in the `application.yaml` file.
5. Run the application:
```bash
java -jar target/api-gateway-0.0.1-SNAPSHOT.jar
```

## Configuration

### Route Configuration
The API Gateway is configured using Spring Cloud Gateway, where routes are defined to forward requests based on certain criteria.

#### Example Route Configuration
```java
package com.example.apigateway.config;

import org.springframework.cloud.gateway.route.RouteLocator;
import org.springframework.cloud.gateway.route.builder.RouteLocatorBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ApiGatewayConfig {
    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
                .route("tasks-service", r -> r.path("/api/v1/tasks/**")
                        .uri("lb://tasks-service"))
                .route("products-service", r -> r.path("/api/v1/products/**")
                        .uri("lb://products-service"))
                .build();
    }
}
```
In the above example, routes are defined for the [tasks-service](https://github.com/ChijiokeOkorji/tasks-service) and [products-service](https://github.com/ChijiokeOkorji/products-service), forwarding requests with specific paths to their respective microservices. Note that all microservices (including the API gateway) need to be registered in the [service registry](https://github.com/ChijiokeOkorji/service-registry).

### Application Configuration
The API Gateway Microservice is configured using the `application.yaml` file, where properties such as the application name and server port are defined.

Example application.yaml
```yaml
spring:
  application:
    name: api-gateway
server:
  port: ${PORT:8765}
eureka:
  client:
    service-url:
      defaultZone: ${EUREKA_SERVICE_URL}
```

## Documentation
For more information about how to use and configure the API Gateway Microservice, refer to the following resources:

* [Official Apache Maven documentation](https://maven.apache.org/guides/index.html)
* [Spring Boot Maven Plugin Reference Guide](https://docs.spring.io/spring-boot/docs/3.2.5/maven-plugin/reference/html/)
* [Create an OCI image](https://docs.spring.io/spring-boot/docs/3.2.5/maven-plugin/reference/html/#build-image)
* [Gateway](https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/)
* [Eureka Discovery Client](https://docs.spring.io/spring-cloud-netflix/docs/current/reference/html/#service-discovery-eureka-clients)

## Additional Resources
Explore an additional guide that illustrate how to use some features concretely:

* [Service Registration and Discovery with Eureka and Spring Cloud](https://spring.io/guides/gs/service-registration-and-discovery/)