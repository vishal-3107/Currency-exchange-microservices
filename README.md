# Currency-exchange-microservices

## Overview
### This project is a Currency Exchange Microservices application built using Spring Boot and Microservices architecture. It allows users to retrieve exchange rates for different currency pairs and provides resilience mechanisms such as Circuit Breaker, Rate Limiter, Retry, and Bulkhead using Resilience4j.

## Features

    -> Retrieves currency exchange rates from a database
    -> Implements Circuit Breaker, Rate Limiter, and Retry mechanisms
    -> Provides REST APIs for fetching exchange rates

## Project Structure

    Currency Exchange Service
    ├── src/main/java/com/learning/microservices/currency_exchange_service
    │   ├── CurrencyExchangeServiceApplication.java  # Main entry point
    │   ├── controller
    │   │   ├── CurrencyExchangeController.java      # Handles API requests
    │   │   ├── CircuitBreakerController.java        # Implements Circuit Breaker & Retry
    │   ├── entity
    │   │   ├── CurrencyExchange.java                # Entity class for exchange rates
    │   ├── repository
    │   │   ├── CurrencyExchangeRepository.java      # JPA repository for DB access
    │   ├── proxy
    │   │   ├── CurrencyExchangeProxy.java           # Feign client for inter-service calls
    └─── pom.xml                                      # Dependencies

## Key Components

### 1. Resilience4j (Fault Tolerance)

    Q-> What is Resilience4j?
    Ans -> Resilience4j is a lightweight, Java-based fault-tolerance library designed for handling failures in microservices. 
           It provides various resilience patterns to help applications gracefully recover from failures instead of crashing.
    Q-> Why Use Resilience4j?
    Ans -> Microservices often rely on external services (databases, APIs, etc.), which may become slow or fail. 
           Resilience4j helps improve system stability by preventing cascading failures and reducing downtime.

    Key Features of Resilience4j
    1. Circuit Breaker 🛑

    -> Monitors API failures and temporarily stops requests when failures exceed a threshold.

    -> Automatically recovers after a defined time.

    -> Example: If an external service is down, the circuit breaker prevents unnecessary requests until the service is stable.

    2. Retry 🔄

    -> Retries a failed request a specified number of times before giving up.

    -> Useful for handling temporary network failures or service timeouts.

    3. Rate Limiter ⏳

    -> Controls the number of requests per second to avoid overloading services.

    -> Example: Limits users to 10 requests per second to prevent excessive API usage.

    4. Bulkhead 🚪

    -> Limits concurrent requests to a service to prevent resource exhaustion.

    -> Ensures that failures in one part of the system do not affect others.

    5. Time Limiter ⏰

    -> Cancels requests that take too long to complete.

    -> Helps prevent slow responses from blocking system resources.

    Example Implementation in CircuitBreakerController.java:
    
    @CircuitBreaker(name = "sample-api", fallbackMethod = "hardcodedResponse")
    public String sampleApi() {
        return new RestTemplate().getForEntity("http://localhost:8080/dummy", String.class).getBody();
        }

### 2. Feign Client (Inter-Service Communication)

    -> Feign is a declarative REST client in Spring Boot that simplifies HTTP communication between microservices. 
    -> Instead of manually creating HTTP requests using RestTemplate, Feign allows you to define interfaces and let Spring handle the implementation.

    Feign simplifies REST client calls between microservices:
    @FeignClient(name= "currency-exchange")
    public interface CurrencyExchangeProxy {
        @GetMapping("/currency-exchange/from/{from}/to/{to}")
        CurrencyConversion getCurrencyConversionRate(@PathVariable String from, @PathVariable String to);
    }

## Conclusion

    -> This project demonstrates a microservices-based currency exchange system with fault tolerance mechanisms using Resilience4j. 
    -> It showcases best practices for Spring Boot Microservices, REST APIs, and Database Integration.
    



