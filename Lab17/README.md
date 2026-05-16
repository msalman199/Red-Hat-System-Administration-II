# Using setenforce and semanage for SELinux

This repository contains a hands-on lab focused on advanced Mandatory Access Control (MAC) engineering, runtime mode switching, network socket policy management, and compilation of custom rule templates using `setenforce` and `semanage`. You will master changing operational states, re-authoring port type assignments permanently, and building custom policy modules from security audit records.

---

## 🎯 Objectives
By the end of this lab, you will be able to:
- Check system security enforcing statuses and switch between operational modes using `setenforce`.
- Implement permanent mode alterations inside core system initialization scripts (`/etc/selinux/config`).
- Configure permanent networking policies by binding non-standard application sockets using `semanage`.
- Isolate and resolve system restrictions by compiling automated policy blocks via `audit2allow` and `semodule`.

## 📋 Prerequisites
- A Linux operating system featuring an active SELinux enforcement engine (Red Hat Enterprise Linux, CentOS Stream, or Fedora recommended).
- Terminal access featuring elevated `sudo` or root administrative privileges.
- Basic familiarity running system command-line interface (CLI) commands.

---

## 🛠️ Lab Tasks

### Task 1: Switching SELinux Modes Using setenforce
**Objective**: Interrogate kernel state tables and apply temporary and persistent runtime enforcement mode drops.

#### Subtask 1.1: Check Current SELinux Status
1. Open your terminal window and verify the deep configuration parameters driving the security engine:
   ```bash
   sestatus
   ```
   *Expected Output Grid:*
   ```text
   SELinux status:                 enabled  
   SELinuxfs mount:                /sys/fs/selinux  
   SELinux root directory:         /etc/selinux  
   Loaded policy name:             targeted  
   Current mode:                   enforcing  
   Mode from config file:          enforcing  
   ```
2. Run an abbreviated check to return only the raw operational mode context:
   ```bash
   getenforce
   ```
   *Expected Output:* `Enforcing`

#### Subtask 1.2: Temporarily Change SELinux Mode
1. Transition the kernel security state into logging-only mode (bypasses active access restrictions but logs policy violations):
   ```bash
   sudo setenforce 0
   ```
2. Validate that the runtime mode tracking updated instantly:
   ```bash
   getenforce
   ```
   *Expected Output:* `Permissive`
3. Restore absolute protection rules across all operational lanes by shifting enforcement back online:
   ```bash
   sudo setenforce 1
   ```
   *Expected Output Verification:* Running `getenforce` returns `Enforcing`.

📌 **Volatile State Rule**: Variables rewritten using the `setenforce` binary command are strictly temporary. The settings live exclusively inside volatile memory structures and reset back to baseline configuration values after a system reboot.

#### Subtask 1.3: Permanently Change SELinux Mode
1. Open the primary kernel security configuration tracking registry file:
   ```bash
   sudo vi /etc/selinux/config
   ```
2. Locate the initialization variable option string and rewrite it to match your target persistent state:
   ```text
   SELINUX=permissive  # Options run: enforcing | permissive | disabled
   ```
3. Save edits and exit the text workspace. Reload the complete machine environment to apply changes persistently across the filesystem:
   ```bash
   sudo reboot
   ```

---

### Task 2: Managing SELinux Policies with semanage
**Objective**: Provision advanced policy adjustment toolchains and map custom application ports permanently.

#### Subtask 2.1: Install policycoreutils-python-utils
1. Ensure the advanced security management python utilities are fully deployed on your filesystem:
   ```bash
   sudo dnf install policycoreutils-python-utils -y
   ```

#### Subtask 2.2: List SELinux Port Contexts
1. Interrogate the security policy database to output a complete ledger of network port type assignments:
   ```bash
   sudo semanage port -l
   ```
   *Expected Output Section:*
   ```text
   SELinux Port Type              Proto    Port Number  
   http_port_t                    tcp      80, 443, 8080  
   ```

#### Subtask 2.3: Add a Custom Port to SELinux Policy
1. Authorize an application to run on a non-standard network socket path (e.g., exposing web services over port `8081`) by appending a binding declaration:
   ```bash
   sudo semanage port -a -t http_port_t -p tcp 8081
   ```
2. Verify that your permanent socket type assignment successfully integrated into the database:
   ```bash
   sudo semanage port -l | grep 8081
   ```
   *Expected Output Trace:* `http_port_t                    tcp      8081`

#### Subtask 2.4: Remove a Custom Port
1. Purge a custom socket policy rule to drop its network authorization profile:
   ```bash
   sudo semanage port -d -t http_port_t -p tcp 8081
   ```

---

### Task 3: Troubleshooting SELinux Denials
**Objective**: Parse raw AVC denial records and compile automated binary overrides to heal application faults.

#### Subtask 3.1: Check SELinux Denials in Logs
1. Search through the system security logs database to extract active Access Vector Cache (AVC) violations:
   ```bash
   sudo ausearch -m avc -ts recent
   ```
2. Alternatively, perform plain text pattern lookups directly against raw system security audit ledgers:
   ```bash
   sudo grep "avc: denied" /var/log/audit/audit.log
   ```

#### Subtask 3.2: Generate and Apply a Custom Policy Module
When a complex, specialized custom application triggers persistent denials despite having valid standard directory file permissions, compile an explicit rule exemption bundle.
1. Pipe filtered raw text violation logs into the policy generation engine to automatically construct a customized mitigation module:
   ```bash
   sudo grep "avc: denied" /var/log/audit/audit.log | audit2allow -M mypolicy
   ```
2. Inject the compiled active binary policy payload block back into the running kernel memory space:
   ```bash
   sudo semodule -i mypolicy.pp
   ```

#### Subtask 3.3: Debugging Best Practices
If you encounter obscure access faults, shift your configuration temporarily to permissive tracking (`sudo setenforce 0`) to determine if the issue is a policy enforcement error. Test your target service, and immediately restore complete protection profiles (`sudo setenforce 1`) once verified.

---

## 🏁 Conclusion
During this lab session, you developed crucial enterprise security administration and triage capabilities, including:
- Toggling live runtime security modes dynamically with `setenforce`.
- Hardcoding structural persistent baseline states within `/etc/selinux/config`.
- Modifying permanent application network port type restrictions via `semanage`.
- Compiling customized policy modules (`.pp`) via `audit2allow` to automatically remediate application service blocks.

---

## 💡 Troubleshooting Guide




| Log Issue Symptom | Root Cause Verification Method & Resolution Fix |
| :--- | :--- |
| `semanage` command returns missing error lines | The engine packages are missing from the OS image. Deploy them immediately via: `sudo dnf install policycoreutils-python-utils -y`. |
| Core variable state adjustments fail to apply | Double-check configurations. Always cross-verify actual operating parameters using `sestatus` or `getenforce`. |
| Low-level logs are hard to read or interpret | Pipe raw logs straight into explanations lookup modules to evaluate the human-readable cause: `sudo ausearch -m avc -ts recent \| audit2why`. |

---

## 🚀 Next Steps
- Deepen your system troubleshooting proficiency by leveraging the graphical alerting engine parser via `sealert`.
- Master granular rule tuning parameters by exploring how to check and alter core functional security flags on-the-fly using **SELinux Booleans** (`getsebool` / `setsebool`).
