### Incident 1: Windows 11 input failure > partition setup
Date: 5-May-2026
Status: Resolved

- Problem:
After opening and starting the laptop after almost 3 years yesterday, I had removed many of the games and other apps I had on the laptop, freeing up 200+ gb of storage. After that and speeding up the pc through basics, like startup apps, cache clearing etc, I had left it alone.

This morning when trying to turn on and boot the laptop, for some reason keyboard and trackpad inputs were not getting detected by Windows 11, external device did not work either. Making normal operation impossible

- Diagnosis:
  * Ruled out hardware failure - the keyboard and trackpad worked within the bios screen
  * Ruled out USB Port failure - multiple ports tested within bios and had no issues
  * The fault was in the Windows 11 software/driver layer.

- Fix:
I did not need Windows 11 to work for this project, but i couldn't get rid of it for now due to have personal files and data on the drive.

So instead I:
* Forced shutdown 3 times to trigger Windows Recovery.
* Navigated through it to access Command Prompt
* Using command prompt I had partitioned drive to install ubuntu onto

Commands used:

```bash
diskpart
list disk
select disk 0
list partition
shrink desired=256000
create partition primary
format fs=ntfs quick
assign
exit
```

- Result:
250gb partition created and ready for ubuntu server install, also confirmed that the i/o device issue was software not hardware issue. I will keep Windows temporarily as it won't affect what I want to achieve with home server project for now, and in the background try to fix the software/driver issue.



<img width="4000" height="3000" alt="92431" src="https://github.com/user-attachments/assets/503975bd-ee21-4eed-bc2e-b9401ed06c96" />

<img width="4000" height="3000" alt="92433" src="https://github.com/user-attachments/assets/239dbe78-4ced-48d4-b575-d0d5a8524664" />

<img width="4000" height="3000" alt="92435" src="https://github.com/user-attachments/assets/bf0206b7-76cc-4dfc-a177-82044c601977" />

<img width="4000" height="3000" alt="92435" src="https://github.com/user-attachments/assets/d94139ce-2900-46da-a3ec-7d46793197d4" />



- What I learned:
  * BIOS-level input working while Windows fails means its software/driver issue
  * Windows recovery mode, allows full adminstrator level access to the command line
  * Always try to isolate which layer a failure is in before trying to fix

---
[Back to README](./README.md) | [Next Incident →](./incident-02-ubuntu-wifi-setup.md)
