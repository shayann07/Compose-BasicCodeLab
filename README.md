# Jetpack Compose Basics Codelab

[![Platform](https://img.shields.io/badge/Platform-Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)](https://developer.android.com)
[![Language](https://img.shields.io/badge/Kotlin-2.0+-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)](https://kotlinlang.org)
[![UI Framework](https://img.shields.io/badge/Jetpack%20Compose-BOM-4285F4?style=for-the-badge&logo=jetpackcompose&logoColor=white)](https://developer.android.com/jetpack/compose)
[![Design System](https://img.shields.io/badge/Material%203-Latest-FF6F00?style=for-the-badge&logo=materialdesign&logoColor=white)](https://m3.material.io)
[![License](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](LICENSE)

> An interactive Jetpack Compose starter project demonstrating state hoisting, survival across configuration changes with `rememberSaveable`, spring-physics list animations, and Material 3 design systems.

---

## 📖 Overview

The **Compose-BasicCodeLab** project demonstrates fundamental paradigm shifts introduced by Jetpack Compose. Unlike imperative Android UI toolkits that mutate existing view trees, this application showcases **declarative UI rendering**, where the interface is a direct mathematical function of state ($UI = f(State)$).

### 🎯 Key Learning Objectives
- **Unidirectional Data Flow (UDF)**: Understanding how state flows down and events flow up through composable hierarchies.
- **State Hoisting**: Promoting state to common ancestors (`MyApp`) to decouple business logic and make composables stateless, reusable, and easily testable.
- **Configuration Survival**: Leveraging `rememberSaveable` to retain UI state (such as onboarding status and card expansion) across device rotations and process recreations.
- **Physics-Based UI Animations**: Utilizing Jetpack Compose's `animateContentSize` with customized `spring` specs (`DampingRatioMediumBouncy` and `StiffnessLow`).
- **High-Performance Lists**: Rendering virtualized collections containing 1,000+ dynamic items via `LazyColumn`.

---

## 🏗️ Architecture & State Flow

```mermaid
graph TD
    classDef state fill:#1A365D,stroke:#63B3ED,stroke-width:2px,color:#fff;
    classDef comp fill:#2D3748,stroke:#4FD1C5,stroke-width:2px,color:#fff;
    classDef event fill:#744210,stroke:#F6E05E,stroke-width:2px,color:#fff;

    subgraph StateManagement ["Root State Hoisting (MyApp)"]
        StateOnboard["shouldShowOnboarding: Boolean<br/>(rememberSaveable)"]:::state
    end

    StateOnboard -->|true| OnboardingScreen["OnboardingScreen()"]:::comp
    StateOnboard -->|false| GreetingsScreen["Greetings()"]:::comp

    OnboardingScreen -->|User clicks 'Continue'| EventContinue["onContinueClicked()"]:::event
    EventContinue -->|Mutates state to false| StateOnboard

    GreetingsScreen --> LazyList["LazyColumn(1000 items)"]:::comp
    LazyList --> GreetingCard["Greeting(name)"]:::comp

    subgraph CardComponent ["CardContents (Internal Component State)"]
        CardState["expanded: Boolean<br/>(rememberSaveable)"]:::state
        SpringAnim["animateContentSize(spring: Bouncy)"]:::comp
        CardState --> SpringAnim
    end

    GreetingCard --> CardComponent
    CardComponent -->|Toggle expanded| CardState
```

---

## ✨ Core Concepts & Patterns Demonstrated

### 1. State Hoisting & Unidirectional Data Flow
State is elevated to `MyApp`, allowing seamless transitions between the Onboarding screen and the Greeting feed:
```kotlin
@Composable
fun MyApp(modifier: Modifier = Modifier) {
    var shouldShowOnboarding by rememberSaveable { mutableStateOf(true) }

    Surface(modifier, color = MaterialTheme.colorScheme.background) {
        if (shouldShowOnboarding) {
            OnboardingScreen(onContinueClicked = { shouldShowOnboarding = false })
        } else {
            Greetings()
        }
    }
}
```

### 2. Spring-Physics Animated Card Expansion
Cards dynamically expand to show detailed text with a fluid spring oscillation curve:
```kotlin
@Composable
fun CardContents(name: String) {
    var expanded by rememberSaveable { mutableStateOf(false) }

    Row(
        modifier = Modifier
            .padding(12.dp)
            .animateContentSize(
                animationSpec = spring(
                    dampingRatio = Spring.DampingRatioMediumBouncy,
                    stiffness = Spring.StiffnessLow
                )
            )
    ) {
        Column(
            modifier = Modifier
                .weight(1f)
                .padding(12.dp)
        ) {
            Text(text = "Hello ")
            Text(
                text = name,
                style = MaterialTheme.typography.headlineMedium.copy(fontWeight = FontWeight.Bold)
            )
            if (expanded) {
                Text(
                    text = ("Compose ipsum colors sit smoothly, padding themes elegantly... ").repeat(4)
                )
            }
        }
        IconButton(onClick = { expanded = !expanded }) {
            Icon(
                imageVector = if (expanded) Icons.Filled.ExpandLess else Icons.Filled.ExpandMore,
                contentDescription = if (expanded) stringResource(R.string.show_less) else stringResource(R.string.show_more)
            )
        }
    }
}
```

### 3. Virtualized Lazy Lists (`LazyColumn`)
Replaces legacy `RecyclerView` adapters and view holders with a single declarative construct that efficiently emits visible items on demand.

### 4. Material 3 Theming & Multipreview
Pre-configured with light and dark mode `@Preview` definitions, verifying UI contrast and typography across form factors.

---

## 📱 Key Components & Project Structure

```
Compose-BasicCodeLab/
├── app/
│   ├── src/main/java/com/example/basiccodelab/
│   │   ├── MainActivity.kt                # Host Activity, Onboarding & Greeting Composables
│   │   └── ui/theme/
│   │       ├── Color.kt                   # Material 3 Color tokens (Purple / Blue variants)
│   │       ├── Theme.kt                   # BasicCodeLabTheme wrapper & dynamic color logic
│   │       └── Type.kt                    # Custom Typography hierarchy
│   ├── src/main/res/
│   │   ├── drawable/                      # Vector drawables & launcher icons
│   │   └── values/                        # Localized string keys (show_more, show_less)
│   └── build.gradle.kts                   # Kotlin DSL Gradle build script & Compose BOM
└── gradle/
    └── libs.versions.toml                 # Version Catalog for dependencies & plugins
```

---

## 🛠️ Technology Stack Matrix

| Component | Library / Framework | Version / Notes |
|---|---|---|
| **Platform** | Android | API 24+ (Target SDK 35, Compile SDK 35) |
| **Language** | Kotlin | 2.0+ with Kotlin Compose Compiler Plugin |
| **UI Toolkit** | Jetpack Compose | Compose BOM (`androidx.compose:compose-bom`) |
| **Design System** | Material Design 3 | `androidx.compose.material3:material3` |
| **Animation Engine** | Compose Animation Core | Physics-based spring animations |
| **Architecture** | Unidirectional Data Flow | State hoisting & `rememberSaveable` |
| **Tooling** | Android Studio Preview | Multipreview (Light & Dark theme variants) |

---

## 🚀 Getting Started

### Prerequisites
- **Android Studio Ladybug (2024.2.1+)** or newer.
- **JDK 17 or JDK 21**.
- **Android SDK 35**.

### Build & Run
1. **Clone the repository**:
   ```bash
   git clone https://github.com/shayann07/Compose-BasicCodeLab.git
   cd Compose-BasicCodeLab
   ```
2. **Open in Android Studio**: Open the folder and allow Gradle to complete dependency resolution.
3. **Execute Build**:
   ```bash
   ./gradlew assembleDebug
   ```
4. **Deploy**: Run on an emulator or physical device running Android 7.0 (Nougat) or higher.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE) — Copyright (c) 2026 [shayann07](https://github.com/shayann07).
