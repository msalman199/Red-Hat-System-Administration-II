# Brace Expansion and Globbing

This repository contains a hands-on lab focused on rapid file generation, advanced wildcard manipulation, and structural file system automation. You will master standardizing generation operations using curly brace iterations (`{}`), filtering live filesystems via globbing tokens (`*`, `?`, `[]`), and blending both mechanics to construct complex directory trees and execute bulk migrations.

---

## 🎯 Objectives
By the end of this lab, you will be able to:
- Understand and utilize brace expansion for highly efficient string and file generation.
- Master native shell globbing wildcards to match filename patterns safely and reliably.
- Blend brace expansion generation loop rules with globbing filters for advanced file system restructuring.
- Leverage character ranges and numerical sequence brackets to optimize storage maintenance routines.

## 📋 Prerequisites
- A Linux-based operating system (Ubuntu 22.04 LTS or any modern distribution).
- Basic familiarity running system command-line interface (CLI) utilities.
- An environment running Bash shell version 5.0 or newer.

---

## ⚙️ Setup Requirements
1. Open your terminal application window.
2. Initialize and move your session directly into an isolated practice environment folder path:
   ```bash
   mkdir lab4_brace_globbing && cd lab4_brace_globbing
   ```

---

## 🛠️ Lab Tasks

### Task 1: Brace Expansion Fundamentals
**Objective**: Generate arbitrary string combinations and mass-produce directory structures instantly using internal expansions.

#### Subtask 1.1: Basic Brace Expansion
Brace expansion works by evaluating comma-separated lists or incremental range steps inside curly brackets to spit out arbitrary sequence strings.
1. Evaluate an incremental numerical integer sequence step using a pure echo block:
   ```bash
   echo file_{1..5}.txt
   ```
   *Expected Output String Matrix:*
   ```text
   file_1.txt file_2.txt file_3.txt file_4.txt file_5.txt
   ```

#### Subtask 1.2: Create Multiple Files
1. Batch create multiple structured log configurations at once by nesting text variables inside your braces:
   ```bash
   touch report_{jan,feb,mar}_2023.log
   ```
2. Run a filtered query list check to confirm the file creation matches the pattern specifications:
   ```bash
   ls -l report_*.log
   ```

💡 **Troubleshooting**: If file generations drop out or throw errors, inspect your current path workspace permissions to guarantee your user profile holds active write authorization.

---

### Task 2: Globbing Patterns
**Objective**: Filter active folder systems using native wildcard symbols to isolate specific targets.

#### Subtask 2.1: Basic Globbing
Globbing patterns look up filenames directly matching criteria via file system filters.
1. Isolate and display *only* those objects carrying a `.log` extension map:
   ```bash
   ls *.log
   ```
2. Narrow your query scope to scan for records that begin with the exact phrase "report":
   ```bash
   ls report_*
   ```

#### Subtask 2.2: Advanced Pattern Matching
1. Extract and map assets containing integer digits within a set window criteria range:
   ```bash
   ls report_[0-9]*.log
   ```
2. Target localized character matches by supplying a list array boundary constraint:
   ```bash
   ls report_[abc]*.log
   ```

📌 **Key Concept**: The standard `?` glob token acts as an explicit wildcard space matching exactly **one** arbitrary character string slot. The `*` token acts as an elastic wildcard boundary matching **zero or more** arbitrary character sets.

---

### Task 3: Combining Brace Expansion and Globbing
**Objective**: Combine expansion mechanisms and filtering tools to process nested layouts and execute bulk actions.

#### Subtask 3.1: Complex File Operations
1. Structure a multi-tier, complex environment development folder setup recursively using a single generation matrix:
   ```bash
   mkdir -p projects/{src,bin,doc}/{web,cli,api}_{dev,prod}
   ```
2. Print out the resulting layout map dynamically using a structural viewer to verify alignment:
   ```bash
   tree projects/
   ```

#### Subtask 3.2: Bulk File Operations
1. Mass-produce a grid of sample temporary text items crossing multiple variables types:
   ```bash
   touch file_{a..d}{1..3}.tmp
   ```
2. Build an isolated backup folder, then route targeted objects cleanly using boundary characters:
   ```bash
   mkdir backup
   mv file_[a-b]?.tmp backup/
   ```
   *Expected Outcome:* The moving loop isolates and relocates *only* the specific files running from `file_a1.tmp` through to `file_b3.tmp`, leaving rows `c` and `d` untouched.

---

## 🏁 Conclusion
During this lab session, you developed crucial shell resource management and automation capabilities, including:
- Generating dynamic text arrays efficiently with brace expansions (`{}`).
- Querying live directory states with accuracy using glob wildcards (`*`, `?`, `[]`).
- Engineering massive folder trees and storage matrix models in a single line.
- Restructuring and cleaning bulk data paths safely using character ranges.

---

## 🚀 Verification Lab Exercise
Test and prove your newly developed command-line automation capabilities by tracking and completing this test problem sequence step-by-step:
1. Generate exactly 10 initial text elements following a precise `data_` convention padded with leading digits.
2. Isolate and list *only* those target entities that end with an **even** integer value.
3. Construct a dedicated subdirectory, then copy all entities containing a `1` or a `3` inside their filename string.

### 🔑 Solution Matrix Key
```bash
# Step 1: Generate the ten test files with leading zeros padding
touch data_{01..10}.txt

# Step 2: Use an explicit range glob check to isolate even integers
ls data_*[02468].txt

# Step 3: Create target storage and match internal character targets
mkdir filtered
cp data_*[13].txt filtered/
```

---

## 💡 Troubleshooting Guide


| Issue Symptom | Root Cause Verification Method & Resolution Fix |
| :--- | :--- |
| `"No matches found"` error | Confirm target files actually exist on disk, or ensure your pattern syntax does not contain accidental spacing. |
| `Permission denied` block | Interrogate the parent path context using directory security diagnostics: `ls -ld`. |
| Unexpected excessive file matching | Tighten validation parameters by defining specific sets (`[a-c]`), or encapsulate string variables in quotes. |

---

## 🚀 Next Steps
- Advance your matching logic capability rulesets by turning on the extended shell pattern tracker system: `shopt -s extglob`.
- Combine range glob structures directly inside shell validation statements (`if` statements).
- Incorporate file production automation loops into your infrastructure setup scripts.

---

## 📚 Additional Resources
- [GNU Bash Manual: Brace Expansion Utilities](https://gnu.org)
- The Linux Documentation Project: Globbing Patterns Reference
- Mendel Cooper's Architecture Guide: [Advanced Bash-Scripting Guide (Chapter 5)](https://tldp.org)
