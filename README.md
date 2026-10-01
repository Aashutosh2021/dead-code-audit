# Dead Code & Dead Feature Audit

A read-only, evidence-based AI coding-agent skill for auditing software projects for dead code, abandoned features, unused APIs, legacy logic, duplicate implementations, unused dependencies, database artifacts, configuration, assets, and debug/test exposure.

Designed for coding agents such as **Claude Code** and **Google Antigravity**, while following the standard `SKILL.md` skill format.

---

## What This Skill Does

The skill performs a deep repository audit before making conclusions.

It analyzes:

- Dead and orphaned code
- Abandoned or partially implemented features
- Unused API endpoints
- Unused screens, pages, routes, and navigation entries
- Unused components, hooks, services, utilities, classes, and functions
- Unused dependencies and imports
- Duplicate implementations
- Legacy API versions and authentication logic
- Debug/test/development exposure
- Dead database models, fields, queries, collections, and tables
- Unused environment variables and configuration
- Feature flags and unreachable branches
- Unused assets, icons, translations, and constants
- Background jobs, workers, queues, and cron tasks
- WebSocket/socket events
- Dead middleware and error handlers
- Stale documentation

The agent first reconstructs the architecture and runtime dependency graph, then traces suspicious items through the project.

---

## Core Principle

> **No evidence → No "Confirmed Dead".**

An item must not be considered dead merely because:

- It has no obvious import
- Its filename looks old
- It is not referenced by the visible UI
- It looks similar to another implementation
- A dependency is not directly imported

The audit explicitly checks for indirect usage such as:

- Dynamic imports
- Dependency injection
- Reflection
- Framework auto-discovery
- Decorators / annotations
- Runtime registration
- Feature flags
- Environment variables
- Build configuration
- Generated code
- Serverless handlers
- ORM conventions
- WebSocket events
- Background jobs
- Plugin systems

---

# Read-Only Guarantee

This skill is strictly analytical.

The agent must **not**:

- Modify files
- Delete files
- Rename files
- Move files
- Refactor code
- Generate replacement code
- Install dependencies
- Uninstall dependencies
- Upgrade dependencies
- Change configuration
- Modify environment files
- Modify databases
- Create patches
- Commit changes
- Push changes
- Perform cleanup

The expected result is an on-screen audit report only.

---

# Finding Classification

| Classification | Meaning |
|---|---|
| **Confirmed Dead** | Strong repository evidence demonstrates that the item has no meaningful reachable usage |
| **Likely Dead** | Evidence strongly suggests the item is unused, but some uncertainty remains |
| **Potentially Used / Needs Verification** | Static analysis cannot safely determine whether it is used |
| **Duplicate / Legacy** | Duplicate or older implementation exists |
| **Debug/Test Exposure** | Debug, test, or development functionality may be exposed in an inappropriate environment |

Every finding also receives:

- Risk level
- Confidence level
- Evidence
- Usage trace
- Recommended next action

---

# Audit Output

The final report contains:

1. Executive Summary
2. Project Architecture Understanding
3. Confirmed Dead Code
4. Likely Dead Code
5. Dead Features
6. Unused APIs / Endpoints
7. Unused Screens / Routes
8. Unused Dependencies
9. Duplicate Implementations
10. Debug / Test / Legacy Exposure
11. Dead Database / Config / Assets
12. False Positives / Code That Must Be Kept
13. Dependency & Feature Dependency Map
14. Risk Assessment
15. Cleanup Recommendations

For every finding, the report includes:

```text
Location
Symbol / Endpoint / Route
What it does
Why it appears dead / legacy / duplicated
Evidence / Usage Trace
Runtime Path
Risk
Confidence
Recommended Action
Verification Needed
```

---

# Repository Structure

```text
dead-code-audit/
├── SKILL.md
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   ├── methodology.md
│   ├── classification.md
│   └── examples.md
└── examples/
    └── sample-audit.md
```

`SKILL.md` is the actual agent skill.

The other files provide documentation and examples for humans.

---

# Installation

The repository contains a portable skill bundle. Installation means copying or cloning the skill folder into the agent's supported skill directory.

## Claude Code

Claude Code project skills use:

```text
<project-root>/.claude/skills/<skill-name>/SKILL.md
```

For this skill, from the root of the project you want the skill to be available in:

```bash
mkdir -p .claude/skills/dead-code-audit
git clone https://github.com/Aashutosh2021/dead-code-audit.git /tmp/dead-code-audit
cp /tmp/dead-code-audit/SKILL.md .claude/skills/dead-code-audit/SKILL.md
```

Or simply copy the `dead-code-audit` skill directory into:

```text
.claude/skills/
```

The resulting structure should be:

```text
your-project/
├── .claude/
│   └── skills/
│       └── dead-code-audit/
│           └── SKILL.md
├── src/
├── ...
└── ...
```

Start Claude Code from the project:

```bash
cd your-project
claude
```

Then invoke:

```text
/dead-code-audit
```

You can also ask Claude to perform a dead-code audit; Claude Code can automatically select relevant skills based on their descriptions.

### User-level Claude Code installation

If you want the skill available across your projects, install it in your user-level Claude skills directory:

```text
~/.claude/skills/dead-code-audit/SKILL.md
```

This makes the skill available beyond a single repository.

---

## Google Antigravity

