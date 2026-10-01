---
name: dead-code-audit
description: "Perform a complete read-only audit of a software project to identify confirmed dead code, abandoned features, unused APIs, routes, dependencies, database artifacts, configuration, assets, duplicate implementations, legacy logic, and debug/test exposure. Use this skill when the user asks to audit, detect, identify, or report dead or unused code or features. Never modify the project."
---

# Dead Code & Dead Feature Audit

## Skill Objective

Perform a **complete, evidence-based, READ-ONLY audit** of the entire project.

The objective is to determine what code, features, APIs, configuration, dependencies, database artifacts, assets, background processes, and documentation are:

* Confirmed dead
* Likely dead
* Potentially used
* Duplicate or legacy
* Debug/test exposed
* Abandoned or partially implemented

The audit must be based on actual repository evidence rather than filenames, assumptions, or superficial searches.

---

# 1. NON-NEGOTIABLE READ-ONLY RULE

This skill is strictly analytical.

### NEVER:

* Modify files
* Delete files
* Rename files
* Move files
* Refactor code
* Rewrite code
* Generate replacement code
* Install packages
* Uninstall packages
* Upgrade packages
* Downgrade packages
* Modify configuration
* Modify environment files
* Modify databases
* Run destructive commands
* Automatically format files
* Automatically fix issues
* Create patches
* Commit changes
* Push changes
* Change deployment configuration

Do not perform any action that changes project state.

If a command could modify the repository, do not execute it.

The only permitted output is the audit report.

---

# 2. CORE PRINCIPLE

Never classify something as dead merely because it appears unused.

A symbol/file/dependency/feature is considered dead only after tracing its possible usage through the relevant project layers.

Consider:

```text
File
 ↓
Imports
 ↓
Exports
 ↓
Components / Services
 ↓
Routes / Navigation
 ↓
API calls
 ↓
Backend endpoints
 ↓
Database
 ↓
Workers / Jobs
 ↓
Configuration
 ↓
Build system
 ↓
Runtime entry points
 ↓
Deployment
```

Also investigate dynamic usage where applicable:

* Dynamic imports
* Reflection
* Dependency injection
* String-based route registration
* String-based event handlers
* Plugin systems
* Runtime module loading
* Code generation
* Framework conventions
* Annotation/decorator-based registration
* Environment-controlled features
* Feature flags
* Build-time replacement
* Serverless handlers
* Auto-discovered files
* ORM conventions
* Serialization/deserialization
* WebSocket event names
* CLI command registration
* Background workers

---

# 3. AUDIT WORKFLOW

Follow this sequence.

Do not skip directly to conclusions.

## Phase 1 — Repository Discovery

First inspect the complete project structure.

Identify:

* Root directories
* Applications
* Frontend
* Backend
* Mobile/Desktop clients
* Shared libraries
* Packages
* Services
* Database layer
* API layer
* Workers
* Scripts
* Tests
* Build tooling
* Deployment configuration
* Documentation
* Generated directories
* Configuration files

Determine the project type and technologies.

Examples:

```text
React
Next.js
Vue
Angular
Android
Kotlin
Java
Node.js
Express
NestJS
Python
FastAPI
Django
Spring
.NET
Electron
React Native
Flutter
etc.
```

Do not assume the architecture from the README alone.

---

# 4. Phase 2 — Architecture Reconstruction

Before identifying dead code, build a mental model of the complete system.

Determine:

### Frontend

* Application entry points
* Screens/pages
* Components
* Hooks
* State management
* Services
* API clients
* Navigation
* Routing
* Assets
* Feature modules

### Backend

* Application entry points
* Controllers
* Routes
* Middleware
* Services
* Utilities
* Authentication
* Authorization
* Database access
* Workers
* Queues
* Cron jobs
* WebSockets
* Error handling

### Database

Identify:

* Models
* Schemas
* Entities
* Tables
* Collections
* Migrations
* Repositories
* Queries
* Relations
* Indexes
* Seed scripts

### Infrastructure

Inspect:

* Environment variables
* Config files
* Docker
* CI/CD
* Deployment configuration
* Build scripts
* Runtime scripts
* Serverless configuration
* Hosting configuration

### Testing

Identify:

* Unit tests
* Integration tests
* E2E tests
* Test utilities
* Fixtures
* Mocks
* Test-only APIs
* Test configuration

---

