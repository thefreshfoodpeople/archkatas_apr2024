# ADR004 - Use of GraphQL Federation for Service composition
## Status
Accepted
## Context
Farmers may utilise various devices, including rugged industrial devices designed for use on the sea during harvest. This necessitates a UI pattern that is adaptable enough to accommodate a wide range of frontend solutions. 

Maintaining the integrity of domain boundaries is crucial, and the UI pattern should offer flexibility to support different types of frontend applications.

Additionally, fish farms may encounter limited network availability, emphasizing the importance of considering lower bandwidth usage.

## Decision
The decision is to go with Graph QL Federation pattern. In this pattern, a client interacts with a single entry point, known as the router, which acts as the central hub for orchestrating and distributing requests across multiple backend services. The router plays a crucial role in ensuring efficient and consistent data retrieval and aggregation.


## Consequences
The GraphQL Federation pattern offers several key advantages:
- Modularity: GraphQL federation supports modularity by supporting multiple subgraphs representing different domains
- Scalability: The load is distributed across multiple services, enhancing scalability and performance.
- Flexibility: New services can be added or removed without affecting the overall architecture, providing flexibility to adapt to changing requirements.
- Low latency: GraphQL mitigates the issues with under-fetching / over-fetching.
