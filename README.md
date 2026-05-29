# Home Server Project

Just a repo of me documenting my adventure building a low power home server from an old laptop, covering everything from hardware setup and OS configuration, to self-hosted services, real-life troubleshooting and incidents along the way. 

A personal project by a CS student at TU Dublin to develop real System Administrator skills.

---

**Hardware:**
| Component | Spec |
|-----------|------|
| CPU | Intel i7-5600U (2 cores / 4 threads @ 2.6GHz) |
| RAM | 12GB DDR3 |
| Disk | SK Hynix SC300 mSATA 512GB SSD |
| GPU | Intel HD Graphics 5500 (integrated) |
| Network | Built-in ethernet port & Intel Wireless 7265 (rev 59) WiFi Card |
  
**Goals:**
- [x] Ubuntu Server installed and SSH accessible
- [x] OpenWebUI self-hosted (with Docling RAG)
- [ ] Hermes Agent self-hosted (connected to OpenWebUI)
- [ ] Basic webpage served on local network
- [ ] Jellyfin media server running
- [x] Tailscale Mesh Network/VPN (Basic Setup for on the go access to my server)
- [ ] Server monitoring dashboard
- [ ] NAS configured




**Status:** In progress — started May 2026

--- 

## Latest Build Log Entry:

### Entry 5: Tailscale Remote Access
Date: DD-Mon-2026

I am going to implement Tailscale today. Orginially I had planned to set this up after doing media stuff like Jellyfin. But I forgot a prime problem. I am going on holidays. So I need this asap to be continue working on this project while I am gone.

<br>

**Step 1 - Installing Tailscale on the server**

Tailscale has a really easy and nice, one line install script which handles everything (server-side);

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

<br>

**Step 2 -  Connecting Devices**

Next I downloaded and installed tailscale on my main laptop, login into my account and connect it.

Afterwards, I did command below on my server;

```bash
sudo tailscale up # Connects the server to the Tailscale network
```

This gave me a URL which I used to login/connect my server to my tailscale account. Which now officially connects my server and my laptop together.

And then I got the IP address of my server on the Tailscale network, I will be using this to SSH into later;

```bash
tailscale ip -4
```

<br>

**Step 3 - Sleep Prevention**

Although I already disabled lid behaviours in Entry 02, I just also want to cover any other sleep triggers, which this command does;

```bash
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

<br>

**Step 5 - Testing SSH**

I SSH into my server using my new Tailscale IP for my server, on my mobile data, and it connected successfully!

With this my rushed setup of Tailscale is done!

This is going to allow me to use my server on the go which great.

<br>

Images:

<img width="1713" height="712" alt="image" src="https://github.com/user-attachments/assets/67b49e69-66f8-468e-8070-cf55aa487ac3" />

<img width="1919" height="1026" alt="Screenshot 2026-05-29 185024" src="https://github.com/user-attachments/assets/26073f95-0f75-491a-ad45-bf1a8eae2d88" />

<img width="1198" height="1107" alt="Screenshot 2026-05-29 185656" src="https://github.com/user-attachments/assets/b73602b6-90c8-4241-8d15-a1c4741e94d2" />

<img width="749" height="140" alt="Screenshot 2026-05-29 184617" src="https://github.com/user-attachments/assets/9e0611ae-48a2-4988-8d7f-2ace471191dc" />

<img width="1818" height="914" alt="Screenshot 2026-05-29 185319" src="https://github.com/user-attachments/assets/abedbf41-779a-4b6e-9e01-7b1d16975210" />

<img width="1474" height="141" alt="Screenshot 2026-05-29 190650" src="https://github.com/user-attachments/assets/d88e22dc-b173-45a4-8e53-a1fa6f56b28a" />

<img width="1115" height="1061" alt="Screenshot 2026-05-29 190753" src="https://github.com/user-attachments/assets/34825346-ddd0-446c-901b-f623ec38547e" />

---

## Latest Incident Log

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





