# Graktor Mobile

Thin native wrappers around the live Graktor admin app (`https://app.graktor.com`), via
[Capacitor](https://capacitorjs.com/) — same "just load the real app" approach as
[graktor-desktop](https://github.com/graktor/graktor-desktop), just packaged for a phone instead
of a desktop window. No bundled frontend, no local server: every install talks straight to the
real API.

## android/

```bash
cd android
npm install
npx cap sync android
cd android && ./gradlew assembleDebug
```

Produces an installable (unsigned debug) APK at
`android/android/app/build/outputs/apk/debug/app-debug.apk` — sideload it directly, no Play
Store account needed. CI (`.github/workflows/build-android.yml`) builds this on every push and
uploads it as a release asset.

## ios/

```bash
cd ios
npm install
npx cap sync ios
cd ios/App && pod install
```

Then open `ios/App/App.xcworkspace` in Xcode.

**Unlike Android, there is no way to get a real iOS app onto a physical iPhone, or into
TestFlight/the App Store, without an Apple Developer Program account and real signing
certificates** — Apple doesn't allow unsigned installs on real hardware at all, not even for
internal testing. CI (`.github/workflows/build-ios.yml`) builds this for the iOS **Simulator**
only, which proves the app actually builds and runs, but produces nothing installable on a real
device. Wiring up real device/App Store distribution needs the owner's own Apple Developer
account and certificates added as repo secrets.

## Updating the app's own URL

Both apps point at `https://app.graktor.com` via each project's own `capacitor.config.json` —
change it there (and re-run `npx cap sync`) if that ever needs to move.
