I'll updating this repo with almost all the information, this is my twitter, please follow me to updates questions or suggestions.
[@acardenasnet](https://twitter.com/acardenasnet)

This api-gateway just register into consul and route the requests `/orders` to the order-service,

```properties
spring.application.name=api-gateway
spring.cloud.gateway.discovery.locator.enabled=true
```

In the following snippet, we can see that all that comes with `/orders/**` will access to this route, into the route we filter and strip the prefix, that means remove the `/orders`, to send the other information to the order-service.
You can realize that to call the service we don't use `http` protocol instead we are using `lb` that means LoadBalancer, by default spring provide the use of Ribbon.

```java
  @Bean
  public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
    return builder.routes()
        .route("ordersRoute", r -> r.path("/orders/**")
            .filters(f -> f.stripPrefix(1))
            .uri("lb://order-service"))
        .build();
  }
```