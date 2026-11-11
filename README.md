# Alphabets Teacher

Alphabets Teacher is a Flutter learning app for children and beginner readers learning the Dari/Persian-script alphabet and vocabulary. Learners can explore letters and word lists grouped by length, open a focused learning screen, and use the audio player on the example screen to hear a pronunciation where an audio asset is available.

## Use cases

- **Learn the alphabet:** Browse a right-to-left grid of letters and tap a letter to see it in a large, focused view and play its bundled pronunciation.
- **Build vocabulary in steps:** Explore word lists arranged as one-, two-, three-, and multiple-syllable/length categories. The entries are defined in `lib/words.dart`.
- **Practice independently:** Use the visual cards, custom Parastoo font, illustrations, and bundled audio without requiring a server or account.
- **Explore on different devices:** The project includes Flutter runner projects for Android, web, Linux, macOS, and Windows.

> **Current implementation notes:** The home screen's “Start Challenge” button is presentational and has no action yet. The multiple-word list currently contains blank entries. Letter items have audio paths in the data model; the word-list screens pass the word text as an audio path, but no matching per-word audio mapping is defined, so word playback is not configured. The example screen also contains a hard-coded time check; it is not backed by persistent daily progress or a user setting.

## Architecture

This is a small, client-side Flutter app with a direct widget-navigation structure and static, bundled learning content. There is no backend, API, database, authentication, or separate domain/service layer in the current implementation.

```text
lib/
  main.dart                  App entry point; launches MaterialApp and HomePage
  words.dart                 Letter/audio records and static vocabulary lists
  components/
    hover_letters.dart       Reusable RTL learning card; opens ExamplePage
    curve_painter.dart       Decorative custom painter for the home screen
  screens/
    home_page.dart           Learning-category carousel and challenge panel
    alphabets_page.dart      Right-to-left alphabet grid
    one_syllable.dart        One-syllable/short-word grid
    two_syllable.dart        Two-syllable/medium-word grid
    three_syllable.dart     Three-syllable/longer-word grid
    multiple_syllable.dart   Additional word list
    example_page.dart        Large item display and audio playback controls
assets/
  voices/                    Bundled M4A pronunciation files
  fonts/                     Parastoo font files
  *.png                      Learning-screen backgrounds and illustrations
test/
  widget_test.dart           Widget-test scaffold (currently stale; see Testing)
android/                     Android Flutter host project
web/                         Web bootstrap and manifest
linux/, macos/, windows/     Desktop Flutter host projects
```

### App flow

`lib/main.dart` starts `MyApp`, which displays `HomePage`. The home carousel routes each category to its corresponding screen. Alphabet and vocabulary screens build their cards from the static lists in `lib/words.dart`; tapping a `HoverLetter` navigates to `ExamplePage` with the selected item. `ExamplePage` uses `audioplayers` to play an asset and provides seek/play/pause controls. Visuals, fonts, and audio are packaged locally through the Flutter asset declarations in `pubspec.yaml`.

## Tech stack

- **Language:** Dart (SDK constraint `^3.6.2`)
- **Framework:** Flutter
- **Key packages:** `carousel_slider` for the home category carousel, `audioplayers` for audio playback, and `shared_preferences` (declared as a dependency; not currently used by the app code).
- **UI:** Material widgets, custom painter, right-to-left text direction, and the bundled Parastoo typeface.

## Getting started

Install the Flutter SDK compatible with the Dart constraint in `pubspec.yaml`, then from the repository root run:

```bash
flutter pub get
flutter run
```

To target a specific available device, list devices first and pass its identifier:

```bash
flutter devices
flutter run -d <device-id>
```

No environment variables, credentials, or backend services are required for the current app.

### Build targets

Flutter's platform runners are included for Android, web, Linux, macOS, and Windows. For example:

```bash
flutter build apk
flutter build web
flutter build linux
flutter build macos
flutter build windows
```

Build only for platforms supported by your development host and installed platform toolchains.

## Testing and analysis

Run the project's static analysis with:

```bash
flutter analyze
```

Run the widget tests with:

```bash
flutter test
```

`test/widget_test.dart` is still Flutter's generated counter smoke-test template. It expects a counter and add button that this learning app does not have, so it needs to be replaced with tests for the actual screens before it can serve as a passing app test suite.

## Project data and assets

- Update letters, their pronunciation asset paths, and vocabulary in `lib/words.dart`.
- Add or replace images, audio, and fonts under `assets/`, and register new files in the `flutter.assets` or `flutter.fonts` sections of `pubspec.yaml`.
- Alphabet pronunciations use assets under `assets/voices/`; the `Letter` records connect each displayed character to a voice file.

## Potential next steps

- Implement challenge navigation and activities for the home-screen challenge card.
- Connect word entries to real audio assets or disable their playback until clips are available.
- Remove empty vocabulary entries and clarify whether categories represent syllables or word length.
- Replace the generated counter test with tests for category navigation, RTL alphabet rendering, and audio controls.
- If a daily reading limit is intended, store and check its state reliably rather than using a hard-coded timestamp.
