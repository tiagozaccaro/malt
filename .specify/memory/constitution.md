# Malt Constitution

## Core Principles

### I. Mission

Malt is an open-source macOS compatibility platform for running Windows applications and games on Apple Silicon.

Malt SHALL provide a polished native macOS experience while exposing the underlying compatibility stack in a transparent, reproducible, and maintainable manner.

Malt SHALL aim to provide functionality comparable to modern commercial Wine-based solutions while remaining independently developed and based primarily on open-source technologies.

Malt is NOT an operating system, emulator, or Windows virtual machine.

Its primary purpose is compatibility-layer execution of Windows software directly within macOS.

### II. Open-Source Foundation

Malt SHALL prioritize established open-source projects rather than reimplementing mature compatibility technology.

Relevant upstream technologies MAY include:

* Wine
* Wine-derived CodeWeavers contributions where available upstream
* DXVK
* DXVK-macOS
* VKD3D
* VKD3D-Proton where appropriate
* DXMT
* FAudio
* Wine Mono
* MoltenVK
* msync
* SDL
* LLVM
* FreeType
* GnuTLS
* Apple Game Porting Toolkit components where legally redistributable
* Other compatible open-source projects

Every dependency SHALL have:

* Source repository
* Version
* Commit/tag
* License
* Patch set
* Build instructions

recorded by the build system.

### III. CrossOver Compatibility Without CrossOver Dependency

Malt MAY reproduce publicly observable architectural concepts and functionality found in CrossOver.

Malt SHALL NOT:

* copy proprietary CrossOver source code
* redistribute proprietary CrossOver binaries without appropriate rights
* copy proprietary CrossOver databases
* copy proprietary CrossOver assets
* depend on a commercial CrossOver installation
* represent itself as CrossOver

Malt SHOULD independently implement equivalent functionality using legally available open-source technologies.

Where CodeWeavers has contributed functionality to upstream Wine or other open-source projects, Malt SHOULD prefer the upstream implementation.

### IV. Whisky Lessons

Malt SHALL treat the archived Whisky project as an architectural reference and source of lessons, not as the project's runtime foundation.

Whisky demonstrated a useful macOS architecture consisting of:

* Native SwiftUI interface
* Wine bottles
* Wine runtime
* Apple Game Porting Toolkit
* D3DMetal
* DXVK-macOS
* MoltenVK
* msync
* Native command-line tooling
* Debugging and profiling
* Application/game installation

Malt MAY reuse compatible open-source ideas and code from Whisky where licensing permits.

Malt SHALL NOT assume that Whisky's architecture or component versions remain appropriate for modern macOS.

Malt SHALL specifically avoid reproducing Whisky's long-term maintenance limitations, including dependence on an obsolete bundled compatibility stack.

### V. Runtime Independence

The Malt application SHALL be separated from the compatibility runtime.

The application SHOULD NOT contain hard-coded assumptions about one Wine version.

The runtime SHALL be independently versioned.

Conceptually:

Malt Application
→ Runtime Manager
→ Compatibility Runtime
→ Wine
→ Graphics Backend
→ macOS

This SHALL allow runtime upgrades without requiring an application release for every Wine update.

### VI. Runtime Components

A Malt runtime SHALL be composed of independently identifiable components.

A runtime MAY contain:

* Wine
* Wine patches
* Wine Mono
* FAudio
* DXVK
* DXVK-macOS
* DXMT
* VKD3D
* D3DMetal
* MoltenVK
* msync
* Supporting libraries
* Supporting tools

The runtime manager SHALL know which versions of each component belong together.

A runtime manifest SHOULD resemble:

```yaml
runtime:
  wine: ...
  dxvk: ...
  dxmt: ...
  vkd3d: ...
  d3dmetal: ...
  moltenvk: ...
  msync: ...
```

### VII. Apple Silicon First

Apple Silicon SHALL be the primary platform.

Malt SHALL explicitly support the execution model:

