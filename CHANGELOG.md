# Changelog — Sovereign Supervisor Dispatch

All notable changes follow [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format.
Versioning follows [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- Full ecosystem documentation suite (ARCHITECTURE, DEVELOPER_GUIDE, SME_PLAYBOOK, SOP)
- GitHub Actions CI matrix (Python 3.10 / 3.11 / 3.12)
- Multistage Dockerfile with non-root user, health check, OCI labels
- docker-compose.yml with SBB platform network
- n8n custom node integration via `SovereignTools`
- `.env.example` environment template
- CONTRIBUTING, CODE_OF_CONDUCT, SECURITY governance files
- OpenAPI 3.1 compatible REST API spec
- Bandit security scan in CI

## [1.0.0] — 2024-01-01

### Added
- Initial production release of Sovereign Supervisor Dispatch (PKG-014)
- Core microservice on port `8792`
- n8n webhook adapter (`n8n/webhook_adapter.py`)
- REST API (`POST /api/v1/execute`, `GET /health`)
- Components: RoutingEngine, CapacityPlanner, SLATracker, GeoFenceMonitor, DispatchDashboard
- pyproject.toml packaging with `[dev]` extras
- CLI: `sovereign-supervisor-dispatch --help`
- Unit test suite (pure `unittest.TestCase`, no external test framework required)

### Domain: Real-Time Field Logistics
AI-powered field service dispatch engine with technician routing, capacity planning, SLA tracking, GPS geofencing, and live supervisor dashboards.

[Unreleased]: https://github.com/BlackFoxgamingstudio/supervisor-dispatch/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/BlackFoxgamingstudio/supervisor-dispatch/releases/tag/v1.0.0
