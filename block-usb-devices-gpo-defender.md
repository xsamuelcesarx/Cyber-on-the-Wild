Title: Block USB Devices on Windows

Author: Samuel Cesar

Role: Cyber Security Professional

Date: March 2026

Description:

Practical walkthrough demonstrating how to secure Windows endpoints by blocking and managing USB devices using enterprise tools such as Group Policy and Microsoft Defender XDR. 

Block All USB Devices on Windows

Check the DOCX to see images to help you understand, it’s easy, not hard. 

Using Windows GPO/GPM

Tools:

- Local Group Policy Editor: gpedit.msc

- Group Policy Management (Active Directory): gpmc.msc

Steps:

1. Open Group Policy Editor or GPO Management.

2. Navigate to:

Computer Configuration → Administrative Templates → System → Removable Storage Access

Next

3. Enable the following policies:

- All Removable Storage Classes: Deny All Access

- Removable Disks: Deny Read Access

- Removable Disks: Deny Write Access

Note: These settings block all USB storage devices by default.

OR

Block USB Devices via GPMC (Windows Server)

1. Open GPMC

   - Press Windows + R, type gpmc.msc, press Enter

2. Create or select a GPO

- Right-click Group Policy Objects -> New

   - Name it: Block USB Devices

3. Edit the GPO

- Right-click the new GPO -> Edit

- Navigate to:

Computer Configuration -> Administrative Templates -> System -> Removable Storage Access

- Enable the following policies:

- All Removable Storage Classes: Deny All Access

- Removable Disks: Deny Read Access

     - Removable Disks: Deny Write Access

4. Link the GPO to an OU

   - In GPMC, right-click the Organizational Unit (OU) where you want the policy applied -> Link an Existing GPO -> select Block USB Devices -> OK

5. Force Group Policy Update (Optional)

- On target computers, run:

gpupdate /force

   - Or wait for automatic policy refresh

6. Verify Policy

   - Test by plugging in a USB drive; it should be blocked for read/write

---------------------------------------------

Block USB devices Via Microsoft Defender XDR

1. Open Microsoft 365 Defender portal (Using E5 trial for learning/testing purposes)

2. Navigate to the Admin Centers tab and select "Security."

3. Open the Microsoft Security Portal (Defender XDR).

4. Go to "Endpoints," then access "Configuration Management."

5. Select Endpoint security policies.

6. In this section of Microsoft Defender XDR Endpoint, you will see policies for three operating systems. 
Select "Windows Policies" and then click "Create New Policy" to begin configuring a new endpoint security policy.

Select platform Windows
Select template Device Control
and click Create.

7. Now we need to configure this policy with a name; I will use “GitHub Walkthrough.”  
Next, we will configure the settings for this policy, focusing on all the configurations that a cybersecurity team would require.

In this section, Microsoft Defender for Endpoint provides a comprehensive set of options to configure all necessary controls for blocking or allowing USB devices.

Example:  
Microsoft Defender for Endpoint allows granular control over removable devices. For instance, you can:  
- Enable scanning of removable drives for threats  
- Deny write access to USB storage per user or computer  
- Deny read access to USB storage per user or computer  
- Allow specific USB connections while blocking all others  
- Block storage cards (SD, microSD)  
- Enforce encryption requirements for USB devices  
- Require user approval for new USB devices  
- Monitor and log all USB device activity for auditing purposes  

This provides a comprehensive and technical way to manage USB access according to organizational security policies.

After configuring all necessary settings to secure the environment, the next step is to assign the policy.

You can target specific groups, such as:
- All users
- Specific device groups (e.g., Sales team)
- Exclude certain groups if required (e.g., HR team)

Once assignments are configured, proceed to review all settings carefully to ensure accuracy and alignment with security requirements.

After clicking "Create" or "Save," the policy will be deployed to the selected endpoints.

Policy Enforcement:
- Policies are typically enforced within a few minutes but may take up to 15–30 minutes depending on device sync status.
- Devices must be onboarded to Microsoft Defender for Endpoint and connected to the network.
- A manual sync can be triggered from the endpoint or via the portal to speed up enforcement.

Verification:
- Check policy status in the Microsoft Defender XDR portal under Endpoint Security Policies.
- Validate on a test machine by inserting a USB device and confirming that the configured restrictions are applied.
- Review logs and alerts to confirm that actions (block/allow) are being enforced correctly.

Final Notes:
- Always test policies on a small group before full deployment.
- Monitor continuously using Defender XDR to detect bypass attempts or anomalies.
- Update policies regularly based on new security requirements or threat intelligence.
