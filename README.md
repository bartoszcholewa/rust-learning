# Rust learning
Rust Learning Repository

# Installation
___
## [rustup.rs](https://rustup.rs/)  - Rust compiler installer (WSL2/Linux)
- rustup.rs - online script for installing rust compiler version control.
- cargo - package manager
- rustc - rust compiler
- rustdoc - documentation generator

### Install rustup
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
- `/home/bartosz/.rustup` - Rustup metadata and toolchains
- `/home/bartosz/.cargo` - The Cargo home directory
- `/home/bartosz/.cargo/bin` - The cargo, rustc, rustup and other commands

### Uninstall
```bash
rustup self uninstall
```

### Update
```bash
rustup update
```


### Post-installation

    To get started you may need to restart your current shell.
    This would reload your PATH environment variable to include
    Cargo's bin directory ($HOME/.cargo/bin).
    
    To configure your current shell, you need to source
    the corresponding env file under $HOME/.cargo.
    
    This is usually done by running one of the following (note the leading DOT):
    . "$HOME/.cargo/env"            # For sh/bash/zsh/ash/dash/pdksh
    source "$HOME/.cargo/env.fish"  # For fish

### Usefull commands
```bash
cargo --version
# cargo 1.78.0 (54d8815d0 2024-03-26)

rustc --version
# rustc 1.78.0 (9b00956e5 2024-04-29)

rustdoc --version
# rustdoc 1.78.0 (9b00956e5 2024-04-29)
```
