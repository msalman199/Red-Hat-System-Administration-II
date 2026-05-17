# Managing Logical Volumes with LVM

This repository contains a comprehensive step-by-step guide for managing storage using Logical Volume Manager (LVM) on Linux systems.

## 🎯 Objectives
By the end of this lab, you will be able to:
* **Create & Manage Storage:** Initialize Physical Volumes (PVs) and Volume Groups (VGs).
* **Allocate & Resize:** Dynamically create and extend Logical Volumes (LVs).
* **Backup & Recover:** Utilize LVM snapshots for data recovery.

## 📋 Prerequisites
* **OS:** Linux-based system (RHEL, CentOS, or Fedora recommended).
* **Access:** Root or `sudo` privileges.
* **Storage:** Unallocated disk space or additional virtual/physical disks.
* **Skills:** Basic familiarity with the Linux command line.

---

## 🛠️ Step-by-Step Implementation

### Task 1: Creating Physical Volumes and Volume Groups

#### 1.1 Identify Available Disks
Locate the unallocated block devices on your system:
```bash
lsblk
```
* **Expected Output:** Lists available block devices (e.g., `/dev/sdb`, `/dev/sdc`).

#### 1.2 Create Physical Volumes
Initialize the identified disks into LVM Physical Volumes (PVs):
```bash
sudo pvcreate /dev/sdb /dev/sdc
```
Verify the creation:
```bash
sudo pvs
```
* **Expected Output:** Displays initialized PVs with their attributes and sizes.

#### 1.3 Create a Volume Group
Combine your physical volumes into a single logical pool named `my_vg`:
```bash
sudo vgcreate my_vg /dev/sdb /dev/sdc
```
Verify the creation:
```bash
sudo vgs
```
* **Expected Output:** Shows `my_vg` aggregating the storage capacity of both PVs.

---

### Task 2: Allocating and Resizing Logical Volumes

#### 2.1 Create a Logical Volume
Slice a 5GB allocation from your volume group pool:
```bash
sudo lvcreate -L 5G -n my_lv my_vg
```
Verify the creation:
```bash
sudo lvs
```
* **Expected Output:** Shows `my_lv` with a designated size of 5GB.

#### 2.2 Format and Mount the LV
Prepare the volume with an `ext4` filesystem and attach it to the directory tree:
```bash
sudo mkfs.ext4 /dev/my_vg/my_lv
sudo mkdir -p /mnt/mylv
sudo mount /dev/my_vg/my_lv /mnt/mylv
```
Verify operational mounting:
```bash
df -h /mnt/mylv
```
* **Expected Output:** Displays a mounted file system showing 5GB capacity.

#### 2.3 Extend the Logical Volume
Increase the capacity of your volume dynamically by 2GB and scale the underlying filesystem:
```bash
sudo lvextend -L +2G /dev/my_vg/my_lv
sudo resize2fs /dev/my_vg/my_lv
```
Verify the new storage size:
```bash
df -h /mnt/mylv
```
* **Expected Output:** Shows expanded filesystem capacity of approximately 7GB.

---

### Task 3: Creating and Managing Snapshots

#### 3.1 Create a Snapshot
Generate a 1GB point-in-time snapshot of your active logical volume:
```bash
sudo lvcreate -L 1G -s -n my_snapshot /dev/my_vg/my_lv
```
Verify snapshot status:
```bash
sudo lvs
```
* **Expected Output:** Shows `my_snapshot` linked to its origin volume (`my_lv`).

#### 3.2 Restore from Snapshot
Rollback your target volume to the exact moment the snapshot was captured:
```bash
sudo umount /mnt/mylv
sudo lvconvert --merge /dev/my_vg/my_snapshot
sudo mount /dev/my_vg/my_lv /mnt/mylv
```
* **Expected Output:** Target directory filesystem state reverts to its historical snapshot window.

---

## 💡 Troubleshooting Tips
* **Filesystem Errors:** If `resize2fs` encounters errors, verify the volume integrity or temporarily unmount the block device.
* **Missing Disks:** Execute `dmesg | grep sd` or `udevadm trigger` if newly attached virtual hardware is missing from `lsblk`.
* **Snapshot Exhaustion:** Snapshots drop if data changes exceed snapshot capacity. Track pool metrics frequently via `sudo vgs`.

## 🚀 Next Steps
* Learn cleanup procedures with `lvremove`, `vgremove`, and `pvremove`.
* Maximize allocation efficiency by exploring thin provisioning via `lvcreate --thin`.
