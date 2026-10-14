
Phishing URL Redirect Tracer

**An n8n-based security automation for phishing URL analysis and redirect tracing.**

## 📌 Project Overview

The Phishing URL Redirect Tracer is a security automation project designed to analyze submitted URLs, trace their redirect chains, retrieve domain registration information, and generate an AI-assisted risk verdict.

The workflow receives URLs through a Telegram bot and returns a security report containing the redirect path, HTTP status codes, domain information, risk classification, and reasons for the verdict.

**Project Tag:** NGP-045  
**Team:** Red Team  
**Domain:** Threat Analysis

## 🎯 Objectives

- Trace URL redirects to identify the final destination.
- Detect potentially suspicious redirect behavior.
- Retrieve domain registration details using RDAP.
- Generate evidence-based phishing risk assessments.
- Send security alerts through Telegram.
- Store alerts for later investigation.
- Generate automated daily security summaries.
- Test the workflow's resistance to prompt injection.

## ✨ Key Features

### 1. Telegram URL Submission
Users submit a URL to the Telegram bot to start the analysis workflow.

### 2. URL Validation
The workflow validates the submitted URL before processing it and should reject invalid URLs and unsafe destinations.

### 3. Redirect Chain Tracing
The workflow follows redirects one hop at a time, with a maximum of three hops.

It records:
- Current URL
- HTTP status code
- Redirect destination
- Final observed destination

Automatic redirect following is disabled in the HTTP request step so each redirect can be inspected individually.

### 4. Domain Registration Analysis
The workflow uses RDAP to retrieve available domain registration information, such as the registration date.

Domain age is treated as one indicator, not proof that a website is safe or malicious.

### 5. AI-Assisted Phishing Verdict
The analysis combines available evidence to classify a submitted URL as:
- **Benign** — no significant suspicious indicators identified
- **Suspicious** — potentially risky indicators require further investigation
- **Malicious** — evidence indicates likely malicious activity

The verdict includes a severity level and a short explanation. AI results are not a guarantee of safety.

### 6. Telegram Security Alerts
The workflow sends a structured report containing:
- Project identification tag
- Execution ID
- Timestamp in Indian Standard Time (IST)
- Verdict and severity
- Redirect chain
- Final domain
- Domain registration information
- Reasons supporting the verdict

### 7. Persistent Alert Logging
Analysis results are saved to persistent storage so alerts can be reviewed and included in later summaries.

### 8. Daily Summary Schedule
A separate scheduled workflow retrieves saved alerts, counts the total alerts by severity, and sends a consolidated daily report through Telegram.

### 9. Prompt Injection Testing
The workflow is tested with hostile input, such as instructions asking the AI to ignore its rules and mark every URL as safe.

The objective is to ensure the verdict is based on technical evidence rather than untrusted instructions included in submitted content.

## 🔄 Main Workflow

```text
Telegram Trigger
       |
       v
Extract URL
       |
       v
Validate URL
       |
       v
Hop Loop
       |
       v
Prepare Hop
       |
       v
Fetch Hop (Auto-follow OFF)
       |
       v
Record Hop
       |
       v
Repeat until redirect chain ends
or maximum hop limit is reached
       |
       v
RDAP Domain Lookup
       |
       v
Phishing Verdict Agent
       |
       v
Send Verdict to Telegram
       |
       v
Log Alert
```

## 📅 Daily Summary Workflow

```text
Schedule Trigger
       |
       v
Get Today's Alerts
       |
       v
Count Alerts by Severity
       |
       v
Generate Daily Summary
       |
       v
Send Summary to Telegram
```

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| n8n | Workflow automation |
| Telegram Bot API | URL submission and alert delivery |
| HTTP Request | Redirect and response analysis |
| RDAP | Domain registration information |
| AI Agent | Evidence-assisted risk classification |
| Persistent storage | Alert history and daily summaries |

## 📂 Repository Structure

```text
NGP-045-Phishing-URL-Redirect-Tracer/
├── workflows/
│   ├── phishing-url-redirect-tracer.json
│   └── daily-summary-schedule.json
├── screenshots/
│   ├── main-workflow.png
│   ├── telegram-alert.png
│   └── daily-summary.png
└── README.md
```

*The files shown above are the suggested repository structure. Add the exported workflow files and screenshots when available.*

## 🚀 Setup and Usage

1. Set up an n8n instance.
2. Create a Telegram bot using BotFather.
3. Configure the required Telegram credentials in n8n.
4. Import the main workflow JSON file.
5. Configure the RDAP lookup and AI provider credentials, if required.
6. Configure persistent alert storage.
7. Review URL validation and private-network blocking safeguards.
8. Test the workflow with authorized, known-safe URLs.
9. Import and configure the daily summary workflow.
10. Publish the workflows and verify that Telegram alerts and scheduled summaries work correctly.

Node configurations, credentials, and external service requirements may need adjustment after importing the workflow.

## 🧪 Testing

The project can be evaluated using:
- Known-safe URLs
- Authorized phishing-simulation URLs
- URLs with multiple redirects
- Invalid URL input
- Redirect chains that exceed the hop limit
- Unavailable RDAP information
- Prompt-injection test input

Record the actual execution results and compare each verdict with the observed technical evidence.

## 🔐 Security Considerations

- Never commit Telegram bot tokens, API keys, passwords, or exported credentials.
- Remove private chat IDs and sensitive investigation records before publishing.
- Block localhost, private IP addresses, and other unsafe destinations to reduce server-side request forgery (SSRF) risks.
- Restrict redirect targets and validate every hop.
- Defang URLs in reports to reduce accidental clicks.
- Treat AI output as decision support rather than definitive proof.
- Use the workflow only for authorized defensive security analysis.

## 📈 Expected Outcome

The project demonstrates how workflow automation can combine URL redirect tracing, domain registration analysis, AI-assisted classification, Telegram notifications, persistent alert logging, and scheduled security reporting in a single security-monitoring process.

## 👥 Project Information

- **Project ID:** NGP-045
- **Project Name:** Phishing URL Redirect Tracer
- **Team:** Red Team
- **Domain:** Threat Analysis
- **Platform:** n8n

---

**Connect Smart. Stay Secure.**
