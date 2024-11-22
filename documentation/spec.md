# Holochain Network Resource Planning 2.0

## Project Overview
A distributed, peer-to-peer contribution logging system built using Holochain, enabling users to track and verify contributions across various projects in a decentralized manner.

## System Architecture

### Core Components
1. **Contextual Work**
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

#[derive(Serialize, Deserialize)]
pub enum ContextType {
    Project { category: String },
    Venture { focus_area: String },
    Community { domain: String },
    Other { specification: String },
}
```

2. **Process**
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

#[derive(Serialize, Deserialize)]
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

#[derive(Serialize, Deserialize)]
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

#[derive(Serialize, Deserialize)]
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

#[derive(Serialize, Deserialize)]
pub enum MaterialEconomicProcess {
    Create { cost: Option<f64> },
    Consume { rate: Option<f64> },
    Use { duration: Option<TimeUnit> },
    Destroy { reason: String },
    Transfer { 
        transfer_type: TransferType,
        conditions: Option<String>,
    },
}

#[derive(Serialize, Deserialize)]
pub enum MaterialManagement {
    Deplete { rate: f64 },
    Wear { condition_status: String },
    Replace { reason: String },
    Replenish { quantity: f64 },
    Maintain { 
        procedure: String,
        frequency: TimeUnit,
    },
    Repair {
        issue: String,
        cost: Option<f64>,
    },
    Store {
        storage_location: Location,
        conditions: Option<String>,
    },
    Transport {
        from: Location,
        to: Location,
        cost: Option<f64>,
    },
}

#[derive(Serialize, Deserialize)]
pub enum ImmaterialEconomicProcess {
    Create,
    Cite { reference: String },
    Fork { 
        parent_id: EntryHash,
        changes: String,
    },
    Mix { 
        sources: Vec<EntryHash>,
        description: String,
    },
    Transfer { 
        transfer_type: TransferType,
        conditions: Option<String>,
    },
}

#[derive(Serialize, Deserialize)]
pub enum ImmaterialManagement {
    Maintain { 
        procedure: String,
        frequency: TimeUnit,
    },
    Store {
        storage_type: StorageType,
        location: Location,
    },
    Share {
        access_type: AccessType,
        conditions: Option<String>,
    },
}

#[derive(Serialize, Deserialize)]
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

#[derive(Serialize, Deserialize)]
pub enum GovernanceRequirement {
    Training { 
        certification: String,
        expiry: Option<Timestamp>,
    },
    Certification {
        type_: String,
        issuer: String,
        expiry: Option<Timestamp>,
    },
    Membership {
        role: String,
        level: String,
    },
    Justification {
        required_fields: Vec<String>,
        approval_process: String,
    },
    SecurityClearance {
        level: String,
        issuer: String,
    },
}

#[derive(Serialize, Deserialize)]
pub enum TransferType {
    Exchange { terms: String },
    Share { conditions: String },
    Loan { duration: TimeUnit },
    Gift,
}

#[derive(Serialize, Deserialize)]
pub enum StorageType {
    Digital { format: String },
    Document { format: String },
    PhysicalMedia { medium_type: String },
}

#[derive(Serialize, Deserialize)]
pub enum AccessType {
    Public,
    Restricted { conditions: String },
    Private { authorized_agents: Vec<AgentPubKey> },
}

#[derive(Serialize, Deserialize)]
pub enum Source {
    Context(Context),
    Partner {
        partner_id: EntryHash,
        partner_name: String,
        sharing_terms: Option<String>,
        agreement_ref: Option<String>,
    },
    Purchased {
        vendor: String,
        purchase_date: Option<Timestamp>,
        purchase_price: Option<f64>,
        currency: Option<CurrencyUnit>,
        invoice_ref: Option<URL>,
    },
    Custom {
        source_type: String,
        description: String,
    },
}

#[derive(Serialize, Deserialize)]
pub enum PropertyRegime {
    Private, // Ownership of an agent
    Public, // Public ownership by civil society
    PoolOfShareable, // Pool of shareable resources determined by context
    Communs, // Public domain
    NonDominion, // No ownership possible
}

#[derive(Serialize, Deserialize)]
pub enum Transferability {
    Transferable, // Ownership transfert
    NonTransferable, // No ownership transfert
    Shareable, // Communs ownership
}

#[derive(Serialize, Deserialize)]
pub enum Availability {
    Abundant,
    Scarce,
}

#[derive(Serialize, Deserialize)]
pub enum UnitType {
    Currency(CurrencyUnit),
    Time(TimeUnit),
    Weight(WeightUnit),
    Volume(VolumeUnit),
    Distance(DistanceUnit),
    Percentage(f64),
    Other(String),
}

#[derive(Serialize, Deserialize)]
pub enum CurrencyUnit {
    Coins,
    Fiat,
    Custom(String),
}

#[derive(Serialize, Deserialize)]
pub enum TimeUnit {
    Minutes,
    Hours,
    Days,
    Weeks,
    Months,
    Years,
}

#[derive(Serialize, Deserialize)]
pub enum WeightUnit {
    Milligram,
    Gram,
    Kilogram,
    Tonne,
    Pound,
    Ounce,
}

#[derive(Serialize, Deserialize)]
pub enum VolumeUnit {
    Milliliter,
    Liter,
    CubicMeter,
    Gallon,
    FluidOunce,
}

#[derive(Serialize, Deserialize)]
pub enum DistanceUnit {
    Millimeter,
    Centimeter,
    Meter,
    Kilometer,
    Mile,
    Foot,
    Yard,
    Inch,
}

#[derive(Serialize, Deserialize)]
pub enum ResourceType {
    HumanLabor,    // Time spent doing work
    Material,      // Physical resources (tools, equipment)
    Digital,       // Software, digital assets
    Space,         // Physical or virtual space
    Method,        // Protocols, procedures
    Currency,      // Financial resources
    Usable,        // Usable resources
    Consumable,    // Consumable resources (impact the inventory)
    Other(String)  // Custom resource
}

#[derive(Serialize, Deserialize)]
pub enum ProcessCategory {
    Maintenance(MaintenanceType),
    CapacityBuilding(CapacityBuildingType),
    Deliverable(DeliverableType),
    WrappingUp(WrappingType),
    Dissemination(DisseminationType),
}

#[derive(Serialize, Deserialize)]
pub enum ProcessStatus {
    Planning,
    InProgress,
    Completed,
    OnHold,
    Cancelled,
}

#[derive(Serialize, Deserialize)]
pub enum MaintenanceType {
    Stewardship,      // Ongoing care and maintenance
    Formalization,    // Documentation and standardization
    Initialization,   // Setup and preparation
}

#[derive(Serialize, Deserialize)]
pub enum CapacityBuildingType {
    HumanResources,     // Training, skill development
    AdditionalFunding,  // Resource acquisition
    Infrastructure,     // System improvements
}

#[derive(Serialize, Deserialize)]
pub enum DeliverableType {
    Prototype,    // Early-stage implementation
    Design,       // Formal specifications
    Product,      // Final implementation
    Research,     // Studies and analysis
}

#[derive(Serialize, Deserialize)]
pub enum WrappingType {
    Packaging,    // Final preparation
    Documentation, // End-stage documentation
    Archival,     // Long-term storage
}

#[derive(Serialize, Deserialize)]
pub enum DisseminationType {
    Distribution,  // Sharing with stakeholders
    Publication,   // Public release
    Presentation, // Formal demonstration
}

#[hdk_entry_defs]
#[unit_enum(UnitDefIndex)]
pub enum EntryTypes {
    Resource(Resource),
    Context(Context),
    Process(Process),
}

#[hdk_entry_helper]
impl Resource {
    pub fn validate(&self) -> Result<(), String> {
        // Name validation
        if self.name.is_empty() || self.name.len() > 100 {
            return Err("Name must be between 1 and 100 characters".into());
        }

        // Description validation
        if self.description.len() > 1000 {
            return Err("Description must not exceed 1000 characters".into());
        }

        // Unit validation
        if self.unit_of_use < 0.0 {
            return Err("Unit of use must be non-negative".into());
        }

        Ok(())
    }
}

#[derive(Serialize, Deserialize, Clone)]
pub struct URL(String);

impl URL {
    pub fn new(url: String) -> Result<Self, &'static str> {
        // Basic URL validation
        if url.starts_with("http://") || url.starts_with("https://") {
            Ok(URL(url))
        } else {
            Err("Invalid URL format")
        }
    }
}

#[derive(Serialize, Deserialize)]
pub enum SkillLevel {
    Beginner,
    Intermediate,
    Advanced,
    Expert,
}

3. **Roles**
```rust
#[hdk_entry(id = "role")]
pub struct Role {
    pub id: EntryHash,
    pub name: String,
    pub description: String,
    pub profile: RoleProfile,
    pub responsibilities: Vec<String>,
    pub benefits: Vec<Benefit>,
    pub activities: Vec<Activity>,
    pub created_at: Timestamp,
    pub updated_at: Option<Timestamp>,
    pub version: u32,
}

