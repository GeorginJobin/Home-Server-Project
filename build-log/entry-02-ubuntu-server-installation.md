### Entry 2: Ubuntu Server Installation
Date: 19-May-2026

(Had to take a break to do some exams, but back at this project)

I have decided to flash Ubuntu Server 26.04 LTS onto this server, to me this is currently one of the server OS's that will allow me to do everything I have set out to do, from an nas, to a minecraft server. Also this OS is variant with long term support hence LTS.

The steps I took to install this OS.

***Step 1 - Downloading ISO and Flashing USB**
- Downloaded Ubuntu Server 26.04 LTS ISO
- Used Rufus 4.13.2316, to flash onto my USB (settings can be seen in the image)
- USB flashed successfully!

<img width="1920" height="1080" alt="Screenshot 2026-05-19 165608" src="https://github.com/user-attachments/assets/91f540ca-2a0c-4c06-82af-d4b7d7e40e72" />

<img width="590" height="784" alt="Screenshot 2026-05-19 165632" src="https://github.com/user-attachments/assets/23fcef5c-d828-497c-9706-cf3b9b302bf3" />



***Step 2 - Turn off Bitlocker**
I wanted to turn off bitlocker on this device just in case I needed to get back into the windows user for old files or troubleshooting, as I don't have access to the Bitlocker key on this device.

I had gone through the Windows Recovery Environment to do this as again my keyboard and trackpad was not working, and also that Microsoft wont allow for bitlocker changes through Windows 11 unless you have Windows 11 Pro or Enterprise. 

- Turn off laptop
- Turn on laptop when boot screen comes on
- Repeat 3 times, and let boot normally on 4th
- Windows Recovery Enviroment pops up

But surprisely the bitlocker was already off for this device, either way these would've been the steps to turn them off;
- Go to cmd, type manage-bde -status
- Find your drive letter
- Type manage-bde off X: (X being your drive letter), and wait

<img width="4000" height="3000" alt="IMG_20260519_171934" src="https://github.com/user-attachments/assets/4ae3b11e-8638-4e20-a400-76108b84c213" />

<img width="4000" height="3000" alt="IMG_20260519_171947" src="https://github.com/user-attachments/assets/9377931a-6fe7-49bb-a4e7-c21ccca1fb60" />



***Step 3 - Booting from USB**
I plugged my laptop into power before flashing, And I have finally removed the battery so it is currently only being powered directly with the power cable.

<img width="4096" height="3072" alt="IMG_20260519_171329" src="https://github.com/user-attachments/assets/1473ac23-54dc-4bfe-8866-ff363c87509b" />

<img width="4096" height="3072" alt="IMG_20260519_173733" src="https://github.com/user-attachments/assets/0d7e44bb-4efa-4243-855e-66d0dd2a6e74" />



- Plug in USB and turn on laptop
- Spam F12 until boot menu comes up

In my case it opened a legacy boot menu from Dell, which I chose "UEFI: Sandisk", which was my drive.

<img width="4096" height="3072" alt="IMG_20260519_172924" src="https://github.com/user-attachments/assets/40d11ce5-dd10-4e7a-a06a-1400d34a7b96" />


Then it brought me to GRUB, the linux boot manager, and I selected "Try or Install Ubuntu Server", which it started the setup for.

<img width="4000" height="3000" alt="IMG_20260519_172944" src="https://github.com/user-attachments/assets/3cb81d8e-cda1-43aa-9bed-ca1f6ca84521" />

<img width="4096" height="3072" alt="IMG_20260519_173935" src="https://github.com/user-attachments/assets/8848fcf3-7654-461b-a1e4-74cd6ba8e069" />



***Step 4 - Configure Ubuntu Server**
Next was the Ubuntu Server Config.

- Language: English
- Keyboard: English (US)
- Network Configuartion: Continue without network
(currently having some ethernet issues, so skipped for now)
- Proxy Configuartion: Skipped
- Mirror Configuartion: Default
- Custom Layout Storage: Custom storage layout;
  
I used this to be able use my partition I made for this server instead of the whole drive.
-- I reformatted the partion as ext4
-- Mounted it to be / (or root)
-- Confirmed the destructive action

- Profile Configuration: Just normal naming and passwording things
- Ubuntu Pro: Skipped
- SSH configuration: Installed openssh server

And we are finished the install, I just clicked done, finished the full install and removed the usb when prompted

Next steps where to just reboot, go back into boot menu and open Ubuntu (have realised I need to change the boot order soon, to stop this issue)

Couple more cofiguartion stuff later we are officially into Ubuntu server, just need to login, save my ip address

