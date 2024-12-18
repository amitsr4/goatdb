<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/user-attachments/assets/4975e49c-e73c-435e-8e10-97adc2c0aaeb">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/user-attachments/assets/270caf47-3ed8-49d4-b3b9-74a51bd2d6c0">
  <img alt="GoatDB Logo" src="https://github.com/user-attachments/assets/270caf47-3ed8-49d4-b3b9-74a51bd2d6c0">
</picture>

---

# A Scalable, Edge-Native Database Built for Modern Workloads

GoatDB is a distributed open source database system inspired by the principles of version control systems like Git. It provides real-time synchronization and conflict resolution for application data, enabling independent nodes to operate collaboratively and efficiently scale to modern workloads. GoatDB is particularly well-suited for building edge-native applications by leveraging client-side capabilities.

## Project Status

GoatDB has been in production use as part of Ovvio’s real-time collaboration system since January 2024 ([https://ovvio.io](https://ovvio.io)). This repository aims to decouple the database core from Ovvio’s platform and make it available as an open-source project.

The first public release (v0.1) is targeted for Q1 2025. Progress is tracked in the Issues tab.

For inquiries, contact ofri [at] goatdb.com.

## Documentation

[API Overview](docs/api.md)
• [Architecture Overview](docs/architecture.md)
• [Concepts](docs/concepts.md)
• [Queries](docs/query.md)
• [Schemas](docs/schema.md)

#### Advanced Topics

[Commit Graph](docs/commit-graph.md)
• [Conflict Resolution](docs/conflict-resolution.md)
• [Synchronization Protocol](docs/sync.md)

## Design Rationale

### Motivation

GoatDB addresses limitations of traditional cloud-centric architectures, which were designed for centralized hardware (e.g., mainframes of the 1950s) and do not fully exploit the computational capabilities of modern client devices. By leveraging client-side processing power, GoatDB minimizes reliance on centralized infrastructure, improving performance and scalability.

This architecture aligns with the principles of the [Local First Community](https://localfirstweb.dev/), which advocates for prioritizing client-side capabilities.

### Supporting Edge-Native Applications

Edge-native applications run significant portions of their logic and data storage on client devices rather than relying solely on centralized cloud infrastructure. GoatDB’s architecture supports this paradigm by enabling the following:

- **Local Processing:** Queries and updates are executed locally on edge devices, reducing latency and dependency on network connectivity.
- **Real-Time Synchronization:** Changes made on one edge device are synchronized efficiently with others, maintaining consistency across the network.
- **Offline Operation:** Full functionality is preserved even when devices are disconnected from the network, with automatic reconciliation upon reconnection.
- **Data Ownership:** Users retain control over their data, as it resides on their devices rather than a central server.

By addressing these needs, GoatDB empowers developers to create applications optimized for performance, resilience, and user privacy.

### Key Architectural Features

#### Symmetric Peer-to-Peer (P2P) Network

GoatDB implements a managed P2P network where clients act as active participants in data processing and storage. The backend primarily handles permissions enforcement and data backups, simplifying its complexity.

#### Isomorphic TypeScript

The database logic is written in TypeScript, enabling seamless execution on both client and server environments. This approach reduces redundant code and streamlines development.

#### Append-Only Commit Graph

GoatDB’s core data structure is an append-only commit graph, which tracks changes incrementally. This design ensures data integrity, facilitates real-time synchronization, and provides a natural mechanism for conflict resolution.

## Core Capabilities

### Real-Time Synchronization

GoatDB synchronizes data across nodes in real time using an [efficient protocol](docs/sync.md) based on Bloom Filters. Updates propagate incrementally, ensuring low latency and consistency without developer intervention.

### Incremental Query Mechanism

GoatDB dynamically updates query results as underlying data changes. Queries remain "open," incorporating real-time updates with minimal computation overhead. This ensures responsive and consistent results, ideal for modern, interactive applications.

### Scalability for Modern Workloads

GoatDB is designed to handle the demands of modern applications, from lightweight mobile clients to enterprise-scale systems. By distributing processing and storage across client devices, GoatDB can scale horizontally with minimal infrastructure, making it ideal for applications with large datasets or high complexity.

### Offline Operation

Each node maintains an independent local copy of the database, enabling full offline functionality. When connectivity is restored, the system automatically reconciles changes.

### Conflict Resolution

The database employs a deterministic conflict resolution strategy based on three-way merging, ensuring consistent outcomes across nodes. Details are available in the [Conflict Resolution Documentation](docs/conflict-resolution.md).

### Performance Optimization

By offloading most processing to client devices, GoatDB reduces latency and scales efficiently. Clients act as data replicas, enabling faster reads and updates compared to cloud-only systems.

## Development and Deployment Workflow

### Simplified Development

Developers interact with GoatDB using an in-memory model, eliminating the need for complex API layers. The system supports [dynamic queries](docs/query.md) and real-time updates, streamlining application logic.

### Lightweight Deployment

Applications using GoatDB can be packaged as single executable containers that embed both the database and application logic. This design simplifies deployment to any environment, including on-premises servers.

### Fault Tolerance

Client devices actively participate in data recovery, reducing reliance on centralized backups. In the event of backend failures, clients ensure data continuity and availability.

## Technical Details

### Current Implementation

The database is implemented primarily in TypeScript, with performance-critical components written in C++ and compiled to WebAssembly (WASM).

### Future Plans

The codebase is being incrementally migrated to C++ to enhance performance and enable support for additional languages in the future.
