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

This gave me a URL which I used to login/connect my server to my tailscale account.
And then I got the IP address of my server on the Tailscale network;

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


<br>

Images:
<img width="1713" height="712" alt="image" src="https://github.com/user-attachments/assets/67b49e69-66f8-468e-8070-cf55aa487ac3" />





---
[Back to README](../README.md) | [Next Entry →]( )