Final things I need to do, due to me using a laptop is the systemd, so I can turn off when the option lid is closed that it doesn't turn off the laptop;
- sudo nano /etc/systemd/logind.conf
- Remove '#' from;
-- HandleLidSwitch
-- HandleLidSwitchExternalPower
-- HandleLidSwitchDocked
-- LidSwitchIgnoreInhibited
- Change 'suspend' to 'ignore' on;
-- HandleLidSwitch
-- HandleLidSwitchExternalPower
- Change 'yes' to 'no'
-- LidSwitchIgnoreInhibited
- Save and exit

Connect to wifi;
- ls /sys/class/net
- Save 'w' name
- ls /etc/netplan
- Save '0x-xxx.yaml' file (different for everyone)
- sudo nano /etc/netplan/0x-xxx.yaml
- and then set this up
```yaml
network:
  version: 2
  renderer: networkd
  wifis:
    YOUR_WIFI_INTERFACE_NAME:
      dhcp4: true
      access-points:
        "YOUR_WIFI_NAME":
          password: "YOUR_WIFI_PASSWORD"
```

- Save and exit
- Had errors applying doing the command below ([incident log 2](../incident-log/incident-02-ubuntu-server-wifi-setup.md))
- sudo netplan apply
- And we are in
- Test by ping -c 3 google.com, and returns "64 bytes from..."
- And it works

And finally connect via SSH:

- sudo reboot
- Spam f2 to go into setup
- Change boot config (got automatically changed at some point)
- And load into the OS
- Close lid
- Open CMD on my page and connect via ip

And I am fully done with the setup of Ubuntu Server and SSH!

Images:
<img width="4000" height="3000" alt="0" src="https://github.com/user-attachments/assets/94748961-9406-4b4f-afa3-7c593ea7e08f" />

<img width="4096" height="3072" alt="1" src="https://github.com/user-attachments/assets/dba61fa5-cf3e-402e-a559-8e2a27ef81ac" />

<img width="4096" height="3072" alt="2" src="https://github.com/user-attachments/assets/43e7fddf-616d-474b-aada-b6e9a677366a" />

<img width="4096" height="3072" alt="3" src="https://github.com/user-attachments/assets/16564674-1820-4e04-9e4a-e7261e2a8c67" />

<img width="4000" height="3000" alt="4" src="https://github.com/user-attachments/assets/2d7c81d8-2d8d-4737-8a6a-e4d865f62f31" />

<img width="4096" height="3072" alt="5" src="https://github.com/user-attachments/assets/caab9a20-7caa-4445-a8b5-5449fa005faa" />

<img width="4096" height="3072" alt="6" src="https://github.com/user-attachments/assets/318040f5-6b42-4ca8-bce7-9705ea57c451" />

<img width="4096" height="3072" alt="7" src="https://github.com/user-attachments/assets/d6074aca-6f56-42ef-8c6b-cf557342e3ba" />

<img width="4000" height="3000" alt="8" src="https://github.com/user-attachments/assets/20dc21ce-86c8-4ec9-b177-907a58f0c807" />

<img width="4000" height="3000" alt="9" src="https://github.com/user-attachments/assets/263b6328-6376-4ec3-a377-09a08c65624d" />

<img width="4000" height="3000" alt="10" src="https://github.com/user-attachments/assets/0ce40eeb-f191-405b-a1ec-538404cac7bb" />

<img width="4096" height="3072" alt="11" src="https://github.com/user-attachments/assets/6e7aca1c-ba36-488b-a320-50243df4d04d" />

<img width="4000" height="3000" alt="12" src="https://github.com/user-attachments/assets/c3be69f8-c0c2-4904-bdb5-f5eb1bd5ff87" />

<img width="4096" height="3072" alt="13" src="https://github.com/user-attachments/assets/c2260278-7635-4dd1-a77f-462f1328ac7a" />

<img width="4096" height="3072" alt="14" src="https://github.com/user-attachments/assets/c7076235-653a-4223-a3fc-01b05d3b3bbf" />

<img width="3000" height="2594" alt="15" src="https://github.com/user-attachments/assets/552667dd-49fb-4400-9834-a8bba6748163" />

<img width="3072" height="4096" alt="16" src="https://github.com/user-attachments/assets/f6ee100c-2c0e-4a10-982e-a2e71a787f46" />

<img width="3000" height="4000" alt="17" src="https://github.com/user-attachments/assets/e19045ba-2f6d-4ba9-9a98-22a6ee1a2943" />

<img width="4096" height="3072" alt="IMG_20260519_193525_edit_57553522646011" src="https://github.com/user-attachments/assets/76784905-1670-471a-aa15-c56248de298b" />

[Back to README](./README.md) | [Next Entry →]( )