Windows x86-64 application
→ Wine
→ x86-64 translation where required
→ ARM64 macOS
→ Metal

The architecture SHALL account for:

* Apple Silicon
* Rosetta 2
* x86-64 Windows applications
* ARM64 macOS
* Metal
* macOS filesystem permissions
* macOS security restrictions

Native Windows ARM64 applications MAY be supported later.

Intel macOS MAY be supported where practical but SHALL NOT compromise the Apple Silicon architecture.

### VIII. Graphics Backend Abstraction

Graphics translation SHALL be modular.

Malt SHOULD support multiple rendering technologies when technically and legally possible.

Potential backends include:

* D3DMetal
* DXMT
* DXVK-macOS
* DXVK
* VKD3D
* WineD3D
* MoltenVK

The application SHALL NOT hard-code one universal graphics backend.

A game/application profile MAY select a specific backend.

The architecture SHALL permit new graphics backends to be introduced without redesigning bottle management or the user interface.

### IX. Compatibility Selection

Malt SHALL support application-specific compatibility configuration.

A compatibility profile MAY specify:

* Wine version
* Runtime version
* Windows version
* Graphics backend
* DXVK version
* DXMT version
* D3DMetal configuration
* VKD3D version
* Synchronization implementation
* Environment variables
* DLL overrides
* Registry changes
* Launch arguments
* Dependencies
* Workarounds
* Performance settings

Compatibility profiles SHOULD be declarative and version controlled.

Application-specific settings SHALL NOT silently modify unrelated bottles.

### X. Compatibility Database

Malt SHALL provide a structured compatibility database.

The database SHOULD contain:

* Application identifier
* Executable information
* Application version
* Recommended runtime
* Recommended graphics backend
* Required dependencies
* Known workarounds
* Known regressions
* Performance notes
* macOS compatibility
* Apple Silicon compatibility
* Verification status

Compatibility information SHALL distinguish between:

* Verified
* Community verified
* Reported
* Untested
* Broken

The system SHALL NOT represent community reports as guaranteed compatibility.

### XI. Bottles

Windows applications SHALL run inside isolated Wine prefixes called bottles.

A bottle SHALL encapsulate application-specific Windows state.

A bottle MAY contain:

* Windows filesystem
* Registry
* Installed Windows software
* DLLs
* Application settings
* Compatibility configuration
* Environment configuration

Malt SHOULD make bottles portable where technically practical.

Deleting one bottle SHALL NOT affect unrelated bottles.

### XII. Installation and Discovery

Malt SHOULD provide automated installation workflows.

The system SHOULD support:

* Windows installers
* EXE files
* MSI files
* Portable applications
* Game launchers
* Existing installations
* Drag-and-drop installation

Installation logic SHOULD be profile-driven rather than implemented as an ever-growing collection of hard-coded special cases.

The system MAY maintain application-specific installers or scripts where required.

### XIII. Steam and Game Launchers

Malt SHOULD support Windows game distribution platforms where technically possible.

Potential integrations include:

* Steam
* Epic Games
* GOG
* Battle.net
* EA App
* Ubisoft Connect
* Standalone installers

Integration SHALL NOT assume that every launcher behaves identically.

Each launcher SHOULD be treated as an application with its own compatibility requirements.

### XIV. CLI and GUI

Malt SHALL provide both:

1. Native macOS GUI
2. Command-line interface

The GUI SHOULD provide:

* Bottle management
* Application installation
* Application launching
* Runtime selection
* Graphics backend selection
* Configuration
* Logs
* Diagnostics
* Updates

The CLI SHOULD provide automation-friendly equivalents.

Example:

```bash
malt create MyGame
malt install MyGame game.exe
malt run MyGame
malt configure MyGame
malt logs MyGame
malt diagnose MyGame
malt runtime list
malt runtime install <runtime>
```

The GUI SHALL use the same underlying services as the CLI.