#[derive(Serialize, Deserialize)]
pub struct RoleProfile {
    pub required_skills: Vec<Skill>,
    pub experience_level: ExperienceLevel,
    pub time_commitment: TimeCommitment,
}

#[derive(Serialize, Deserialize)]
pub struct Skill {
    pub name: String,
    pub level: SkillLevel,
}

#[derive(Serialize, Deserialize)]
pub enum ExperienceLevel {
    Entry,
    Intermediate,
    Expert,
}

#[derive(Serialize, Deserialize)]
pub struct TimeCommitment {
    pub hours_per_week: u32,
    pub duration_weeks: u32,
}

#[derive(Serialize, Deserialize)]
pub struct Benefit {
    pub description: String,
    pub benefit_type: BenefitType,
}

#[derive(Serialize, Deserialize)]
pub enum BenefitType {
    Learning,
    Financial,
    Social,
    Other(String),
}

#[derive(Serialize, Deserialize)]
pub struct Activity {
    pub name: String,
    pub description: String,
    pub frequency: ActivityFrequency,
}

#[derive(Serialize, Deserialize)]
pub enum ActivityFrequency {
    Daily,
    Weekly,
    Monthly,
    Occasional,
}

4. **Contribution Entry**
```rust
#[hdk_entry(id = "contribution")]
pub struct Contribution {
    pub id: EntryHash,
    pub title: String,
    pub description: String,
    pub url: Option<URL>,
    pub context: EntryHash,
    pub process: EntryHash,
    pub role: EntryHash,
    pub contribution_type: ContributionType,
    pub metrics: Vec<Metric>,
    pub tags: Vec<String>,
    pub status: ContributionStatus,
    pub created_at: Timestamp,
    pub updated_at: Option<Timestamp>,
    pub version: u32,
}

#[derive(Serialize, Deserialize)]
pub struct Metric {
    pub name: String,
    pub value: f64,
    pub unit: String,
}

#[derive(Serialize, Deserialize)]
pub enum ContributionStatus {
    Draft,
    Submitted,
    Verified(Vec<Verification>),
    Disputed(Vec<Dispute>),
}

#[derive(Serialize, Deserialize)]
pub struct Verification {
    pub verifier: AgentPubKey,
    pub timestamp: Timestamp,
    pub comment: Option<String>,
    pub rating: Option<u8>,
}

#[derive(Serialize, Deserialize)]
pub struct Dispute {
    pub reporter: AgentPubKey,
    pub timestamp: Timestamp,
    pub reason: String,
    pub evidence: Vec<String>,
    pub status: DisputeStatus,
}

#[derive(Serialize, Deserialize)]
pub enum DisputeStatus {
    Open,
    UnderReview,
    Resolved(DisputeResolution),
}

#[derive(Serialize, Deserialize)]
pub enum DisputeResolution {
    Accepted,
    Rejected,
    Modified,
}

#[derive(Serialize, Deserialize)]
pub enum ContributionType {
   Time { hours: f64 },
   Currency { amount: f64, currency_type: String },
   Material { items: Vec<ResourceItem> },
   SocialCapital { impact: String },
   Knowledge { category: String },
   Other { specification: String },
}

#[derive(Serialize, Deserialize)]
pub struct ResourceItem {
    pub name: String,
    pub quantity: f64,
    pub unit: String,
}
```

