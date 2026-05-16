# Analyzing System Logs with journalctl

This repository contains a hands-on lab focused on advanced system telemetry, binary log querying, and operational diagnostics within the `systemd-journald` architecture. You will master standardizing analytical log workflows, extracting precision time-bounded data blocks, isolating structural severity planes, and enforcing disk-retention policy limits on live nodes.

---

## 🎯 Objectives
By the end of this lab, you will be able to:
- Query system logs using `journalctl` with complex temporal boundaries and system unit filters.
- Filter, sort, and isolate logging layers using priority fields and explicit boot lifecycle tracking maps.
- Export structured telemetry fields into JSON matrices to feed programmatic automation parsing tools.
- Audit central ledger storage volumes and safely truncate historical journals.

## 📋 Prerequisites
- A Linux operating system driven by the `systemd` supervisor engine (Fedora, RHEL, CentOS, or Ubuntu 16.04+).
- Terminal access featuring elevated `sudo` or administrative privileges.
- Basic familiarity running system command-line interface (CLI) commands.

---

## 🛠️ Lab Tasks

### Task 1: Query Logs Using journalctl Based on Time and Unit
**Objective**: Access the master unified logging book and target system data fields with explicit criteria.

#### Subtask 1.1: Display All Logs
1. Open your terminal window.
2. Initialize an all-inclusive query tracking the complete persistent journal ledger repository:
   ```bash
   journalctl
   ```
   *Expected Outcome:* Opens a searchable, paginated screen layout outputting complete historical logs.

💡 **Troubleshooting**: If the tracking sheet maps out as entirely empty or returns access faults, confirm that the underlying collection daemon engine is operational: `sudo systemctl status systemd-journald`.

#### Subtask 1.2: Filter Logs by Time
The system journal uses an indexed engine to isolate specific chronological metrics pools on-demand.
1. Extract logs recorded strictly within the most recent hourly system window block:
   ```bash
   journalctl --since "1 hour ago"
   ```
2. Narrow the extraction scope to run between precise wall-clock timestamp parameter markers:
   ```bash
   journalctl --since "09:00:00" --until "10:00:00"
   ```
   *Explanation:* The `--since` and `--until` parameters accept absolute calendar arrays (`YYYY-MM-DD HH:MM:SS`) or relative text expressions (e.g., `"today"`, `"yesterday"`, `"2 hours ago"`).

#### Subtask 1.3: Filter Logs by Unit
1. Isolate and view log captures exclusively tracking a particular background service daemon (e.g., the Secure Shell server):
   ```bash
   journalctl -u sshd
   ```
2. Combine multi-criteria parameters to filter service behaviors within a strict temporal lane:
   ```bash
   journalctl -u sshd --since "today"
   ```

---

### Task 2: Filter Logs by Priority Level and Boot Session
**Objective**: Target severe system execution faults and compare application states across hardware restarts.

#### Subtask 2.1: Filter by Priority Level
Systemd journals group logs across standard numeric severity tiers.
1. Strip out informational entries and display *only* those rows carrying explicit error flags:
   ```bash
   journalctl -p err
   ```
2. Filter an inclusive severity spectrum range to capture any messages ranking from warnings up to critical panic alerts:
   ```bash
   journalctl -p warning..emerg
   ```

📌 **System Priority Reference Key Matrix**:
*   `0`: **emerg** (System unusable)
*   `1`: **alert** (Action must be taken immediately)
*   `2`: **crit** (Critical conditions)
*   `3`: **err** (Error conditions)
*   `4`: **warning** (Warning conditions)
*   `5`: **notice** (Normal but significant condition)
*   `6`: **info** (Informational messages)
*   `7`: **debug** (Debug-level messages)

#### Subtask 2.2: Filter by Boot Session
1. Generate an index inventory identifying every historical system boot sequence cached on disk storage:
   ```bash
   journalctl --list-boots
   ```
   *Expected Output Matrix:* A lookup table mapping relative boot indices (e.g., `0` for active boot, `-1` for previous boot), boot string signatures, and precise temporal bounds.
2. Query the active log database exclusively filtering metrics tracking your current hardware up-time session:
   ```bash
   journalctl -b
   ```
3. Extract error paths logging anomalies during the immediate prior system operational run:
   ```bash
   journalctl -b -1
   ```

---

### Task 3: Advanced journalctl Options for Detailed Analysis
**Objective**: Stream live diagnostic lines and enforce storage vacuum limits to optimize disk properties.

#### Subtask 3.1: Follow Logs in Real-Time
1. Initialize a dynamic, open-ended logging feed stream to observe system events live on-the-fly:
   ```bash
   journalctl -f
   ```
   *(Similar to running `tail -f`, this updates instantly. Strike `Ctrl + C` to break out of the trace).*

#### Subtask 3.2: Output in JSON for Scripting
1. Export your time-bounded system log data using raw machine-readable JSON mapping structures:
   ```bash
   journalctl -u sshd --since "1 hour ago" -o json
   ```
   *Use Case:* Standardizing formatting onto JSON streamlines piping arrays directly into third-party text parsing engines like `jq`, or logging index engines like Splunk.

#### Subtask 3.3: Show Disk Usage and Vacuum Old Logs
1. Interrogate storage partitions to calculate the precise space footprint consumed by binary journal logs on disk:
   ```bash
   journalctl --disk-usage
   ```
2. Enforce an administrative rotation maintenance policy, safely vacuuming and purging the oldest records to shrink total log space:
   ```bash
   sudo journalctl --vacuum-size=100M
   ```

---

## 🏁 Conclusion
During this lab session, you developed crucial production-level logging and system analytics capabilities, including:
- Indexing deep binary journals with accuracy using time limits and unit flags (`-u`, `--since`).
- Filtering system telemetry lines based on explicit priority thresholds (`-p`) and boot logs (`-b`).
- Exporting raw structured data fields to alternate outputs (`-o json`) to feed external scripts.
- Managing journal logs maintenance tasks to optimize machine disk capacities.

---

## 🚀 Next Steps
- Dig deeper into advanced query flags by researching the comprehensive tool manuals via `man journalctl`.
- Investigate the integration of local `systemd` logs into centralized enterprise logging collectors like the **ELK Stack** or **Grafana Loki**.

---

## 💡 Troubleshooting Guide



| Issue Symptom | Root Cause Verification Method & Resolution Fix |
| :--- | :--- |
| Query processing runs exceptionally slow | Constrain massive trace searches by narrowing your temporal lookup scope, or disable the viewer pager entirely by passing `--no-pager`. |
| `"No journal files"` error returned | Ensure the log path storage tree located at `/var/log/journal/` carries valid system permissions and is correctly owned by the `systemd-journal` group. |
| Non-root users cannot query logs | Add the user account to the system administration logging permissions group using: `sudo usermod -aG systemd-journal <username>`. |
