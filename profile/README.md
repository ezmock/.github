# ezmock

**Fixture-driven API mocking for mobile developers.** Build Android, iOS and multiplatform features before backend endpoints are available, without running an external mock server.

| Repository | Developer ecosystem | Status |
|---|---|---|
| [ezmock-spec](https://github.com/ezmock/ezmock-spec) | Shared manifest format, examples and behavior contract | Draft V1 |
| [ezmock-android](https://github.com/ezmock/ezmock-android) | Kotlin / Android / OkHttp | Starter |
| [ezmock-ios](https://github.com/ezmock/ezmock-ios) | Swift / Foundation URLSession | Starter |
| [ezmock-kmp](https://github.com/ezmock/ezmock-kmp) | Kotlin Multiplatform / Ktor | Starter |
| [ezmock-flutter](https://github.com/ezmock/ezmock-flutter) | Flutter / Dart / Dio / http | Starter |
| [ezmock-nuxt](https://github.com/ezmock/ezmock-nuxt) | Official developer website | Planned / intentionally empty |

Each SDK is imported **independently**; `ezmock-spec` is not an application dependency. Developers bundle `routes.json` and their mock JSON fixtures. Mocking preserves HTTPS URL shapes but does **not** perform a TLS handshake or start an HTTP listener.

**Open source:** Apache License 2.0 (initial choice). **Stewardship:** ezSoft Technologies Inc. **Domains reserved:** ezmock.io and ezmock.net; a website will follow SDK publication.

Contribute via issues and pull requests in the relevant repository. See [contributing](https://github.com/ezmock/.github/blob/main/CONTRIBUTING.md) and [security](https://github.com/ezmock/.github/blob/main/SECURITY.md).
