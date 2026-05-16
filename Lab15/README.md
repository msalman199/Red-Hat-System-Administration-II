# Adjusting CPU Scheduling with nice and renice

This repository contains a hands-on lab focused on Linux process scheduling, execution resource weights, and CPU core utilization tuning. You will master standardizing CPU prioritization parameters, configuring initial execution offsets using `nice`, adjusting live runtime threads using `renice`, and evaluating task competition metrics within dynamic scheduler configurations.

---

## 🎯 Objectives
By the end of this lab, you will be able to:
- Comprehend the architecture of the Linux CPU scheduler and the mapping of process priority scales.
- Spin up resource-intensive tasks with custom initial execution weights using `nice`.
- Modify the priority allocation parameters of active, live threads using `renice`.
- Monitor process execution trends and troubleshoot compute starvation loops using graphical monitors (`htop`).

## 📋 Prerequisites
- A Linux operating system (Red Hat Enterprise Linux, CentOS Stream, or Fedora recommended).
- Terminal access featuring elevated `sudo` or root administrative privileges.
- Basic familiarity running system command-line interface (CLI) commands.

---

## ⚙️ Setup Requirements
1. Open your terminal window and update local repository metadata caches:
   ```bash
   sudo dnf update -y
   ```
2. Provision and deploy the color-coded interactive visual engine process monitor:
   ```bash
   sudo dnf install htop -y
   ```

---

## 🛠️ Lab Tasks

### Task 1: Setting Process Priority with nice
**Objective**: Understand scheduler priority brackets and launch background tasks with custom constraints.

#### 1.1 Understanding Nice Values
The Linux kernel manages thread weights across a clear numeric grid:
*   **Scale Limits**: Runs from `-20` (**Absolute Highest Priority / Maximum Core Urgency**) up to `19` (**Absolute Lowest Priority / Least Urgency**).
*   **Default Baseline**: New processes inherit a default nice value of `0`.
*   **The Nice Inverse Matrix**: Lower numerical nice scores grant a process a higher share of CPU scheduler attention. Higher nice scores defer resources to other system tasks.

#### 1.2 Launching a Test Process
1. Initialize a highly compute-bound data-churn task into an asynchronous background thread to serve as a workload generator:
   ```bash
   dd if=/dev/zero of=/dev/null &
   ```

#### 1.3 Checking Current Priority
1. Interrogate the process database using a long-listing profile to inspect your background worker's attributes:
   ```bash
   ps -l -p \$(pgrep -n dd)
   ```
   *Expected Output Matrix:* Locate the **NI** column header cell line. The value should reflect the default baseline score of `0`.

#### 1.4 Starting a Process with Custom Priority
1. Launch a secondary compute workload thread, setting a restrictive initialization weight parameter from the start:
   ```bash
   nice -n 10 dd if=/dev/zero of=/dev/null &
   ```
   *Explanation:* This drops the scheduler precedence of the new task, forcing it to act "nicely" to other system applications.

#### 1.5 Verification
1. Re-run an optimization check targeting the newest worker iteration:
   ```bash
   ps -l -p \$(pgrep -n dd)
   ```
   *Expected Output Trace:* The **NI** field registers an explicit value of `10`.

---

### Task 2: Adjusting Priority with renice
**Objective**: Interrogate live process listings and execute mid-flight priority modifications.

#### 2.1 Identifying a Running Process
1. Query the process index table to capture the exact, unique Process IDs (PIDs) running your workloads:
   ```bash
   pgrep -l dd
   ```
   *(Note the specific PID number assigned to your target task trace).*

#### 2.2 Changing the Priority of a Running Process
1. Shift the execution priority metric of your live running process downstream to drop its performance weight:
   ```bash
   sudo renice -n 15 -p <PID>  # Be sure to swap <PID> out for your literal live numerical index target
   ```

#### 2.3 Verification
1. Validate that the running thread shifted properties on-the-fly inside the execution scheduler:
   ```bash
   ps -l -p <PID>
   ```
   *Expected Output Trace:* The active **NI** tracking state updates to `15`.

#### 2.4 Increasing Priority (Requires Root)
1. Elevate a thread's urgency by forcing a high-priority negative integer score onto the PID tracker:
   ```bash
   sudo renice -n -5 -p <PID>
   ```

📌 **Security Privilege Rule**: Standard unprivileged users can only lower the priority of tasks they own (increasing the nice score). Restructuring processes to claim *more* CPU scheduling power (dropping scores below zero or altering other users' tasks) requires elevated administrative privileges via `sudo`.

---

### Task 3: Monitoring CPU Usage Effects
**Objective**: Execute load competition diagnostics and observe scheduler prioritization balancing live.

#### 3.1 Launching the Monitoring Tool
1. Launch the interactive performance tracking dashboard:
   ```bash
   htop
   ```
   *(Toggle your data organization layout grid by pressing `F6` to sort rows explicitly by `CPU%` or `NICE` parameters).*

#### 3.2 Observing Priority Effects
Observe how the Linux kernel favors your background processes based on their assigned nice parameters. Threads carrying negative or lower scores claim significantly larger slices of raw CPU core execution time than low-priority tasks.

#### 3.3 Creating Priority Competition
1. Force an explicit compute race by launching two highly competing background worker processes with opposite scheduler priorities:
   ```bash
   nice -n 19 dd if=/dev/zero of=/dev/null &
   nice -n -5 dd if=/dev/zero of=/dev/null &
   ```
2. Return to your open `htop` dashboard interface window and watch how the kernel automatically restricts the low-priority worker (`19`) to keep the high-priority task (`-5`) running at maximum throughput.

---

## 🏁 Conclusion
During this lab session, you developed crucial performance optimization and systems engineering capabilities, including:
- Mapping process priorities across the twenty-point scheduler scale.
- Spawning performance-tuned tasks using initial `nice` parameter boundaries.
- Modifying resource profiles on live execution lanes utilizing `renice` arguments.
- Triaging core resource starvations graphically within the `htop` process environment.

Managing scheduler weights is a vital production requirement for optimizing core system performance, ensuring critical databases do not choke on generic system tasks, and preventing runaway workloads from crashing host systems.

---

## 🧹 Cleanup
Maintain absolute hygiene across your deployment machine by killing all active workload background tasks:
```bash
pkill dd
```
*(Verify system usage returns to baseline resting states via `htop` or `top`).*

---

## 💡 Troubleshooting Guide



| Log Issue Symptom | Root Cause Verification Method & Resolution Fix |
| :--- | :--- |
| `renice` statement returns failures | Confirm that the target task is still running by checking its PID status via `ps -p <PID>`. |
| Unprivileged access restriction drops | Raising a process's priority requires administrative access. Always prepend target optimization lines with `sudo`. |
| Core processors show 0% utilization lines | If the machine isn't running under load, metrics can read as static. Install and run heavy stress scripts if needed: `sudo dnf install stress -y && stress --cpu 4`. |

---

## 🚀 Next Steps
- Explore advanced kernel architecture containment systems by researching Control Groups (**cgroups**) to isolate CPU cores cleanly.
- Investigate true real-time processing scheduling operations by evaluating the `chrt` binary utility tools.
