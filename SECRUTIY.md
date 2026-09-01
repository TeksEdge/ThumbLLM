
# Security Policy

## Official ThumbLLM Releases

Official ThumbLLM releases are distributed through the TeksEdge ThumbLLM GitHub repository:

**https://github.com/TeksEdge/ThumbLLM**

Download executable files only from the official **Releases** section of this repository.

TeksEdge does not guarantee the authenticity, integrity, or safety of ThumbLLM executables redistributed through third-party websites, file-sharing services, mirrors, or unofficial repositories.

## Verifying Downloads

Official ThumbLLM releases may include a SHA-256 checksum for each executable.

Users are encouraged to verify the checksum of a downloaded executable before running it.

On Windows PowerShell:

```powershell
Get-FileHash ".\ThumbLLM.exe" -Algorithm SHA256
```

Compare the resulting SHA-256 hash with the hash published with the corresponding GitHub release.

If the hashes do not match, do not run the executable.

## Local API Security

ThumbLLM provides a local OpenAI-compatible API.

By default, the API is intended to listen only on the local computer.

Users should use caution when changing networking, binding, firewall, or port settings that could expose the ThumbLLM API to other devices or to the public Internet.

ThumbLLM should not be assumed to provide authentication, authorization, or hardened Internet-facing API security unless explicitly documented for a particular release.

## Model Downloads

Some ThumbLLM editions automatically download model files from third-party model repositories.

The model files are separate from the ThumbLLM executable and are governed by their respective licenses and distribution terms.

ThumbLLM releases may verify downloaded model files when verification information is available for that edition.

## Reporting a Security Vulnerability

Please do not publicly disclose an unpatched security vulnerability through a GitHub issue.

If you discover a vulnerability that could affect ThumbLLM users, please contact TeksEdge privately with:

* the affected ThumbLLM version
* the affected operating system or backend
* a description of the vulnerability
* steps to reproduce the issue
* potential impact, if known
* any suggested mitigation, if available

Please allow reasonable time for investigation and remediation before publicly disclosing a vulnerability.

## Supported Versions

ThumbLLM is under active development.

Security fixes will generally target the most recent release of an affected ThumbLLM edition. Older model, runtime, or backend-specific builds may be superseded rather than independently patched.

Users are encouraged to use the most recent applicable ThumbLLM release whenever practical.

## Third-Party Components

ThumbLLM relies on third-party software, including components such as llama.cpp.

Security issues originating in those components may require updates to the underlying runtime and a new ThumbLLM build.

Third-party licensing and attribution information is available in:

`THIRD-PARTY-NOTICES.md`