## Technical Requirements

### Holochain Specifics
- Use Holochain for peer-to-peer data synchronization
- Implement robust validation rules
- Ensure data consistency across peers
- Handle offline-first scenarios

### Validation Rules
1. Contribution Creation
   - Title and description must not be empty
   - Project name must be valid
   - Time spent must be positive
   - Created timestamp must be valid
   - At least one tag required

2. Contribution Update
   - Only original creator can modify contribution details
   - Status changes must follow logical progression
   - Timestamps must be chronologically valid

3. Verification Rules
   - Cannot verify own contributions
   - Verification must include meaningful feedback
   - Multiple verifications allowed per contribution

### Advanced Features
1. **Collaborative Verification**
   - Multi-agent verification system
   - Weighted verification based on agent reputation
   - Dispute resolution mechanism

2. **Reporting and Analytics**
   - Time tracking and statistics
   - Contribution patterns analysis
   - Project-based reporting
   - Personal contribution portfolio

3. **Privacy and Security**
   - End-to-end encryption
   - Granular access controls
   - Agent-based authentication

## Development Milestones

### Phase 1: Basic Functionality
- [ ] Set up Holochain development environment
- [ ] Define contribution entry types
- [ ] Implement create and read operations
- [ ] Basic validation rules
- [ ] Local testing

