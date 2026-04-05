---
title: "Cloud Mastery 2026 #1 AI From Scratch"
date: 2026-03-14
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: "AI Agents, Prompt Engineering & AIoT on AWS"

### Event Objectives

- Analyze the inherent limitations of standalone Large Language Models (LLMs) and implement comprehensive mitigation strategies utilizing AI Agent architectures.
- Master the engineering discipline of AI communication via standardized Prompt Engineering methodologies to optimize token expenditure and elevate response fidelity.
- Explore the practical potential of AIoT ecosystems when seamlessly integrated with AWS Cloud managed services (specifically AWS IoT Core and Amazon Rekognition).

### Speakers

- **Banh Cam Vinh** - Speaker on: Architecting AI Agents with the Strands Framework
- **Nguyen Tuan Thinh** - DevOps Engineer, Speaker on: Automated Prompt Engineering Methodologies
- **Aiden Dinh** - Operation Engineer (Katalon), Speaker on: Applied AIoT Projects

### Key Highlights

#### Building AI Agent with Strands

Standalone Large Language Models (LLMs) frequently exhibit limitations due to the absence of real-time data access and the inability to directly interface with external systems. AI Agent architectures overcome these operational barriers by providing:
- **Multi-step reasoning:** The capability to autonomously plan and execute highly complex, sequential workflows.
- **Tool integration:** Granting AI the authority to actively invoke APIs, query databases, and communicate with external services.
- Leveraging the **Strands Agents** framework operating on an **Agentic Loop** (tool-invocation mechanism), which tightly couples System Prompts and Knowledge Bases to make autonomous decisions and adapt dynamically to shifting contexts.

![AI Agent Workflow](/images/4-Event/Agent.png "AI Agent Architecture")

#### Automated Prompt Engineering

Effective communication with AI is an engineering discipline requiring high precision. Ambiguous prompts not only yield sub-optimal outputs but also cause excessive token consumption and operational inconsistency. 
A standardized, enterprise-grade Prompt must synthesize 7 essential components:
1. **Role** (Assigning an expert persona to the AI)
2. **Instruction** (Explicit and unambiguous task directives)
3. **Context** (Providing foundational background information)
4. **Input Data** (The specific payload to be processed)
5. **Output Format** (Strictly dictating the expected data structure)
6. **Examples** (Supplying reference paradigms via Few-shot prompting)
7. **Constraints** (Establishing strict guardrails and operational limitations)

The optimized AWS architecture for enterprise prompt management is engineered using: **Amazon DynamoDB** (for low-latency, millisecond metadata storage), **Amazon CloudWatch** (for comprehensive telemetry, logging, and error-rate monitoring), integrated with the core **Amazon Bedrock** platform.

![Prompt Structure and AWS Architecture](/images/4-Event/prompt.png "Prompt Engineering & AWS")

#### AIoT Projects: Smart Locker Management

Digitizing and automating manual equipment borrowing processes in clubs via a smart locker ecosystem:
- **Hardware Infrastructure:** Deploying a Raspberry Pi to serve as the Main Controller and local MQTT Broker; utilizing Arduino microcontrollers for sensor telemetry acquisition; synchronously integrating Reed Switches, RFID Card Readers, and surveillance Cameras.
- **AWS Cloud Integration:** - **AWS IoT Core:** Functions as the centralized routing hub, securely directing sensor telemetry (RFID scans, door actuation states) to AWS Lambda and persisting state in DynamoDB, thereby guaranteeing elastic scalability independent of local server constraints.
  - **Amazon Rekognition:** Applies advanced Computer Vision to analyze images, cross-referencing user facial biometrics against member databases to grant highly secure access authorization.

![Hardware and AWS Architecture for AIoT](/images/4-Event/AIot.png "AIoT Smart Locker Architecture")

#### Some event photos

![Event Check-in](/images/4-Event/event1a.jpg "Check-in")
![Photo with speakers](/images/4-Event/event1b.jpg "Networking")
![Photo with speakers](/images/4-Event/event2c.jpg "Networking")

> In summary, the event not only provided profound perspectives on cutting-edge AI trends such as Agentic Systems and Prompt Engineering, but also delivered invaluable, battle-tested insights into converging IoT hardware with flexible AWS cloud infrastructures to resolve complex, real-world challenges.