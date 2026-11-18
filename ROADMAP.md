# EZMock roadmap

**Mock APIs. Build independently.**

Updated 2026-11-18 · Maintained by ezSoft Technologies Inc. in the [ezMock GitHub organization](https://github.com/ezmock)

EZMock lets developers build API-driven features before the backend is ready. Apps
keep their HTTP clients, API services, DTOs, repositories, and UI. Developers bundle
JSON routes and fixtures, start EZMock during development, and give the returned
base URL to their existing client. Switching to a live backend changes the URL in
the host's network configuration. The same feature code can keep working when the
API contract stays the same.

## Release and implementation status

| Repository | Current implementation | Status and distribution |
| --- | --- | --- |
| [`ezmock-android`](https://github.com/ezmock/ezmock-android) | Kotlin SDK with an embedded HTTP server and optional Compose controls | Stable `io.ezmock:ezmock-android:1.0.0` on Maven Central. Native Android and the Android host of a KMP app can use the same artifact. |
| [`ezmock-ios`](https://github.com/ezmock/ezmock-ios) | Swift package with an embedded HTTP server and optional SwiftUI controls | Stable `v1.0.0` Git tag with SwiftPM product `EZMock`. Installation requires authorized repository access while it remains private. Supports native iOS and the iOS host of a KMP app. |
| [`ezmock-flutter`](https://github.com/ezmock/ezmock-flutter/tree/develop) | Independent Dart runtime, embedded HTTP server, and optional Flutter controls | Source preview for mobile and desktop. The package is `ezmock_flutter`. Its manifest sets `publish_to: none`, and pub.dev distribution is deferred. |
| [`ezmock-kmp`](https://github.com/ezmock/ezmock-kmp/tree/develop) | Shared Kotlin starter using Ktor `MockEngine` on Android, iOS, and JVM targets | Unpublished and deferred until after Flutter. A dedicated Kotlin distribution depends on a use case beyond the existing native SDKs. |
| `ezmock-spec` | Private, versioned contract for manifests, runtime behavior, and conformance | Stable `v1.0.0` tag. SDKs keep their own reviewed pins. Maintainers use the contract during development and verification. |
| [`ezmock-nuxt`](https://github.com/ezmock/ezmock-nuxt) | Documentation website repository | README placeholder. Website implementation remains a later phase. |

SDK versions, the manifest's `schemaVersion`, and conformance result formats
are versioned independently. A new spec tag does not change an installed SDK.

## Architecture in use

Android, iOS, and Flutter own their respective embedded servers. Each listener
binds IPv4 loopback on an OS-assigned port, runs inside the host process, and
stops with its owner or when the process exits.

```text
App → existing HTTP client → http://127.0.0.1:<assigned-port>/
    → route and scenario match → bundled fixture → HTTP response → app
```

The manifest's HTTPS `baseUrl` identifies the logical API origin. Mock requests
travel over local HTTP/1.1. Clients handle their usual deadlines, retries,
cancellation, and deserialization. A missing route returns a local 404. The
server never forwards requests to a live service.

The host owns its fixtures, startup, client configuration, and production build
policy. Startup can return a live base URL when mocking is disabled. Rebuild
clients that retain the old URL after a server restarts on a new port. Allow
local HTTP in development using the host platform's security settings and
keep development dependencies and fixtures out of production builds where
required. Turning mocking off leaves any packaged files in the application.

KMP's deferred starter operates inside the Ktor client via `MockEngine`.
It uses a platform-supplied fixture loader and a named scenario. Its Android
and SwiftUI demos use the shared Kotlin implementation, including the
`latency` scenario. Developers using KMP can use the stable Android and iOS
SDKs in their respective hosts and pass the returned URL to shared client code.

## What developers can use today

| Capability | Android and iOS stable | Flutter preview | KMP starter |
| --- | --- | --- | --- |
| Startup and client integration | `EZMock.start(context)` on Android and `try await EZMock.start()` on iOS. Existing clients accept the returned URL. Explicit server owners are available for independent sessions. | `await EZMock.start()` with the host's existing client, such as Dio or `package:http`. | `HttpClient(mock.engine())` for Ktor. The app selects its live engine separately. |
| Routes and responses | V1 JSON manifests, path parameters, query constraints, deterministic precedence, and fixtures for GET, POST, PUT, PATCH, DELETE, HEAD, and OPTIONS. | The Dart runtime implements those routes and methods, with checks against the shared contract still to be recorded for release. | Implements fixture-backed method/path/query matching and named scenarios in the starter. |
| Failure and latency | Configured HTTP errors, response delays, offline and timeout simulation, and fail-closed 404s. | Configured status, delay and transport failure behavior through the Dart server. | Configured delays, status and mock transport exceptions. |
| Scenario selection | Named, status, and success/failure selectors. Android offers optional persistence. App-wide iOS startup remembers selection by default. | Named, status, and success/failure selectors, with remembered selection by default. | Mutable scenario name with default-route fallback. |
| Development UI | Optional scenario and Requests controls in the same SDK artifact or package. | Optional Flutter scenario and Requests controls in the same package. | Example scenario selectors. |

Successful write requests return the configured acknowledgment. Static fixtures
remain unchanged, so host apps update their own list or form state as they
would with a real API. The mobile and desktop Flutter example uses a Users
feature with read, create, edit, and delete flows, including a connected
PATCH/PUT selector. Android and iOS also ship Users examples with their
optional scenario and inspection controls.

Request inspection shows exchanges that reach the embedded server. The
in-memory history is bounded and shows details such as method, path, selected
scenario, timing, status, headers, and JSON previews. Authentication and cookie
headers are redacted. URLs and bodies can still contain app data, so use
synthetic fixtures and development requests. Live calls, cache hits, and UI
actions do not appear in this history.

### Adoption guides

- **Android** supports API 24+ at runtime. The current artifact requires
  compileSdk 37+. Guides cover XML Views, Jetpack Compose, KMP Android, manual
  setup, Hilt, Koin, Dagger, OkHttp, Retrofit, and Ktor.
  [Android SDK guide](https://github.com/ezmock/ezmock-android)
- **iOS** requires Swift 6 and iOS 16+. The package declares macOS 13+.
  Guides cover native URLSession and KMP iOS with a shared Ktor Darwin client.
  [iOS SDK guide](https://github.com/ezmock/ezmock-ios)
- **Flutter** requires Flutter 3.50+ and Dart 3.14+ below 4.0. The runtime and
  controls are implemented in Dart, and the package depends on neither native
  EZMock SDK. Try a local checkout or an accessible Git revision.
  [Flutter SDK guide](https://github.com/ezmock/ezmock-flutter/tree/develop)

## Release evidence and CI

| Area | Available evidence and remaining coverage |
| --- | --- |
| Android stable | Maintainer release records cover local API 24 and 37 verification, the official embedded-server profile, and an independent minified Maven consumer. The stable package was also verified in Jokerary. |
| Apple stable | Local shared/socket checks ran on the iOS 18.6 simulator and macOS 27.2.1, including an independent SwiftPM consumer. The maintainer verified Jokerary on a physical iOS 16 device. This is adoption evidence at the iOS minimum. The macOS 13 minimum remains unverified. SwiftPM packages the privacy manifest for app-local scenario persistence. |
| Flutter preview | The current Dart runtime has local preview checks on Android API 37, iOS 27.2 Simulator, and macOS 27.2.1. Linux and Windows runners and app branding are present. Builds and runtime checks on those hosts, minimum-version coverage, and clean host adoption of the independent package are still release work. |
| KMP starter | The Android example builds and the starter includes JVM tests. CI is configured for JVM tests, the Android example build, and an iOS simulator framework build. Claim release support only after testing the chosen Kotlin targets and a clean consumer. |
| Shared contract | V1 contains 90 response vectors, 55 rejection vectors, seven profiles, and 30 native test definitions. Official native loopback profiles are `android-http-server` and `ios-http-server`. The historical interceptor and Flutter adapter results apply to the implementations that produced them. Flutter's Dart listener needs its own evidence. |

Android and iOS CI build and package the SDKs. Flutter CI analyzes and builds the
Android and iOS examples. Runtime, device, and conformance verification for these
three SDKs is local. Spec CI validates its own schemas, corpus, tooling, and
release bundle. The KMP starter retains its separate workflow.

Local conformance uses immutable spec revisions. The Android and iOS SDK
checkouts currently pin `1.0.0-rc.4` and `1.0.0-rc.5`, respectively, with
result format 3. Update a pin only alongside evidence for the affected SDK and
runtime. A package build, tag, or passing spec validator does not certify SDK
execution. Record the SDK revision, spec pin, package identity, runtime, and
any skipped or missing checks.

## Priorities and next releases

### P0 — Stable foundation and Flutter release readiness

The V1 contract and both native stable releases are complete. Maintain their
documented startup, ownership, scenario and inspection APIs, and observable
fixture behavior. Introduce breaking changes with a major version and migration
guidance. Refresh installation pages that still describe the native 1.0.0 versions
as upcoming.

The next distribution to evaluate is Flutter. Verify Linux and Windows on
their own hosts, agree on the initial supported targets, and retain minimum
OS and independent consumer evidence for those targets. Verify the standalone
Dart package in a separate app, including Jokerary Flutter when its pin moves
to the current runtime. Record conformance for the actual listener and exercise
scenario persistence, shutdown, failures, and request inspection. When public
distribution is approved, review package contents, publisher ownership and
release notes, enable publication in the manifest, and complete a pub.dev dry
run before publishing. Flutter Web remains deferred because browsers cannot
bind this local HTTP listener.

### P1 — Developer experience

- Improve fixture validation and onboarding. A CLI or build helper could flag
  invalid manifests, missing files, and route conflicts before app startup.
- Improve accessibility and ergonomics of optional scenario and request controls.
  Keep settings and fixture ownership appropriate to each host.
- Reassess a dedicated Kotlin SDK after Flutter. Evaluate a platform-neutral
  runtime or shared-code use case with demand beyond the native host integrations
  before investing in the existing Ktor starter or committing to an artifact name.
- Build the documentation site after SDK installation paths are stable. Link
  to SDK guides, release notes, and examples to keep one maintainer source per SDK.

### P2 — Advanced mocking

Evaluate header and body matching, response templates, pagination, deterministic
sequences, and opt-in stateful simulations. Specify precedence, isolation, and
reset behavior before changing the contract. OpenAPI-based fixture generation
and opt-in recording require explicit data handling and redaction rules.

### P3 — Transport and platform research

Evaluate embedded HTTPS/TLS, streaming, WebSockets, SSE, other protocols, and
browser-specific mocking separately when demand justifies it. Current loopback
HTTP behavior cannot verify TLS, certificates, pinning, DNS, proxies, or actual
backend performance. Loopback restricts network exposure but does not authenticate
other local processes. Use real integrations for transport and security testing.

## Independent release process

1. Review compatibility, changelog, documentation, and exact local verification for
   the candidate. Resolve the package from a clean consumer on each claimed target.
2. For native releases, package an RC on `develop`, then promote the reviewed
   source on `main`. Android uses **Android package**, then **Maven Central
   staging** to sign and stage the retained package for maintainer review and
   publication. iOS uses **iOS package** to create the SwiftPM Git tag.
3. Resolve the published version, record its coverage and limitations, and publish
   release notes. Review repository visibility independently of package publication.

The shared contract remains private. SDK installation uses the relevant SDK
distribution and app-owned JSON. Treat P0–P3 as planning labels rather than
permanent conformance IDs or workflow names.
