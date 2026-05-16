# File Transfer with scp and rsync

This repository contains a hands-on lab focused on secure remote data transport, incremental synchronization algorithms, and storage replication pipelines within a multi-node Linux network architecture. You will master standardizing remote file operations using Secure Copy Protocol (`scp`), optimizing bandwidth usage with the delta-transfer engine (`rsync`), and constructing automated backup routines over encrypted SSH channels.

---

## 🎯 Objectives
By the end of this lab, you will be able to:
- Transfer individual files and full subdirectories securely across systems using `scp`.
- Synchronize distributed storage blocks efficiently using `rsync` differential delta passes.
- Enforce specific file preservation rules (permissions, timestamps, symlinks) during migrations.
- Architect network-aware transport lanes featuring bandwidth constraints and resumption checkpoints.
- Compare and evaluate when to deploy `scp` vs. `rsync` based on real-world engineering constraints.

## 📋 Prerequisites
- Two network-reachable Linux nodes (physical machines or virtual guests) with active SSH daemons.
- Basic working knowledge of standard command-line interface (CLI) operations.
- SSH key-pair authentication configured between the systems (highly recommended to prevent interactive prompts).

---

## ⚙️ Lab Setup
1. Launch terminal windows corresponding to your local and target systems.
2. Verify that the master OpenSSH background listener service is operational:
   ```bash
   sudo systemctl status sshd
   ```
3. If necessary, deploy missing dependencies across your nodes using native platform tools:
   ```bash
   sudo apt-get install -y openssh-client openssh-server rsync  # For Ubuntu / Debian
   ```
   ```bash
   sudo dnf install -y openssh-clients openssh-server rsync    # For RHEL / CentOS Stream
   ```

---

## 🛠️ Lab Tasks

### Task 1: Secure File Transfer with scp
**Objective**: Package and push files over networks leveraging standard SSH channel encryption primitives.

#### 1.1 Basic scp File Transfer
1. Generate an initial text file block on your local machine to act as source data:
   ```bash
   echo "This is a test file" > local_file.txt
   ```
2. Stream and write the data file securely out to a targeted destination path location on the remote server host:
   ```bash
   scp local_file.txt username@remote_host:/home/username/
   ```
3. Run a quick remote execution inspection query loop over SSH to verify successful delivery:
   ```bash
   ssh username@remote_host "cat /home/username/local_file.txt"
   ```
   *Expected Output:* `This is a test file`

📌 **Key Concept**: The `scp` utility uses the standard SSH framework to build its communications channel. This encrypts both the data payload streams crossing network nodes and your authorization credentials.

#### 1.2 Directory Transfer with scp
1. Mass-produce an index folder containing multiple placeholder items to test batch structural transfers:
   ```bash
   mkdir local_dir
   touch local_dir/file{1..3}.txt
   ```
2. Sync the complete tree out to the target server by enabling recursive processing operations:
   ```bash
   scp -r local_dir username@remote_host:/home/username/
   ```
3. Interrogate the remote destination point directory list to validate the data matrix arrived perfectly:
   ```bash
   ssh username@remote_host "ls -l /home/username/local_dir"
   ```

💡 **Optimization Tip**: If copying large files over slow or high-latency network interfaces, append the uppercase `-C` flag parameter modifier to enable on-the-fly compression during the network stream.

---

### Task 2: File Synchronization with rsync
**Objective**: Deploy delta-transfer routines to isolate and synchronize only changed data bytes between files.

#### 2.1 Basic rsync Local to Remote
1. Flood additional text strings into your local directory stack to force file alterations:
   ```bash
   echo "Updated content" >> local_dir/file1.txt
   ```
2. Initiate a network synchronization sweep using comprehensive archive synchronization parameters:
   ```bash
   rsync -avz local_dir/ username@remote_host:/home/username/remote_dir/
   ```
   - `-a` : **Archive** mode (combines flags to preserve owner identities, file permissions, groups, and creation timestamps).
   - `-v` : **Verbose** operational report (lists specific items passing over the network interface line).
   - `-z` : **Compresses** data chunks inline during flight execution routines.

3. Execute a verification inspection: notice that `rsync` only processes and copies the modified delta segments tracking `file1.txt`, bypassing unchanged blocks to maximize efficiency.

#### 2.2 Incremental Backup with rsync
1. Establish a local system directory checkpoint node to test isolated recovery logic:
   ```bash
   mkdir backup
   ```
