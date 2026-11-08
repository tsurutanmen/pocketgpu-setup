# Contributing

## Before you start

- This repository contains only tools for install and repair. It does not include core components such as drivers, apps, receivers, or `PocketGPU.exe`. Contributions that add these items cannot be accepted.
- Do not include passwords, tokens, personal identifiers, email addresses, or local PC paths in code, logs, issues, or PRs.

## Setup

- Windows 10 or 11, Windows PowerShell 5.1, and Python 3
- Tests: `powershell -ExecutionPolicy Bypass -File tests\run-tests.ps1`
  - Specific scenarios only: `-Scenario loader,files`
  - Does not use real iPhones, Android devices, Apple services, GPUs, or the internet
- Search for leaked secrets: `powershell -ExecutionPolicy Bypass -File tests\Test-NoSecrets.ps1`

## Style

- `.ps1`: UTF-8 (with BOM), CRLF line endings. Use only syntax compatible with Windows PowerShell 5.1 (do not use `&&`, `?:`, or `??`)
- `.cmd`: ASCII only, CRLF line endings (to ensure compatibility across Windows locales and code pages)
- Screen messages must be written as pairs of `M '日本語' 'English'`
- Repair actions must always prompt the user with `y/N` before proceeding. Never delete anything automatically. Always keep backups of replaced items
- Test environment variables must be named `PGPU_TEST_*` and documented in comments at the top of the file
- Normalize formatting: `powershell -ExecutionPolicy Bypass -File tests\Normalize-Files.ps1`

## Adding a fix

1. Add inspection, repair, and re-verification logic to `kit/tools/fix.ps1`.
2. Add a deliberately broken test scenario to `tests/run-tests.ps1`. Run it using the following three modes:
   - `fix`: Verify that the issue is resolved in the actual state (files, processes, port).
   - `decline`: Verify that answering "n" results in no changes.
   - `sabotage`: Verify that removing the repair logic causes the verification to fail.
3. Add a single line to the table in `README.md`.

## Pull requests

- Describe the reason for the changes and the test results.
- Keep pull requests small and focused.

## Issues

- In the "Bug report" section, include your device model, OS, kit version, and the output of Fix-PocketGPU.