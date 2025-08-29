# Neon Azure CLI Extension - Preview API Integration Plan

## Overview
Integrating new commands from 2025-06-23-preview API version into the existing Neon Azure CLI extension.

## Current Status
- ✅ Fixed existing command structure (from `az neon postgres` to `az neon`)
- ✅ All current tests are working with 2025-03-01 stable API
- ✅ Created new branch: `feature/neon-preview-api-updates`

## New Endpoints in 2025-06-23-preview

### 1. Individual Endpoint Management
**New endpoint:** `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Neon.Postgres/organizations/{organizationName}/projects/{projectName}/branches/{branchName}/endpoints/{endpointName}`

**Proposed CLI commands:**
- `az neon endpoint show --resource-group RG --organization-name ORG --project-name PROJECT --branch-name BRANCH --endpoint-name ENDPOINT`
- `az neon endpoint update --resource-group RG --organization-name ORG --project-name PROJECT --branch-name BRANCH --endpoint-name ENDPOINT`
- `az neon endpoint delete --resource-group RG --organization-name ORG --project-name PROJECT --branch-name BRANCH --endpoint-name ENDPOINT`

### 2. Individual Database Management  
**New endpoint:** `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Neon.Postgres/organizations/{organizationName}/projects/{projectName}/branches/{branchName}/neonDatabases/{neonDatabaseName}`

**Proposed CLI commands:**
- `az neon neon-database show --resource-group RG --organization-name ORG --project-name PROJECT --branch-name BRANCH --database-name DB`
- `az neon neon-database create --resource-group RG --organization-name ORG --project-name PROJECT --branch-name BRANCH --database-name DB`
- `az neon neon-database update --resource-group RG --organization-name ORG --project-name PROJECT --branch-name BRANCH --database-name DB`
- `az neon neon-database delete --resource-group RG --organization-name ORG --project-name PROJECT --branch-name BRANCH --database-name DB`

### 3. Individual Role Management
**New endpoint:** `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Neon.Postgres/organizations/{organizationName}/projects/{projectName}/branches/{branchName}/neonRoles/{neonRoleName}`

**Proposed CLI commands:**
- `az neon neon-role show --resource-group RG --organization-name ORG --project-name PROJECT --branch-name BRANCH --role-name ROLE`
- `az neon neon-role create --resource-group RG --organization-name ORG --project-name PROJECT --branch-name BRANCH --role-name ROLE`
- `az neon neon-role update --resource-group RG --organization-name ORG --project-name PROJECT --branch-name BRANCH --role-name ROLE`
- `az neon neon-role delete --resource-group RG --organization-name ORG --project-name PROJECT --branch-name BRANCH --role-name ROLE`

### 4. Branch Preflight Validation
**New endpoint:** `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Neon.Postgres/organizations/{organizationName}/projects/{projectName}/branches/{branchName}/preflight`

**Proposed CLI commands:**
- `az neon branch preflight --resource-group RG --organization-name ORG --project-name PROJECT --branch-name BRANCH`

## Implementation Plan

### Phase 1: Create Preview API Directory Structure
1. Create `src/neon/azext_neon/aaz/preview/` directory
2. Create `src/neon/azext_neon/aaz/preview/2025-06-23-preview/` directory 
3. Copy current structure from `latest/` and update API version

### Phase 2: Add Individual Resource Commands
1. Add endpoint show/update/delete commands
2. Add database create/show/update/delete commands  
3. Add role create/show/update/delete commands
4. Add branch preflight command

### Phase 3: Update Command Registration
1. Register new commands under appropriate command groups
2. Update help documentation
3. Add parameter validation

### Phase 4: Add Tests
1. Create test scenarios for new commands
2. Update existing tests to optionally use preview API
3. Add integration tests

### Phase 5: Documentation
1. Update README with new commands
2. Add examples for each new command
3. Document preview vs stable differences

## Technical Notes

### API Version Strategy
- Keep existing stable 2025-03-01 commands as default
- Add new preview commands with `--api-version 2025-06-23-preview` flag option
- Allow users to opt into preview features

### Command Naming Convention
- Follow existing pattern: `az neon <resource-type> <action>`
- Individual resource commands: `show`, `create`, `update`, `delete`
- List commands: `list` (already exists)

### Backwards Compatibility
- All existing commands should continue to work
- New commands should be clearly marked as preview
- Provide migration path documentation

## Next Steps
1. ✅ Create implementation plan
2. 🔄 Generate AAZ command files for new endpoints
3. ⏳ Implement individual endpoint commands
4. ⏳ Implement individual database commands
5. ⏳ Implement individual role commands  
6. ⏳ Implement preflight validation
7. ⏳ Add comprehensive tests
8. ⏳ Update documentation
