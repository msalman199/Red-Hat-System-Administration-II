# Environment and Shell Variables

This repository contains a hands-on lab focused on context scoping, dynamic configuration data parsing, and environment standardization within the Linux Bash runtime shell. You will master differentiating system-wide environment variables from local script scopes, enforcing persistent variables templates via terminal configurations (`~/.bashrc`), and structuring isolated application configurations for programmatic automation.

---

## 🎯 Objectives
By the end of this lab, you will be able to:
- Distinguish between globally inherited environment variables and restricted shell variables.
- Configure temporary session variables and implement persistent environment architectures.
- Create, manipulate, and pass local scoped variables within functional automation scripts.
- Design modular runtime configuration text wrappers (`.cfg`) to power automation pipelines.

## 📋 Prerequisites
- A Linux operating system running an active Bash shell environment (version 4.x or newer).
- Basic familiarity running foundational command-line interface (CLI) utilities.
- Access to a standard terminal text editor (such as `nano`, `vim`, or `gedit`).

---

## 🛠️ Lab Tasks

### Task 1: Working with Environment Variables
**Objective**: Query system-wide configuration blocks and establish persistent global environmental scopes.

#### Subtask 1.1: Viewing Environment Variables
1. Open your terminal application and list all currently running global variables:
   ```bash
   printenv
   ```
   *Expected Outcome:* Generates a comprehensive tracking table tracing system parameters (e.g., `USER`, `LANG`, `SHELL`).

📌 **Key Concept**: **Environment variables** are globally available properties maintained across the shell environment. Crucially, they are inherited automatically by any child processes, subshells, or applications spawned from that specific terminal thread.

2. Extract and print specific isolated variables from the active memory cache using the expansion prefix (`$`):
   ```bash
   echo $HOME
   echo $PATH
   ```

#### Subtask 1.2: Setting Temporary Environment Variables
1. Instantiate an on-demand, session-restricted global variable using the `export` statement:
   ```bash
   export LAB_USER="dev_user"
   echo $LAB_USER
   ```
   *Expected Output:* `dev_user`

*(Note: This memory pointer is strictly volatile and disappears entirely the moment your current shell session terminates or the terminal tab is closed).*

#### Subtask 1.3: Making Variables Persistent
1. Open your personal user profile execution script inside your text editor:
   ```bash
   nano ~/.bashrc
   ```
2. Append your persistent target workspace directory mapping rule to the absolute bottom of the layout:
   ```text
   # Persistent Lab Projects Storage Context
   export PROJECT_DIR="/opt/my_project"
   ```
3. Commit and write out changes, drop out of the editor, and synchronize the active session parameters to load the updates:
   ```bash
   source ~/.bashrc
   echo $PROJECT_DIR
   ```

---

### Task 2: Shell Variables in Scripts
**Objective**: Script localized variables and enforce strict programmatic boundary containment rules.

#### Subtask 2.1: Basic Variable Usage
1. Spin up an automation script file workspace named `variables.sh`:
   ```bash
   nano variables.sh
   ```
2. Populate the workspace with the following token expansion logic strings:
   ```bash
   #!/bin/bash
   greeting="Welcome"
   user=$(whoami)
   echo "$greeting $user! Today is $(date)"
   ```
3. Apply standard system execution parameters onto the file and run the execution path:
   ```bash
   chmod +x variables.sh
   ./variables.sh
   ```

#### Subtask 2.2: Variable Scope Demonstration
1. Construct an independent diagnostic file named `scope_demo.sh` to demonstrate visibility layers:
   ```bash
   nano scope_demo.sh
   ```
2. Insert the functional block code below to track boundary enforcement:
   ```bash
   #!/bin/bash
   global_var="I'm global"

   function demo() {
       local local_var="I'm local"
       echo "Inside function: $global_var, $local_var"
   }

   demo
   echo "Outside: $global_var"
   echo "Local outside: $local_var"
   ```
3. Run the evaluation script track:
   ```bash
   chmod +x scope_demo.sh
   ./scope_demo.sh
   ```

📌 **Key Concept**: Affixing the `local` declaration prefix strictly chains variable existence limits exclusively to the internal scope of the defining function block wrapper. The terminal line output tracing `$local_var` from outside the function will print out completely empty.

---

### Task 3: Automation with Variables
**Objective**: Author modular, isolated text definitions arrays to feed script pipelines cleanly.

#### Subtask 3.1: Configuration File with Variables
1. Build an application parameter profile text wrapper file named `config.cfg`:
   ```bash
   nano config.cfg
   ```
2. Insert your isolated automation metrics parameters:
   ```text
   # Application runtime configuration
   MAX_RETRIES=3
   LOG_LEVEL="DEBUG"
   BACKUP_DIR="/var/backups"
   ```
3. Build a distinct driver execution script named `app.sh` that actively pulls in the configurations from your storage sheet:
   ```bash
   nano app.sh
   ```
4. Populate `app.sh` with the extraction logic block below:
   ```bash
   #!/bin/bash
   # Source the local external configuration properties
   source config.cfg

   echo "Running with:"
   echo "Retries: \$MAX_RETRIES"
   echo "Log Level: \$LOG_LEVEL"
   mkdir -p \$BACKUP_DIR
   ```
5. Commit and run the application script check:
   ```bash
   chmod +x app.sh
   ./app.sh
   ```

#### Subtask 3.2: Command Substitution Reporting
1. Create a script named `sys_report.sh` that leverages dynamic execution mapping outputs to record real-time system metrics:
   ```bash
   #!/bin/bash
   current_users=\$(who | wc -l)
   disk_usage=\$(df -h / | awk 'NR==2 {print \$5}')

   echo "System Report:"
   echo "Active users: \$current_users"
   echo "Root FS usage: \$disk_usage"
   ```
2. Trigger the performance reporting check pipeline:
   ```bash
   chmod +x sys_report.sh
   ./sys_report.sh
   ```

---

## 💡 Troubleshooting Guide

*   **Variables Fail to Persist Over Time**: Ensure you are editing the exact, authentic initialization script matching your active shell environment tier (e.g., `~/.bashrc` tracking user states).
*   **"Command Not Found" Exceptions**: Verify that your target file paths hold active execute permissions vectors using `chmod +x filename`.
*   **Variables Fail to Expand properly**: Encapsulate complex context variable lookups inside standard **double quotes** (`"$var"`). Wrapping pointers inside *single quotes* (`'$var'`) treats the string literally and completely neutralizes token expansion rules.
*   **System-Wide Variable Access Rejections**: Restructuring persistent variable tables globally across the physical machine filesystem layer (`/etc/environment`) requires root administrative access. Prepend target edits with `sudo`.

---

## 🏁 Conclusion
During this lab session, you developed crucial infrastructure scripting and automation capabilities, including:
- Deciphering global inheritance paths versus localized user shell allocations.
- Structuring persistent custom profiles fields using standard `.bashrc` targets.
- Building functional local parameter scripts capable of processing runtime variable scopes.
- Architecting independent configuration wrappers (`.cfg`) to power clean infrastructure playbooks.

These variable mapping methodologies are fundamental requirements for passing runtime arguments safely across deployment tasks, creating robust automation modules, and prepping underlying host templates for enterprise-grade containers orchestration networks like OpenShift.

---

## 🚀 Next Steps
- Dig into complex multidimensional data sets by investigating array storage constructs natively inside Bash.
- Deepen your telemetry visibility by researching the behavior of special automated variables built directly into the shell framework (e.g., `$?` for tracking previous exit codes, or `$$` for identifying the current Process ID).
- Practice engineering wide machine baseline defaults securely within `/etc/environment`.
