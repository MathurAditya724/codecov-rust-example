# Codecov Rust Example

A Rust code coverage example using [getsentry/codecov-action](https://github.com/getsentry/codecov-action) with GitHub Actions.

## Overview

This repository demonstrates how to set up code coverage for a Rust project using modern tooling:

- **cargo-llvm-cov** - LLVM-based coverage instrumentation for Rust
- **getsentry/codecov-action** - Self-hosted coverage reporting using GitHub Artifacts (no external service or token required)

## How It Works

1. Tests run with `cargo llvm-cov` which instruments the code and generates LCOV coverage data
2. The coverage report is uploaded using `getsentry/codecov-action`
3. Coverage results are stored as GitHub Artifacts and compared against the base branch on PRs

## GitHub Actions Workflow

```yaml
- name: Install Rust toolchain
  uses: dtolnay/rust-toolchain@stable
  with:
    components: llvm-tools-preview

- name: Install cargo-llvm-cov
  uses: taiki-e/install-action@cargo-llvm-cov

- name: Run tests with coverage
  run: cargo llvm-cov --lcov --output-path lcov.info

- name: Upload coverage
  uses: getsentry/codecov-action@v1
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    files: ./lcov.info
    coverage-format: lcov
```

## Why getsentry/codecov-action?

Unlike the traditional Codecov service, this action:

- Uses **GitHub Artifacts** for storage — no external service required
- Only needs `GITHUB_TOKEN` — no separate Codecov token needed
- Provides **PR comments** and **status checks** out of the box
- Supports coverage thresholds and patch coverage enforcement

## Running Tests Locally

```bash
# Install cargo-llvm-cov
cargo install cargo-llvm-cov

# Run tests with coverage
cargo llvm-cov

# Generate LCOV report
cargo llvm-cov --lcov --output-path lcov.info
```

## References

- [getsentry/codecov-action](https://github.com/getsentry/codecov-action) - The GitHub Action used for coverage
- [cargo-llvm-cov](https://github.com/taiki-e/cargo-llvm-cov) - LLVM-based coverage tool for Rust
- [codecov/example-rust](https://github.com/codecov/example-rust) - Original Codecov Rust example (uses kcov)
