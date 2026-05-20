### Incident 2: Ubuntu Server Wifi Setup
Date: 19-May-2026
Status: Resolved

- Problem:
Was setting up wifi after full Ubuntu installation, for some reason wont let me, gives me error say the permissions for my netplan file are too open. I troubleshooted by trying these commands again;

- Diagnosis
  - sudo netplan apply [x]
  - sudo chmod 600 /etc/netplan/0x-xxx.yaml --> sudo netplan apply [x]
  - sudo chown root:root /etc/netplan/0x-xxx.yaml --> sudo chmod 600 /etc/netplan/0x-xxx.yaml --> sudo netplan apply [x]
  - sudo chown -R root:root /etc/netplan/ --> sudo chmod 600 /etc/netplan/*.yaml --> sudo netplan --debug apply [✓]

  One last check to see if it works
    - ip a
    - Check to see if there is a ip under the wifi interfernce name beside inet, which there is and we are in.

- Fix:
Netplan won't let you apply/save the file with your wifi password and name in it, when it is accessible to everyone, so give only adminstartor/root  permission to see for all the .yaml files and the entire folder. Not too difficult of a fix, but still annoying.

What I Learned:
- netplan can see and detect if you have your wifi password in a file (well for wifi config files)and will not let you save/use your wifi until permissions are set to only adminstrator/root can see them.
- Good to know in the future when changing wifi networks or setting this up again on a different device

[Back to README](./README.md) | [Next Incident →]( )
