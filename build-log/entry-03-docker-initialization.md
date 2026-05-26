### Entry 3: Docker Initialization
Date: 21-May-2026

I am going to be setting up Docker on my server today. Its going to be mainly used to help me self-host multiple websites, like in my plan to self host openwebui to allow me, to have a service which I can use for my local LLM's on my main laptop, without taking up more RAM or resources while running said LLM's. I also want to set it up for personal portfolio, different projects and a minecraft server (if my hardware allows for it).

These are steps I took to setup Docker on my server.

<br>

**Step 1 - Removing old conflicting packages**

Although this is a new install, this is just to remind me in the future when setting up/reinstalling/updating docker to do this. And it is really just good practice. And it's just a simple comamnd as well!

```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

<br>

**Step 2 - Installing Docker**

This is the main part of just installing it, first install the dependencies;



```bash
sudo apt-get update # Refreshes the package list
sudo apt-get install ca-certificates curl # Installs the ca cert tool and curl which used for web requests from cli
```



Next is just everything related to the GPG keys (a cryptographic key pairs which just secure file and makes sure they not tampered with);



```bash
sudo install -m 0755 -d /etc/apt/keyrings # Makes a directory at the location of where trusted GPG keys are stored
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc # Downloads dockers official GPG key from their website, and then saves it into the keyring folder

sudo chmod a+r /etc/apt/keyrings/docker.asc # Makes the key readable by all the users on the system
```



Next to add Docker's repo itself;



```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```



And finally just to actually install;



```bash
# Installing Docker
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```



From this I also found out that Ubuntu installed a new kernal, which I haven't updated to yet, although it doesn't effect this installation process for Docker, I am just going to quickly reboot so it doesn't mess up any future steps I will be doing

<br>

**Step 3 - Config**

Now all that my Ubuntu kernal is up to date, all I have to do is do some basic config to add myself as a user to the Docker group, so I don't have to sudo in each time



```bash
sudo usermod -aG docker $USER
newgrp docker
```

<br>

**Step 4 - Testing**

Last but not least actually testing if it all worked



```bash
docker run hello-world # Returns 'Hello from Docker!'
```



It successfully returned meaning the install was successful

<br>

Images:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bb64e007-161c-4b8d-a02e-d7e979d6c392" />

<img width="1220" height="441" alt="image" src="https://github.com/user-attachments/assets/1f1383fb-69dc-4c30-b412-f62388a5f838" />

<img width="923" height="298" alt="image" src="https://github.com/user-attachments/assets/9e6ffc3f-b431-443c-a1e2-ae71a55cb4d2" />

<img width="1364" height="220" alt="image" src="https://github.com/user-attachments/assets/f685d8a0-c4ff-4e8c-9e35-333422fd679c" />

<img width="1541" height="125" alt="image" src="https://github.com/user-attachments/assets/80c39794-e711-45d6-8edf-db4668500291" />

<img width="1918" height="798" alt="image" src="https://github.com/user-attachments/assets/85659788-38ef-441a-9ec2-936b441cd12b" />

<img width="1919" height="1076" alt="image" src="https://github.com/user-attachments/assets/7fbb2b89-2a8e-4a40-a8dc-813fd71fc2be" />

<img width="904" height="83" alt="image" src="https://github.com/user-attachments/assets/e5f7c94b-f37f-4e80-8163-3d402b7b4a5b" />

<img width="982" height="772" alt="image" src="https://github.com/user-attachments/assets/68224914-a828-4225-85b0-54e0819754f0" />

Next Step: Self-host OpenWebUI and Setup Docling RAG

[Back to README](./README.md) | [Next Entry →](./entry-04-openwebui-docling-setup.md)
