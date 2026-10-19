# AI-Powered SOC Lab

**Local AI-Assisted Security Alert Analysis with Wazuh, Suricata, FastAPI, and Ollama**

An intermediate-level cybersecurity portfolio project demonstrating how a locally hosted Large Language Model (LLM) can assist Security Operations Center (SOC) analysts with initial alert triage, evidence review, and investigation planning.

## Project Overview

Security teams often need to review large numbers of alerts before deciding which ones require further investigation.

This project integrates a local AI analysis service into a Wazuh-based security monitoring lab. Selected alerts are sent to a FastAPI application, which communicates with Ollama and the Qwen2.5:1.5B language model to generate a preliminary security assessment.

The objective is to support analyst decision-making while keeping inference local rather than relying on a paid cloud AI API.

**Important:** AI-generated assessments are advisory and must be validated by a human analyst.

## Architecture

```text
Suricata Network Alert
          |
          v
     Wazuh SIEM
          |
          v
 Custom Rule 100121
          |
          v
 Custom AI Integration
          |
          v
     FastAPI Service
          |
          v
  Ollama Local Runtime
          |
          v
     Qwen2.5:1.5B
          |
          v
 AI-Generated Analysis
          |
          v
 AI Analysis Log
          |
          v
 Wazuh AI Event Rule 100120
          |
          v
 Wazuh Dashboard
```

The workflow above represents the configured lab pipeline. The AI analysis is intended to support investigation, not independently confirm that an alert is malicious.

## Technology Stack

| Technology | Purpose |
|---|---|
| Wazuh 4.14 | Security monitoring, alert management, and SIEM dashboard |
| Suricata | Network threat detection |
| Ollama | Local LLM inference |
| Qwen2.5:1.5B | Lightweight language model for preliminary analysis |
| FastAPI | REST API for AI-assisted alert analysis |
| Python | Integration and application logic |
| Ubuntu 22.04 LTS | Lab operating system |

## Current Features

- Local LLM integration using Ollama and Qwen2.5:1.5B
- FastAPI health-check endpoint
- API endpoint for security alert analysis
- Wazuh custom integration for selected alerts
- Preliminary AI-assisted alert triage
- Evidence-focused analysis instructions
- Investigation recommendations
- AI analysis log integration with Wazuh
- Local inference without per-request cloud AI API charges

## API Endpoints

### Health Check

`GET /health`

Checks API availability and Ollama connectivity.

Test:

```bash
curl http://127.0.0.1:8000/health
```

### Analyze a Security Alert

`POST /analyze`

Example request:

```bash
curl -X POST http://127.0.0.1:8000/analyze \
  -H "Content-Type: application/json" \
  -d '{"alert":"Example security alert for analysis"}'
```

The service sends the alert for local LLM analysis and returns the application's analysis response.

### Interactive API Documentation

When the API is running, open:

`http://127.0.0.1:8000/docs`

## Lab Setup

These steps start the AI API component. A complete integrated deployment also requires a working Wazuh installation, Suricata configuration, and the corresponding Wazuh integration and rules.

### Prerequisites

- Ubuntu 22.04 LTS
- Python 3
- Ollama
- Git
- Sufficient RAM and CPU resources for local model inference

### 1. Clone the repository

```bash
git clone https://github.com/83uzzal/AI-Powered-SOC-Lab.git
cd AI-Powered-SOC-Lab
```

### 2. Create and activate a virtual environment

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Python dependencies

```bash
pip install -r requirements.txt
```

### 4. Download the local model

Ensure Ollama is installed and running, then execute:

```bash
ollama pull qwen2.5:1.5b
ollama list
```

### 5. Start the FastAPI service

```bash
uvicorn app:app --host 127.0.0.1 --port 8000
```

The API will listen on localhost port 8000.

### 6. Verify the service

In another terminal, run:

```bash
curl http://127.0.0.1:8000/health
```

You can also open `http://127.0.0.1:8000/docs` to inspect the API.

## Wazuh Integration

The configured lab uses a custom integration to forward selected Wazuh alerts to the local AI API.

- **Rule 100121:** Selects the configured Suricata demonstration alert for AI analysis.
- **FastAPI:** Receives the alert and requests local LLM analysis.
- **AI analysis log:** Stores analysis output for the Wazuh integration workflow.
- **Rule 100120:** Handles the configured AI analysis event for visibility in the Wazuh Dashboard.

Exact behavior depends on the local Wazuh rules and integration configuration. Review those settings before reproducing the complete pipeline on another system.

## Security Considerations

- Keep the API bound to localhost unless remote access is explicitly required and secured.
- Never commit credentials, API keys, private keys, or sensitive logs.
- Use sanitized alerts and test data in public demonstrations.
- Apply appropriate file permissions to integration scripts and analysis logs.
- Treat model output as untrusted input and validate findings against original telemetry.
- Do not enable automated containment or response actions without appropriate safeguards and human approval.

## Project Roadmap

Potential future enhancements:

- Structured incident summaries
- Explicit risk-level classification
- Evidence extraction and validation
- Prioritized investigation recommendations
- Improved AI analysis fields for dashboard searches
- Analyst feedback and evaluation of AI accuracy

These items should be considered planned enhancements until implemented and tested.

## Limitations

- Small local models can produce inaccurate or incomplete assessments.
- AI output does not replace investigation, threat intelligence, or analyst judgment.
- Local inference performance depends on available CPU and memory.
- The repository provides the AI API component; the complete SOC workflow also depends on separately configured Wazuh and Suricata components.

## Disclaimer

This project is intended for educational and defensive cybersecurity purposes. It is not a substitute for a production SOC, and AI-generated output must not be treated as a definitive security verdict.

## Author and Repository

**GitHub:** https://github.com/83uzzal/AI-Powered-SOC-Lab
