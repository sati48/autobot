# autobot
# 🚀 DevOps Auto Content Bot

A production-ready Telegram automation bot that fetches DevOps articles from RSS, generates AI-powered LinkedIn content, and delivers it back to the user — deployed securely on AWS with HTTPS.

---

## 📌 Project Overview

This project demonstrates real-world DevOps practices combined with AI automation.

When a user sends a message to the Telegram bot:

1. The webhook triggers an automated workflow.
2. The latest DevOps article is fetched from RSS.
3. An LLM generates LinkedIn-ready structured content.
4. The generated content is sent back to the user.

The system is cloud-hosted, containerized, secure (HTTPS), and production-ready.

---

# 🏗 Visual Architecture Diagram

```
                    ┌──────────────────────┐
                    │      Telegram User    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   Telegram Bot API    │
                    │  (Webhook over HTTPS) │
                    └──────────┬───────────┘
                               │
                               ▼
                ┌──────────────────────────────┐
                │      Domain (mydevopsbot)     │
                │      SSL via Let's Encrypt     │
                └──────────┬────────────────────┘
                           │
                           ▼
                ┌──────────────────────────────┐
                │         Nginx (Reverse Proxy) │
                │  - HTTPS Termination          │
                │  - HTTP → HTTPS Redirect      │
                │  - Proxies to n8n container   │
                └──────────┬────────────────────┘
                           │
                           ▼
                ┌──────────────────────────────┐
                │     Docker Container          │
                │        (n8n Self-Hosted)      │
                └──────────┬────────────────────┘
                           │
                           ▼
                ┌──────────────────────────────┐
                │         n8n Workflow          │
                │                               │
                │ Telegram Trigger              │
                │        ↓                      │
                │ RSS Read (Dev.to DevOps)      │
                │        ↓                      │
                │ Limit (1 item)                │
                │        ↓                      │
                │ HTTP Request → OpenRouter API │
                │        ↓                      │
                │ Telegram Send Message         │
                └──────────────────────────────┘
```

---

# ☁ Infrastructure Layer (DevOps)

## 1️⃣ AWS EC2
- Free-tier eligible cloud VM
- Full OS-level access
- Hosts Docker, Nginx, and SSL
- Public internet-facing instance

## 2️⃣ Elastic IP
- Static public IP
- Prevents IP change after restart
- Required for stable DNS mapping

## 3️⃣ Domain Configuration
- Custom domain mapped to Elastic IP
- Required for:
  - HTTPS
  - Telegram webhook registration
  - Production-grade deployment

---

# 🔐 Security Layer

## SSL (Let's Encrypt via Certbot)
- Free SSL certificate
- Auto-renew enabled
- Required for Telegram webhook
- Required for secure cookies in n8n

## Nginx Reverse Proxy
- Terminates HTTPS
- Redirects HTTP → HTTPS
- Proxies requests to n8n (localhost:5678)
- Industry-standard production setup

---

# 🐳 Containerization

## Docker
- Runs n8n inside a container
- Isolated environment
- Easy restart and portability
- Follows modern DevOps deployment standards

---

# 🔄 Application Layer (n8n Workflow)

## 🔹 Telegram Trigger
- Registers webhook
- Listens for user messages

## 🔹 RSS Read Node
- Source:
  https://dev.to/feed/tag/devops
- Fetches latest DevOps articles

## 🔹 Limit Node
- Max Items = 1
- Prevents API rate limits
- Controls AI cost
- Ensures stable execution

## 🔹 HTTP Request Node
- Calls OpenRouter Chat Completion API
- Model Used:
  openrouter/free
- Generates:
  - Hook
  - LinkedIn Post
  - ASCII Diagram
  - Image Prompt
  - Hashtags

## 🔹 Telegram Send Message
- Sends AI-generated structured content back to user

---

# 🤖 AI Layer

**AI Router:** OpenRouter  
**API Type:** Chat Completions API  
**Model Used:** openrouter/free  
**Purpose:** Content generation from RSS data  

OpenRouter was chosen because:
- Free tier available
- Supports multiple LLM providers
- Easy REST API integration
- Suitable for experimentation and automation

---

# 🛠 Problems Faced & Solutions

### 1️⃣ Secure Cookie Error
Cause: n8n requires HTTPS  
Solution: Installed SSL via Certbot and configured Nginx

### 2️⃣ HTTPS Not Reachable
Cause: Port 443 blocked in AWS Security Group  
Solution: Opened HTTPS inbound rule

### 3️⃣ API Rate Limit (429)
Cause: RSS returned multiple items → multiple AI calls  
Solution: Added Limit node (Max Items = 1)

### 4️⃣ Workflow Not Auto-Triggering
Cause: Workflow not published  
Solution: Activated (Published) workflow

---

# 📈 DevOps Concepts Demonstrated

- Cloud Infrastructure (AWS EC2)
- Elastic IP & DNS Mapping
- Reverse Proxy Architecture
- SSL Termination
- Docker Containerization
- Webhook-Based Automation
- API Rate Limit Handling
- Production Deployment
- Secure Configuration Management

---

# 🎯 Final Outcome

When a user sends a Telegram message:

1. Webhook triggers n8n
2. RSS fetches latest DevOps article
3. LLM generates LinkedIn-ready structured content
4. Telegram delivers the response
5. Fully automated, secure, cloud-hosted workflow

---

# 🔮 Future Improvements

- Add keyword-based trigger (e.g., "generate")
- Add database for history storage
- Use paid LLM for higher-quality output
- Add CI/CD for Docker deployment
- Add monitoring (Prometheus / Grafana)
- Deploy behind Load Balancer

---

# 🧰 Tech Stack

AWS | Docker | Nginx | Let's Encrypt | n8n | OpenRouter | Telegram Bot API

---

# 📄 License

Built for learning, automation practice, and DevOps portfolio demonstration.