# 5. Phase 3 — Runtime Entry-Point Analysis

Identify every actual runtime entry point.

Examples:

```text
main
index
server
app
bootstrap
worker
CLI entry
desktop entry
mobile entry
serverless handler
cron entry
```

Trace execution from these entry points.

Determine which modules can actually participate in runtime behavior.

Do not assume a file is dead simply because it has no obvious direct import.

---

# 6. Phase 4 — Usage Tracing

For every suspicious item, trace usage.

Check:

### Imports

* Direct imports
* Re-exported imports
* Barrel files
* Index files
* Package exports

### Routes

* Route registration
* Navigation
* Deep links
* Redirects
* Dynamic routes
* Server routes
* API routes

### APIs

Trace:

```text
Frontend caller
→ API client
→ HTTP request
→ Backend route
→ Controller
→ Service
→ Database
```

### Database

Trace:

```text
Model
→ Repository
→ Query
→ Service
→ API / Worker
```

### Configuration

Trace:

```text
Environment variable
→ Config loader
→ Runtime consumer
→ Feature behavior
```

### Events

Trace:

```text
Event declaration
→ Event registration
→ Event emitter
→ Listener
```

Include:

* WebSockets
* Event buses
* Message queues
* Pub/Sub
* Background jobs

---

# 7. Phase 5 — Dead-Code Categories

Search for all of the following.

## A. Unused APIs

Find:

* Uncalled endpoints
* Orphaned controllers
* Old API versions
* Duplicate endpoints
* Deprecated endpoints
* Internal endpoints with no consumers
* Test/debug endpoints exposed in production

---

## B. Unused Screens / Pages / Routes

Find:

* Screens with no navigation path
* Routes never registered
* Routes registered but unreachable
* Duplicate screens
* Old screens
* Abandoned UI flows
* Hidden screens
* Debug screens

Check:

```text
Navigation
→ Route
→ Screen
→ Components
→ Services/API
```

---

## C. Unused Components

Investigate:

* Components
* Hooks
* Utilities
* Helpers
* Services
* Classes
* Functions
* Interfaces
* Types
* Constants

Check direct and indirect usage.

---

## D. Dependencies

Inspect dependency declarations and determine whether each dependency is:

* Directly imported
* Used through configuration
* Used through plugins
* Used through build tooling
* Used by scripts
* Used only in tests
* Used indirectly by framework conventions
* Actually unused

Never recommend dependency removal solely from package.json/build.gradle/etc.

Check actual project usage first.

---

## E. Abandoned Features

Look for:

* TODO implementations
* Placeholder implementations
* Partially implemented flows
* UI without backend support
* Backend functionality without UI consumers
* Feature flags that are permanently disabled
* Feature flags that are permanently enabled
* Dead branches
* Incomplete migrations
* Old implementations replaced by newer ones

---

## F. Duplicate Implementations

Identify multiple implementations of the same responsibility.

Examples:

```text
OldAuthService
NewAuthService

LegacyApiClient
ApiClient

OldPlayer
NewPlayer

ServiceA
ServiceB
```

Do not automatically classify duplicates as dead.

Determine:

* Which implementation is active
* Which implementation is referenced
* Whether both serve different contexts
* Whether one is legacy
* Whether migration is incomplete

---

## G. Debug/Test Exposure

Find:

* Debug routes
* Test endpoints
* Development screens
* Test credentials
* Mock services
* Development bypasses
* Debug logging
* Internal admin routes
* Diagnostic endpoints
* Development-only middleware

Determine whether they can be exposed in production.

Classify these separately from normal dead code.

---

## H. Database Dead Artifacts

Inspect:

* Models
* Fields
* Collections
* Tables
* Queries
* Repositories
* Migrations
* Indexes
* Seeds
* Relationships

Determine whether each database artifact has actual consumers.

Do not assume an unused model means the corresponding database collection/table is unused.

---

## I. Configuration

Inspect:

* Environment variables
* Config objects
* Feature flags
* Build flags
* Runtime settings
* Deployment variables
* API keys/configuration references

Trace configuration values to actual consumers.

---

## J. Assets

Check:

* Images
* Icons
* Fonts
* Animations
* Videos
* Localizations
* Translation keys
* Static files

Consider framework-specific asset loading and dynamic references before declaring an asset unused.

---

## K. Background Processing

Inspect:

