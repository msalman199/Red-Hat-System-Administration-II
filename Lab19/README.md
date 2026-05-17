# Configuring NFS for File Sharing

This repository provides a step-by-step guide to installing, configuring, and optimizing a Network File System (NFS) for seamless and secure file sharing between Linux systems.

## 🎯 Objectives
By the end of this lab, you will be able to:
* **Deploy NFS Nodes:** Install and configure both NFS server and client systems.
* **Share Storage Pools:** Export directories from a central server and mount them on remote clients.
* **Harden & Tune Infrastructure:** Optimize operational NFS configurations for enterprise security and performance.

## 📋 Prerequisites
* **Environment:** Two separate Linux instances acting as a server and a client.
* **Access:** Root or `sudo` administrative privileges on both machines.
* **Network:** Active network connectivity and path routing between the instances (verify via `ping`).
* **Tooling:** Basic familiarity with the Linux command-line interface.

---

## 🛠️ Step-by-Step Implementation

### Task 1: Install and Configure NFS Server and Client

#### Subtask 1.1: Install NFS Packages
Execute package installations depending on your target Linux distribution.

##### 🖥️ Server Side
Update local registries and download server components:
```bash
# For RHEL / CentOS / Fedora
sudo dnf update -y && sudo dnf install nfs-utils -y

# For Debian / Ubuntu
sudo apt update -y && sudo apt install nfs-kernel-server -y
```

##### 💻 Client Side
Download the corresponding mounting libraries:
```bash
# For RHEL / CentOS / Fedora
sudo dnf install nfs-utils -y

# For Debian / Ubuntu
sudo apt install nfs-common -y
```
* **Expected Outcome:** Package dependencies install cleanly across both deployment roles.

#### Subtask 1.2: Start and Enable NFS Services
##### 🖥️ Server Side
Bootstrap the runtime engine and open communication doors within the host firewall:
```bash
# Start and enable daemon processes
sudo systemctl enable --now nfs-server
sudo systemctl status nfs-server

# Authorize required network rules persistently
sudo firewall-cmd --add-service={nfs,nfs3,mountd,rpc-bind} --permanent
sudo firewall-cmd --reload
```
* **Expected Outcome:** Core NFS services reach an active running state and firewalls allow inbound storage traffic.

---

### Task 2: Export Directories and Mount on Client

#### Subtask 2.1: Create and Export Directory on Server
##### 🖥️ Server Side
Provision the workspace area, configure system permissions, and expose the share path:
```bash
# Prepare target share directory and unlock permissions
sudo mkdir -p /mnt/nfs_share
sudo chown nobody:nobody /mnt/nfs_share
sudo chmod 777 /mnt/nfs_share
```
Open `/etc/exports` in your editor:
```bash
sudo nano /etc/exports
```
Append the network access rule (replace `client_ip` with your client machine's specific IP or subnet):
```text
/mnt/nfs_share client_ip(rw,sync,no_subtree_check)
```
Reload table definitions to broadcast the share out to the local network fabric:
```bash
sudo exportfs -arv
```
* **Expected Outcome:** `/mnt/nfs_share` becomes available across the wire to the targeted host client.

#### Subtask 2.2: Mount the NFS Share on Client
##### 💻 Client Side
Form an endpoint mapping directly back to the physical server storage:
```bash
# Create local mount path mapping
sudo mkdir -p /mnt/nfs_mount

# Mount remote network block device (replace server_ip with server's actual IP)
sudo mount -t nfs server_ip:/mnt/nfs_share /mnt/nfs_mount
```
Verify active mounting tables:
```bash
df -hT | grep nfs
```
* **Expected Outcome:** Storage shows up successfully mounted directly under local client workspace paths.

---

### Task 3: Optimize NFS for Security and Performance

#### Subtask 3.1: Secure NFS Exports
Tighten security structures to enforce read-only configurations or manage privilege scaling. Edit your `/etc/exports` file on the server:
```text
/mnt/nfs_share client_ip(ro,sync,no_root_squash)
```
* **Key Configuration Parameters:**
  * `ro`: Restricts target systems strictly to Read-Only operational access permissions.
  * `no_root_squash`: Maps client root requests straight to actual server root privileges (use with absolute caution).

Apply changes to the server instantly:
```bash
sudo exportfs -arv
```

#### Subtask 3.2: Automount NFS Share at Boot
##### 💻 Client Side
Ensure system mounts survive host machine reboots. Edit your local client file systems table:
```bash
sudo nano /etc/fstab
```
Append the automation map declaration at the bottom (replace `server_ip` with actual values):
```text
server_ip:/mnt/nfs_share  /mnt/nfs_mount  nfs  defaults  0  0
```
Test configurations live to guarantee syntax correctness:
```bash
sudo mount -a
```
* **Expected Outcome:** The file engine passes check validations and targets remount silently at every system boot.

---

## 🔬 Validation
Verify end-to-end access functionality across nodes:
1. **On the client system:** Drop a test payload validation file inside `/mnt/nfs_mount`.
2. **On the server system:** Verify the exact file reflects instantly under `/mnt/nfs_share`.

## 💡 Troubleshooting Tips
* **Permission / Mount Failures:** If connections drop, look up host error metrics inside `/var/log/messages` or execute `journalctl -xe`.
* **Firewall Blocks:** Double check if network access ports are locked using tools like `rpcinfo -p server_ip`.

## 🚀 Next Steps
* Integrate secure network fabrics by exploring advanced Kerberos authentication profiles (`sec=krb5`).

---
_This deployment lab utilizes standard enterprise components aligned with Red Hat OpenShift Development I certification tracks._
