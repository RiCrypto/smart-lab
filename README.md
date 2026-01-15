# smart-lab


# Cloud Smart-Lab Tutorial

## Overview
This tutorial guides you to create a **Cloud Smart-Lab** using **GitPod** — a reproducible cloud environment ideal for researchers who code, automate, and experiment with AI and process control models.

---

## 1. Prerequisites
Before starting, ensure you have:
- A **GitHub** account.
- A **GitPod** account (sign in with GitHub at https://gitpod.io).
- Basic familiarity with Git, Python, and Docker.

---

## 2. Create Your Project Repository
1. Go to https://github.com/new
2. Create a new public or private repository, for example: `smart-lab`.
3. Clone it locally or open GitHub’s web editor to add the following files.

Your structure should be:
```
smart-lab/
├── .gitpod.yml
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── notebooks/
│   └── example.ipynb
└── data/
```

---

## 3. Configuration Files

### 3.1 `.gitpod.yml`
This file configures GitPod’s environment setup.
```yaml
image:
  file: Dockerfile

tasks:
  - name: Start Smart-Lab
    init: |
      pip install -r requirements.txt
      docker compose up -d
    command: |
      echo "Smart-Lab ready! Access n8n at port 5678 or use Python in VS Code."
ports:
  - port: 5678
    onOpen: open-preview
    name: n8n
  - port: 8888
    onOpen: ignore
    name: Jupyter
vscode:
  extensions:
    - ms-python.python
    - ms-toolsai.jupyter
    - ms-azuretools.vscode-docker
```

### 3.2 `Dockerfile`
Defines your base Ubuntu image with Python, Docker, and AI libraries.
```Dockerfile
FROM gitpod/workspace-full:latest

USER root
RUN apt-get update && apt-get install -y     docker.io docker-compose &&     apt-get clean

RUN pip install --upgrade pip &&     pip install pandas numpy matplotlib seaborn scikit-learn jupyterlab

RUN usermod -aG docker gitpod
USER gitpod
```

### 3.3 `docker-compose.yml`
Runs **n8n** workflow automation inside your lab.
```yaml
version: "3.1"

services:
  n8n:
    image: n8nio/n8n:latest
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=gitpod
      - N8N_BASIC_AUTH_PASSWORD=gitpod123
      - GENERIC_TIMEZONE=America/Vancouver
    volumes:
      - ./data/n8n:/home/node/.n8n
```

### 3.4 `requirements.txt`
Python libraries for AI and process control.
```
pandas
numpy
matplotlib
seaborn
scikit-learn
jupyterlab
requests
pyyaml
```

---

## 4. Launching the Smart-Lab
1. Commit and push all files to GitHub.
2. Open your lab in GitPod:
   ```
   https://gitpod.io/#https://github.com/YOUR_USERNAME/smart-lab
   ```
3. GitPod builds your workspace.
4. Wait 1–2 minutes for setup.
5. Once ready:
   - Access n8n at port 5678 (opens automatically).
   - Use Python or Jupyter notebooks in the editor.

---

## 5. Persistent Data
All your workflows and datasets under the `/data` directory are saved between sessions.
You can also export n8n workflows or sync with Git for version control.

---

## 6. Extending the Smart-Lab
You can enhance your lab with:
- **PostgreSQL** for logging experiment data.
- **JupyterLab** as a browser-accessible data science interface.
- **AI libraries** (TensorFlow, PyTorch) for advanced modeling.
- **API connectors** to link with external systems or cloud databases.

---

## 7. Best Practices
- Keep workflows versioned in Git.
- Export n8n flows regularly for backup.
- Use `.env` files for sensitive credentials.
- Run heavy models on external compute (AWS, Paperspace, etc.).
- Deploy validated n8n flows to Railway or n8n.cloud for production.

---

## 8. Conclusion
You now have a **Cloud Smart-Lab** — a reproducible, Linux-based, Docker-powered environment for coding, automation, and AI experimentation. It combines the flexibility of n8n workflows with Python-based machine learning tools, making it ideal for scientific and industrial process applications.

Enjoy your cloud-powered research workspace!
