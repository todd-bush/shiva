# GitHub Actions Workflows

This repository uses several GitHub Actions workflows to automate CI/CD processes.

## Workflows

### 1. CI (`ci.yml`)

**Triggers:** Push to `main`/`develop` branches, Pull Requests to `main`

**Jobs:**

- **Test**: Runs on multiple platforms (Linux, Windows, macOS)
  - Builds the project with all features
  - Runs tests
  - Runs clippy linting
  - Checks code formatting
- **Test WASM**: Builds and tests the WebAssembly target
- **Security**: Runs `cargo audit` for security vulnerabilities
- **Coverage**: Generates code coverage reports using cargo-tarpaulin

### 2. Release (`release.yml`)

**Triggers:** Push of version tags (e.g., `v1.0.0`)

**Jobs:**

- **Build**: Builds release binaries for multiple platforms
- **Create Release**: Creates GitHub release with assets
- **Publish Crate**: Publishes to crates.io (requires `CARGO_REGISTRY_TOKEN` secret)

### 3. Documentation (`docs.yml`)

**Triggers:** Push to `main`, Pull Requests to `main`

**Jobs:**

- **Build Documentation**: Builds API docs and mdBook documentation
- **Deploy Documentation**: Deploys docs to GitHub Pages (main branch only)

## Dependabot Configuration

The repository uses Dependabot to automatically update dependencies:

- **Rust dependencies**: Weekly updates on Mondays
- **GitHub Actions**: Weekly updates on Mondays
- **npm dependencies**: Weekly updates on Mondays (if any)

## Required Secrets

To use all workflows, you need to set up these secrets in your repository:

1. **`CARGO_REGISTRY_TOKEN`**: Your crates.io API token for publishing
2. **`GITHUB_TOKEN`**: Automatically provided by GitHub

## Local Development

To run the same checks locally:

```bash
# Install Rust toolchain
rustup default stable

# Install additional tools
cargo install cargo-tarpaulin
cargo install mdbook

# Run checks
cargo build --all-features
cargo test --all-features
cargo clippy --all-features -- -D warnings
cargo fmt --all -- --check
cargo audit
cargo tarpaulin --out Html

# Build documentation
cargo doc --no-deps --all-features
mdbook build  # if book.toml exists
```

## Workflow Status Badges

Add these badges to your README.md:

```markdown
![CI](https://github.com/igumnoff/shiva/workflows/CI/badge.svg)
![Release](https://github.com/igumnoff/shiva/workflows/Release/badge.svg)
![Documentation](https://github.com/igumnoff/shiva/workflows/Documentation/badge.svg)
```
