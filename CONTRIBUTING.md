# Contributing to Global-432Hz-Cap-Audio

First off, thank you for considering contributing to **Global-432Hz-Cap-Audio**! It’s contributions like yours that make open-source audio software better for everyone.

Please take a moment to review this document before submitting your contributions.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
  - [Reporting Bugs](#reporting-bugs)
  - [Suggesting Enhancements](#suggesting-enhancements)
  - [Pull Requests](#pull-requests)
- [Development Workflow](#development-workflow)
- [Audio & DSP Guidelines](#audio--dsp-guidelines)
- [Commit Message Conventions](#commit-message-conventions)

---

## Code of Conduct

We aim to foster an open, welcoming, and respectful environment. Please be respectful and constructive in all discussions, issues, and pull requests.

---

## How Can I Contribute?

### Reporting Bugs

Before creating a bug report, please check the existing issues to see if the problem has already been reported.

When creating a bug report, please include as many details as possible:

1. **Use a clear and descriptive title.**
2. **Describe the exact steps to reproduce the issue.**
3. **Provide your environment details:**
   - Operating System (Linux, macOS, Windows)
   - Audio Subsystem / Sound Server (e.g., PipeWire, PulseAudio, WASAPI, CoreAudio)
   - Audio hardware / Sample rates used (e.g., 44.1 kHz, 48 kHz, 96 kHz)
4. **Expected behavior vs. actual behavior** (e.g., audio latency, pitch calculation errors, distortion, or crash logs).

> ⚠️ **Note:** For security vulnerabilities, please refer to our [SECURITY.md](SECURITY.md) policy instead of opening a public issue.

---

### Suggesting Enhancements

Enhancement suggestions are tracked as GitHub Issues. When suggesting a feature:

- Use a clear and descriptive title.
- Provide a step-by-step explanation of the proposed feature or workflow improvement.
- Explain **why** this feature would be useful to users (e.g., support for a new DSP algorithm, lower buffer latency, GUI integration).

---

### Pull Requests

1. **Fork the repository** and create your branch from `main`.
2. **Keep PRs focused:** A pull request should fix one issue or add one specific feature.
3. **Test your changes:** Verify that audio processing remains stable and no pitch drift or latency spikes are introduced.
4. **Update documentation:** Ensure README files or inline documentation reflect any API or CLI changes.
5. **Submit the PR:** Describe your changes clearly in the pull request body and link relevant issues.

---

## Development Workflow

1. Clone your fork locally:
   ```bash
   git clone https://github.com/YOUR-USERNAME/Global-432Hz-Cap-Audio.git
   cd Global-432Hz-Cap-Audio
   ```
2. Create a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. Test audio stream capture and DSP output across standard sample rates ($44.1\text{ kHz}$, $48\text{ kHz}$).
4. Commit your changes using descriptive commit messages.
5. Push to your fork and submit a Pull Request.

---

## Audio & DSP Guidelines

When contributing code related to audio processing and pitch conversion:

* **Frequency Math:** Keep the pitch shift accurate to the standard ratio $\frac{432}{440} \approx 0.981818$ ($-31.766$ cents).
* **Latency & Buffering:** Minimize buffer sizing where possible to prevent real-time audio delay, but keep buffers large enough to prevent underruns/crackling.
* **Format Support:** Ensure proper handling of float vs. integer PCM audio samples ($16$-bit, $24$-bit, $32$-bit float).

---

## Commit Message Conventions

Use clear, concise commit messages. Prefix your commits to indicate the type of change:

- `feat:` A new feature
- `fix:` A bug fix
- `docs:` Documentation changes
- `dsp:` Signal processing or pitch tuning improvements
- `refactor:` Code refactoring without changing logic
- `ci:` Build systems or CI configuration updates

Example:
```text
dsp: optimize resampler buffer for 48kHz audio streams
```

Thank you for contributing!
