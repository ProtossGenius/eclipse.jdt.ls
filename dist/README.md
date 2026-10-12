# jdtls build with nested-lambda completion fix (JDK 21+ compatible)

This branch hosts a pre-built `eclipse.jdt.ls` (jdtls) distribution for
Neovim's `mason.nvim` to download, built from `eclipse.jdt.core` commit
`4969cad78b8f796479a56a9be90e1ed4b00fafee`
("Fix completion for nested expression lambdas").

## What's fixed

`AssistParser.triggerRecoveryUponLambdaClosure(...)` no longer creates an
invalid parser snapshot for expression-bodied lambdas that live outside a
real block (e.g. lambdas nested inside a field initializer's builder chain,
themselves nested inside `List.of(...)`/`Map.of(...)`). Previously this
caused ECJ's completion engine to lose the lambda's target type and return
zero completion candidates for 2-3 level nested lambda expressions.

## Compatibility

Runs on standard **Java 21+**.
The experimental Java 25-only javac bundle (`org.eclipse.jdt.core.javac`) has been removed, and filesystem/security bundles requiring JavaSE-25 have been replaced with standard JavaSE-17 compatible builds (`org.eclipse.core.filesystem` 1.11.600 and `org.eclipse.equinox.security.linux` 1.1.400).

## Contents

- `jdtls-nested-lambda-fix-linux-x64.tar.gz` — full jdtls product
  distribution (`bin/`, `features/`, `plugins/`, `config_linux/`,
  `config_mac/`, `config_win/`, etc.), built with:
  - `org.eclipse.jdt.core` 3.48.0-SNAPSHOT (patched with nested-lambda completion fix)
  - `eclipse.jdt.ls` 1.62.0-SNAPSHOT

## Usage

This is consumed by a custom Lua `mason-registry` package spec
(`lua/user/mason_registry_jdtls_fix.lua` in the
[nvim_config](https://github.com/ProtossGenius/nvim_config) repo) that
overrides mason's `jdtls` download URL to point at this archive instead of
the official `download.eclipse.org` snapshot.
