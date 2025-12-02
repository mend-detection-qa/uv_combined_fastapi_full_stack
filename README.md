# Phase 4 Combined Integration Test: FastAPI Full Stack

## Test ID
T-P4-001

## Category
Combined Integration - Real-World Web Application

## Priority
P1

## Description
This fixture simulates a production-ready FastAPI application that combines multiple UV features commonly found in real-world web projects.

## Features Combined
1. **Runtime Dependencies** - FastAPI, Uvicorn, SQLAlchemy, Pydantic
2. **Development Dependencies** - pytest, ruff, mypy, pytest-cov
3. **Optional Dependencies** - PostgreSQL support, Redis support, S3 support
4. **Dependency Groups (PEP 735)** - dev, test, docs groups
5. **Version Constraints** - Mix of exact pins, ranges, and compatible releases
6. **Transitive Dependencies** - Deep dependency tree with shared dependencies

## Real-World Inspiration
Based on patterns from:
- gustavocadev/example-fastapi-uv-ruff
- astral-sh/uv-fastapi-example
- Production FastAPI applications with full testing setup

## Expected Dependency Tree Structure
```
fastapi-full-stack
├── Runtime Dependencies (7 direct)
│   ├── fastapi>=0.109.0
│   │   ├── pydantic>=2.0.0
│   │   ├── starlette>=0.36.0
│   │   └── typing-extensions
│   ├── uvicorn[standard]>=0.27.0
│   │   ├── click
│   │   ├── h11
│   │   └── uvloop (optional extra)
│   ├── sqlalchemy>=2.0.0
│   ├── python-dotenv>=1.0.0
│   └── ...
├── Development Dependencies (6 direct)
│   ├── pytest>=8.0.0
│   ├── pytest-cov>=4.1.0
│   ├── pytest-asyncio>=0.23.0
│   ├── ruff>=0.3.0
│   ├── mypy>=1.8.0
│   └── httpx>=0.27.0 (for testing)
└── Optional Dependencies
    ├── postgres group: psycopg2-binary, asyncpg
    ├── redis group: redis
    └── aws group: boto3
```

## Test Objectives
1. Verify correct separation of runtime vs development dependencies
2. Verify optional dependency groups are properly identified
3. Verify deep transitive dependency resolution (depth 3-4)
4. Verify shared dependencies (typing-extensions used by multiple packages)
5. Verify extras syntax (uvicorn[standard]) is handled correctly
6. Verify PEP 735 dependency groups are parsed correctly

## Success Criteria
- [ ] All 7 runtime dependencies identified
- [ ] All 6 development dependencies identified
- [ ] All 3 optional dependency groups identified
- [ ] Transitive dependencies correctly mapped to parents
- [ ] Shared dependencies appear in multiple dependency paths
- [ ] Total dependency count: ~35-40 packages
- [ ] Max depth: 3-4 levels

## UV Version Compatibility
- Minimum: UV 0.4.0+ (for dependency groups)
- Recommended: UV 0.7.0+ (for enhanced metadata)

## Files in this Fixture
- `pyproject.toml` - Project configuration with all dependencies
- `uv.lock` - Lock file with resolved dependency tree
- `README.md` - This file
- `expected_dependencies.json` - Expected dependency tree structure

## Usage
```bash
# Generate lock file
uv lock

# Run dependency tree builder
dependency-tree-builder scan . > output.json

# Compare with expected output
diff output.json expected_dependencies.json
```

## Mend Integration Notes
This fixture is particularly useful for testing Mend integration:
- Tests dev dependency filtering
- Tests optional dependency handling
- Tests transitive vulnerability tracking
- Tests dependency tree visualization in Mend platform

## Related Tests
- T-P1-001: Basic Dependency Tree Resolution
- T-P1-002: Development Dependencies Resolution
- T-P2-008: Optional Dependencies
- T-P2-002: Complex pyproject.toml