# Aliases and Functions

This repository contains a hands-on lab focused on shell customization, command abstraction, and container administration workflows. You will master standardizing terminal operations using shortcuts (`aliases`), building reusable conditional command sequences (`functions`), and integrating both layers directly into automated architecture deployment scripts.

---

## 🎯 Objectives
By the end of this lab, you will be able to:
- Create, manage, and verify temporary and persistent shell aliases.
- Construct re-usable shell functions that accept positional parameters and exit variables.
- Enable macro expansion rules inside shell scripts to parse existing shortcuts safely.
- Apply abstraction concepts to streamline and simplify enterprise container management (`podman`) routines.

## 📋 Prerequisites
- A Linux operating system, macOS, or Windows WSL running a Bash shell environment.
- Access to a basic text editor (such as `vim`, `nano`, or VS Code).
- `podman` installed locally to execute container abstraction scenarios.
- Standard user access permissions for basic operations.

---

## ⚙️ Setup Requirements
1. Open your terminal application window.
2. Confirm your environment features a supported execution shell version:
   ```bash
   bash --version
   ```
3. Verify that your underlying microservice container engine is deployed and reachable:
   ```bash
   podman --version
   ```

---

## 🛠️ Lab Tasks

### Task 1: Creating Simple Aliases
**Objective**: Build rapid terminal execution macros to eliminate long command strings and standard options.

#### 1.1 Understanding Aliases
Aliases serve as customized shortcuts mapping straight onto underlying command structures to:
* Reduce manual typing strings.
* Build easily memorable, context-specific command names.
* Hardcode defensive default parameter flags onto standard execution commands.

#### 1.2 Creating Temporary Aliases
1. Inject an enhanced, detailed structural file directory listing shortcut into your volatile terminal memory block:
   ```bash
   alias ll='ls -alF'
   ```
2. Build a customized formatting macro tracking container runtime states to dump a clean, legible columns grid:
   ```bash
   alias pps='podman ps --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"'
   ```
3. Test your active operational macros immediately:
   ```bash
   ll
   pps
   ```
   *Expected Outcome:* The shell evaluates your short strings and replaces them instantly with explicit directory lists and structured container grids.

#### 1.3 Making Aliases Persistent
1. Open your persistent system profile customization file inside your text editor:
   ```bash
   nano ~/.bashrc
   ```
2. Append your persistent structural definitions to the absolute bottom of the configuration layout:
   ```text
   # Custom Persistent Aliases
   alias update='sudo dnf update -y'
   alias c='clear'
   ```
3. Write out changes, drop out of the editor, and force your session layout to synchronize and pull variables inline:
   ```bash
   source ~/.bashrc
   ```

💡 **Troubleshooting**: If modifications fail to carry over into newly spawned terminal frames, check that you target the correct profile (`~/.bashrc` vs `~/.bash_profile`), and confirm you ran the reload script sequence (`source`).

---

### Task 2: Creating Shell Functions
**Objective**: Develop logical blocks capable of evaluating inputs and iterating multi-command operations.

#### 2.1 Basic Function Structure
1. Construct a basic parameter block to evaluate a list of all containers alongside an aggregated calculation string:
   ```bash
   function pcount() {
       podman ps -a
       echo "Total containers: \$(podman ps -a -q | wc -l)"
   }
   ```
2. Call your newly generated function macro block directly in the terminal interface line:
   ```bash
   pcount
   ```

#### 2.2 Function with Parameters
1. Formulate a conditional routing map evaluating internal status arguments to govern active container state flags:
   ```bash
   function cstate() {
       if [ "\$1" = "start" ]; then
           podman start \$2
       elif [ "\$1" = "stop" ]; then
           podman stop \$2
       else
           echo "Usage: cstate [start|stop] [container]"
       fi
   }
   ```
2. Fire your validation code track against a placeholder test container target string name:
   ```bash
   cstate start my_container
   ```

#### 2.3 Persistent Functions
1. Append an independent background cleanup logic macro block to your profile config by piping text streams directly:
   ```bash
   echo '
   # Container cleanup function
   function pclean() {
       podman container prune -f
       podman image prune -a -f
   }' >> ~/.bashrc
   ```
2. Synchronize parameters online and clear out orphaned storage remnants immediately:
   ```bash
   source ~/.bashrc
   pclean
   ```

---

### Task 3: Using Aliases in Scripts
**Objective**: Configure external automation script structures to extend shell macros and complex reports.

#### 3.1 Creating a Script with Aliases
1. Spin up a script file workspace named `manage_system.sh` and populate it with the alias configuration code below:
   ```bash
   #!/bin/bash

   # Force subshell scopes to parse and allow alias replacement macros
   shopt -s expand_aliases

   # Pull internal custom user environment parameters inline
   source ~/.bashrc

   # Invoke definitions
   echo "System update starting..."
   update

   echo "Current containers:"
   pps
   ```
2. Apply operational execute flags onto the data node and trigger the execution path:
   ```bash
   chmod +x manage_system.sh
   ./manage_system.sh
   ```

#### 3.2 Advanced Script with Functions
1. Formulate a unified telemetry gathering loop within a separate execution file context:
   ```bash
   #!/bin/bash

   # Container telemetry compilation tracking script
   function container_report() {
       echo "=== Container Report ==="
       podman ps --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"
       echo "Disk Usage:"
       podman system df
   }

   container_report
   ```

---

## 🏁 Conclusion
During this lab session, you developed crucial shell logic optimization capabilities, including:
- Mapping command string expansions to brief system shortcut symbols.
- Authoring custom conditional function code blocks passing external variables (`$1`, `$2`).
- Setting persistent user environments via continuous loop tracking inside `.bashrc`.
- Instructing the bash option manager (`shopt`) to handle alias operations inside independent script parameters.

---

## 🚀 Next Steps
- Identify and isolate your most repetitive day-to-day administrative command routines and replace them with short custom macros.
- Design advanced helper logic statements to manage massive container pods and images with clean execution pipelines.
- Build orchestration layout scripts that parse configuration telemetry profiles cleanly.

---

## 🔎 Verification & Cleaning Checklist

- [ ] **Test Execution Mapping Expansion**: Verify operation tracking by provisioning at least 3 custom shortcuts.
- [ ] **Test Positional Logic Mapping**: Build 2 container management operations logic functions.
- [ ] **Wipe Temporary Memory Scopes**: Clear out your interactive terminal session placeholder macros to return to system defaults:
  ```bash
  unalias ll
  unalias pps
  ```

---

## 📚 Additional Resources
- Absolute Shell Syntax Documentation: `info bash`
- Container Command Line Manuals: [Podman Documentation](https://podman.io)
- Mendel Cooper's Architecture Guide: [Advanced Bash-Scripting Guide](https://tldp.org)
