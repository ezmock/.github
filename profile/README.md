<p align="center">
  <img src="https://assets.ezsoft.ca/logos/ezmock-logo.svg" alt="ezmock logo" width="128" />
</p>

<h1 align="center">ezmock</h1>
<p align="center"><strong>Mock APIs. Build independently.</strong></p>
<p align="center">
  Open-source, developer-first API mocking SDKs for native and cross-platform applications.
</p>

---

### 🚀 Build features without waiting for the backend

**ezmock** helps developers build and test API-driven features even when endpoints are unfinished, staging environments are down, or failure scenarios are difficult to reproduce.

The goal is simple: keep your application's real HTTP client, repositories, and data models, while using **local JSON fixtures** to simulate HTTP responses. No separate mock server, proxy, Docker container, or live API is required.

### ✨ What developers gain

- ⚡ **Move independently.** Develop screens and data flows before the backend is ready.
- 🧩 **Keep production-shaped code.** Exercise your existing API clients, serializers, and response handling instead of writing mock-only repositories.
- 🧪 **Test realistic scenarios.** Simulate success, empty states, HTTP errors, network failures, and latency.
- 💻 **Work offline.** Run repeatable API-driven workflows in emulators and simulators using bundled fixtures.
- 🔁 **Stay consistent across platforms.** Share route definitions and scenario behavior through a common specification.

### 🔄 How ezmock works

```text
HTTPS request → ezmock adapter → Local JSON fixture → Simulated HTTP response
```

Define an endpoint, add its response fixture, select a scenario, and run your app. When mock mode is enabled, an ezmock adapter handles matching requests locally; otherwise, your configured client can use its normal networking path.

**Same data layer. Different response source.**

### 🛠️ Designed for developers' existing tools

We're building SDK integrations for **Android (Kotlin / OkHttp)**, **iOS (Swift / URLSession)**, **Kotlin Multiplatform (Ktor Client)**, and **Flutter (Dart / Dio and `package:http`)**.

Our shared direction includes fixture-based routing, configurable responses and delays, controlled failure scenarios, explicit mock-mode boundaries, and small runnable example applications.

### 🧭 Our engineering principles

- **Native-first:** integrations that feel natural in each ecosystem.
- **Minimal intrusion:** mocking at the HTTP boundary, not throughout feature code.
- **Predictable behavior:** deterministic fixtures and clearly selected scenarios.
- **Safe by design:** opt-in mocking and explicit handling of unmatched requests.
- **Open collaboration:** practical documentation, examples, and community contributions.

### 🤝 Build with us

Explore the project, report issues, propose integrations, and help make API-independent development easier for everyone.

> **Project status:** ezmock is in early development. SDK APIs and distributions may change before the first stable releases.

---

<sub>Initiated and maintained by <strong>[ezSoft Technologies Inc.](https://github.com/ezsoft-tech)</strong></sub>
