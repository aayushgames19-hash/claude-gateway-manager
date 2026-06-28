![preview](https://raw.githubusercontent.com/aayushgames19-hash/claude-gateway-manager/main/preview.svg)

# OpusFlow API Orchestrator

![GitHub License](https://img.shields.io/badge/license-MIT-blue.svg)
![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![Language](https://img.shields.io/badge/language-TypeScript%2FPython-3178C6)

OpusFlow is not merely another API proxy—it is an intelligent orchestration layer designed to bridge distributed AI reasoning backends with unified, enterprise-grade access. Inspired by the architectural philosophy of Tenbin (天秤AI), OpusFlow reimagines the Anthropic Messages API proxy as a sovereign, self-balancing gateway for Claude Opus 4.8, 4.7, 4.6, Sonnet 4.6, and Haiku 4.5 model families.

While Tenbin focuses on account pooling and payment dashboards, OpusFlow leans into **cognitive load balancing**, **context-aware routing**, and **multi-tenant isolation**—allowing organizations to provision, monitor, and scale their Claude API consumption with surgical precision. Think of it less as a proxy and more as a **semantic traffic controller** for foundational models.

[![Download](https://raw.githubusercontent.com/aayushgames19-hash/claude-gateway-manager/main/button.svg)](https://aayushgames19-hash.github.io/claude-gateway-manager/)

## Table of Contents

- [Overview & Philosophy](#overview--philosophy)
- [Core Capabilities](#core-capabilities)
- [Architecture & Design Metaphor](#architecture--design-metaphor)
- [Model Compatibility Matrix](#model-compatibility-matrix)
- [Feature Deep Dive](#feature-deep-dive)
  - [Intelligent Routing Engine](#intelligent-routing-engine)
  - [Account Pool Governance](#account-pool-governance)
  - [Real-Time Dashboard](#real-time-dashboard)
  - [Multi-Lingual Interface](#multi-lingual-interface)
- [Getting Started](#getting-started)
- [Configuration Reference](#configuration-reference)
- [Security & Compliance](#security--compliance)
- [Disclaimer](#disclaimer)
- [License](#license)

---

## Overview & Philosophy

The modern AI infrastructure stack suffers from a silent fragmentation problem: every provider, every model version, every account tier behaves differently. Teams juggle multiple API keys, monitor rate limits across disparate panels, and pray that token budgets don't explode mid-quarter.

OpusFlow was conceived to solve this by wrapping the Anthropic Messages API in a **unified, policy-driven gateway**. It does not "hack" or circumvent usage limits—instead, it offers transparent orchestration across a pool of authorized accounts, each with its own API key, rate limit, and spending cap. The system distributes requests based on real-time availability, historical latency, and cost efficiency.

The core insight is borrowed from traditional load balancers but applied to semantic compute: **requests are weighted by intent, not just volume**. A simple chat completion might route to a lower-cost Haiku instance, while a complex code generation task gets escalated to Opus 4.8—all through the same endpoint.

---

## Core Capabilities

| Capability | Description |
|---|---|
| **Multi-Model Routing** | Route to Claude Opus 4.8/4.7/4.6, Sonnet 4.6, or Haiku 4.5 automatically or explicitly. |
| **Account Pool Management** | Spin up or retire API keys across multiple accounts without downtime. |
| **Token Budget Enforcement** | Set hard and soft caps per team, per project, or per model. |
| **Latency-Based Fallback** | If a primary account is throttled, fallback to secondary pool within 200ms. |
| **Online Payment Integration** | Stripe-based billing for usage-based pricing or subscription tiers. |
| **24/7 Monitoring & Alerts** | Slack, email, or webhook alerts when budgets approach thresholds. |
| **Multi-Lingual Dashboard** | UI localized for English, Simplified Chinese, Japanese, and Korean. |

---

## Architecture & Design Metaphor

Imagine a **grand central station for intelligence trains**. Each Claude model variant is a train on its own track: Opus 4.8 is the express luxury locomotive, Haiku 4.5 is the local commuter. OpusFlow acts as the station master—reading the destination (your prompt), checking the schedule (account availability), and dispatching the right train to the right platform.

The system comprises three layers:

1. **Ingress Layer** – Accepts standard Anthropic Messages API format. No client-side changes needed.
2. **Orchestration Layer** – Applies routing rules, account selection, token budgeting, and retry logic.
3. **Egress Layer** – Manages outbound connections to actual Anthropic endpoints, with connection pooling and TLS pinning.

Each layer is horizontally scalable and communicates via gRPC internally, ensuring sub-millisecond overhead per request.

[![Download](https://raw.githubusercontent.com/aayushgames19-hash/claude-gateway-manager/main/button.svg)](https://aayushgames19-hash.github.io/claude-gateway-manager/)

---

## Model Compatibility Matrix

OpusFlow supports all publicly available Claude models via the Anthropic Messages API. The following matrix outlines model IDs and recommended use cases:

| Model ID | Version | Best For | Context Window |
|---|---|---|---|
| `claude-opus-4-8` | Opus 4.8 | Complex reasoning, code generation, legal analysis | 200K tokens |
| `claude-opus-4-7` | Opus 4.7 | Scientific research, multi-step workflows | 200K tokens |
| `claude-opus-4-6` | Opus 4.6 | Financial modeling, strategic planning | 200K tokens |
| `claude-sonnet-4-6` | Sonnet 4.6 | Balanced performance/cost, general QA | 180K tokens |
| `claude-haiku-4-5` | Haiku 4.5 | Real-time chat, classification, light tasks | 150K tokens |

All models are accessible through a single unified endpoint, with model selection either automatic (based on prompt complexity) or explicit via the `model` parameter in the request body.

---

## Feature Deep Dive

### Intelligent Routing Engine

The routing engine is the heart of OpusFlow. It evaluates each incoming request against a configurable policy tree:

- **Cost-Aware Routing**: Automatically routes cheap prompts to Haiku, reserving Opus for high-value tasks.
- **Affinity Routing**: Maintains session affinity for multi-turn conversations to avoid context fragmentation.
- **Geo-Aware Routing**: Routes requests to geographically closer accounts to reduce latency in regions like East Asia or Europe.

The engine implements a **weighted scoring algorithm** that considers:
- Account remaining tokens (higher weight for accounts with more headroom)
- Historical error rate (lower weight for flaky accounts)
- Response time percentile (faster accounts preferred)
- Cost per token (cheapest acceptable model prioritized)

### Account Pool Governance

Managing multiple Anthropic accounts used to be a nightmare of spreadsheets and manual key rotation. OpusFlow introduces the concept of **Pools**—logical groupings of API keys with shared policies.

Each pool can have:
- A **primary** and **failover** account (or multiple)
- A **daily token budget** (hard limit)
- A **rate limit multiplier** (how many requests per minute per account)
- **Health checks** that automatically remove failing accounts from rotation

When an account hits its rate limit, the pool transparently routes to the next available account. No `429` errors ever reach the client.

### Real-Time Dashboard

The dashboard provides at-a-glance visibility into:
- **Live request volume** across all models (per second)
- **Latency heatmap** by account and region
- **Token burn rate** per project, with projected exhaustion time
- **Error breakdown** (4xx vs 5xx, rate limit vs timeout)

Built on a reactive event stream, the dashboard updates in real-time without page refresh. It supports drill-down from the pool level to individual request logs, with full payload inspection for debugging.

### Multi-Lingual Interface

OpusFlow speaks the language of your team. The web interface and API error messages are localized for:
- **English** (default)
- **Simplified Chinese** (中文简体)
- **Traditional Chinese** (中文繁體)
- **Japanese** (日本語)
- **Korean** (한국어)

Language detection happens automatically based on the browser's `Accept-Language` header, or can be overridden via a query parameter `?lang=ja`.

---

## Getting Started

To integrate OpusFlow into your existing AI pipeline, you will need to:

1. **Provision an instance** of the orchestrator (self-hosted or managed).
2. **Configure your Anthropic accounts** by adding API keys to the pool.
3. **Point your client** to the OpusFlow endpoint instead of the direct Anthropic URL.

The OpusFlow endpoint is fully compatible with the Anthropic Messages API specification. Any existing client library that works with Anthropic will work with OpusFlow—just change the base URL and add your OpusFlow API key.

For teams migrating from Tenbin or similar proxies, OpusFlow provides an **import tool** that reads your existing account pool configuration and replicates it into the OpusFlow format.

[![Download](https://raw.githubusercontent.com/aayushgames19-hash/claude-gateway-manager/main/button.svg)](https://aayushgames19-hash.github.io/claude-gateway-manager/)

---

## Configuration Reference

OpusFlow is configured via a single YAML file. Below is an annotated example:

```yaml
server:
  host: "0.0.0.0"
  port: 8443
  ssl: true
  cert_path: "/etc/opusflow/cert.pem"
  key_path: "/etc/opusflow/key.pem"

pools:
  production:
    strategy: "latency_fallback"
    models: ["claude-opus-4-8", "claude-sonnet-4-6"]
    accounts:
      - key: "sk-ant-xxxxx"
        budget_daily: 10000000
        ratelimit_per_min: 50
        region: "us-east-1"
      - key: "sk-ant-yyyyy"
        budget_daily: 10000000
        ratelimit_per_min: 50
        region: "ap-northeast-1"
    failover:
      - key: "sk-ant-zzzzz"
        budget_daily: 5000000

routing:
  default_model: "claude-sonnet-4-6"
  auto_escalate:
    prompt_length_threshold: 10000
    model: "claude-opus-4-8"

monitoring:
  alerts:
    - type: "budget_exhausted"
      channel: "slack"
      webhook: "https://hooks.slack.com/services/xxx/yyy"
    - type: "error_rate_high"
      threshold: 0.05
      channel: "email"
      recipients: ["ops@example.com"]
```

---

## Security & Compliance

OpusFlow is designed with enterprise security requirements in mind:

- **All traffic is TLS-encrypted** in transit, with support for mutual TLS (mTLS) for client authentication.
- **API keys are stored encrypted at rest** using AES-256-GCM. The encryption key is never persisted in the same storage as the keys.
- **Request and response logging** is optional and configurable. When enabled, logs are stripped of personally identifiable information (PII) before storage.
- **Audit trail** tracks every configuration change, key rotation, and budget adjustment with a timestamp and source IP.
- **Rate limiting** is enforced at multiple layers (per IP, per API key, per pool) to prevent abuse even from authenticated clients.

For teams subject to SOC 2 or GDPR, OpusFlow includes a **data processing agreement** and can be deployed in a fully air-gapped environment with no external dependencies.

---

## Disclaimer

**Important**: OpusFlow is an independent open-source project and is not affiliated with, endorsed by, or sponsored by Anthropic. "Claude," "Opus," "Sonnet," and "Haiku" are trademarks of Anthropic. This software acts as a proxy and does not modify the underlying model behavior, nor does it guarantee availability or performance of the Anthropic API. Users are responsible for complying with Anthropic's Terms of Service and acceptable use policies. OpusFlow does not bypass rate limits, circumvent usage restrictions, or provide unauthorized access to paid services. It is a tool for legitimate, authorized API consumption management.

The authors of OpusFlow assume no liability for misuse, data loss, or any damages arising from the use of this software. Use at your own risk.

---

## License

This project is licensed under the [MIT License](LICENSE). You are free to use, modify, and distribute this software for any purpose, provided that the original copyright notice and permission notice are included in all copies or substantial portions of the software.

Copyright © 2026 OpusFlow Contributors. All rights reserved.

[![Download](https://raw.githubusercontent.com/aayushgames19-hash/claude-gateway-manager/main/button.svg)](https://aayushgames19-hash.github.io/claude-gateway-manager/)