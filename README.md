# Cognyzer SWE-Bench Samples

Production-grade SWE-bench style benchmark samples for evaluating code generation models.

## Overview

This benchmark contains **21 samples** across **8 programming languages**:

| Language | Samples | F2P Tests | P2P Tests | Status |
|----------|---------|-----------|-----------|--------|
| Python | 4 | 41 | 2,277 | Validated |
| TypeScript | 2 | 190 | 49 | Validated |
| JavaScript | 2 | 7 | 436 | Validated |
| Ruby | 2 | 5 | 173 | Validated |
| Rust | 2 | 7 | 3600 | Validated |
| Go | 2 | 873 | 38868 | Validated |
| Java | 3 | - | - | Structure ready |
| C++ | 2 | - | - | Structure ready |

## What is SWE-Bench?

SWE-bench is a benchmark for evaluating LLMs on real-world software engineering tasks. Each sample contains:

1. **Problem Statement** - A bug report or feature request from a real GitHub issue
2. **Base Commit** - The codebase state before the fix
3. **Gold Patch** - The actual fix that was merged (ground truth)
4. **Test Patch** - Tests that verify the fix works
5. **F2P (Fail-to-Pass)** - Tests that FAIL before the fix, PASS after
6. **P2P (Pass-to-Pass)** - Regression tests that should always pass

## Folder Structure

```
cognyzer_samples/
├── README.md                    # This file
├── benchmark.json               # Summary statistics
├── sft_data/                    # Training data for fine-tuning
│   └── combined_sft.jsonl       # All SFT samples
│
├── Python/
│   ├── README.md
│   ├── deepset-ai__haystack-8813/
│   ├── Lightning-AI__pytorch-lightning-20379/
│   ├── pallets__click-2940/
│   └── pallets__click-3004/
│
├── TypeScript/
│   ├── apollographql__apollo-client-12236/
│   └── facebook__lexical-7544/
│
├── JavaScript/
│   ├── facebook__react-32461/
│   └── facebook__react-33941/
│
├── Ruby/
│   ├── rails__rails-53890/
│   └── rails__rails-54133/
│
├── Rust/
│   ├── rust-lang__cargo-14830/
│   └── rust-lang__mdBook-2796/
│
├── Go/
│   ├── open-policy-agent__opa-7153/
│   └── open-policy-agent__opa-7155/
│
├── Java/
│   ├── apache__commons-lang-1571/
│   ├── apache__commons-lang-1577/
│   └── google__gson-2951/
│
└── C++/
    ├── nlohmann__json-5039/
    └── nlohmann__json-5052/
```

## Sample Structure

Each sample folder contains:

```
<instance_id>/
├── instance.json           # Main metadata and test results
├── patches/
│   ├── code_fix.diff       # The gold solution (code changes)
│   └── test_additions.diff # Test changes that verify the fix
├── docker/
│   └── Dockerfile          # Reproducible test environment
└── logs/                   # Test execution logs (if validated)
    ├── base.log
    ├── before.log
    └── after.log
```

## Instance JSON Schema

```json
{
  "instance_id": "owner__repo-123",
  "repo": "owner/repo",
  "language": "Python",
  "base_commit": "abc123...",
  "patch_commit": "def456...",
  "problem_statement": "Description of the bug...",
  "difficulty": "medium",
  "test_command": "pytest tests/",
  "files_changed": ["src/module.py", "tests/test_module.py"],
  "gold_patch": "diff --git a/...",
  "test_patch": "diff --git a/...",
  "fail_to_pass": ["test_module::test_function"],
  "pass_to_pass": ["test_module::test_other"]
}
```

## Evaluation Criteria

A model-generated patch is considered **successful** if:
1. All F2P tests pass after applying the patch
2. All P2P tests continue to pass (no regressions)

## SFT Training Data

The `sft_data/` folder contains training data derived from these samples:
- **bug_fix** - Problem statement → Solution patch
- **test_writing** - Problem statement → Test patch

Format: JSONL with fields `task_type`, `instance_id`, `prompt`, `completion`, `metadata`

## Usage Example

```python
import json

# Load a sample
with open("Python/deepset-ai__haystack-8813/instance.json") as f:
    sample = json.load(f)

# Access fields
problem = sample["problem_statement"]
solution = sample["gold_patch"]
f2p_tests = sample["fail_to_pass"]
p2p_tests = sample["pass_to_pass"]

# Evaluate a model-generated patch
def evaluate(model_patch, sample):
    # 1. Apply model_patch to codebase at base_commit
    # 2. Run tests
    # 3. Check: all F2P pass AND all P2P pass
    pass
```

## Docker Validation

To validate F2P/P2P tests for a sample:

```bash
cd <Language>/<instance_id>/docker
docker build -t test-env .

# Run tests at base (before fix) - F2P should FAIL
docker run test-env <test_command>

# Apply gold_patch, run again - all should PASS
```

## Repository Sources

| Language | Repositories |
|----------|--------------|
| Python | deepset-ai/haystack, Lightning-AI/pytorch-lightning, pallets/click |
| TypeScript | apollographql/apollo-client, facebook/lexical |
| JavaScript | facebook/react |
| Ruby | rails/rails |
| Rust | rust-lang/cargo, rust-lang/mdBook |
| Go | open-policy-agent/opa |
| Java | apache/commons-lang, google/gson |
| C++ | nlohmann/json |

## Created

2026-01-29

## Credits

This benchmark and sample dataset are created and maintained by **[Cognyzer](https://github.com/cognyzer)**.

If you find this helpful, please consider giving us a **★ Star** on GitHub: [Cognyzer-SWE-Bench_Samples](https://github.com/cognyzer/Cognyzer-SWE-Bench_Samples)
