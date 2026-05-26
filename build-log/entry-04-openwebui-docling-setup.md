### Entry 4: OpenWebUI + Docling RAG Setup
Date: 26-May-2026

The next thing I will be adding to my server is OpenWebUI, its a service I have been using for a while with local models from Ollama/LM Studio. The reason I am going to be doing this on my server, is to move the docker/hosting side of this platform to my server to free up more resources on my main pc to actually be able to run better models, or the ones I have been using just at high context lengths. Also this will allow me to access this site 24/7, and anywhere (after I setup Tailscale). What I will be doing is hybird setup, with OpenWebUI of course running on my server, while I have my models running on my main pc. I'll also be adding the Docling RAG tool, to give me extra functionality with my base models, for parsing and other tasks I will be doing.

<br>

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

Now to just start the containers

```bash
docker compose up -d
docker ps
```

<br>

**Step 2 - Exposing Ollama & LM Studio on my main PC**

Ollama need to be exposed to the local netowkr so my server can reach, by default it only detects localhost.

- In enviroment variables create a new variable and name it 'OLLAMA_HOST'
- And set the value to 0.0.0.0

For LM Studio it's even easier;

- Open LM Studio
- Go to the Developer tab
- Load the model than I want to seve
- Turn on Serve on Local Network
- Turn on Enable CORS
- Click Start Server
- And note the port, default is 1234

<br>

**Step 3 - Connect everything in OpenWebUI**

Next step for me is to connect everything on OpenWebUI.

- Go to http://SERVER_IP:3000
- Make an account and login into openwebui (completely local)
- Go to Admin Panel > Settings > Add Connections
- Add URL: http://PC_IP:11434/
- Click verify to ensure its running and save

- For LM Studio, OpenAI > Add Connection
- Add URL: http://PC_IP.3:1234/v1
- API Key: lm-studio
- Save

<br>

**Step 4 - Configure Docling**

Next to finish all of the setup is just to configure Docling within OpenWebUI

- Go to Admin Panel > Settings > Documents
- Context extraction engine > Docling
- Docling URL: http://docling-server:5001
- Embedding model engine > Ollama
- Embedding model > nomic-embed-text
- Save

Then on cmd on my main pc I pulled the embedding model;

```bash
ollama pull nomic-embed-text
```

<br>

**Step 5 - Test Everything**

Now at this point I did realise that I forgot to LM Studio server which I ended up doing, by just flipping the switch beside Status.

All I did now was test that everything worked.

And it did LM Studio and Ollama worked like a charm.

The final bit of testing is to test Docling;

- Upload a document to Workspace > Knowledge > Ask a question using #
- I used this document: [docling_rag_table_test.pdf](https://github.com/user-attachments/files/28276732/docling_rag_table_test.pdf)
- And this prompt: "Convert the table under the section titled "Structured Product Table" into a markdown table exactly as it appears in the document."
- And it worked, once I copy the table generated into notepad, it is in a markdown format, the table is below;

| Product | Category | Price | Stock |
|---|---|---|---|
| Keyboard | Electronics | $49 | 120 |
| Notebook | Stationery | $5 | 340 |
| Coffee Mug | Kitchen | $12 | 85 |
| USB Cable | Accessories | $8 | 210 |

And wallah, everything works, from my OpenWebUI and Docling being hosted on my server, to them connecting to my Ollama and LM Studio from my main pc. And everything working together!
And officially I have completed one of my goals that I wanted to complete when starting this project.

<br>

Images:

<img width="694" height="71" alt="Screenshot 2026-05-26 154812" src="https://github.com/user-attachments/assets/8473aabe-ba8d-4d23-ad17-fb0b41957735" />

<img width="1919" height="1076" alt="Screenshot 2026-05-26 154826" src="https://github.com/user-attachments/assets/dc84440d-8e06-4fd9-af7d-196d6c1d44f1" />

<img width="1911" height="452" alt="image" src="https://github.com/user-attachments/assets/9d6f6a1e-ddda-49fa-b416-7fb7e184a740" />

<img width="1041" height="991" alt="image" src="https://github.com/user-attachments/assets/76380c8f-6adb-4365-960e-9442b2d8002a" />

<img width="1919" height="853" alt="image" src="https://github.com/user-attachments/assets/3d58d0ed-f423-4ee1-9d27-ca2208f453b9" />

<img width="1919" height="1076" alt="image" src="https://github.com/user-attachments/assets/51d0b6ef-5aec-446a-9fde-6003c27b3ad6" />

<img width="1919" height="1020" alt="image" src="https://github.com/user-attachments/assets/e343513b-2026-4f16-95ee-6515290e8af0" />

<img width="1801" height="309" alt="image" src="https://github.com/user-attachments/assets/96845363-de52-4b75-aebf-2ae37f58f00b" />

<img width="1915" height="1025" alt="image" src="https://github.com/user-attachments/assets/1877f3e5-f7f3-4889-b270-1fe6c3a51bba" />

<img width="1919" height="931" alt="image" src="https://github.com/user-attachments/assets/7ae81147-4112-49df-a3ad-ecf3d6816ab7" />

<img width="1576" height="341" alt="image" src="https://github.com/user-attachments/assets/2376638f-f546-4a90-a24e-76a245b8cf9e" />

<img width="1910" height="927" alt="image" src="https://github.com/user-attachments/assets/3d1f0831-144e-4eeb-93ca-06680079c8cc" />

<img width="481" height="275" alt="image" src="https://github.com/user-attachments/assets/8690e0b9-c0c0-4d7d-b60a-9f5b7bfee361" />








[Back to README](./README.md) | [Next Entry →]()



