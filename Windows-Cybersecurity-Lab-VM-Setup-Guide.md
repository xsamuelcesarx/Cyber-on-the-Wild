**Before starting, I use Oracle VirtualBox as my preferred hypervisor. It minimizes compatibility issues, is lightweight, and is easy to operate.**

Windows VM Setup Recommendation
------------------------------

This section outlines my recommended setup for building a clean, stable, and realistic Windows virtual machine environment for cybersecurity labs, malware analysis, and hands-on practice.

The goal is to ensure optimal performance while minimizing system lag, crashes, and unwanted conflicts that could interfere with analysis activities.

I will document the exact configuration I use, including system resources, VM settings, and optimization techniques to maintain a reliable and efficient lab environment.

Hypervisor Choice:
- I use Oracle VirtualBox as my primary virtualization platform.
- It provides a lightweight, stable, and easy-to-manage environment.
- Reduces compatibility issues and allows fast deployment of analysis VMs.

Key Objectives:
- Maintain a clean and controlled analysis environment
- Avoid performance bottlenecks during dynamic analysis
- Ensure system stability during malware execution
- Support repeatable and safe testing procedures

Additional Notes:
- All lab activities are performed in an isolated Windows VM
- Snapshots are used to restore clean states after each test
- The environment can be configured to simulate realistic user behavior when needed

This setup is designed to support practical cybersecurity exercises while maintaining safety, stability, and efficiency.


Windows ISO Download – Initial Setup
-----------------------------------

The first step is to download the Windows operating system directly from the official Microsoft website.

Procedure:
1. Access the official Microsoft download page
2. Select the desired Windows version
3. Choose the system language

Recommendation:
- Use **English (United States)** as the system language
- This helps reduce compatibility issues with tools, scripts, and documentation (most are standardized in English)

Download Process:
- Select the option to download the installation file in **ISO format**
- The ISO file provides a complete image of the operating system and is the most reliable method for creating a virtual machine

Notes:
- ISO format allows easy integration with virtualization platforms like VirtualBox
- Ensures a clean and controlled installation environment


Initial VM Configuration – Best Practices
----------------------------------------

The key to a clean and error-free Windows installation is to first create a base virtual machine with optimized settings. This base VM can later be cloned and reused for multiple analysis scenarios.

Basic Configuration:

Name:
- Assign a generic and reusable name
- This VM will serve as a “clean baseline” image for cloning and future lab use

Memory (RAM):
- Recommended: Minimum 8 GB
- Windows 10/11 requires significant memory due to background services and system processes
- Using less than 8 GB may cause:
  - Installation failures or freezes
  - System instability
  - Poor performance during analysis

Critical Note:
- Insufficient RAM can impact real-time memory analysis (volatile memory), leading to inaccurate results or system crashes

Storage (Disk):
- Recommended:
  - 50 GB+ for baseline VM
  - 80 GB+ for long-term projects and tool-heavy environments

- Ensures enough space for:
  - Analysis tools
  - Malware samples
  - Logs and memory dumps

Network Configuration:
- Mode: Bridged Adapter
  - Provides a more realistic network environment
  - Allows the VM to behave like a real host on the network

- Promiscuous Mode: Allow VMs
  - Useful for traffic analysis and packet capture

Security Warning:
- When performing malware analysis:
  - Always isolate the environment when required
  - Avoid exposing infected VMs to production networks
  - Prefer Host-Only or Internal Network for safer scenarios

Notes:
- Keep this base VM clean (no malware execution)
- Use snapshots and cloning for each analysis session
- Ensures repeatability, safety, and consistency


Important Installation Tips
--------------------------

1. Avoid Direct ISO Attachment During VM Creation
- Do NOT attach the Windows ISO during the initial VM creation
- First, create the VM with only hardware settings (Name, RAM, Disk, CPU)

- After creation:
  Settings → Storage → Optical Drive → Select Windows ISO

- This prevents boot issues and ensures a cleaner installation process


2. Disable Network Adapter Before Installation
- Go to:
  Settings → Network → Disable Network Adapter

