# Validation Rules

## Core Validation Rules

### Context Validation

1. **Context Creation**
   - Name must be unique within the system
   - Description cannot be empty
   - Creation timestamp must be valid
   - Version must start at 1

2. **Context Updates**
   - Only authorized agents can modify contexts
   - Update timestamp must be after creation timestamp
   - Version number must increment
   - Context type cannot be changed after creation

### Process Validation

1. **Process Creation**
   - Must have a valid name and description
   - Must have at least one resource
   - Status must be valid for initial state
   - Creation timestamp must be valid

2. **Process Updates**
   - Only process owner can modify details
   - Status changes must follow logical progression
   - Resource list can only be modified by authorized agents
   - Update timestamp must be after creation timestamp

### Resource Validation

1. **Resource Creation**
   - Must have unique ID within context
   - Must have valid resource type
   - Must specify behavior type
   - Unit of use must be positive number

2. **Resource Updates**
   - Cannot change resource type after creation
   - Only authorized agents can modify resource details
   - Availability updates must be validated
   - Location changes must be tracked

## Security Validation

### Access Control

1. **Agent Authorization**
   - Verify agent capabilities
   - Check access permissions
   - Validate role assignments

2. **Resource Access**
   - Check accessibility rules
   - Verify credentials if protected
   - Follow formal procedures if restricted

### Data Integrity

1. **Entry Validation**
   - Verify data structure
   - Check required fields
   - Validate data types

2. **Link Validation**
   - Verify link types
   - Check source and target validity
   - Validate link permissions
