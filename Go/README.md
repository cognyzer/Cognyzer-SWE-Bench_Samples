# Go Samples

This folder contains SWE-bench style benchmark samples for Go.

## Samples

| Instance ID | Repository | F2P Tests | P2P Tests | Difficulty |
|-------------|------------|-----------|-----------|------------|
| open-policy-agent__opa-7155 | open-policy-agent/opa | 677 | 19820 | medium |
| open-policy-agent__opa-7153 | open-policy-agent/opa | 196 | 19048 | medium |

## Structure

Each sample folder contains:
- `instance.json` - Main metadata and test results
- `patches/code_fix.diff` - The gold solution (code changes)
- `patches/test_additions.diff` - Test changes that verify the fix
- `docker/Dockerfile` - Reproducible test environment
- `logs/` - Test execution logs (if available)

## Usage

1. **Reproduce the environment:**
   ```bash
   cd <instance_id>/docker
   docker build -t test-env .
   ```

2. **Run tests before fix (should see F2P tests fail):**
   ```bash
   docker run test-env <test_command>
   ```

3. **Apply the fix and re-run (all tests should pass):**
   ```bash
   # Apply patches/code_fix.diff
   # Re-run tests
   ```
