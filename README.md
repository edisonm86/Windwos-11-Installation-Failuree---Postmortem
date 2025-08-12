# Windows-11 Installation Failure - Postmortem
## Summary
This postmortem documents the troubleshooting process and resolution of a Windows 11 installation failure on an MSI MAG X570 Tomahawk WiFi motherboard using an NVMe M.2 SSD. The installation consistently failed after the first restart, resulting in boot errors and repair loops. Root cause was identified as a failing M.2 SSD.

---

## Incident Timeline

### **Initial Symptom**
- Windows 11 system failed to boot into the OS.
- Encountered **Startup Repair couldn’t repair your PC** message.
- Log file referenced: `E:\Windows\System32\Logfiles\Srt\SrtTrail.txt`.

**Screenshot:**  
<img width="1470" height="983" alt="image" src="https://github.com/user-attachments/assets/34813c5d-e15f-4ae0-9f60-89d87481a483" />


---

### **First Troubleshooting Attempts**
1. Tried booting into **Safe Mode** — looped back into Startup Repair.
2. Attempted **Reset this PC** — failed with an error about improper shutdown during installation.
3. Began a fresh Windows 11 installation — during setup, converted disk from **MBR → GPT**.

---

### **Secondary Symptom**
After removing the Windows installation USB on first restart, system displayed:

> `Reboot and Select proper Boot device or Insert Boot Media`

**Screenshot:**  
<img width="1230" height="400" alt="image" src="https://github.com/user-attachments/assets/48336fed-cc8c-451e-89c2-bbefb45b9797" />


---

## Root Cause Investigation
- Initially suspected incorrect boot mode (UEFI vs. Legacy/CSM).
- Updated motherboard (MAG X570 Tomahawk WiFi) BIOS to latest firmware (`3/20/2025`, Version `E7C84AMS.1I0`).
- Confirmed M.2 drive was installed correctly and detected in BIOS.
- Verified Boot Mode set to **UEFI**, CSM disabled, and drive visible under NVMe devices.
- Still encountered installation interruption after first restart.

---

## Resolution
- Swapped out problematic M.2 SSD with another NVMe drive.
- Windows 11 installed successfully on replacement drive with no errors.
- Repurposed the original M.2 SSD as a secondary storage drive after confirming BIOS detection.

---

## Post-Resolution Actions
1. **Drive Health Testing**  
   - Ran **CrystalDiskInfo**
   - **chkdsk /f /r** for full surface scan.

2. **System Configuration Verification**  
   - UEFI boot confirmed.
   - Windows Boot Manager set as primary boot device in BIOS.

---

## Lessons Learned
- A failing NVMe drive can cause installation interruptions even when detected by BIOS.
- Always verify drive health before OS installation on suspect media.
- Removing the USB install media at the correct time is critical to avoid boot loop issues.
- BIOS update can resolve hardware detection issues, but not underlying drive health problems.

---

## Next Steps
- Perform full health check on the faulty M.2 SSD.
- Keep spare bootable USB media for recovery and diagnostics.