* Workers
* Queues
* Consumers
* Producers
* Cron jobs
* Scheduled tasks
* Background services
* Task processors
* Event handlers

Determine whether each has an actual trigger and consumer.

---

## L. Documentation

Find documentation that refers to:

* Removed APIs
* Old features
* Deprecated commands
* Deleted configuration
* Legacy architecture
* Non-existent routes
* Obsolete setup steps

Classify stale documentation separately from executable dead code.

---

# 8. INDIRECT USAGE CHECK

Before declaring something dead, explicitly consider indirect usage.

Check for:

```text
Dynamic imports
Reflection
Dependency injection
Decorators
Annotations
Convention-based discovery
String references
Runtime registration
Plugin loading
Generated code
Build configuration
Framework auto-discovery
Environment variables
Feature flags
CLI registration
ORM discovery
Serialization
WebSocket event names
```

If indirect usage cannot be conclusively verified, classify the item as:

> Potentially Used / Needs Verification

Do not classify it as Confirmed Dead.

---

# 9. CLASSIFICATION SYSTEM

Every finding must belong to one of these categories.

## Confirmed Dead

Use only when strong repository evidence demonstrates that the item has no reachable or meaningful usage.

Evidence should include multiple relevant checks where applicable.

---

## Likely Dead

Use when available evidence strongly indicates the item is unused, but some uncertainty remains.

Explain the uncertainty.

---

## Potentially Used / Needs Verification

Use when static analysis cannot safely determine usage.

Typical causes:

* Dynamic loading
* External consumers
* Reflection
* Runtime configuration
* Framework conventions
* External API consumers
* Deployment-specific behavior

---

## Duplicate / Legacy

Use when multiple implementations exist and evidence indicates one or more are obsolete, duplicated, or part of an incomplete migration.

---

## Debug/Test Exposure

Use for debug/test/development functionality that may remain accessible in an inappropriate runtime environment.

This category should not automatically mean "dead."

---

# 10. EVIDENCE STANDARD

Every finding must have evidence.

Weak evidence:

```text
"I couldn't find an import."
```

Strong evidence:

```text
No imports
+ no exports
+ no route registration
+ no navigation reference
+ no API consumer
+ no runtime registration
+ no configuration reference
+ no build/script reference
```

The required evidence depends on the artifact type.

Never manufacture evidence.

Never infer runtime behavior without repository evidence.

---

# 11. FALSE POSITIVE ANALYSIS

Explicitly identify code that appears unused but must be retained.

Examples:

* Framework entry points
* Auto-discovered modules
* Dependency-injected services
* Build plugins
* CLI commands
* ORM models
* Migration files
* Serverless handlers
* Runtime-loaded modules
* Reflection targets
* Serialization types
* Generated-code dependencies
* Test infrastructure
* Deployment-only configuration

For every important false positive explain:

```text
Location
→ Why it looks unused
→ Actual usage mechanism
→ Why it must be kept
```

---

# 12. DUPLICATE LOGIC ANALYSIS

When duplicate functionality is found, compare:

* Purpose
* Inputs
* Outputs
* Callers
* Runtime path
* Business rules
* Error handling
* Authentication
* Database behavior
* Performance characteristics

Do not decide which implementation should be deleted unless repository evidence clearly establishes that one is obsolete.

The audit is analytical, not a refactoring operation.

---

# 13. CONFIDENCE LEVEL

Every finding must contain a confidence level.

Use:

```text
High
Medium
Low
```

### High

Direct repository evidence conclusively supports the finding.

### Medium

Evidence strongly suggests the finding but indirect usage cannot be completely excluded.

### Low

The finding is suspicious but requires external/runtime verification.

Never use confidence as a substitute for evidence.

---

# 14. RISK ASSESSMENT

For each finding assess risk.

Consider:

* Security
* Production exposure
* Data integrity
* Runtime behavior
* API compatibility
* Authentication/authorization
* Database impact
* Build/deployment impact
* Maintenance complexity
* Developer confusion

Use:

```text
Critical
High
Medium
Low
Informational
```

Risk describes the consequence of the finding, not whether the code is definitely dead.

---

# 15. REQUIRED FINDING FORMAT

For every finding use this structure:

