# Home-Server-Project
Just a repo of me documenting my adventure on how to create a low power home seever, from a old laptop i had laying around.

**Hardware:**
- CPU: Intel i7-5600U (2 cores / 4 threads @ 2.6GHz)
- RAM: 12GB DDR3
- Disk: SK Hynix SC300 mSATA 512GB SSD
- GPU: Intel HD Graphics 5500 (integrated)
- Network: Built-in ethernet port confirmed
  
**Goals:**
- [ ] Ubuntu Server installed and SSH accessible
- [ ] Basic webpage served on local network
- [ ] NAS configured (Samba)
- [ ] Jellyfin media server running
- [ ] OpenWebUI self-hosted
- [ ] Server monitoring dashboard (JavaScript)

**Status:** In progress — started May 2026

--- 

## Incident Logs

### Incident 1: Windows 11 input failure > partition setup
Date: 5-May-2026
Status: Resolved

- Problem:
After opening and starting the laptop after almost 3 years yesterday, i had removed many of the games and other apps i had on the laptop, freeing up 200+ gb of storage. After that and speeding up the pc through basics, like statup apps, cache clearing etc, I had left it alone.

This morning when trying to turn on and boot the laptop, for some reason keyboard and trackpad inputs were not getting detected by windows 11, external device did not work either. Making normal operation impossible

- Diagnosis:
  * Ruled out hardware failure - the keyboard and trackpad worked within the bios screen
  * Ruled out USB Port failure - multiple ports tested within bios and had no issues
  * The fault was in the winodws 11 software/driver layer.

- Fix:
I did not need windows 11 to work for this project, but i couldn't get rid of it for now due to have personal files and data on the drive.

So instead I:
* Forced shutdown 3 times to trigger Windows Recovery.
* Navigated through it to access Command Prompt
* Using command prompt I had partitioned drive to install ubuntu onto

Commands used:
diskpart
list disk
select disk 0
list partition
shrink desired=256000
create partition primary
format fs=ntfs quick
assign
exit

<img width="4096" height="3072" alt="92419" src="https://github.com/user-attachments/assets/a5a880bc-8d05-405e-9d51-31d8ba6c329c" />

<img width="4000" height="3000" alt="92426" src="https://github.com/user-attachments/assets/1d1f15b0-6a41-4b97-934d-c1d22677598e" />

<img width="4000" height="3000" alt="92429" src="https://github.com/user-attachments/assets/8e5c4640-6459-48d2-8158-20a01014e5af" />

<img width="4096" height="3072" alt="92420" src="https://github.com/user-attachments/assets/7ada70ed-41d0-43cd-88e4-46bdb71539bc" />


- Result:
250gb partion created and ready for ubuntu server install, also confirmed that the i/o device issue was software not hardware issue. I will keep windows temporaily as it won't affect what I want to achieve with home server project for now, and in the background try to fix the software/driver issue.

- What I learned:
* BIO-level input wokring whiel windows fails means its software/driver issue
* Windows recovery mode, allows full adminstrator level access to the command line
* Always try to isloate which layer a failure is in before trying to fix


## Build Log:

### Entry 1: Project Start
Date: 5-May-2026

Decided to convert a old personal laptop into a low power home server, to help build practical skills while also setting up useful software which i and my entire family can use, e.g. Jellyfin, NAS, OpenWebUi, and hopefully a server monitor dashboard created through javascript

Next step: Flash Ubuntu Sever 24.04 LTS
