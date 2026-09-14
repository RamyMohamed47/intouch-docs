# Expo EAS

The Expo application under `apps/mobile` is linked to the `intouch-mobile` EAS
project and uses three build profiles from `eas.json`.

## Profiles

| Profile | Purpose | Distribution |
| --- | --- | --- |
| `development` | Native development client used with Metro | Internal Android APK |
| `preview` | Tester-facing release candidate against configured services | Internal Android APK |
| `production` | Store-ready configuration and production update channel | Production artifact |

The current acceptance workflow is Android-first. Public store submission and
iOS runtime acceptance remain separate work.

## Build Commands

Run EAS commands from `apps/mobile`:

```powershell
npx eas-cli@latest build --platform android --profile development
npx eas-cli@latest build --platform android --profile preview
npx eas-cli@latest build --platform android --profile production
```

The development client starts JavaScript from Metro with `npm run dev:mobile`.
A preview APK contains its exported JavaScript bundle and can be installed by a
tester without the development laptop.

## Native Configuration

Rebuild development and preview profiles after changing native modules, Expo
config plugins, Android permissions, notification sounds/channels, LiveKit or
WebRTC native dependencies, Google sign-in configuration, or Firebase service
configuration. JavaScript-only changes compatible with the current
`runtimeVersion` may use the configured update channel instead.

EAS environments provide public runtime values, build-only Sentry values, the
`GOOGLE_SERVICES_JSON` file variable, and credentials needed for signing and
FCM V1. Signing keys and service-account JSON files are not committed.

## Release Checks

Before distributing an APK:

1. Run `npm run check` from the monorepo root.
2. Run `npm run mobile:doctor`.
3. Confirm the profile targets the intended API origin and EAS environment.
4. Install the resulting APK on a physical Android device.
5. Verify authentication, realtime messaging, push, Echo, uploads, and the
   Mobile V2.0 media acceptance path against the intended backend.
