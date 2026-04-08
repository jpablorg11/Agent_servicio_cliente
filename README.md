# AI Sales Agent & CRM Orchestrator | Sulink

An enterprise-grade pre-sales automation system designed for **Sulink**. This project leverages **Autonomous AI Agents** to manage corporate communication via **Office 365**, qualify leads based on technical business rules, and automate data entry into **Escala CRM**.

---

## 🚀 Project Overview

The system serves as an intelligent first responder for incoming sales inquiries. Beyond simple replies, the agent performs deep technical analysis to classify products (Standard vs. Custom) and executes real-time operations within the CRM without human intervention.

### Key Features
* **Autonomous Classification:** Identifies if a request is for standard products or custom-engineered solutions (e.g., Fire-rated doors) based on dimensions.
* **Intelligent Lead Extraction:** High-precision extraction of Name, Company, Email, and Phone using LLM-based reasoning.
* **"Find or Create" CRM Logic:** Implemented sub-workflow to search for existing contacts via API before creating new entries, preventing database duplication.
* **Contextual Memory:** Utilizes a Window Buffer Memory to maintain coherent, multi-turn conversations with clients.
* **Real-time Stakeholder Alerts:** Automatically notifies the Commercial Coordinator via Outlook when a high-value opportunity is identified.

---

## 🛠️ Tech Stack

* **Orchestrator:** [n8n](https://n8n.io/) (Self-hosted on Docker).
* **AI Engine:** [Groq](https://groq.com/) (Llama 3 model) for ultra-low latency reasoning.
* **Email Integration:** Microsoft Graph API (Office 365).
* **CRM:** Escala REST API.
* **Infrastructure:** Dockerized environment with GitHub version control.

---

## 🏗️ System Architecture

The project is architected into two decoupled layers:

### 1. Reasoning Layer (Main Workflow)
* **Trigger:** Incoming email via Office 365.
* **Agent:** AI Agent node processing instructions in Markdown.
* **Tools:** Dynamic tools for "Email Response" and "CRM Execution".

### 2. Execution Layer (Escala Sub-workflow)
* **Search:** `GET` request to validate existing contact.
* **Logic:** Conditional `IF` node + `Merge` (Choose Branch mode).
* **Action:** `POST` request to create Opportunity and trigger internal alerts.

---

## ⚙️ Configuration & Deployment

### Environment Variables
To ensure stability within Docker and resolve IPv6 connection issues (`ENETUNREACH`) with Microsoft or Escala servers, the following variable must be set in your `docker-compose.yml`:

```yaml
environment:
  - NODE_OPTIONS=--dns-result-order=ipv4first