- This prevents:
  - Forced updates
  - Microsoft account requirement
  - Unnecessary dependencies

- Useful for:
  - Malware analysis labs
  - C2 testing
  - Offline environments
  - Faster setup


3. Bypass Network Requirement (Windows 10/11)
- If Windows requires internet during setup:

- Press:
  Shift + F10

- This opens Command Prompt (CMD)

- Execute:
  OOBE\BYPASSNRO

- The system will restart and allow:
  - Offline installation
  - Local account creation

Notes:
- Common technique in lab environments
- Avoid unnecessary updates in controlled analysis setups
- Always create a clean snapshot before executing samples


Post-Installation Essentials & Optimization
------------------------------------------

1. Install Guest Additions (Critical Step)
- Ensures full compatibility and performance
- Improves:
  - Graphics performance
  - Mouse integration
  - Clipboard sharing
  - File transfer

Steps:
1. Start VM
2. Devices → Insert Guest Additions CD Image
3. Open "This PC"
4. Run VBoxWindowsAdditions.exe
5. Complete installation
6. Reboot VM

Benefits:
- Copy & paste between host and VM
- Drag-and-drop support
- Better usability and workflow


2. Enable Clipboard & File Transfer
Settings → General → Advanced

- Shared Clipboard: Bidirectional
- Drag and Drop: Bidirectional

Use Cases:
- Copy commands/scripts
- Transfer tools and samples
- Improve workflow efficiency

⚠ Security Note:
- Be careful when transferring malware
- Use controlled or isolated directories


3. PowerShell Script Execution Policy
Run PowerShell as Administrator:

Temporary:
Set-ExecutionPolicy Bypass -Scope Process

Less secure:
Set-ExecutionPolicy Unrestricted

Recommendation:
- Use "Bypass" for lab scenarios


4. Re-enable Network
Settings → Network → Enable Network Adapter

Allows:
- Tool downloads
- Updates (if needed)
- Controlled external communication


5. Final Recommendation
- Take a snapshot of the clean VM
- Use it as a baseline for cloning
- Ensures:
  - Consistency
  - Safety
  - Fast recovery


Summary:
- Install Guest Additions
- Enable clipboard and file sharing
- Allow PowerShell scripts (temporary)
- Re-enable network when needed
- Always maintain a clean snapshot baseline

- --------------------------------------------

Windows Sysprep – Preparing Cloned VMs

Purpose:
Use Sysprep to reset a cloned Windows VM to a clean state. This ensures the VM is ready for use from zero, with a new user account, PC name, and configuration.
Important: Run only on cloned VMs, never on the reference VM.

CMD Method:
Open Command Prompt as Administrator and run:

sysprep /oobe /generalize /shutdown

Options explained:
- /oobe      Boot into Out-of-Box Experience on next startup
- /generalize Remove unique system info (SID, PC name, logs, hardware identifiers)
- /shutdown  Turn off the VM after Sysprep completes

GUI Method (Click):
1. Open File Explorer and navigate to: C:\Windows\System32\Sysprep\
2. Double-click sysprep.exe
3. In the Sysprep window:
   - System Cleanup Action: Select "Enter System Out-of-Box Experience (OOBE)"
   - Check "Generalize"
   - Shutdown Options: Select "Shutdown"
4. Click OK to start the process

Workflow for Cloned VMs:
1. Clone the Windows VM from the reference image
2. Boot the cloned VM
3. Run Sysprep (CMD or GUI)
4. Boot the VM again
5. Complete OOBE:
   - Create a new local user
   - Assign a new PC name
   - Configure region, language, and network settings
6. Join Active Directory (if applicable)
7. Apply organizational policies / GPOs

Important Notes:
- Sysprep prepares cloned VMs to be clean and ready for use from zero
- Limited to ~3 runs per VM
- Do not run on a VM already joined to a domain
- Always test the VM after Sysprep before production use
- Use only after cloning, never on the original reference VM

Summary:
Sysprep ensures cloned Windows VMs are clean, unique, and ready for deployment, with no leftover settings, SIDs, or user data from the source image.
