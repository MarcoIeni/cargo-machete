<div align="center">
  <h1><code>cargo-machete</code></h1>

  <p>
    <strong>Remove unused Rust dependencies with this one weird trick!</strong>
  </p>

  <p>
    <a href="https://github.com/bnjbvr/cargo-machete/actions?query=workflow%3ARust"><img src="https://github.com/bnjbvr/cargo-machete/workflows/Rust/badge.svg" alt="build status" /></a>
    <a href="https://matrix.to/#/#cargo-machete:delire.party"><img src="https://img.shields.io/badge/matrix-join_chat-brightgreen.svg" alt="matrix chat" /></a>
    <img src="https://img.shields.io/badge/rustc-stable+-green.svg" alt="supported rustc stable" />
  </p>
</div>

## Introduction

`cargo-machete` is a Cargo tool that detects unused dependencies in Rust
projects, in a fast (yet imprecise) way.

See also the [blog post](https://blog.benj.me/2022/04/27/cargo-machete/) for a
detailed writeup.

## Installation

Install `cargo-machete` with cargo:

`cargo install cargo-machete`

## Usage

Run cargo-machete in a directory that contains one or more Rust projects (using Cargo for
dependency management):

```bash
cd my-directory && cargo machete

# alternatively

cargo machete /absolute/path/to/my/directory
```

The **return code** gives an indication whether unused dependencies have been found:

- 0 if machete found no unused dependencies,
- 1 if it found at least one unused dependency,
- 2 if there was an error during processing (in which case there's no indication whether any unused
  dependency was found or not).

This can be used in CI situations.

### False positives

To ignore a certain set of dependencies in a crate, add
`package.metadata.cargo-machete` to `Cargo.toml`, and specify an `ignored` array:

For example:

```toml
[dependencies]
prost = "0.10" # Used in code generated by build.rs output, which cargo-machete cannot check

[package.metadata.cargo-machete]
ignored = ["prost"]
```

If there are too many false positives, consider using the `--with-metadata` CLI
flag, which will call `cargo metadata --all-features` to find final dependency
names, more accurate dependencies per build type, etc. ⚠ This may modify the
`Cargo.lock` files in your projects.

## Cargo Machete Action

A github action for cargo machete.

### Example usage

The step given by,
```
      - uses: bnjbvr/cargo-machete@main
```
can be added to any workflow.

An example workflow is shown below:

```yaml
name: Cargo Machete
on:
  pull_request: { branches: "*" }

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Machete
        uses: bnjbvr/cargo-machete@main
```

## Contributing

[![Contributor Covenant](https://img.shields.io/badge/contributor%20covenant-v1.4-ff69b4.svg)](https://www.contributor-covenant.org/version/1/4/code-of-conduct/)

We welcome community contributions to this project.

## License

[MIT license](LICENSE.md).
