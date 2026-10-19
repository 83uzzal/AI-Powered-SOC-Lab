# AI-Powered SOC Lab

A hands-on cybersecurity portfolio project exploring local AI-assisted security alert analysis using Wazuh, Suricata, FastAPI, and Ollama.

## Project Overview

This project explores how a locally hosted Large Language Model (LLM) can assist security analysts with initial alert triage and investigation.

The FastAPI application sends security alert data to Ollama, which generates a preliminary analysis for human review.

## Technology Stack

- Wazuh — Security Information and Event Management (SIEM)
- Suricata — Network threat detection
- Ollama — Local LLM runtime
- Qwen2.5:1.5B — Lightweight language model
- FastAPI — AI analysis API
- Python — Application development
- Ubuntu 22.04 LTS — Lab environment

## Architecture

Suricata Network Alerts
        |
        v
    Wazuh SIEM
        |
        v
 Custom Integration
        |
        v
   FastAPI Service
        |
        v
 Ollama + Qwen2.5:1.5B
        |
        v
 AI-Generated Alert Analysis

## Current Features

- Local AI model integration using Ollama
- FastAPI health-check endpoint
- Security alert analysis endpoint
- Preliminary alert assessment
- Evidence-focused analysis instructions
- Investigation recommendations
- Local inference without per-request cloud AI API costs

## API Endpoints

### Health Check

`GET /health`

Checks API availability and Ollama connectivity.

### Analyze an Alert

`POST /analyze`

Example request:

    {
      "alert": "Example security alert for analysis"
    }

The API returns the alert and its AI-generated analysis.

## Setup

### 1. Create a Python virtual environment

    python3 -m venv venv
    source venv/bin/activate

### 2. Install dependencies

    pip install -r requirements.txt

### 3. Download the Ollama model

    ollama pull qwen2.5:1.5b

### 4. Start the API

    uvicorn app:app --host 127.0.0.1 --port 8000

### 5. Test the API

    curl http://127.0.0.1:8000/health

Interactive API documentation: `http://127.0.0.1:8000/docs`

## Security Notes

- AI-generated assessments are preliminary and require human validation.
- Do not expose the API publicly without appropriate security controls.
- Never commit credentials, private keys, or sensitive security logs.
- Use sanitized sample alerts when demonstrating the project.

## Project Status

An educational cybersecurity portfolio project focused on local AI-assisted SOC alert analysis.

## Disclaimer

This project is intended for educational and defensive cybersecurity purposes. AI output may be inaccurate and must not be treated as a definitive security verdict.
