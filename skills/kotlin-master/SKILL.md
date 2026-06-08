---
name: kotlin-master
description: Use when writing, reviewing, debugging, or explaining Kotlin code, Android apps, Jetpack Compose UI, Kotlin coroutines, Flow, StateFlow, SharedFlow, ViewModel, Room, DataStore, Hilt, WorkManager, Navigation Compose, Kotlin Multiplatform, Gradle Kotlin DSL, `build.gradle.kts`, `libs.versions.toml`, nullability, sealed/data/value classes, or modern Kotlin architecture and testing choices may affect correctness.
---

# Kotlin Master

Use this skill for modern Kotlin work. For Android, assume Kotlin-first and Compose-first defaults unless the user explicitly asks for legacy Views or Java interop.

## 1. When to use this skill

- Use it any time you write or review Kotlin.
- Use it any time Android, Jetpack Compose, coroutines, Flow, or Gradle Kotlin DSL are involved.
- Use it any time the task mentions `build.gradle.kts`, `libs.versions.toml`, Room, DataStore, Hilt, WorkManager, or Kotlin Multiplatform.
- Use it any time nullability, sealed types, state management, or lifecycle-aware async behavior matters.

## 2. First classify the target

- State the target before giving advice:
  - plain Kotlin/JVM
  - Android app
  - Jetpack Compose screen
  - Kotlin Multiplatform
  - Gradle/build configuration
- Do not blur Kotlin language advice, Android framework rules, and Compose runtime rules together.
- For modern Android, prefer Kotlin + Jetpack Compose + `ViewModel` + `StateFlow` as the default stack.

## 3. Kotlin language rules

- Prefer `val` over `var`.
- Prefer immutable models and read-only collection types at API boundaries.
- Use `data class` for value/state models.
- Use `sealed class` or `sealed interface` for closed result or UI state hierarchies.
- Use `enum class` only for fixed constants, not for rich state with different payloads.
- Use nullable types only when absence is a real state.
- Avoid `!!`. Fix the model, validate the boundary, or use `requireNotNull`, safe calls, and Elvis defaults.
- Keep scope functions readable:
  - `let` for nullable transforms
  - `apply` for object configuration
  - `also` for side effects
  - `run` for compute-and-return
- Do not stack nested scope functions just to look concise.
- Prefer top-level functions only when they are actually cohesive with the package.

## 4. Coroutines and Flow rules

- Use `suspend` for asynchronous work.
- Use structured concurrency. Prefer `viewModelScope`, `lifecycleScope`, or an injected scope with a real owner.
- Never use `GlobalScope` in app code.
- Re-throw `CancellationException`. Do not hide coroutine cancellation inside broad `catch` blocks.
- Use `StateFlow` for current UI state.
- Use `SharedFlow` for broadcast-like or event-style streams when a single current state value is not the model.
- Use plain `Flow` for reactive data streams that do not need an always-current state holder.
- Use `collectLatest` when stale in-flight work should be cancelled by new values.
- Do not block the main thread. Move I/O and heavy computation out of UI code.

## 5. Modern Android and Compose rules

- For modern Android, prefer Jetpack Compose over XML/View patterns unless legacy code requires otherwise.
- Keep composables pure, cheap, and side-effect free except through Compose effect APIs.
- Do not run network, database, or repository writes directly inside a composable body.
- Let `ViewModel` own screen state and expose immutable UI state.
- Model screen state explicitly with immutable `data class` or sealed UI state types.
- Use unidirectional data flow: state down, events up.
- Collect flows in Compose with `collectAsStateWithLifecycle()` on Android.
- Hoist state when a composable should be reusable or testable.
- Use:
  - `remember` for composition-local caching
  - `rememberSaveable` for state that should survive recreation
  - `derivedStateOf` for derived UI state that would otherwise recompose too often
- In `LazyColumn` and related lazy containers, provide stable keys.
- Do not write to state after reading it in the same composable path in a way that creates recomposition loops.

## 6. Architecture defaults

- Default to UI layer + data layer.
- Add a domain/use-case layer only when business logic is complex, reused, or worth isolating.
- Keep repositories as the source of truth boundary between local and remote data.
- For offline-first Android apps, prefer local persistence as the observable source of truth and sync remote data through the repository.
- Prefer Room for structured local data.
- Prefer DataStore over SharedPreferences for new preference storage.
- Prefer WorkManager for guaranteed deferrable background work.
- Hilt is the safest Android default for DI. If the project already uses Koin or manual DI, follow the existing pattern unless there is a real reason to change it.

## 7. Gradle and build rules

- Prefer Gradle Kotlin DSL (`build.gradle.kts`) for modern Kotlin and Android projects.
- Prefer version catalogs in `libs.versions.toml` instead of scattering version strings across modules.
- Do not paste Groovy DSL snippets into Kotlin DSL.
- For modern Kotlin Gradle configuration, prefer `compilerOptions {}` over stale `kotlinOptions {}` guidance.
- Be explicit about whether advice is for:
  - Android Gradle Plugin
  - Kotlin Gradle plugin
  - Compose compiler/plugin
  - plain Gradle
- Do not guess version compatibility across Kotlin, AGP, Compose, and libraries. Verify when version-specific behavior matters.

## 8. Testing and quality rules

- Test coroutines with `runTest`.
- Prefer fake repositories over mocking every dependency boundary.
- Test Flow behavior directly instead of only testing final UI snapshots.
- For Compose UI, use semantics-driven UI tests.
- Keep mutable implementation details private and public state immutable.
- Prefer explicit loading, empty, success, and error states over boolean flag soup.

## 9. Performance and platform rules

- Keep expensive transforms out of composable bodies.
- Cache derived UI work with `remember` or compute it in the `ViewModel`.
- Use Baseline Profiles for startup-sensitive Android apps.
- Use lifecycle-aware collection to avoid leaks and wasted background work.
- Avoid passing `Activity` or `View` context into long-lived singletons or `ViewModel`s.
- Ask for Android permissions late and only when genuinely needed.

## 10. AI mistake checklist

Before giving Kotlin or Android guidance, check for these failures:

- Java/XML/View-era Android patterns suggested for a Compose-first task
- business logic or network access placed inside composables
- `GlobalScope` used instead of structured scopes
- `!!` used to silence nullability problems
- `MutableStateFlow` exposed publicly instead of `StateFlow`
- `collect` or `collectAsState` used without lifecycle awareness on Android when `collectAsStateWithLifecycle()` is the safer default
- `SharedPreferences` suggested as the default for new code
- Groovy Gradle syntax pasted into `build.gradle.kts`
- versions hardcoded everywhere instead of using `libs.versions.toml`
- loading/error/empty states ignored
- missing lazy list keys or expensive recomposition work in Compose
- broad exception handling that swallows `CancellationException`

## 11. Output style for future agents

- State the target stack first.
- If the answer is Android-specific, say so explicitly.
- If the answer is Compose-specific, say so explicitly.
- Prefer copyable Kotlin examples over abstract explanation.
- Separate language advice from framework advice.
- When version-sensitive behavior matters, name the exact Kotlin, Android, or Gradle surface that needs verification.
