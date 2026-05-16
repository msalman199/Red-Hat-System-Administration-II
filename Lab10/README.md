# Log Rotation with logrotate

This repository contains a hands-on lab focused on log lifecycle management, automated data preservation, and storage security controls using the `logrotate` system utility. You will master standardizing disk protection frameworks, authoring modular log management scripts, enforcing compression algorithms, and manually validating rotation pipelines to mitigate partition space exhaustion.

---

## 🎯 Objectives
By the end of this lab, you will be able to:
- Configure comprehensive log rotation rules to shield system partitions from disk exhaustion.
- Understand, define, and implement custom target-specific log management parameters.
- Execute manual runtime validations using debugging (`--debug`) and forcing (`--force`) mechanics.
- Deploy script boundaries (`postrotate`) to gracefully notify active system log daemons.

## 📋 Prerequisites
- A Linux operating system (Red Hat Enterprise Linux, CentOS Stream 8+, or Fedora recommended).
- Terminal access featuring elevated `sudo` or root administrative privileges.
- Basic familiarity running system command-line utilities and editing text files (`vim`/`nano`).

---

## ⚙️ Setup Requirements
1. Open your terminal window and verify that the rotation management engine is present on your platform:
   ```bash
   sudo dnf install logrotate -y
   ```
2. Confirm the structural utility software version tracing your engine:
   ```bash
   logrotate --version
   ```

---

## 🛠️ Lab Tasks

### Task 1: Edit logrotate.conf for Custom Log Rotation Settings
**Objective**: Interrogate system default profiles and author an independent, isolated log optimization sheet.

#### Subtask 1.1: Examine Default Configuration
1. View the main system-wide configuration master ledger tracking global baseline defaults:
   ```bash
   sudo cat /etc/logrotate.conf
   ```
   *Expected Items:* Shows system fallback behaviors such as `weekly` intervals, `rotate 4` conservation limits, and `create` modes.
2. Inventory all current application-specific drop-in configuration scripts active on the server node:
   ```bash
   ls /etc/logrotate.d/
   ```

#### Subtask 1.2: Create a Custom Configuration
1. Initialize a new isolated log tracking configuration template profile workspace inside your editor:
   ```bash
   sudo vim /etc/logrotate.d/mycustomlogs
   ```
2. Populate the workspace with the following declarative rule blocks mapping onto your target log file:
   ```text
   /var/log/syslog {
       daily
       rotate 7
       compress
       delaycompress
       missingok
       notifempty
       create 0640 root adm
       postrotate
           /usr/bin/systemctl restart rsyslog >/dev/null 2>&1 || true
       endscript
   }
   ```

📌 **Core Policy Variable Reference Matrix**:
*   `daily`: Triggers the rotation sequence automatically once per calendar day.
*   `rotate 7`: Preserves exactly 7 historical back-logs on disk before evicting the oldest element.
*   `compress`: Compresses rotated log historical files using standard `gzip` algorithms to shrink space footprints.
*   `delaycompress`: Postpones the compression loop of the most recent file until the next sequential rotation cycle, ensuring it remains readable immediately if needed.
*   `missingok`: Bypasses error alerts and proceeds smoothly if the source file is missing temporarily.
*   `notifempty`: Aborts rotation execution tracks if the target log contains zero bytes of new data.
*   `create 0640 root adm`: Generates a brand new, empty replacement file assigning explicit file permissions and group contexts.
*   `postrotate / endscript`: Wraps conditional hook scripts triggered once *after* the file switch completes (e.g., instructing the system logging engine to safely reload and map onto the fresh file descriptor handle).

3. Perform a syntax check parsing validation loop inside your rule sheet without making live state modifications:
   ```bash
   sudo logrotate --debug /etc/logrotate.d/mycustomlogs
   ```
   *💡 Action Step:* Resolve any syntax structural errors flagged by the parser report before proceeding with live operations.

---

### Task 2: Force Log Rotation Using logrotate
**Objective**: Manually trigger immediate file operations and audit the resulting directory footprint.

#### Subtask 2.1: Manual Rotation Execution
1. Run a dry-run test trace forcing execution while logging verbose output onto your prompt:
   ```bash
   sudo logrotate --debug --verbose --force /etc/logrotate.d/mycustomlogs
   ```
2. Commit and run the actual live storage rotation sequence on disk:
   ```bash
   sudo logrotate --force /etc/logrotate.d/mycustomlogs
   ```

#### Subtask 2.2: Verify Rotation
1. Audit your target log directory workspace list to trace filename transformations:
   ```bash
   ls -lh /var/log/syslog*
   ```
   *Expected Outcome:* The listing demonstrates clean structural division, displaying the baseline empty file alongside your newly spawned archival artifact tracking index (e.g., `syslog.1`).

---

### Task 3: Test and Check Log Rotation Effects
**Objective**: Simulate data inflation scenarios, trigger policy actions, and evaluate file compression status profiles.

#### Subtask 3.1: Simulate Log Growth
1. Initialize a quick terminal piping loop to programmatically flood your active log file with placeholder text:
   ```bash
   for i in {1..1000}; do echo "Test log entry \$i" | sudo tee -a /var/log/syslog; done
   ```
2. Measure the physical file size calculation metric of the target log node:
   ```bash
   du -sh /var/log/syslog
   ```

#### Subtask 3.2: Trigger and Verify Rotation
1. Execute a forced execution processing run to cycle out the flooded log entries:
   ```bash
   sudo logrotate --force /etc/logrotate.d/mycustomlogs
   ```
2. Audit the directory listing one final time to verify rotation progression:
   ```bash
   ls -lh /var/log/syslog*
   ```
   *Expected Outcome:* The raw uncompressed log resets down to zero bytes, `syslog.1` matches the placeholder payload, and previous iterations move into compressed targets (e.g., `syslog.2.gz`).
3. Interrogate the structural data format mapping of your compressed archive file:
   ```bash
   file /var/log/syslog.2.gz
   ```
   *Expected Output Verification:* Terminal explicitly states the node matches `gzip compressed data`.

---

## 🏁 Conclusion
During this lab session, you developed crucial enterprise storage protection and platform maintenance capabilities, including:
- Authoring modular custom configuration policies within the `/etc/logrotate.d/` matrix.
- Designing optimization rules containing strict authorization masks, compression tiers, and script markers.
- Executing dry-run syntax audits (`--debug`) and operational overrides (`--force`).
- Gracefully handling daemon processes during active storage transformations.

Automated log control protects server nodes from random service crashes caused by 100% full root directory blocks, and is a mandatory building block for cluster security and storage reliability.

---

## 🚀 Next Steps
- Review automated job execution tracking by auditing the status logs recording your background cron engines:
  ```bash
  sudo cat /var/lib/logrotate/logrotate.status
  ```
- Track your storage capacities across target application mount boundaries dynamically over time: `df -h /var`.
- Explore configuring application-specific behaviors matching complex enterprise packages like Nginx, MariaDB, or Docker runtimes.

---

## 💡 Troubleshooting Guide

*   **Logs Refuse to Rotate**: Confirm the background daily system automation engine task is authorized to run by inspecting the execution tracker script pathway: `/etc/cron.daily/logrotate`.
*   **Access/Write Errors Permitting Actions**: Interrogate the parent file permissions and directory ownership mapping attributes tracking your target log via `ls -ld /var/log/syslog`.
*   **"Unknown group 'adm'" Exception Blocks**: Different Linux systems configure varying administration structures. If your environment throws this exception, update your `create` directive line inside `/etc/logrotate.d/mycustomlogs` to replace the value `adm` with `root`.
