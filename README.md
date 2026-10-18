# FluentXVerse Mobile

The native student-client workstream for FluentXVerse. Kotlin Multiplatform shares student models and API logic between an Android Jetpack Compose app and an iOS SwiftUI app.

## Current Scope

- Live student sign-in, session restoration/refresh, profile, statistics, and booking reads through the dedicated mobile API when deployed and enabled.
- Android Keystore and iOS Keychain storage for mobile session credentials.
- An explicitly separate offline demo covering discovery, booking, cancellation, tickets, materials, inbox, and profile.
- Shared JVM tests and Android UI tests using synthetic data.

The offline demo is not a live payment/booking system. Native live booking, classroom calls, purchases, registration, and push notifications are not complete. This source export does not imply an App Store or Play Store release.

## Structure

- `fluentxverse-mobile/shared/`: common Kotlin models, mobile API client, storage contract, and tests.
- `fluentxverse-mobile/androidApp/`: Android screens, lifecycle, secure token storage, and resources.
- `fluentxverse-mobile/iosApp/`: SwiftUI screens, Keychain integration, and Xcode project.
- `fluentxverse-server/MOBILE_API.md`: exported API contract, not a bundled backend.

See [the detailed mobile guide](fluentxverse-mobile/README.md) for platform structure and integration milestones. References there to sibling web applications refer to the source monorepo, not directories promised in this export.

## Android Development

Use JDK 17, Android Studio, and Android SDK platform 36. Configure `ANDROID_HOME` or an ignored `local.properties` file. From the repository root:

```bash
cd fluentxverse-mobile
./gradlew :androidApp:assembleDebug
./gradlew :shared:jvmTest :androidApp:testDebugUnitTest :androidApp:lintDebug
```

Android requires API 26+. The API server is separately deployed. An Android emulator reaches a host development API through `http://10.0.2.2:8765` where allowed by debug configuration. Release builds require HTTPS.

## iOS Development

On macOS with Xcode 16+ and JDK 17, open `fluentxverse-mobile/iosApp/FluentXverseStudent.xcodeproj`. Select the `FluentXverseStudent` scheme and an iOS 17+ simulator. Configure your own signing team for physical devices. Linux cannot validate an iOS build or Apple signing.

## Authentication and Privacy

Mobile bearer authentication is distinct from browser cookies and depends on the server's mobile module, its own signing secret, and configured session storage. Do not embed server credentials or wallet keys in the app. Native social-provider integration is a future milestone, not automatically supplied by the web app's Privy configuration. Verify secure storage, device logout, offline recovery, and server revocation on physical devices before release.

## Repository Provenance

This repository is an app-focused export of [the FluentXVerse monorepo](https://github.com/fluentxverse/fluentxverse-monorepo). The monorepo remains the source of truth; submit cross-application changes there. Existing directory names are retained so shared imports remain valid. The historical student/tutor folder suffix does not indicate a Preact app: both active web clients use SolidJS.

Exported source commit: [`c9696e491149`](https://github.com/fluentxverse/fluentxverse-monorepo/tree/c9696e491149275bc01df3b8d05d29a15263257a). See [SOURCE.json](SOURCE.json) for the exact scope. Local uncommitted edits, environment files, runtime data, curriculum backups, and generated proving artifacts are not published. Original Git history is not copied.

Cross-app parity tests and full deployment orchestration remain in the monorepo. The checks in this repository do not establish production acceptance, security audit completion, or proof-system review. No production deployment is triggered by this export.

## Security and Licensing

Never commit API tokens, seed phrases, private keys, student records, recordings, or real assessment evidence. Use synthetic fixtures in tests. Report security issues privately to hello@fluentxverse.com rather than opening an issue containing sensitive data.

No root license file was present in the source revision. Public source availability, if enabled, is not itself a license grant. Contact FluentXVerse for licensing terms; third-party dependencies retain their own licenses.
