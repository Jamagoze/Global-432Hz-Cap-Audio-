# Security Policy

## Overview

This security policy outlines vulnerability disclosure guidelines, supported versions, and security best practices for the **Global-432Hz-Cap-Audio** project. Because this software interfaces directly with system audio daemons, low-level audio drivers, and real-time Digital Signal Processing (DSP) pipelines, maintaining system integrity and audio stream privacy is a top priority.

---

## 1. Supported Versions

Only the `main` branch and the most recent stable release actively receive security updates and patches.

| Version | Supported |
| :--- | :--- |
| `main` (Latest) | :white_check_mark: Yes |
| Older Releases | :x: No |

---

## 2. Reporting a Vulnerability

If you discover a security vulnerability—such as an audio buffer overrun, memory corruption risk, privilege escalation vector, or unauthorized access to captured system audio streams—please report it responsibly.

> **Crucial:** Do **not** open public GitHub issues for security vulnerabilities.

### Submission Guidelines
* Submit details using **GitHub Private Vulnerability Reporting** (if enabled), or
* Contact the project maintainers directly via email.

### Information to Include
When submitting a report, please provide:
1. **Description:** A detailed explanation of the vulnerability and its potential impact.
2. **Environment Details:** The OS, sound server (PipeWire, PulseAudio, WASAPI, CoreAudio), and project version.
3. **Reproduction Steps:** A minimal Proof-of-Concept (PoC) script or step-by-step instructions.
4. **Remediation:** Any suggested fixes or mitigation strategies (if available).

### Response Timeline
* **Acknowledgment:** Within **48–72 hours**.
* **Updates:** Periodic status updates during triage, fixing, and patch release phases.

---

## 3. Security Considerations & Best Practices

### Privilege Minimization
Do not run audio capture daemons or pitch-shifting scripts with elevated permissions (`root` or `Administrator`) unless strictly required for driver-level operations. Running within user space limits the potential impact of code execution exploits.

### Audio Stream Privacy
Real-time audio capture software can intercept all sound input and output streams passing through the sound server. Ensure binaries are compiled from verified source code to prevent unauthorized logging or exfiltration of sensitive audio (e.g., private calls, voice inputs).

### Dependency Management
Regularly update and audit third-party Digital Signal Processing (DSP) and system audio libraries. Run dependency security scanners (such as `cargo audit`, `pip-audit`, or `npm audit`) to identify and patch known CVEs.