### Phase 2: Advanced Features
- [ ] Implement verification system
- [ ] Add reporting functionality
- [ ] Implement filtering and sorting
- [ ] Comprehensive error handling

### Phase 3: Distributed Features
- [ ] Multi-agent verification
- [ ] Analytics and statistics
- [ ] Dispute resolution
- [ ] Performance optimization

## Technology Stack
- Holochain
- Rust
- hdk (Holochain Development Kit)
- wasmer (WebAssembly runtime)

## Development Environment Setup

[Install Holochain Development Environment](https://developer.holochain.org/get-started/#2-installing-holochain-development-environment) (if not already installed)

```bash
# Create new Holochain web app project
nix run github:/holochain/holochain#hc-scaffold -- web-app contribution-log
cd contribution-log

# Run the development environment using Nix (instlled with the Holochain dev environment)
nix develop
```

## Testing Strategy
1. Unit Tests
   - Individual zome function tests
   - Validation rule verification
2. Integration Tests
   - Multi-agent scenario testing with Tryorama
   - Verification workflow tests
3. Performance Tests
   - Large dataset handling
   - Synchronization speed

## Performance Considerations
- Minimize DHT lookups
- Efficient data serialization
- Optimize validation logic
- Implement caching strategies

## Potential Challenges
1. Complex verification workflows
2. Fair dispute resolution
3. Privacy vs. transparency balance
4. Performance with large datasets

## Recommended Learning Resources
1. Holochain Developer Documentation
2. Rust Programming Language Book
3. Distributed Systems Concepts
4. WebAssembly and Wasm Runtime

## Submission Criteria
1. Fully functional Holochain DNA
2. Comprehensive test coverage
3. Detailed documentation
4. Performance benchmark results
5. Deployment instructions

## Bonus Challenges
- Implement contribution templates
- Add automated verification checks
- Create contribution badges/achievements
- Develop a web or mobile frontend
