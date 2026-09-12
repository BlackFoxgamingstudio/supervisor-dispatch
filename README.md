# Sovereign Supervisor Dispatch (`sovereign-supervisor-dispatch`)

[![PyPI Version](https://img.shields.io/badge/pypi-v1.0.0-blue.svg)](pyproject.toml)
[![Tests](https://img.shields.io/badge/pytest-passing_100%25-brightgreen.svg)](tests/test_solution.py)
[![n8n Integration](https://img.shields.io/badge/n8n-workflow_ready-orange.svg)](n8n/workflow.json)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> **Enterprise Standalone Package**: Field service logistics and dispatch escalation microservice. Tracks real-time technician GPS coordinates, monitors job duration against estimated SLAs, and triggers alerts when service calls exceed acceptable thresholds.

---

## 1. Overview & Architectural Blueprint

`sovereign-supervisor-dispatch` is an independently packaged, zero-dependency software library and microservice engineered as part of Russell Alan Powers' 10-year software engineering portfolio.

It delivers robust capabilities in **Real-Time Field Logistics** and provides seamless integration with n8n event workflows.

```
┌───────────────────────────┐         HTTP POST          ┌───────────────────────────────────────────┐
│        n8n Engine         │ ─────────────────────────> │        sovereign-supervisor-dispatch Adapter        │
│   (Port 5678 Webhook)     │ <───────────────────────── │             (Port 8792)                 │
└───────────────────────────┘       Idempotent JSON      └───────────────────────────────────────────┘
                                                                               │
                                                                               ▼
                                                         ┌───────────────────────────────────────────┐
                                                         │            CoreEngine Domain              │
                                                         │      (SHA-256 Idempotent Execution)       │
                                                         └───────────────────────────────────────────┘
```

---

## 2. Core Exported Classes & Features

- **Primary Module**: `from sovereign_supervisor_dispatch import DispatchScheduler, GPSTracker, JobEscalationWatcher, TruckInventoryMonitor`
- **Deterministic Idempotency**: All executions generate unique SHA-256 idempotency tokens preventing duplicate runs across network retries.
- **Self-Contained Microservice**: Zero external third-party dependencies required for base execution.

---

## 3. Installation & Quickstart

```bash
# Clone the repository
git clone https://github.com/russellpowers/sovereign-supervisor-dispatch.git
cd supervisor-dispatch

# Install in editable mode
pip install -e .

# Verify health status via CLI
sovereign-supervisor-dispatch --health
```

---

## 4. CLI Usage Reference

```bash
# Check service health
sovereign-supervisor-dispatch --health

# Execute core domain action with a JSON payload
sovereign-supervisor-dispatch --exec process_data --payload '{"sample_key": "sample_value"}'
```

---

## 5. n8n Automation & Integration Contract

- **Microservice Port**: `http://localhost:8792`
- **Inbound Trigger Route**: `POST /api/v1/execute`
- **Integration Workflow**: `Technician location/status ping -> Evaluate duration SLA -> Dispatch SMS escalation to supervisor on 30m overage`

### How to Import into n8n:
1. Open your n8n canvas (`http://localhost:5678`).
2. Click **Workflows** > **Import from File**.
3. Select `n8n/workflow.json`.
4. Start the background webhook adapter:
   ```bash
   python3 n8n/webhook_adapter.py
   ```
5. Dispatch your test event to `http://localhost:5678/webhook/supervisor-dispatch-trigger`.

---

## 6. Verification & Automated Testing

This repository includes a 100% passing test suite runnable via `pytest` or `python3`:

```bash
# Run tests with pytest
pytest tests/test_solution.py -v

# Run tests directly (zero dependencies)
python3 tests/test_solution.py
```

---

## 7. Staff/Principal Engineer Technical Defense

> **60-Second Interview Pitch**:
> "Demonstrates operations research, real-time spatial event streaming, SLA alerting algorithms, and clean frontend dispatch dashboarding."
