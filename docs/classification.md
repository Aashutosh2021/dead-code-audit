# Finding Classification

## Confirmed Dead

Use only when repository evidence strongly demonstrates that the item has no reachable or meaningful usage.

## Likely Dead

Evidence strongly suggests that the item is unused, but complete certainty is not possible.

## Potentially Used / Needs Verification

Use when external or dynamic behavior could still consume the item.

Examples:

- Dynamic imports
- Reflection
- External API consumers
- Framework conventions
- Runtime configuration

## Duplicate / Legacy

Multiple implementations perform substantially the same responsibility.

Do not automatically assume one is removable.

## Debug/Test Exposure

Development or testing functionality that may be reachable in an inappropriate environment.