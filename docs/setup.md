### Lab | Laptop Setup Instruction Sheet

#### Architecture: Aider Terminal Orchestrator with Local Model & MCP

This document establishes the official workspace standards for the **Parametric Engineering Design & Automation (PEDA)** course. To ensure strict long-term governance, absolute code traceability, and zero external cloud reliance, the entire toolchain is anchored locally within the Windows Subsystem for Linux (WSL2) ecosystem.

---

## 1. Prerequisites and Host System Preparation

Before initialising the Linux subsystem, hypervisor and virtualisation features must be enabled at the host firmware level.

### Step 1.1: Enable Windows Features

Open Windows PowerShell as an Administrator and execute the following command to enable virtualisation and the WSL subsystem features:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

```

*Action Required:* Restart the host machine immediately after execution to finalise firmware allocations.

### Step 1.2: Install Ubuntu 22.04 LTS via WSL2

From an elevated PowerShell terminal, install the targeted LTS Linux distribution:

```powershell
wsl --set-default-version 2
wsl --install -d Ubuntu-22.04

```

Set up the default UNIX username and secure password when prompted.

---

## 2. Linux Environment Configuration & Toolchain

All compilation, model inference, and AI orchestrations run natively inside the Ubuntu environment to guarantee deterministic file-system operations.

### Step 2.1: System Standardisation & Package Updates

Run the standard package synchronisation and install necessary rendering engines and system utilities inside the Ubuntu terminal:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential python3-pip python3-venv git curl libcairo2-dev pkg-config

```

### Step 2.2: Python Runtime and Dependency Management

To enforce rigid governance over programmatic dependencies, install Python virtual environments globally. We standardise on Python 3.11/3.12:

```bash
# Initialise the global workspace directory
mkdir -p ~/peda_workspace/src
cd ~/peda_workspace

# Setup an isolated Python environment
python3 -m venv .venv
source .venv/bin/activate

```

---

## 3. Local Model Inference Base (Ollama & Models)

To achieve 100% data privacy and offline operational stability, a local inference engine is deployed.

### Step 3.1: Install Ollama Engine

Execute the official installation script inside the WSL terminal:

```bash
curl -fsSL https://ollama.com/install.sh | sh

```

### Step 3.2: Pull Targeted Coding and Spatial Layout Models

Pull the optimized weights for coding and structural comprehension tasks:

```bash
# Code generation and edit-block formatting specialist
ollama pull qwen2.5-coder:7b

# General instruction and reasoning specialist
ollama pull llama3.1:8b

```

---

## 4. AI Orchestration Layer & Context Supply (Aider + MCP)

Aider acts as the headless agent engine, monitoring the workspace and handling structural search-and-replace code refactoring without requiring external GUI extensions.

### Step 4.1: Install Aider Orchestrator

Install Aider within the isolated python runtime:

```bash
pip install --upgrade pip
pip install aider-chat

```

### Step 4.2: Local Model Model Context Protocol (MCP) Setup

To inject deep domain knowledge (such as FreeCAD API reference manuals, local standard operating procedures, and Bureau of Indian Standards metrics), configure an MCP local context engine.

Create an infrastructure configuration file inside your project root at `~/peda_workspace/.aider.conf.yml`:

```yaml
# Aider Configuration for Project Titan (PEDA)
model: ollama/qwen2.5-coder:7b
edit-format: diff
git: true
auto-commits: true
attribute-author: true
attribute-committer: true

```

---

## 5. FreeCAD Automation Connectivity Bridge

FreeCAD executes Python macros natively via its embedded interpreter. To maintain un-modified VS Code usage on the host while controlling FreeCAD, the software must be bridged to read workspace scripts.

### Step 5.1: Windows GUI Setup

1. Download and install **FreeCAD (v0.21 or later)** onto the Windows Host.
2. In FreeCAD, navigate to **Tools > Edit Parameters > BaseApp > Preferences > Macro**.
3. Set the `MacroPath` to point directly to your WSL workspace mount point:
`\\wsl.localhost\Ubuntu-22.04\home\<your-user>\peda_workspace\src`

---

## 6. Verification and Smoke Test

To confirm that code generation, git traceability, and execution integrity are completely functional, execute the following validation loop:

```bash
cd ~/peda_workspace
git init

# Launch Aider using the local offline model pipeline
aider --model ollama/qwen2.5-coder:7b

```

Once inside the Aider terminal interface, issue a structured test instruction to ensure the workspace operates as intended:

```text
/add src/
Create a base configuration module named config.py with local Indian coordinates, and a separate script called test_box.py that defines a FreeCAD parametric cuboid of 100mm x 50mm x 50mm using variables from config.py.

```

Verify that Aider successfully creates the files, applies the diff cleanly, and triggers an automated local Git commit for version tracking.

---

### Clarifying Questions for Project Architecture

To begin building out the Phase 1 template infrastructure files, please clarify the following architectural points:

1. **System Configuration Source**: Do you want our base automation scripts to read parameter variables directly from system `.env` flat-files via a Python parser, or should we use a strictly typed central Python data-class file (`config.py`) to manage spatial scaling limits?
2. **FreeCAD TechDraw Templates**: Shall we pre-configure a standard Bureau of Indian Standards (BIS) A4/A3 sheet template layout (.svg) into the local workspace storage repository to support Phase 3 automation operations later on?
