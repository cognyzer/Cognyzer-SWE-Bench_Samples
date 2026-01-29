# Java Samples

SWE-bench style benchmark samples for Java.

## Samples

| Instance ID | Repository | Description | Status |
|-------------|------------|-------------|--------|
| apache__commons-lang-1571 | apache/commons-lang | Bug fix | Structure ready |
| apache__commons-lang-1577 | apache/commons-lang | Bug fix | Structure ready |
| google__gson-2951 | google/gson | Bug fix | Structure ready |

## Structure

Each sample folder contains:
- `instance.json` - Main metadata
- `patches/code_fix.diff` - The gold solution
- `patches/test_additions.diff` - Test changes
- `docker/Dockerfile` - Maven test environment

## Test Command

```bash
mvn test -B
```

## Docker Validation

To validate F2P/P2P tests:

```bash
cd <instance_id>/docker
docker build -t java-test .
docker run java-test mvn test -B
```

## Note

These samples have the complete structure but need Docker validation to populate F2P/P2P test results. The structure includes:
- Problem statement from GitHub issue/PR
- Gold patch (code fix)
- Test patch (test additions)
- Dockerfile for reproducible Maven environment