```text
### [Classification] Finding

Location:
<exact file path>

Symbol / Endpoint / Route:
<exact identifier>

What it does:
<short explanation>

Why it appears dead / legacy / duplicated:
<reason>

Evidence / Usage Trace:
<actual references and trace>

Runtime Path:
<if applicable>

Risk:
<Critical / High / Medium / Low / Informational>

Confidence:
<High / Medium / Low>

Recommended Action:
<what should be investigated or done later>

Verification Needed:
<if applicable>
```

Never omit the exact location.

---

# 16. PROJECT DEPENDENCY & FEATURE MAP

Create a high-level dependency map showing important relationships.

Example:

```text
UI Screen
  ↓
ViewModel
  ↓
Repository
  ↓
API Client
  ↓
Backend Endpoint
  ↓
Service
  ↓
Database
```

Also identify orphaned sections of the graph.

Example:

```text
LegacyService
      ↓
LegacyRepository
      ↓
No active consumer
```

This helps distinguish isolated dead code from code that is merely indirectly referenced.

---

# 17. FINAL REPORT

The final response must be displayed completely on screen.

Do not create or modify report files unless the user explicitly asks for an exported report.

The report must contain exactly these major sections:

# 1. Executive Summary

Include:

* Project size/scope
* Architecture summary
* Number of findings
* Major categories
* Most important risks
* Important uncertainties

Do not exaggerate findings.

---

# 2. Project Architecture Understanding

Explain the architecture actually discovered.

Include:

* Frontend
* Backend
* Database
* APIs
* Routing
* Services
* Workers
* Configuration
* Testing
* Deployment

---

# 3. Confirmed Dead Code

List all confirmed dead code.

---

# 4. Likely Dead Code

List all likely dead code.

---

# 5. Dead Features

Identify abandoned or partially implemented functionality.

---

# 6. Unused APIs / Endpoints

List endpoint evidence and consumers.

---

# 7. Unused Screens / Routes

List navigation and routing evidence.

---

# 8. Unused Dependencies

List package/dependency evidence.

Include indirect usage checks.

---

# 9. Duplicate Implementations

Explain overlapping implementations and their usage.

---

# 10. Debug / Test / Legacy Exposure

Identify potentially exposed development or legacy functionality.

---

# 11. Dead Database / Config / Assets

Cover:

* Models
* Fields
* Queries
* Collections/tables
* Environment variables
* Feature flags
* Assets
* Translations
* Static resources

---

# 12. False Positives / Code That Must Be Kept

Explicitly document important items that appear unused but are actually required.

---

# 13. Dependency & Feature Dependency Map

Show important dependency chains and orphaned branches.

---

# 14. Risk Assessment

Summarize risks by category.

Do not turn risk into a subjective ranking of the codebase.

---

# 15. Cleanup Recommendations

Recommendations must be based on evidence.

Examples:

```text
Investigate removal
Confirm external consumers
Verify production deployment
Check dynamic loading
Confirm database usage
Remove only after verification
```

Do NOT perform cleanup.

---

# 18. NO-HALLUCINATION RULE

If evidence is unavailable, explicitly say:

```text
Not verifiable from static repository analysis.
```

Do not invent:

* Runtime behavior
* External consumers
* API clients
* Database usage
* Production usage
* Deployment behavior
* User behavior
* Historical implementation details

Separate:

```text
Observed
Inferred
Unknown
```

whenever necessary.

---

# 19. COMPLETENESS CHECK

Before finalizing the audit, verify that you have inspected:

* [ ] Repository structure
* [ ] Entry points
* [ ] Frontend
* [ ] Backend
* [ ] Routes
* [ ] APIs
* [ ] Services
* [ ] Components
* [ ] Hooks
* [ ] Utilities
* [ ] Dependencies
* [ ] Database
* [ ] Configuration
* [ ] Environment variables
* [ ] Feature flags
* [ ] Assets
* [ ] Background jobs
* [ ] Workers
* [ ] Queues
* [ ] WebSockets
* [ ] Scripts
* [ ] Tests
* [ ] Build configuration
* [ ] Deployment configuration
* [ ] Documentation

If any category cannot be inspected, explicitly mention it in the final report.

---

# 20. FINAL SAFETY CHECK

Before producing the final report, confirm internally:

```text
No project files were modified.
No files were deleted.
No files were renamed.
No dependencies were changed.
No configuration was changed.
No code was generated.
No cleanup was performed.
```

Then present the complete audit.

The audit must remain READ-ONLY from beginning to end.
