# Python Samples

This folder contains SWE-bench style benchmark samples for Python.

## Samples

| Instance ID | Repository | F2P Tests | P2P Tests | Difficulty |
|-------------|------------|-----------|-----------|------------|
| deepset-ai__haystack-8813 | deepset-ai/haystack | 20 | 69 | hard |
| Lightning-AI__pytorch-lightning-20379 | Lightning-AI/pytorch-lightning | 18 | 53 | medium |
| pallets__click-2940 | pallets/click | 1 | 1284 | medium |
| pallets__click-3004 | pallets/click | 2 | 871 | medium |

## Statistics

- **Total Samples**: 4
- **Total F2P Tests**: 41  
- **Total P2P Tests**: 2,277
- **Status**: Validated

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
   docker run test-env pytest -xvs
   ```

3. **Apply the fix and re-run (all tests should pass):**
   ```bash
   # Apply patches/code_fix.diff
   # Re-run tests
   ```

## Repository Sources

- **deepset-ai/haystack** - NLP/LLM framework for building search and Q&A systems
- **Lightning-AI/pytorch-lightning** - PyTorch training framework  
- **pallets/click** - Python CLI framework for creating beautiful command line interfaces
