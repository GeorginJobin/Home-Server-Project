### Incident 4: Ethernet Connection Failure
Date: DD-Mon-2026

Status: Unresolved

- **Problem:**

Attempted to connect the server to ethernet for a more stable connection, cable was plugged in but eno1 was just showing NO-CARRIER and state DOWN, it wasn't detecting the cable at all. Spent a good bit of time going through this one.

- **Diagnosis:**

  - Checked ip a - eno1 showing NO-CARRIER and state DOWN [x]
  - Ran sudo ip link set eno1 up - came up but still NO-CARRIER [x]
  - Ran sudo ethtool eno1 - showed Link detected: no [x]
  - Checked dmesg logs - showed the NIC actually flapping, coming up at 100 Mbps then dropping again every 20 seconds [x]
  - Found netplan config split across two files (00-installer-config.yaml and 01-netcfg.yaml), merged them into one [✓]
  - Applied netplan again after merge - still NO-CARRIER [x]

So the hardware and driver are actually fine since the NIC is briefly detecting the cable, its a physical connection issue not a software one.

- **Fix:**

No fix yet. Netplan is now cleaned up and all in one file which is good, but the ethernet is still not staying connected. Most likely causes are a faulty cable, a bad port on the router, or the laptop's ethernet port itself being damaged, which honestly wouldn't be surprising given I had ethernet issues all the way back in Entry 2.

Things to try when I get the chance;
  - Try a different cable
  - Try a different port on the router
  - If both fail, a USB to ethernet adapter (€10) would bypass the built in port entirely

For now its fine, WiFi and Tailscale are both working so its not blocking anything.

- **What I Learned:**

  - When a NIC flaps like that its almost always a physical issue not software
  - Multiple netplan files get applied in filename order, later files override earlier ones, always keep it in one file to avoid conflicts
  - dmesg is really useful for seeing real time hardware events like NIC link changes

---
[← Prev Incident](./incident-03-ssh-connection-failure.md) | [Next Incident →]( )
