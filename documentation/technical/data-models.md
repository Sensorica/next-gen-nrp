# Data Models

## Core Data Structures

### Context
The Context structure represents the environment or scope in which work is performed.

```rust
#[hdk_entry(id = "context")]
pub struct Context {
    pub id: EntryHash,
    pub name: String,
    pub description: String,
    pub context_type: ContextType,
    pub created_at: Timestamp,
    pub updated_at: Option<Timestamp>,
    pub version: u32,
}
```

#### Context Types
```rust
pub enum ContextType {
    Project { category: String },
    Venture { focus_area: String },
    Community { domain: String },
    Other { specification: String },
}
```

### Process
The Process structure represents a workflow or series of actions.

```rust
#[hdk_entry(id = "process")]
pub struct Process {
    pub id: EntryHash,
    pub name: String,
    pub description: String,
    pub resources: Vec<Resource>,
    pub category: ProcessCategory,
    pub status: ProcessStatus,
    pub created_at: Timestamp,
    pub updated_at: Option<Timestamp>,
    pub version: u32,
}
```

### Resource
Resources represent assets or materials used in processes.

```rust
pub struct Resource {
    pub id: EntryHash,
    pub name: String,
    pub description: String,
    pub resource_type: ResourceType,
    pub rivalrous: bool,
    pub transferability: Transferability,
    pub accessibility: Accessibility,
    pub availability: Availability,
    pub scope: EntryHash,
    pub property_regime: PropertyRegime,
    pub source: Source,
    pub behavior: Behavior,
    pub unit_type: UnitType,
    pub unit_of_use: f64,
    pub created_at: Timestamp,
    pub updated_at: Option<Timestamp>,
    pub version: u32,
}
```

## Resource Management

### Accessibility
```rust
pub enum Accessibility {
    Free,
    Protected { 
        credentials: Vec<String>,
        expiry: Option<Timestamp>,
    },
    FormallyRestricted { 
        procedures: String,
        approval_required: bool,
    },
}
```

### Behavior Types
```rust
pub enum Behavior {
    Material {
        economic_processes: Vec<MaterialEconomicProcess>,
        management: Vec<MaterialManagement>,
        location: Location,
        governance: Vec<GovernanceRequirement>,
    },
    Immaterial {
        economic_processes: Vec<ImmaterialEconomicProcess>,
        management: Vec<ImmaterialManagement>,
        location: Location,
        governance: Vec<GovernanceRequirement>,
    },
}
```

## Location Management
```rust
pub enum Location {
    Physical {
        address: String,
        coordinates: Option<(f64, f64)>, // latitude, longitude
    },
    Virtual {
        url: Option<URL>,
        path: Option<String>,
    },
}
```
