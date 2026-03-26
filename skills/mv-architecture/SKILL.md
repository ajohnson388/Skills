---
name: mv-architecture
description: Defines the iOS application's code architectural pattern. Use when implementing new code in an iOS project.
---

> **Note:** Use of `ProjectName` is meant to be replaced with the actual project name.

# Architecture: MV (Model-View)

This project uses a Model-View architecture. There are no ViewModels. Business logic lives in `Presentation` objects, which are injected into the SwiftUI environment and accessed by views directly. Features are self-encapsulated in SwiftUI views combined with environment variables.

A central file `ProjectNameApp` is the entry point of the app and lives under `UI/ProjectNameApp.swift`. This entry point view wraps another view, `MainView` which lives under `UI/Modules/Main/MainView.swift`.

The purpose of the entry point class, `ProjectNameApp`, is to provide a centralized location for handling app-level UI events and system notifications.

## Project Structure
When creating a new project, use the following folder structure for organizing files.

```
ProjectName/
├── Models/              # Data models used throughout the app. Plain old objects
├── Presentation/        # Feature objects for use in feature views. Uses @Observable
├── Services/            # Objects that can peform async operations and/or manage business logic. No SwiftUI
├── Extensions/          # Definitions for type extensions. E.g. `String+Extension.swift` for string extensions
├── Resources/           # Asset type files like sounds, and video
└── UI/                  # All SwiftUI code
    ├── Environment/     # EnvironmentKey definitions for each Presentation object
    ├── Modules/         # Feature views in the app
    ├── Views/           # Reusable UI components (List, Picker, etc.). Not tied to any feature
    ├── ViewModifiers/   # Reusable view modifiers, e.g. liquid glass backwards compatibility
    ├── PreferenceKeys/  # Definition files for preference keys
    ├── Router/          # A special presentation object for controlling app routing
    └── Theme.swift      # Definition file for all colors to be used
```

## Components

| Layer | Path | Role |
|---|---|---|
| Models | `ProjectName/Models/` | Plain data types. No logic, no side effects. |
| Presentation | `ProjectName/Presentation/` | Stateful `@Observable` objects. Business logic, state management, and coordination between services. |
| Services | `ProjectName/Services/` | Low-level infrastructure (audio, permissions, logging). No SwiftUI dependencies. |
| UI | `ProjectName/UI/` | All SwiftUI views, environment keys, modifiers, and reusable components. |
| Resources | `ProjectName/Resources/` | Assets, audio files, MIDI exercises, shaders. |
| Extensions | `ProjectName/Extensions/` | Swift/SwiftUI/AVKit extensions. |

### Feature View
A FeatureView contains references to system SwiftUI views or custom ones under the `UI/Views/` folder. A FeatureView communicates UI events to the appropriate Presentation object and observes state from it. See `./examples/feature-view.md`.

### Presentation Object
Classes defined under `Presentation/` should represent feature services or abstractions and be of type `@Observable public final class`. These objects are defined via EnvironmentKeys and made available through the SwiftUI environment. See `./examples/presentation.md`.

### Environment
The SwiftUI environment handles the dependency injection of the SwiftUI views. Defining the presentation object through an environment makes them available to be referenced in any view. Values can be overridden by passing a different object through the `.environment(_:)` view modifier. See `./examples/environment.md`.

### Services
These objects act as logical backend components for the presentation object. In general, the logic should live as close to the presentation object for simpler reactive state. Services are there for the presentation objects to delegate off some work. See `./examples/service.md`.

### Router
There exists a special environment object, Router, that is responsible for managing what view to display for things like tab views and for controlling the display of various modals and top bar heights. See `./examples/router.md`.

## Code Design Philosophy
Prefer small, minimal code views over large monolithic views. Instead of a button control that has the left button, right button, and center label as one, break it out into 3 different feature views. Instead of them being generic components, have them be feature driven like `IncrementPitchButtonView`.

**Every view (unless it is a pure UI component) is a feature and owns its own functionality.**
- A view is responsible for its own appearance *and* for triggering the actions it initiates (e.g., a button that opens a file picker owns the file picker presentation logic)
- Views read from and write to `Presentation` objects via the environment. They do not delegate upward to a parent view to perform actions on their behalf
- Pure UI components (e.g., reusable list rows, pickers in `UI/Views/`) may be dumb — they accept data and callbacks. Feature views are not

## Conventions
| Type of view | Naming convention |
|---|---|
| Buttons | `MyCustomButton` |
| List item or row | `MyCustomRow` |
| List section (title and rows) | `MyCustomSection` |
| Generic feature view | `MyCustomView` |
| Text label | `MyCustomLabel` |
