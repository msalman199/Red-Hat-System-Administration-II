# Configuring I/O Scheduler

This repository contains a hands-on lab focused on storage subsystem engineering, kernel block layer tuning, and block device throughput optimization. You will master standardizing storage device performance, auditing active multi-queue schedulers (`mq-deadline`, `bfq`, `kyber`), implementing runtime kernel adjustments, and creating persistent hardware device rules using the system configuration framework (`udev`).

---

## 🎯 Objectives
By the end of this lab, you will be able to:
- Comprehend the functional role of alternative Multi-Queue (MQ) I/O schedulers in managing storage operations.
- Interrogate block layer system structures to check active storage path algorithms.
- Execute transient and persistent I/O scheduler swaps across physical and virtual block devices.
- Conduct synthetic read/write performance evaluations under various workload profiles using `iostat` and `fio`.

## 📋 Prerequisites
- A Linux operating system (Red Hat Enterprise Linux, CentOS Stream, Fedora, or Ubuntu).
- Terminal access featuring elevated `sudo` or root administrative privileges.
- Basic familiarity running system command-line interface (CLI) commands.
- At least one active block storage device target disk instance (e.g., `/dev/sda` or `/dev/sdb`) to run profile tests.

---

## 🛠️ Lab Tasks

### Task 1: Check Current I/O Scheduler
**Objective**: Provision platform diagnostic packages, map active disk algorithms, and profile system disk statistics.

#### Subtask 1.1: Install Required Tools
1. Open your terminal window and deploy the system monitoring and synthetic I/O storage benchmarking engines:
   ```bash
   sudo dnf install -y sysstat fio   # For RHEL / CentOS Stream / Fedora
   ```
   ```bash
   sudo apt install -y sysstat fio   # For Ubuntu / Debian
   ```

#### Subtask 1.2: Check Active I/O Schedulers
1. Generate an instant baseline list showing all available scheduling options across your visible SCSI/SATA block paths:
   ```bash
   ls /sys/block/sd*/queue/scheduler
   ```
