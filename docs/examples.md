# Audit Examples

## Example: Confirmed Dead Function


```text
Location:
src/utils/legacyFormatter.ts

Symbol:
legacyFormatDate()

What it does:
Formats dates using the old formatting implementation.

Evidence:
- No imports
- No exports consumed by runtime code
- No route references
- No dynamic references
- No test references
- No build/script references

Classification:
Confirmed Dead

Confidence:
High

## Example: Potentially Used Dependency

Dependency:
some-framework-plugin

Evidence:
No direct source import found.

However:
- Referenced by build configuration
- Loaded through plugin configuration

Classification:
Potentially Used / Needs Verification

Action:
Do not remove without verifying the build pipeline.

## Example: Legacy Implementation

Location:
src/services/LegacyAuthService.ts

Evidence:
A newer AuthService exists and is used by all active authentication routes.

LegacyAuthService has:
- No active callers
- No route registration
- No runtime registration

Classification:
Duplicate / Legacy
```
