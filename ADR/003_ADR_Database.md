# ADR003 - Use of Polygot persistence (NoSQL or SQL Database) depending on the service for the Fishy Watch system  
## Status
proposed
## Context
Choosing the right database for the Fishy Watch system services is crucial because it directly impacts the service's ability to scale, perform, and evolve over time. 

It needs to cater to the below requirements:
-  Diverse Data Types: A wide variety of data is collected from the health metrics of individual fish to environmental factors like water quality and weather conditions.
-  Volume and Velocity: Given its global operation, the service handles large volumes of data at high velocities, especially for real-time monitoring and alerting systems.
-  Evolving Nature: The service is innovative, incorporating features like fish-ual recognition, implying that the data schema could evolve.
- Complex Queries for Insights: The service requires the ability to perform complex queries to derive insights, such as determining optimal harvest times or identifying health trends.

*NoSQL databases*, are well suited for their schema flexibility, scalability, and high performance with large volumes of data, can handle unstructured or semi-structured data effectively.

*SQL databases*, are well suited for their strong consistency, ACID (Atomicity, Consistency, Isolation, Durability) transactions, and structured schema. They are ideal for complex queries and transactions that involve multiple operations needing to be completed successfully to maintain data integrity.
    
## Decision
Considering the requirements for the Fishy Watch system, a hybrid *approach utilizing both NoSQL and SQL databases* might be the most beneficial. This approach is aligned with modern microservices architectures and the concept of polyglot persistence, where different services or components of the application can use different database technologies that best fit their needs​​.

We will use SQL databases for managing structured data that requires complex transactions and relational integrity, such as customer and farm management, where relationships and integrity are important.

We will use NoSQL databases for high-velocity, flexible data requirements such as real-time water quality monitoring, fish health tracking, and environmental alerting, where the schema-less nature of NoSQL can accommodate the evolving data models and will scale well with the volume of data.

This approach allows us to leverage the strengths of both database types, ensuring robust, scalable, and flexible data management across all aspects of the Fishy Watch service.

## Consequences
Choosing a polyglot persistence model has some pros and cons.

*Pros:*
- Optimized Performance: By using the best-suited database for each task, the service can achieve optimal performance. 
- Scalability: Different databases can scale according to their respective data's needs, and handle diverse datasets.
- Flexibility and Adaptability: As new requirements emerge or the service evolves, introducing new database technologies becomes feasible without re-architecting the entire system. 
- Risk Mitigation: By not relying on a single database for all operations, the system's risk of a single point of failure is reduced. Different parts of the system can operate independently, enhancing overall resilience.

*Cons:*
-  Increased Complexity: Managing multiple databases introduces complexity in data integration, and consistent data representation across the system.
- Operational Overhead: Each database technology brings its own set of operational requirements, from maintenance and scaling to backup and recovery processes. This can increase the operational overhead and require more diverse expertise.
- Potential for Data Silos: Without effective data integration strategies, there's a risk of creating data silos where information is isolated in different databases, making holistic analysis difficult.

The decision to adopt a polyglot persistence model aligns with the complex needs of the Fishy Watch system, optimizing for performance, scalability, and flexibility. While it introduces certain challenges, with the right strategies and investment in training, effective data governance and adopting microservices architecture, these can be effectively managed.

