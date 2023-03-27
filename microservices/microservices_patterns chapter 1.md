# Microservices Patterns

## Chapter 1 - Escaping monolithic hell
> Note: Monolith architecture isn't entirely evil, and it has it's uses. 

It is particularly useful when
* rapidly prototype (time to market is more important)
* when scale is not an issue.


### Problems with monolithic architecture

### How to scale a monolith 
* There are different aspects a monolith which can be acted upon to scale it
  * Functional decomposition
     * Separate them based on functions.
     * eg. In an e-commerce site, the monolith can be broken down based on certain functionalities like, Checkout, Order Management, Delivery systems etc.
  * Partitioning on data 
    * eg. partitioning on customerID for certain requests.
  * Horizontal scaling
    * Introduce more copies of the monolithic system.
