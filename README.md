# Holochain Next-Gen Network Resource Planning (Next-Gen NRP)

## Overview

Next-Gen NRP is a distributed, peer-to-peer contribution logging system built using Holochain. It is the next generation implementation of [Sensorica's original Network Resource Planning system](https://github.com/Sensorica/VerdunNRP), which was developed in collaboration with Lynn Foster and Bob Haugen, the creators of ValueFlow and hREA. While the original NRP served as a prototype for implementing Value Network principles, Next-Gen NRP represents a full implementation of ValueFlow specification and hREA framework, combined with the comprehensive toolset of the original NRP.

This system is specifically designed for Open Value Networks (OVN), enabling users to track and verify contributions across various projects in a decentralized manner, with advanced features for resource management and process tracking.

> **Note**: The current project structure is temporary. The system will be reimplemented using the ValueFlow specification and hREA (Holochain Resource Event Architecture) framework to ensure standardized resource and event tracking across the network.

## Key Features

- **Contextual Work Management**
  - Project tracking and organization
  - Venture management
  - Community organization
  - Custom context types

- **Process Management**
  - Resource allocation
  - Status tracking
  - Version control
  - Timeline management

- **Resource Management**
  - Material and immaterial resource tracking
  - Resource accessibility control
  - Property regime management
  - Resource behavior tracking

## Technical Architecture

The system is built on Holochain, leveraging its distributed architecture for:

- DHT (Distributed Hash Table) implementation
- Peer-to-peer data synchronization
- Agent-centric computing model
- Capability-based security
- Signals and event-driven architecture

## Prerequisites

- Rust programming environment
- Holochain development environment

## Getting Started

```bash
# Clone the repository
git clone [repository-url]

# Navigate to the project directory
cd next-gen-nrp

# Build the project
cargo build

# Run tests
cargo test
```

## System Components

### Core Data Structures

- **Context**: Manages different types of organizational units (Projects, Ventures, Communities)
- **Process**: Handles workflow and task management
- **Resource**: Tracks both material and immaterial resources

## Implementation Details

1. **Distributed Architecture**
   - P2P data synchronization
   - DHT implementation
   - Agent-centric model

2. **Holochain Integration**
   - Zomes and DNA structure
   - Entry and link management
   - Validation rules
   - Security implementation

## Documentation

Detailed documentation is available in the `/documentation` directory:

- `overview.md`: High-level project overview
- `spec.md`: Technical specifications
- `/technical`: Technical implementation details
- `/user-guide`: User documentation

## Contributing

Contributions are welcome! Please refer to the technical documentation for development guidelines and standards.

## License

[License Information]