Antigravity uses the Agent Skills format with a required `SKILL.md`.

### Workspace installation

For only the current project:

```text
<workspace-root>/.agents/skills/dead-code-audit/SKILL.md
```

Example:

```text
your-project/
├── .agents/
│   └── skills/
│       └── dead-code-audit/
│           └── SKILL.md
├── src/
└── ...
```

You can clone this repository and copy the skill:

```bash
git clone https://github.com/Aashutosh2021/dead-code-audit.git /tmp/dead-code-audit
mkdir -p .agents/skills/dead-code-audit
cp /tmp/dead-code-audit/SKILL.md .agents/skills/dead-code-audit/SKILL.md
```

Workspace skills are useful when you want to commit the skill with a project and share it with your team.

### Global installation

To make the skill available across Antigravity projects:

```text
~/.gemini/config/skills/dead-code-audit/SKILL.md
```

Antigravity also supports its CLI skill locations. For CLI-specific setups, follow the current Antigravity documentation for the surface you are using.

### Invoke the skill

Antigravity can automatically discover relevant skills.

You can also explicitly invoke it:

```text
/dead-code-audit
```

Then give the agent the audit task, for example:

```text
Perform a complete dead-code and dead-feature audit of this repository.
Keep the entire operation READ-ONLY.
Do not modify anything.
Show the complete findings on screen.
```

---

# Quick Installation

## Claude Code

```bash
mkdir -p .claude/skills/dead-code-audit
curl -L https://raw.githubusercontent.com/Aashutosh2021/dead-code-audit/main/SKILL.md \
  -o .claude/skills/dead-code-audit/SKILL.md
```

Then:

```text
/dead-code-audit
```

## Antigravity

```bash
mkdir -p .agents/skills/dead-code-audit
curl -L https://raw.githubusercontent.com/Aashutosh2021/dead-code-audit/main/SKILL.md \
  -o .agents/skills/dead-code-audit/SKILL.md
```

Then:

```text
/dead-code-audit
```

> If your environment does not have `curl`, download `SKILL.md` from this repository and place it manually in the appropriate skill directory.

---

# Example Prompts

### Basic audit

```text
Run a complete dead-code and dead-feature audit of this repository.

Use the dead-code-audit skill.
Keep everything READ-ONLY.
Do not modify anything.
Show the complete audit report on screen.
```

### Deep audit

```text
Use /dead-code-audit.

Analyze the entire project from scratch before making any conclusions.

Trace frontend, backend, APIs, routes, navigation, services,
database, dependencies, configuration, workers, WebSockets,
scripts, tests, build configuration, and deployment structure.

Find and verify dead code, abandoned features, duplicate
implementations, unused dependencies, legacy APIs, and debug/test
exposure.

Do not modify any project files.
```

---

# Example Finding

```text
### Confirmed Dead Finding

Location:
src/services/legacy/LegacyAuthService.ts

Symbol:
LegacyAuthService

What it does:
Provides the previous authentication implementation.

Why it appears dead:
The active authentication flow uses AuthService instead.

Evidence / Usage Trace:
- No active imports
- No route references
- No dependency-injection registration
- No runtime registration
- No configuration references
- No build/script references

Risk:
Low

Confidence:
High

Recommended Action:
Verify external consumers before removal.
```

---

# False Positive Protection

The skill explicitly protects against incorrectly removing code that may be required indirectly.

Examples include:

- Framework entry points
- Auto-discovered modules
- Dependency-injected services
- Build plugins
- CLI commands
- ORM models
- Database migrations
- Serverless handlers
- Runtime-loaded modules
- Reflection targets
- Serialization types
- Generated-code dependencies
- Deployment-only configuration

If static analysis cannot conclusively determine usage, the finding should be classified as:

```text
Potentially Used / Needs Verification
```

rather than:

```text
Confirmed Dead
```

---

# Recommended Workflow

```text
Repository
    │
    ▼
Discover Project Structure
    │
    ▼
Reconstruct Architecture
    │
    ▼
Identify Runtime Entry Points
    │
    ▼
Trace Dependencies & Usage
    │
    ├── Imports / Exports
    ├── Routes / Navigation
    ├── APIs
    ├── Services
    ├── Database
    ├── Events / WebSockets
    ├── Workers / Queues
    ├── Configuration
    └── Build / Deployment
    │
    ▼
Check Indirect Usage
    │
    ▼
Classify Findings
    │
    ▼
Assess Risk & Confidence
    │
    ▼
Generate Complete Audit
    │
    ▼
NO MODIFICATIONS
```

---

# Compatibility

The skill follows the `SKILL.md` Agent Skills structure used by modern coding agents.

Known supported surfaces include:

- Claude Code
- Google Antigravity IDE
- Google Antigravity CLI
- Other agents that support the Agent Skills standard

Agent-specific installation paths can change over time. If an agent's current documentation differs from the paths above, use the agent's current official documentation.

---

# Contributing

Contributions are welcome.

When improving the skill:

1. Keep the audit READ-ONLY.
2. Do not weaken evidence requirements.
3. Avoid false positives.
4. Do not classify code as dead based only on naming or lack of an obvious import.
5. Preserve explicit uncertainty.
6. Keep the final report evidence-based.
7. Test changes against multiple project architectures when possible.

---

# License

MIT License.

See [`LICENSE`](LICENSE) for details.
