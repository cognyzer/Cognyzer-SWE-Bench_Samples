# JavaScript Samples

This folder contains SWE-bench style benchmark samples for JavaScript.

## Samples

| Instance ID | Repository | F2P Tests | P2P Tests | Difficulty |
|-------------|------------|-----------|-----------|------------|
| facebook__react-32461 | facebook/react | 6 | 272 | hard |
| facebook__react-33941 | facebook/react | 1 | 164 | easy |

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