Business logic SHALL NOT be duplicated between GUI and CLI implementations.

### XV. Native macOS Experience

The GUI SHALL be a native macOS application.

SwiftUI SHOULD be the default UI technology unless a specific technical requirement justifies another approach.

Malt SHOULD follow macOS conventions for:

* Windows
* Menus
* File selection
* Drag and drop
* Notifications
* Settings
* Keyboard shortcuts
* Application lifecycle
* Accessibility
* Dark/light appearance

The UI SHALL hide unnecessary Wine complexity from normal users while making advanced configuration accessible.

### XVI. Diagnostics

Every application launch SHALL be diagnosable.

Malt SHOULD expose:

* macOS version
* Apple Silicon model
* Wine version
* Runtime version
* Graphics backend
* GPU information
* Bottle path
* Environment variables
* DLL overrides
* Launch arguments
* Relevant logs
* Crash information

A diagnostic bundle SHOULD be exportable.

Diagnostics SHALL avoid collecting unnecessary personal information.

### XVII. Performance

Performance SHALL be a first-class concern.

Malt SHOULD minimize unnecessary translation layers.

Where multiple valid graphics or synchronization implementations exist, Malt SHOULD allow empirical benchmarking.

Performance configuration MAY be application-specific.

Malt SHOULD eventually support automated compatibility/performance testing across:

* Wine versions
* Graphics backends
* Runtime versions
* macOS versions

Compatibility SHALL generally take precedence over small performance improvements.

### XVIII. Synchronization

Malt SHOULD support modern Wine synchronization technologies where compatible.

Potential implementations include:

* msync
* esync
* fsync

Synchronization configuration SHALL be isolated from unrelated application settings.

The project SHOULD prefer modern synchronization implementations where they improve compatibility or performance.

### XIX. Reproducible Builds

All Malt runtimes SHALL be reproducible from documented source inputs.

Builds SHALL record:

* Source repositories
* Commit hashes
* Release versions
* Compiler/toolchain versions
* Patches
* Build configuration
* Dependencies

The build system SHOULD avoid undocumented downloads.

Prebuilt binaries MAY be used only when their source, licensing, provenance, and compatibility are documented.

### XX. Upstream First

Malt SHALL avoid maintaining unnecessary forks.

When a fix belongs to an upstream project, Malt SHOULD:

1. Identify the responsible component.
2. Reproduce the problem.
3. Implement the smallest appropriate change.
4. Submit the change upstream where appropriate.
5. Maintain a temporary patch until upstream adoption.

Every local patch SHALL document:

* Why it exists
* Upstream project
* Upstream issue/PR when available
* Whether it is expected to be removed

### XXI. Security

Windows software SHALL be treated as untrusted.

Malt SHALL avoid unnecessary macOS privileges.

Malt SHALL NOT silently:

* Request administrator privileges without justification
* Modify unrelated user files
* Execute arbitrary remote scripts
* Install system-level software without user consent

Downloaded installers and compatibility scripts SHALL be treated as untrusted inputs.

### XXII. Licensing

Malt SHALL comply with the license of every component it distributes.

The project SHALL maintain a machine-readable and human-readable software bill of materials.

Licensing information SHALL distinguish:

* Malt code
* Upstream open-source code
* Modified upstream code
* Apple-provided components
* Optional proprietary components

Malt SHALL NOT redistribute proprietary components without appropriate authorization.

### XXIII. No Vendor Lock-In

Malt SHALL NOT intentionally require a specific commercial compatibility provider.

Users SHOULD be able to replace or update runtime components independently.

The architecture SHOULD permit alternative runtime providers in the future.

A user's bottles SHOULD NOT become permanently dependent on Malt-specific services.

### XXIV. Runtime Compatibility

Malt SHALL treat the runtime as a replaceable compatibility engine.

The application SHALL NOT assume:

* One Wine version
* One graphics backend
* One synchronization mechanism
* One runtime layout

