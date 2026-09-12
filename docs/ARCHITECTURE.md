# Architecture: Sovereign Supervisor Dispatch

## Overview

**Package ID:** `PKG-014`  
**Domain:** Real-Time Field Logistics  
**Microservice Port:** `8792`  
**n8n Webhook Path:** `supervisor-dispatch-trigger`  
**GitHub:** [BlackFoxgamingstudio/supervisor-dispatch](https://github.com/BlackFoxgamingstudio/supervisor-dispatch)

AI-powered field service dispatch engine with technician routing, capacity planning, SLA tracking, GPS geofencing, and live supervisor dashboards.

---

## System Architecture

```
                     ┌──────────────────────────────────┐
                     │       Sovereign Supervisor Dispatch │
                     │       Port: 8792            │
                     ├──────────────┬───────────────────┤
   n8n Webhook ────▶ │  REST API    │   Core Engine     │
   HTTP POST         │  /api/v1/*   │   Dispatcher      │
                     └──────┬───────┴────────┬──────────┘
                            │                │
              ┌─────────────▼────────────────▼─────────┐
              │          Component Layer                 │
              │  RoutingEngine   | CapacityPlanner | SLATracker    │
              └────────────────────────┬────────────────┘
                                       │
              ┌────────────────────────▼────────────────┐
              │      n8n Central Event Bus (:5678)       │
              └─────────────────────────────────────────┘
```

## Core Components

### `RoutingEngine`
Handles all routing operations. Exposes async methods callable from the core dispatcher.

### `CapacityPlanner`
Handles all capacityplanner operations. Exposes async methods callable from the core dispatcher.

### `SLATracker`
Handles all slatracker operations. Exposes async methods callable from the core dispatcher.

### `GeoFenceMonitor`
Handles all geofencemonitor operations. Exposes async methods callable from the core dispatcher.

### `DispatchDashboard`
Handles all dispatchdashboard operations. Exposes async methods callable from the core dispatcher.

---

## API Contract

All interactions follow the SBB standard envelope:

```http
POST /api/v1/execute
Content-Type: application/json
X-SBB-API-Key: <api-key>

{
  "action": "<operation>",
  "payload": {},
  "trace_id": "optional-uuid"
}
```

**Success Response (HTTP 200):**
```json
{
  "status": "success",
  "data": {},
  "trace_id": "...",
  "timestamp": "2025-01-01T00:00:00Z"
}
```

**Health Check:**
```http
GET /health
→ {"status": "healthy", "service": "sovereign-supervisor-dispatch", "port": 8792}
```

## Integration Matrix

| System | Protocol | Direction | Purpose |
|--------|----------|-----------|---------|
| n8n Event Bus (:5678) | HTTP POST | Outbound | Event forwarding |
| n8n Webhook | HTTP POST | Inbound | Trigger execution |
| SBB Codebase Vault (:8766) | HTTP | Outbound | Code analysis |
| SBB Patterns Bible (:8794) | HTTP | Outbound | Standards validation |
| External APIs | HTTPS | Outbound | Domain-specific data |

## Deployment Architecture

```yaml
# docker-compose excerpt
sovereign-supervisor-dispatch:
  image: sovereign-supervisor-dispatch:latest
  ports: ["8792:8792"]
  healthcheck:
    test: curl -f http://localhost:8792/health
    interval: 30s
```

## Security Model

| Control | Implementation |
|---------|---------------|
| Authentication | `X-SBB-API-Key` header (env: `SBB_API_KEY`) |
| Rate Limiting | 100 req/min per client IP |
| Input Validation | Pydantic models (strict mode) |
| Container Security | Non-root user (`appuser:1001`) |
| Secrets | Environment variables only (never hardcoded) |
| TLS | Terminate at reverse proxy (nginx/caddy) |

## Tags
`dispatch`, `logistics`, `routing`, `gps`, `sla`