2. Execute a loop routine to print out the explicit active algorithm driving each separate disk interface node:
   ```bash
   for disk in /sys/block/sd*; do
     echo "Disk: \${disk##*/}, Scheduler: \$(cat \$disk/queue/scheduler)"
   done
   ```
   *Expected Output Format Summary:*
   ```text
   Disk: sda, Scheduler: [mq-deadline] kyber bfq none
   ```
   *(The explicit word encapsulated in brackets `[...]` identifies the system's active runtime I/O scheduler).*

#### Subtask 1.3: Monitor I/O Statistics
1. Initialize a dynamic real-time kernel tracking stream to observe system disk transfer performance metrics:
   ```bash
   iostat -x 2
   ```

📌 **Critical Storage Metrics to Evaluate**:
*   `%util`: Percentage of CPU core cycles spent handling active I/O requests. Scores near 100% flag storage resource saturation.
*   `await`: The absolute average time (in milliseconds) required for block requests to be completely served from queue to disk.
*   `svctm`: The specific internal service time spent explicitly executing raw hardware transfers.

---

### Task 2: Change I/O Scheduler
**Objective**: Implement volatile runtime algorithmic alterations and author persistent hardware configuration policies.

#### Subtask 2.1: Change Scheduler Temporarily
1. Force an on-demand runtime queue override by piping the target algorithm directly into the block path descriptor wrapper:
   ```bash
   echo 'mq-deadline' | sudo tee /sys/block/sda/queue/scheduler
   ```
2. Verify that the runtime memory alteration updated instantly across active tracking tables:
   ```bash
   cat /sys/block/sda/queue/scheduler
   ```

💡 **Privilege Alert**: Piping statements using simple redirect symbols (`>`) can drop out with permission exceptions even when using `sudo`. Always route text assignments through the `tee` binary utility to ensure safe privilege elevation across kernel parameters.

#### Subtask 2.2: Make Changes Persistent
1. Initialize a clean custom device node assignment policy file inside the dynamic manager registry tree:
   ```bash
   sudo nano /etc/udev/rules.d/60-io-schedulers.rules
   ```
2. Insert a declarative rule string targeting your disk, mapping your desired production performance algorithm:
   ```text
   ACTION=="add|change", KERNEL=="sda", ATTR{queue/scheduler}="mq-deadline"
   ```
3. Commit adjustments, drop out of the editor, and command the device configuration subsystem to instantly parse, compile, and reload its rules:
   ```bash
   sudo udevadm control --reload-rules
   sudo udevadm trigger
   ```

---

### Task 3: Test Different I/O Schedulers
**Objective**: Match workloads to optimized scheduling models and benchmark performance under synthetic stress tests.

#### Subtask 3.1: Common Scheduler Types Overview



| Scheduler Profile Token | Target Architectural Design & Optimized Production Use Case |
| :--- | :--- |
| `mq-deadline` | Default baseline; prevents starvation by enforcing strict execution deadlines. Optimal for generic **Server Workloads**. |
| `bfq` (Budget Fair Queueing) | Multi-stream balancer prioritizing latency reduction. Optimal for interactive **Desktop Systems**. |
| `kyber` | Fast multi-queue engine designed to minimize request latencies. Optimal for high-speed **SSDs / NVMe Storage**. |
| `none` | Bypasses kernel processing layers entirely. Optimal for virtualized guests or specialized hardware controllers. |

#### Subtask 3.2: Basic Performance Testing
1. Spin up a massive 1 Gigabyte sequential dataset chunk file block to act as a localized transfer testbed:
   ```bash
   dd if=/dev/zero of=testfile bs=1M count=1024 conv=fdatasync
   ```
2. **Clear Volatile Memory Caches**: Drop background caches before testing to guarantee subsequent reads are forced directly from physical disk media rather than RAM:
   ```bash
   echo 3 | sudo tee /proc/sys/vm/drop_caches
   ```
3. Run a timed reading transfer execution loop, tracking real-time delivery performance speeds:
   ```bash
   time dd if=testfile of=/dev/null bs=1M
   ```
   *(Repeat this test step across varying active scheduler types to record and evaluate raw performance changes).*

#### Subtask 3.3: Workload-Specific Testing
1. **Simulate Random-Write Database Activity**: Unleash a multi-threaded parallel random-write test sequence using the Flexible I/O Tester (`fio`):
   ```bash
   fio --name=random-write --ioengine=posixaio --rw=randwrite --bs=4k \
       --size=1g --numjobs=1 --iodepth=1 --runtime=60 --time_based --end_fsync=1
   ```
2. **Simulate Sequential Web Data Streaming**: Shift benchmarking profiles to run thick sequential read pipelines:
   ```bash
   fio --name=seq-read --ioengine=posixaio --rw=read --bs=1M \
       --size=1g --numjobs=1 --iodepth=32 --runtime=60 --time_based
   ```
   *Analysis Focus:* Compare total **IOPS** (I/O Operations Per Second) and raw bandwidth metrics output by the benchmarks to discover the perfect optimization mapping for your physical hardware.

---

## 🏁 Conclusion
During this lab session, you developed crucial performance tuning and storage engineering capabilities, including:
- Identifying active multi-queue block layer scheduling components.
- Overriding storage queue behaviors transiently and hardcoding persistent parameters using `udev` automation rules.
- Benchmarking file operations metrics via `iostat` performance grids.
- Constructing database and streaming load stress testing pipelines using `fio`.

---

## 🧹 Cleanup
Maintain absolute hygiene across your deployment machine by discarding the large, synthetic performance testing dataset block file:
```bash
rm -f testfile
```

---

## 📚 Additional Resources
- Local documentation lookups: `man udev`, `man iostat`, `man fio`
- Linux Kernel Storage Tree Specifications: `/sys/block/<device>/queue/`
