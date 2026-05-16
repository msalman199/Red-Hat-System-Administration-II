# Using rsyslog for Centralized Logging

This repository contains a hands-on lab focused on log aggregation architecture, distributed telemetry collection, and client-server security frameworks using the `rsyslog` service. You will master standardizing centralized log pipelines, setting up socket listeners (TCP/UDP), structuring dynamic log templates, and hardening multi-node logging paths.

---

## 🎯 Objectives
By the end of this lab, you will be able to:
- Install and provision the `rsyslog` package dependencies across a multi-node architecture.
- Configure a centralized **rsyslog Log Server** capable of parsing concurrent inbound network streams.
- Implement robust **rsyslog Client Forwarding** pipelines via reliable transport layer topologies.
- Isolate, categorize, and troubleshoot centralized log trees using precision searching tools.

## 📋 Prerequisites
- Two Linux system nodes (physical machines or virtual guests) running RHEL 8+ or Ubuntu 20.04+.
- Secure and unimpeded network routing configurations between both system interfaces.
- Terminal access featuring elevated `sudo` or root administrative privileges.

---

## ⚙️ Lab Setup
To validate this architectural layout, we establish the following template network assignments:
*   **Central Log Server Node**: IP Address `192.168.1.100` (`logserver.example.com`)
*   **Remote Client Node**: IP Address `192.168.1.101` (`client.example.com`)

---

## 🛠️ Lab Tasks

### Task 1: Install and Configure rsyslog
**Objective**: Provision the baseline daemon suite and configure the central server to listen on standard network ports.

#### Subtask 1.1: Install rsyslog on Both Systems
Execute the target installation sequence across **both** your server and client machines:
```bash
# For RHEL / CentOS Stream nodes:
sudo dnf install -y rsyslog

# For Ubuntu / Debian nodes:
sudo apt-get update && sudo apt-get install -y rsyslog
```
*Expected Outcome:* The `rsyslog` package is deployed onto the filesystems with no active transaction errors.

💡 **Troubleshooting**: If package lookups fail, verify subscription alignments on RHEL nodes (`subscription-manager status`), or refresh mirror indices on Ubuntu instances (`apt-get update`).

#### Subtask 1.2: Configure the rsyslog Server
Perform the following configuration adjustments exclusively on the **Server System** (`192.168.1.100`):
1. Open the primary logging rules configuration ledger:
   ```bash
   sudo vi /etc/rsyslog.conf
   ```
2. Uncomment or inject the following engine instructions to load network modules and map inbound data streams:
   ```text
   # Enable UDP syslog reception
   module(load="imudp")
   input(type="imudp" port="514")

   # Enable TCP syslog reception
   module(load="imtcp")
   input(type="imtcp" port="514")

   # Dynamic Storage Template: Organizes logs by client hostname and source process binary
   $template RemoteLogs,"/var/log/remotehost/%HOST-NAME%/%PROGRAMNAME%.log"
   *.* ?RemoteLogs
   & ~
   ```
3. Initialize the physical storage folder target defined within your custom template:
   ```bash
   sudo mkdir -p /var/log/remotehost
   ```
4. Recycle the logging service and authorize communication streams across your platform firewall:
   ```bash
   sudo systemctl restart rsyslog
   sudo systemctl enable rsyslog

   # Option A: For firewalld environments (RHEL / CentOS Stream)
   sudo firewall-cmd --add-port=514/tcp --permanent
   sudo firewall-cmd --add-port=514/udp --permanent
   sudo firewall-cmd --reload

   # Option B: For UFW environments (Ubuntu / Debian)
   sudo ufw allow 514/tcp
   sudo ufw allow 514/udp
   ```

---

### Task 2: Configure Client to Forward Logs
**Objective**: Instruct the remote client system to clone and stream system-wide telemetry packets over to the centralized array.

#### Subtask 2.1: Configure rsyslog Client
Perform the following tracking steps exclusively on the **Client System** (`192.168.1.101`):
1. Open the local routing configuration matrix:
   ```bash
   sudo vi /etc/rsyslog.conf
   ```
