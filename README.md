# Dead Code & Dead Feature Audit

A read-only coding-agent skill for performing deep, evidence-based audits of software projects.

It identifies potentially:

- Dead code
- Abandoned features
- Unused APIs
- Unused routes/screens
- Unused dependencies
- Duplicate implementations
- Legacy code
- Debug/test exposure
- Dead database artifacts
- Unused configuration
- Unused assets
- Orphaned background jobs
- Stale documentation

The skill is designed for coding agents such as Claude Code, Antigravity, and other repository-aware AI coding agents.

---

## What This Skill Does

The agent first reconstructs the project's architecture and runtime dependency graph.

It then traces suspicious code through:

```text
Entry Points
    ↓
Routes / Navigation
    ↓
Components / Services
    ↓
API Clients
    ↓
Backend APIs
    ↓
Business Logic
    ↓
Database
    ↓
Workers / Jobs
    ↓
Configuration
    ↓
Deployment