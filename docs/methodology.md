# Audit Methodology

## 1. Discover

Map the entire repository.

## 2. Understand

Reconstruct the architecture and runtime entry points.

## 3. Trace

Follow imports, exports, routes, APIs, services, database access, events and configuration.

## 4. Check Indirect Usage

Investigate dynamic imports, DI, reflection, framework conventions, feature flags and runtime registration.

## 5. Classify

Assign each finding:

- Confirmed Dead
- Likely Dead
- Potentially Used / Needs Verification
- Duplicate / Legacy
- Debug/Test Exposure

## 6. Verify

Cross-check findings against:

- Build configuration
- Tests
- Scripts
- Deployment
- Documentation
- Runtime entry points

## 7. Report

Present evidence and confidence.

## 8. Do Not Modify

The repository remains completely unchanged.