# Neon Preview API Implementation Progress

## Overview
This document tracks the implementation of new preview API commands for the Neon Azure CLI extension using the 2025-06-23-preview API version.

## Completed Work

### 1. Directory Structure Setup
- Created preview API directory structure:
  ```
  src/neon/azext_neon/aaz/preview/
  ├── __init__.py
  └── 2025_06_23_preview/
      ├── __init__.py
      └── neon/
          ├── __init__.py
          ├── endpoint/
          ├── database/
          ├── role/
          └── branch/
  ```

### 2. New Preview Commands Implemented

#### Endpoint Management (Individual)
- **Command**: `az neon endpoint show --name {endpoint_name}`
- **File**: `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/endpoint/_show.py`
- **API Endpoint**: `GET /subscriptions/{}/resourcegroups/{}/providers/neon.postgres/organizations/{}/projects/{}/branches/{}/endpoints/{}`
- **Description**: Get details of a specific endpoint by name

#### Database Management (Individual)  
- **Command**: `az neon database show --name {database_name}`
- **File**: `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/database/_show.py`
- **API Endpoint**: `GET /subscriptions/{}/resourcegroups/{}/providers/neon.postgres/organizations/{}/projects/{}/branches/{}/databases/{}`
- **Description**: Get details of a specific database by name

#### Role Management (Individual)
- **Command**: `az neon role show --name {role_name}`
- **File**: `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/role/_show.py`  
- **API Endpoint**: `GET /subscriptions/{}/resourcegroups/{}/providers/neon.postgres/organizations/{}/projects/{}/branches/{}/roles/{}`
- **Description**: Get details of a specific role by name

#### Branch Preflight Validation
- **Command**: `az neon branch preflight`
- **File**: `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/branch/_preflight.py`
- **API Endpoint**: `POST /subscriptions/{}/resourcegroups/{}/providers/neon.postgres/organizations/{}/projects/{}/branches/{}/preflight`
- **Description**: Run preflight validation for branch operations

### 3. Command Features
- All commands marked with `is_preview=True`
- Use 2025-06-23-preview API version
- Follow AAZ (Auto-generated Azure CLI) patterns
- Include proper parameter validation and help text
- Support standard Azure CLI patterns (--name, --resource-group, etc.)

## Current Issues

### Command Loading Problem
The preview commands are not currently showing up in `az neon --help` output. Investigation shows:

1. **Command Registration Conflict**: Existing stable commands are registered at the same paths
2. **Module Loading**: Preview commands may not be loaded by default
3. **Integration Pattern**: May need different approach for preview command integration

### Existing Command Structure
The stable API already has:
- `az neon branch` commands (create, delete, list, show, update, wait)
- `az neon endpoint` commands (list)
- `az neon neon-database` commands
- `az neon neon-role` commands

## Next Steps

### Short Term (Immediate)
1. **Resolve Command Loading Issues**
   - Investigate why preview commands aren't loading
   - Check if preview commands need explicit enablement
   - Test with `--debug` flag to see loading errors

2. **Command Integration Strategy**
   - Option A: Create unique preview command names (e.g., `az neon endpoint-preview show`)
   - Option B: Integrate with existing command groups using API version flags
   - Option C: Fix module loading to properly handle preview commands

3. **Testing**
   - Create test cases for preview commands
   - Validate API endpoint functionality
   - Test parameter validation

### Medium Term
1. **Expand Command Coverage**
   - Add create/update/delete operations for endpoint, database, role
   - Implement list commands for individual resource management
   - Add more preflight validation options

2. **Documentation**
   - Create user documentation for preview features
   - Add examples for each command
   - Document differences from stable API

### Long Term
1. **Migration to Stable**
   - Plan for promoting preview commands to stable when API graduates
   - Ensure backward compatibility
   - Update documentation and examples

## Technical Details

### API Version Differences
- **Stable (2025-03-01)**: Collection-based operations (list all endpoints/databases/roles)
- **Preview (2025-06-23-preview)**: Individual resource operations (get/create/update/delete specific items)

### Command Pattern
All preview commands follow this pattern:
```python
@register_command(
    "neon {resource} {operation}",
    is_preview=True,
)
class {Operation}(AAZCommand):
    _aaz_info = {
        "version": "2025-06-23-preview",
        "resources": [...]
    }
```

### File Organization
- Commands organized by resource type (endpoint, database, role, branch)
- Each resource has its own directory with __cmd_group.py and operation files
- Follows existing AAZ patterns for consistency

## Files Created/Modified

### New Files (16 total)
- `src/neon/azext_neon/aaz/preview/__init__.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/__init__.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/__init__.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/endpoint/__cmd_group.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/endpoint/__init__.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/endpoint/_show.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/database/__cmd_group.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/database/__init__.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/database/_show.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/role/__cmd_group.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/role/__init__.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/role/_show.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/branch/__cmd_group.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/branch/__init__.py`
- `src/neon/azext_neon/aaz/preview/2025_06_23_preview/neon/branch/_preflight.py`

### Modified Files (1 total)  
- `src/neon/azext_neon/aaz/__init__.py` - Added preview import

## Commit History
- **25eab57ec**: "Add initial preview API commands for 2025-06-23-preview"
  - Initial implementation of 4 preview commands
  - Directory structure setup
  - AAZ pattern implementation
  - Preview API version integration

## Commands Ready for Testing (Once Loading Issue Resolved)
1. `az neon endpoint show --organization-name <org> --project-name <project> --branch-id <branch> --endpoint-name <endpoint> --resource-group <rg>`
2. `az neon database show --organization-name <org> --project-name <project> --branch-id <branch> --database-name <database> --resource-group <rg>`  
3. `az neon role show --organization-name <org> --project-name <project> --branch-id <branch> --role-name <role> --resource-group <rg>`
4. `az neon branch preflight --organization-name <org> --project-name <project> --branch-id <branch> --resource-group <rg>`
