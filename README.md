# Build a Local AI Email Auto-Responder with n8n and Ollama

## Overview

This project is a fully local AI-powered email auto-responder built using **n8n** for workflow automation and **Ollama** for local LLM inference.

The system automatically:

- Reads incoming emails using IMAP
- Sends the email content to a local AI model running in Ollama
- Generates an intelligent response
- Sends the AI-generated reply back using SMTP

The entire workflow runs locally using Docker containers, ensuring:

- Privacy
- No external AI API costs
- Full control over data and models

---

# Technologies Used

- Docker
- Docker Compose
- n8n
- Ollama
- Phi3 Mini / Llama3
- IMAP
- SMTP

---

# Project Structure

```bash
.
├── docker-compose.yml
├── .env.example
├── workflow.json
├── submission.json
└── README.md
```

---

# Features

- Fully local AI email responder
- Dockerized architecture
- Automated email processing
- Dynamic AI-generated replies
- Loop prevention logic
- Email thread maintenance
- Ollama local LLM integration

---

# Prerequisites

Before starting, install:

- Docker Desktop
- Git

---

# Step 1: Clone Repository

```bash
git clone <your-repository-url>
cd Build_a_LocalAIEmail_Auto-Responder_with_n8nandOllama
```

---

# Step 2: Configure Environment Variables

Create a `.env` file from `.env.example`.

```bash
cp .env.example .env
```

Update the following values:

```env
IMAP_HOST=imap.gmail.com
IMAP_PORT=993
IMAP_USER=your_email@gmail.com
IMAP_PASSWORD=your_gmail_app_password

SMTP_HOST=smtp.gmail.com
SMTP_PORT=465
SMTP_USER=your_email@gmail.com
SMTP_PASSWORD=your_gmail_app_password
```

Important:

- Use a Gmail App Password
- Do NOT use your normal Gmail password

---

# Step 3: Start Docker Services

Run:

```bash
docker-compose up -d
```

Verify containers:

```bash
docker ps
```

Expected containers:

- n8n_responder
- ollama_responder

---

# Step 4: Pull Ollama Model

Run:

```bash
docker exec -it ollama_responder ollama pull phi3:mini
```

Verify model:

```bash
docker exec -it ollama_responder ollama list
```

---

# Step 5: Open n8n

Open browser:

```text
http://localhost:5678
```

Create your n8n owner account.

---

# Step 6: Import Workflow

1. Open n8n
2. Click **Import from File**
3. Select `workflow.json`
4. Save workflow

---

# Step 7: Configure Credentials

## IMAP Credentials

- Host: `imap.gmail.com`
- Port: `993`
- SSL/TLS: Enabled

## SMTP Credentials

- Host: `smtp.gmail.com`
- Port: `465`
- SSL/TLS: Enabled

Use your Gmail App Password for both.

---

# Step 8: Configure Ollama Node

Inside the `Call Ollama LLM` node, ensure:

```json
"model": "phi3:mini"
```

---

# Step 9: Activate Workflow

Click:

```text
Publish
```

in the top-right corner of n8n.

---

# Step 10: Test the System

Send an email from another account to your configured Gmail address.

Example:

Subject:

```text
Hello
```

Body:

```text
Can you help me?
```

The workflow should:

1. Detect the email
2. Generate AI response
3. Send reply automatically

---

# Workflow Architecture

```text
IMAP Trigger
   ↓
Loop Prevention
   ↓
HTTP Request → Ollama
   ↓
Parse AI Response
   ↓
SMTP Send Reply
```

---

# Loop Prevention

The workflow includes IF-node logic to avoid:

- Replying to its own emails
- Infinite email loops

---

# Email Threading

Replies maintain proper conversation threading using:

- Message-ID
- In-Reply-To
- References headers

---

# Health Checks

Both services include Docker health checks:

- n8n health endpoint
- Ollama process validation

---

# Troubleshooting

## Ollama Model Not Found

Run:

```bash
docker exec -it ollama_responder ollama pull phi3:mini
```

---

## Docker Containers Not Running

Restart services:

```bash
docker-compose down
docker-compose up -d
```

---

## Gmail Authentication Issues

Enable:

- 2-Step Verification
- Gmail App Passwords

Use App Password instead of normal password.

---

# Future Improvements

- GPU acceleration
- Multiple AI model support
- Smart email categorization
- RAG integration
- Web dashboard

---

# Author

Malla Charmi

---

# License

This project is for educational and learning purposes.
