# C++ Samples

SWE-bench style benchmark samples for C++.

## Samples

| Instance ID | Repository | Description | Status |
|-------------|------------|-------------|--------|
| nlohmann__json-5039 | nlohmann/json | Bug fix in JSON library | Structure ready |
| nlohmann__json-5052 | nlohmann/json | Bug fix in JSON library | Structure ready |

## Structure

Each sample folder contains:
- `instance.json` - Main metadata
- `patches/code_fix.diff` - The gold solution
- `patches/test_additions.diff` - Test changes
- `docker/Dockerfile` - CMake/Ninja build environment

## Test Command

```bash
cmake --build build && ctest --test-dir build -V
```

## Docker Validation

To validate F2P/P2P tests:

```bash
cd <instance_id>/docker
docker build -t cpp-test .
docker run cpp-test bash -c "mkdir -p build && cd build && cmake -G Ninja .. && ninja && ctest -V"
```

## Note

These samples have the complete structure but need Docker validation to populate F2P/P2P test results. The nlohmann/json library uses:
- CMake build system
- Catch2 test framework
- Ninja for fast builds
