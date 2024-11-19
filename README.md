# postgresql-env-vuln
Details and proof-of-concept for a vulnerability affecting PostgreSQL environment variable handling via PL/Python functions.

# PostgreSQL Environment Variable Manipulation Vulnerability

## Description
This repository documents a vulnerability identified in PostgreSQL versions 16.3 and 17.1 that allows for unauthorized manipulation of environment variables using `PL/Python` functions. This issue can lead to unauthorized command execution, privilege escalation, and other potential security impacts.

## Affected Products/Versions
- **Product**: PostgreSQL
- **Versions Affected**: 16.3, 17.1

## Proof-of-Concept (PoC)
### Steps to Reproduce
1. Connect to the PostgreSQL database as a user with permissions to create functions.
   ```bash
   psql -U [username] -d [database_name]

