### Entry 4: OpenWebUI + Docling RAG Setup
Date: 26-May-2026

The next thing I will be adding to my server is OpenWebUI, its a service I have been using for a while with local models from Ollama/LM Studio. The reason I am going to be doing this on my server, is to move the docker/hosting side of this platform to my server to free up more resources on my main pc to actually be able to run better models, or the ones I have been using just at high context lengths. Also this will allow me to access this site 24/7, and anywhere (after I setup Tailscale). What I will be doing is hybird setup, with OpenWebUI of course running on my server, while I have my models running on my main pc. I'll also be adding the Docling RAG tool, to give me extra functionality with my base models, for parsing and other tasks I will be doing.

**Step 1 - Creating the Docker Compose file**

Rather than running all the containers indivually, I used Docker Compose so both OpenWebUI and Docling can run together and talk to each at the same time on the same Docker network. These are the steps I took to make this;

```bash
# Creates and enters a new folder called openwebui
mkdir ~/openwebui 
cd ~/openwebui

nano docker-compose.yml # Creates the Docker compose file
```

Within file I added this into it;

```bash
services:  
  open-webui:
    image: ghcr.io/open-webui/open-webui:main  # Pulls the premade OpenWebUI package from their GitHub container registry
    container_name: open-webui
    ports:
      - "3000:8080"  # Maps port 3000 within the docker file to port 8080, esstenially port forwarding the 'server_IP:3000' to 8080 which is where OpenWebUI runs
    volumes:
      - open-webui:/app/backend/data  # The location to hold OpenWebUI data, like chats and uploaded documents
    environment:  # Tells OpenWebUI to find Ollama on my main pc/laptop, and to enable to API
      - OLLAMA_BASE_URL=http://LAPTOP_IP:11434  
      - ENABLE_OLLAMA_API=true
    extra_hosts:
      - "host.docker.internal:host-gateway"  
    networks:  # Puts the contaienr on the shared network, and just makes sure it always restarts if the server crashes or restarts
      - webui-net 
    restart: always

  docling-serve:
    image: quay.io/docling-project/docling-serve:latest  # Pulls the latest Docling image, the cpu specific one
    container_name: docling-serve
    ports:
      - "5001:5001"
    environment: 
      - DOCLING_SERVE_ENABLE_UI=true  # Turns on the Docling test UI at port 5001, so you can verify it works
      - DOCLING_SERVE_ENABLE_REMOTE_SERVICES=true  # Allows Doclign to connect to external services like Ollama for stuff like image descriptions inside documents
      - DOCLING_SERVE_MAX_SYNC_WAIT=600  # Limits the max wait time for large documents to 10 mins
      - DOCLING_SERVE_ENG_LOC_NUM_WORKERS=2  # Limits how many documents it can process at once
      - UVICORN_WORKERS=1  # Set to 1 so there is only one worker, and not multple taking up extra memory
      # Controls how many CPU threads the processing libraries can use
      - OMP_NUM_THREADS=4 
      - MKL_NUM_THREADS=4
    networks:  
      - webui-net
    restart: unless-stopped

networks:  # Creates a private internal network, that both containers share. This allows OpenWebUI talk to Docling using just 'http://docling-serve:5001', and Docker automatically resolves the container names to their internal IP's, so no need to hard code them in
  webui-net:
    driver: bridge

volumes:  # Creates a volume for OpenWebUI allowing data to be persistant through restars
  open-webui:
```