This enables future support for newer Wine releases and alternative compatibility technologies without redesigning Malt.

### XXV. Testing

Malt SHALL use layered testing.

Testing SHOULD include:

#### Application layer

* UI tests
* Bottle lifecycle tests
* Runtime management tests
* CLI tests

#### Runtime layer

* Wine startup
* Prefix creation
* Windows executable launch
* DLL loading

#### Graphics layer

* D3D9
* D3D10
* D3D11
* D3D12
* Vulkan
* Metal

#### Compatibility layer

* Known Windows applications
* Known games
* Regression cases

Compatibility regressions SHOULD become automated tests whenever practical.

### XXVI. Reference Applications

Malt SHALL maintain a representative compatibility test suite.

The suite SHOULD include applications from different categories:

* Simple Win32 applications
* .NET applications
* DirectX 9 games
* DirectX 11 games
* DirectX 12 games
* Vulkan applications
* Launchers
* DRM-heavy applications
* Multiplayer applications
* Productivity software

The goal SHALL be to detect regressions across different compatibility mechanisms rather than optimize for a single game.

### XXVII. No Premature Monolith

Malt SHALL be modular.

The architecture SHOULD separate:

```text
Malt UI
   ↓
Malt Services
   ↓
Bottle Manager
   ↓
Runtime Manager
   ↓
Compatibility Engine
   ↓
Wine
   ↓
Graphics Backend
   ↓
macOS / Metal
```

The GUI SHALL NOT directly manipulate Wine internals.

Runtime management SHALL NOT depend on UI implementation details.

The compatibility database SHALL NOT be tightly coupled to a single runtime.

### XXVIII. Intelligent Compatibility

Malt SHOULD eventually provide automatic compatibility selection.

Given an application, Malt MAY determine:

* Best runtime
* Best Wine version
* Best graphics backend
* Required dependencies
* Known workarounds
* Recommended synchronization mode
* Known launch arguments

The system SHOULD use compatibility evidence and testing rather than arbitrary heuristics.

Future versions MAY use automated benchmarking or machine learning to improve configuration selection.

Automatic changes SHALL remain explainable and reversible.

### XXIX. User Control

Malt SHOULD work automatically for normal users.

Advanced users SHALL retain control.

Users SHOULD be able to override:

* Runtime
* Wine version
* Graphics backend
* Environment variables
* DLL overrides
* Windows version
* Synchronization
* Launch arguments
* Bottle configuration

Automatic configuration SHALL never permanently prevent manual configuration.

### XXX. Long-Term Maintainability

Malt SHALL be designed for continuous maintenance.

Unlike the archived Whisky project, Malt SHALL not depend on a permanently frozen Wine/GPTK stack.

Runtime updates SHALL be independent from application releases.

The project SHOULD continuously evaluate:

* New Wine releases
* New Apple macOS releases
* New Apple Silicon generations
* New Metal capabilities
* New graphics translation technologies
* New Windows compatibility requirements

Compatibility SHALL be treated as an evolving target.

### XXXI. Project Identity

The project SHALL be named **Malt**.

"Malt" refers to the project's relationship with Wine while establishing an independent identity.

Malt SHALL NOT present itself as:

* Wine
* Proton
* CrossOver
* Whisky

It is an independent compatibility platform built on open-source technologies.

## Governance

This constitution is the highest-level engineering contract for Malt.

Specifications, plans, tasks, implementations, and architectural decisions MUST comply with this constitution.

When a specification conflicts with this constitution, the constitution SHALL take precedence.

Constitution amendments SHALL document:

* Motivation
* Affected principles
* Migration requirements
* Compatibility implications

The project SHOULD favor incremental amendments over large, frequent changes.

The constitution SHALL evolve when technical reality changes, but compatibility, openness, reproducibility, and maintainability SHALL remain foundational principles.

**Version**: 1.0.0 | **Ratified**: 2026-10-07 | **Last Amended**: 2026-10-07