2. Append the target connection rule to the absolute baseline bottom of the file (double `@` symbols mandate highly reliable TCP streaming, a single `@` switches behavior to UDP packets):
   ```text
   *.* @@192.168.1.100:514
   ```
3. Cycle the client daemon engine to initialize the output pipeline:
   ```bash
   sudo systemctl restart rsyslog
   ```

#### Subtask 2.2: Test Log Forwarding
1. **On the Client Node**: Use the system shell logging utility to dispatch a custom payload diagnostic message:
   ```bash
   logger "This is a test message from client"
   ```
2. **On the Server Node**: Open a live tail stream tracking the newly spawned client subdirectory:
   ```bash
   sudo tail -f /var/log/remotehost/client.example.com/logger.log
   ```
   *Expected Outcome:* The raw text string payload dispatched by the client prompt surfaces instantly across the server terminal layout view.

---

### Task 3: Analyze Centralized Logs
**Objective**: Run diagnostic system audits and navigate multiple-client log archives.

#### Subtask 3.1: Using journalctl
If your server kernel leverages integrated systemd-journal configurations to mirror syslog captures, follow events dynamically via:
```bash
journalctl -f
```

#### Subtask 3.2: Using rsyslog Log Files
1. List the compartmentalized files automatically generated and split by the server engine:
   ```bash
   sudo ls -l /var/log/remotehost/client.example.com/
   ```
2. Execute rapid search patterns targeting explicit anomalies within the remote logging repositories:
   ```bash
   sudo grep -i "error" /var/log/remotehost/client.example.com/*.log
   ```

---

## ⚡ Advanced Configuration Options (Optional)

### Restrict Forwarding Targets
To limit network traffic and forward only highly critical or explicit subsystems (e.g., authentication logs and generic system errors), configure the client's `/etc/rsyslog.conf` ruleset as follows:
```text
authpriv.*  @@192.168.1.100:514
*.error     @@192.168.1.100:514
```

### Encrypted Log Transfer (TLS Hardening)
To encrypt logging packets crossing insecure network channels, deploy the generic security layer modules across both environments:
```bash
sudo dnf install -y rsyslog-gnutls   # Or apt-get equivalent on Debian engines
```
*(Requires deploying valid x509 encryption keys and configuring TLS handshake variables within unit files).*

---

## 🏁 Conclusion
During this lab session, you developed crucial production-level cluster infrastructure and log aggregation capabilities, including:
- Establishing a reliable multi-node central log harvester platform using `rsyslog`.
- Defining dynamic output paths and filters using flexible rsyslog variables templates.
- Enforcing structural firewall rules to guard open diagnostic network sockets.
- Sifting out anomaly paths safely across grouped multi-host targets.

These architecture mechanics are fundamental enterprise prerequisites for auditing security compliance, tracking system health trends, and prepping server hosts to pipe application records across modern cloud clusters like Red Hat OpenShift.

---

## 🚀 Next Steps
- Implement automated log maintenance routines across your incoming `/var/log/remotehost/` tree using **logrotate**.
- Explore pipeline expansions by loading files arrays straight into indexing engines like the **ELK Stack** or **Grafana Loki**.

---

## 💡 Troubleshooting Guide

*   **Logs Fail to Arrive on Server**: Validate low-level socket packet listening capabilities using standard connection checks: `telnet 192.168.1.100 514`. Verify the running daemon health status via `systemctl status rsyslog`.
*   **SELinux Blocking Network Streams**: SELinux policies may block unstandardized socket assignments or customized folder read/writes. Transition the engine temporarily to Permissive mode (`sudo setenforce 0`) to isolate enforcement faults from configuration faults.
*   **Internal Engine Errors**: Check local system outputs or the standard text syslog ledger paths (`/var/log/messages` or `/var/log/syslog`) to isolate internal system warnings.

---

## 🧹 Cleanup
To revert your sandbox machines back to default conditions, run the following:
```bash
# Wipe custom rule additions from /etc/rsyslog.conf
# Stop and disable the server port visibility rules if no longer required
# Purge firewall socket access permissions tracking port 514
```