2. Trigger an absolute baseline duplicate sync, instructing the tool to cleanly purge orphaned objects at the destination destination:
   ```bash
   rsync -av --delete local_dir/ backup/
   ```
3. Append unique data layers into your text block arrays to force a system drift condition:
   ```bash
   echo "New line" >> local_dir/file2.txt
   ```
4. Re-run your storage synchronization backup pipeline:
   ```bash
   rsync -av --delete local_dir/ backup/
   ```
   *Expected Behavior:* The tool dynamically evaluates modification dates and targets *only* `file2.txt` for replication, ensuring your backup states align perfectly with minimal overhead.

---

### Task 3: Secure rsync with SSH
**Objective**: Encrypt differential data streams across custom network security wrappers.

#### 3.1 rsync Over SSH
1. Explicitly instruct the synchronization manager to route all communication data blocks through an encrypted SSH tunnel proxy:
   ```bash
   rsync -avz -e ssh local_dir/ username@remote_host:/home/username/secure_backup/
   ```
2. Confirm arrival conditions across the remote secure boundary:
   ```bash
   ssh username@remote_host "ls -l /home/username/secure_backup"
   ```

#### 3.2 Advanced rsync Options
Explore advanced parameter modifications designed to control complex production pipeline tasks:
```bash
# Resume Interrupted Large Transfers with Checksum Verification
rsync -avz --partial --progress -e ssh large_file.iso username@remote_host:/backups/

# Throttling Network Bandwidth Usage (Hard-limits data streams to exactly 100 KB/s)
rsync -avz --bwlimit=100 -e ssh local_dir/ username@remote_host:/backups/
```
*   `--partial`: Keeps partially transferred chunks if a connection drops mid-flight, allowing you to instantly resume file transfers from the exact moment of failure.
*   `--progress`: Displays real-time calculations tracking speed metrics, percent completion bars, and remaining time valuations.

---

## ⚖️ Technology Comparison Architecture Matrix



| Performance Evaluation Criteria | Secure Copy Protocol (`scp`) | Remote Synchronization (`rsync`) |
| :--- | :--- | :--- |
| **Transfer Methodology** | Copies files completely, overwriting targets regardless of prior existence. | Implements a delta-transfer matrix to sync only modified file portions. |
| **Bandwidth Management** | Basic; lacks built-in traffic throttling metrics out-of-the-box. | Exceptional; features granular transfer throttling parameters (`--bwlimit`). |
| **Interruption Resilience** | Fails entirely if disrupted; must restart the copy task from zero bytes. | Robust; allows partial file captures to pick up and resume right from failure points. |
| **Directory Optimization** | Standard recursive listing operations. | Native capability to prune destination file structures to mirror sources (`--delete`). |

---

## 🏁 Conclusion
During this lab session, you developed crucial system engineering data delivery and replication capabilities, including:
- Performing raw cryptographic remote file drops using `scp`.
- Implementing high-efficiency differential file synchronization maps with `rsync`.
- Setting defensive metadata rules to retain system asset ownership permissions matrices.
- Engineering throttled and resilient data recovery pipelines over secure SSH tunnels.

---

## 🚀 Next Steps
- Leverage automated scheduling loops by combining `rsync` commands directly into system timers or periodic engine queues (`crontab`).
- Investigate file sifting constraints by testing out directory restriction rules arrays: `--exclude="*.tmp"`.

---

## 💡 Troubleshooting Guide



| Log Issue Symptom | Root Cause Verification Method & Resolution Fix |
| :--- | :--- |
| `Permission denied` block | Verify remote file access permissions or check that public SSH key signatures are deployed correctly into the server's `authorized_keys`. |
| Network bandwidth exhaustion | Inject the transfer rate throttling parameter variable boundary rule: `--bwlimit=<speed>`. |
| Connection timeout alerts | Check physical transport routes using `ping` and inspect platform edge sockets/firewall barriers. |
| Host identity verification failures | An updated host key implies a server rebuild. Manually drop old index references via: `ssh-keygen -R <remote_host_ip>`. |

---

## 🧹 Cleanup
Maintain absolute hygiene on your engineering testing nodes by running these file eviction commands across both host prompts:
```bash
# Local teardown actions
rm -rf local_file.txt local_dir backup

# Remote teardown actions
ssh username@remote_host "rm -rf /home/username/{local_dir,remote_dir,secure_backup}"
```
