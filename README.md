# Compose Basic Codelab

Educational Android sample demonstrating foundational Jetpack Compose state, lists, animation, previews, and Material 3 theming.

## Overview

This project is a small Kotlin application based around a Compose onboarding-to-list flow. It starts with a welcome screen and Continue button, then renders 1,000 greeting cards in a lazy list. Each card can expand to reveal additional text with an animated size transition.

The sample keeps onboarding and expansion state with `rememberSaveable`, includes light and dark previews, and uses Material 3 with dynamic colors on supported Android versions.

## Concepts Demonstrated

- Compose activity content with `setContent`
- Reusable composable functions
- Conditional UI based on state
- `rememberSaveable` state restoration
- `LazyColumn` for a large list
- Expand/collapse controls with Material icons
- `animateContentSize` with a spring animation
- Material 3 cards, buttons, typography, surfaces, and color schemes
- Dynamic color on Android 12+
- Light, dark, and size-constrained Compose previews
- String resources for accessibility descriptions

## Tech Stack

- Kotlin
- Android SDK
- Jetpack Compose
- Material 3
- Compose Material Icons Extended
- AndroidX Activity Compose and Lifecycle Runtime
- Gradle Kotlin DSL

## App Flow

1. `MainActivity` applies `BasicCodeLabTheme` and displays `MyApp`.
2. `MyApp` initially renders `OnboardingScreen`.
3. Pressing Continue switches to `Greetings`.
4. `Greetings` lazily creates cards for values from `0` through `999`.
5. Each `Greeting` card preserves its own expanded state and animates between collapsed and expanded content.

## Project Structure

```text
app/src/main/
|-- java/com/example/basiccodelab/
|   |-- MainActivity.kt       # Composables and application flow
|   `-- ui/theme/             # Color, type, and theme definitions
|-- res/                      # Strings, launcher resources, and themes
`-- AndroidManifest.xml
```

## Getting Started

### Prerequisites

- Android Studio with a JDK compatible with Android Gradle Plugin 8.9.0
- Android SDK 35
- An Android 7.0 (API 24) or newer emulator or device

### Build

```bash
git clone https://github.com/shayann07/Compose-BasicCodeLab.git
cd Compose-BasicCodeLab
./gradlew assembleDebug
```

On Windows PowerShell, use `./gradlew.bat assembleDebug`.

## Current Status and Limitations

- This is a learning sample rather than a complete application.
- It has no navigation framework, persistence layer, network access, or domain data.
- The onboarding choice and expanded cards use saved UI state, not durable application storage.
- Most user-facing text is hardcoded in composables rather than stored in resources.
- Only generated example unit and instrumentation tests are present.
- No license file is included.
