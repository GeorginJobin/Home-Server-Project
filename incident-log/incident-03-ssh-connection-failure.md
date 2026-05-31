### Incident 3: SSH Connection Failure
Date: 22-May-2026

Status: Partially Resolved 

- **Problem:**
  
Was trying to SSH into my server from my laptop and it kept just timing out and refusing to connect, spent a little bit of time troubleshooting.

- **Diagnosis:**
  
  - Checked if the server was and running. [✓]
  - Checked if SSH service was still running. [✓]
  - Checked if I was on the same network. [x]
 
Yeah it ended being kind of a dumb reason for this report. I realised I just wasn't on the same network which is my fault not the server's.

- **Fix:**
  
Connected my laptop back to the same network as my server, and it worked immeadiately. Again not being a fix just me being stupid.

However this did highlight a real issue, I currently have no way to access/connect to my server if I'm away/not on the same network.

- **What I Learned:**
  
  - Always the check the obvious stuff first before assuming something broke
  - I need a way to access my server remotely, not just on my local network
  - After further research will be setting up Tailscale, proably after Jellyfin, as they go hand in hand (see new Readme goals)

---

[← Prev Incident](./incident-02-ubuntu-server-wifi-setup.md) | [Next Incident →](./incident-04-ethernet-connection-failure.md)
