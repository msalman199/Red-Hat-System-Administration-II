# Archiving Files with tar

This repository contains a hands-on lab focused on storage consolidation, data compression pipelines, and archive optimization using the standard Linux utility ecosystem (`tar`, `gzip`, `bzip2`). You will master bundling unstructured data nodes, applying alternative algorithmic compression streams, auditing archive headers, and designing automated script-based file consolidation systems.

---

## 🎯 Objectives
By the end of this lab, you will be able to:
- Combine multiple independent system files recursively into a single uncompressed tape archive (`.tar`).
- Implement high-ratio stream data reduction pipelines using both **Gzip** and **Bzip2** utilities.
- Audit, list, and verify internal metadata index trees wrapped inside closed archive blobs.
- Extract targeted assets from raw, `.gz`, and `.bz2` compressed blocks securely.

## 📋 Prerequisites
- A Linux-based operating system distribution (e.g., Ubuntu, RHEL, Fedora, or CentOS Stream).
- Basic working knowledge of standard command-line interface (CLI) operations.
- Core archiving and compression binary packages (`tar`, `gzip`, `bzip2`) pre-installed.

---

## 🛠️ Lab Tasks

### Task 1: Create a Tarball with tar to Archive Files
**Objective**: Package distributed file components securely into a single, unified archive wrapper.

#### Subtask 1.1: Create Sample Files
1. Open your terminal window and initialize an isolated, clean directory path for testing:
   ```bash
   mkdir lab_files && cd lab_files
   ```
2. Mass-produce simple text files to act as placeholder data blocks:
   ```bash
   touch file1.txt file2.txt file3.txt
   echo "This is file1" > file1.txt
   echo "This is file2" > file2.txt
   echo "This is file3" > file3.txt
   ```

#### Subtask 1.2: Create a .tar Archive
1. Combine the placeholder data structures into a single file bundle using the core tape archiver:
   ```bash
   tar -cvf archive.tar file1.txt file2.txt file3.txt
   ```
   - `-c` : **Create** a brand new archive container file.
   - `-v` : **Verbose** mode (outputs file tracking streams dynamically onto your prompt).
   - `-f` : **File** designation (explicitly states the target archive filename target).

#### Subtask 1.3: Verify the Archive
1. Inspect the internal indexing file parameters without extracting the elements onto your active folder space:
   ```bash
   tar -tvf archive.tar
   ```
   *Expected Output Matrix:* Returns a permissions matrix, file owner indices, file size in bytes, timestamps, and matching target file names.

---

### Task 2: Compress the Tarball with gzip or bzip2
**Objective**: Shrink storage resource footprints by channeling raw containers through data reduction algorithms.

#### Subtask 2.1: Compress with gzip
1. Run a standard Lempel-Ziv (`LZ77`) reduction stream directly over the raw archive container:
   ```bash
   gzip archive.tar
   ```
   *(Note: This operation shrinks the file and appends the `.gz` extension, automatically dropping the uncompressed parent file).*
2. Verify the size metrics optimization of the resulting compressed archive:
   ```bash
   ls -lh archive.tar.gz
   ```

#### Subtask 2.2: Compress with bzip2 (Alternative)
1. Re-bundle your source files to recreate the uncompressed file layer wrapper:
   ```bash
   tar -cvf archive.tar file1.txt file2.txt file3.txt
   ```
2. Process the archive through a high-efficiency Burrows-Wheeler block-sorting reduction pipe:
   ```bash
   bzip2 archive.tar
   ```
3. Run a baseline cross-comparison check measuring the space efficiency differences between the algorithms:
   ```bash
   ls -lh archive.tar.*
   ```
   *Expected Outcome:* Both `archive.tar.gz` and `archive.tar.bz2` are inventoried. Generally, Bzip2 provides smaller final file sizes but demands higher CPU compute overhead during processing.

---

### Task 3: Extract and List Files from an Existing Tar Archive
**Objective**: Decompress and restore archival containers back into matching local directory nodes.

#### Subtask 3.1: Extract a .tar Archive
1. Extract a raw, uncompressed archive package:
   ```bash
   tar -xvf archive.tar
   ```
   - `-x` : **Extract** content files from an archive.

#### Subtask 3.2: Extract a .tar.gz Archive
1. Instruct the archiver to dynamically invoke a Gzip extraction pass directly during the unpacking run:
   ```bash
   tar -xzvf archive.tar.gz
   ```
   - `-z` : Decompress the stream using **gzip** engines automatically.

#### Subtask 3.3: Extract a .tar.bz2 Archive
1. Instruct the archiver to dynamically route the stream through a Bzip2 decompressor sequence:
   ```bash
   tar -xjvf archive.tar.bz2
   ```
   - `-j` : Decompress the stream using **bzip2** engines automatically.

---

## ⚡ Additional Automation Practice (Optional)
To relate these bundling skills to production tasks (such as automated log compilation), construct an automation shell script named `backup_logs.sh`:
```bash
#!/bin/bash
# Automatically bundle and compress all logs inside the system log pool
tar -czvf logs_archive.tar.gz /var/log/*.log
echo "Logs archived and compressed successfully!"
```

---

## 🏁 Conclusion
During this lab session, you developed crucial data maintenance and storage optimization capabilities, including:
- Utilizing the `tar` ecosystem to package multiple independent items cleanly.
- Implementing file compression architectures via `gzip` and `bzip2` modifiers.
- Querying internal archive directory schemas directly via text validation flags (`-t`).
- Scaling data operations inside shell scripts to automate bulk host maintenance.

Consolidation mechanics form the operational bedrock for maintaining tidy filesystems, bundling log data pools, transferring storage blocks across networks, and creating deployment layers for containers inside enterprise cloud native clusters like Red Hat OpenShift.

---

## 💡 Troubleshooting Guide

*   **"No such file or directory"**: Verify your absolute terminal path positioning utilizing `pwd`. Ensure that all target component paths are spelled exactly right.
*   **Permission Exceptions During Processing**: Interrogating or backing up system paths (e.g., `/var/log`) requires elevated security scopes. Prepend your commands using `sudo`.
*   **Corrupt Archive Error Alerts**: Mismatched flags (e.g., trying to read a bzip2 container using a gzip `-z` flag) will throw terminal errors. Always match compression flags precisely (`-z` for `.gz`, `-j` for `.bz2`).

---

## 🚀 Next Steps
- Reinforce your understanding by experimenting with alternative compression tools like **XZ** (using the `-J` parameter flag inside tar).
- Explore maximum data compression tuning profiles by experimenting with explicit compression scales (e.g., running `gzip -9`).
