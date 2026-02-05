# AI Agents for Linux Command Library Development

## Overview

This document provides comprehensive guidance on leveraging AI agents and automation tools when contributing to the Linux Command Library project. AI agents can significantly enhance development productivity, code quality, and project maintenance across all supported platforms (Android, iOS, Desktop, CLI, and Web).

## About the Linux Command Library

The Linux Command Library is a comprehensive, offline-first reference application containing over 7,600 Linux command manual pages, 23+ categorized command groups, and practical terminal tips. Built with Kotlin Multiplatform, it delivers a consistent user experience across Android, iOS, Desktop (macOS, Windows, Linux), CLI, and Web platforms.

The application works entirely offline with no internet connection required and includes no tracking software, making it an ideal companion for developers, system administrators, and Linux enthusiasts who need quick access to command documentation.

The project's content is organized into three main categories stored in the `assets/` directory. **Commands** contains 7,675+ individual command manual pages like `ls.md` and `grep.md`. **Basics** includes 23+ category files organizing related commands by topic such as "Git", "Package manager", and "Coding agents". **Tips** consists of a single `tips.md` file containing general terminal usage guidance. Each command and category is stored as a markdown file, which are parsed at runtime and rendered through platform-specific UI implementations.

### Application Execution Flow

When the app launches, it follows this execution flow:

1. **Platform Initialization**: Each platform (Android, iOS, Desktop, CLI) initializes its specific implementation of the `AssetReader` interface, which provides access to the markdown files in the `assets/` directory.

2. **Data Loading**: The application uses repository classes (`CommandsRepository`, `BasicsRepository`, `TipsRepository`) located in `composeApp/src/commonMain/kotlin/com/linuxcommandlibrary/app/data/` to load and cache content. These repositories read markdown files through the `AssetReader`, parse them using `MarkdownParser`, and transform them into data models (`CommandInfo`, `BasicCategory`, etc.).

3. **UI Rendering**: The Compose Multiplatform UI (shared across Android, iOS, and Desktop) displays the content through ViewModels that manage state and user interactions. The CLI platform uses a simpler text-based interface. Navigation is handled through a bottom navigation bar with three main sections: Commands, Basics (categorized commands), and Tips.

4. **Search and Browse**: Users can search for commands by name, browse commands alphabetically, explore categorized command groups, or read general tips. When a command is selected, its markdown content is parsed into sections (NAME, SYNOPSIS, DESCRIPTION, etc.) and rendered with proper formatting and clickable cross-references to other commands.

5. **Deep Linking**: The Android app supports deep links (e.g., `linuxcommandlibrary://man/ls`) for direct navigation to specific commands, useful for integration with other apps or documentation.

### Code Organization

The codebase follows a clean Kotlin Multiplatform architecture:

- **`common/`**: Core shared logic including platform abstractions, markdown parsing, and data models
- **`composeApp/`**: Compose Multiplatform UI code shared across Android, iOS, and Desktop
  - **`commonMain/`**: Shared UI components, ViewModels, and repositories
  - **`androidMain/`, `iosMain/`, `desktopMain/`**: Platform-specific implementations (AssetReader, sharing, preferences)
- **`android/`**: Android-specific application wrapper and configuration
- **`iosApp/`**: iOS application wrapper (SwiftUI integration)
- **`desktopApp/`**: Desktop application entry point
- **`cli/`**: Command-line interface implementation using Kotlin/Native
- **`websiteBuilder/`**: Static website generation for linuxcommandlibrary.com
- **`assets/`**: Content directory containing all markdown files
  - **`commands/`**: 7,675+ individual command manual pages
  - **`basics/`**: 23+ category files organizing related commands
  - **`tips.md`**: General terminal tips and tricks

### Current Features

- **Comprehensive Command Reference**: 7,600+ Linux command manual pages with detailed documentation
- **Categorized Command Groups**: 23+ categories including Git, System Information, Package Managers, Hacking Tools, Text Editors, and Coding Agents
- **Offline-First Design**: All content is bundled with the app; no internet connection required
- **Cross-Platform Support**: Native apps for Android, iOS, macOS, Windows, Linux (desktop and CLI)
- **Powerful Search**: Fast command search by name with intelligent ranking (exact matches first, then prefix matches)
- **Terminal Tips**: Practical guidance on terminal usage, keyboard shortcuts, and command syntax
- **Markdown Rendering**: Rich text formatting with code blocks, cross-references, and clickable command links
- **Deep Linking**: Direct navigation to specific commands via URL schemes (Android)
- **Dark Mode Support**: Platform-aware theming for comfortable reading in any environment
- **No Tracking**: Privacy-focused design with no analytics or telemetry
- **Open Source**: Apache 2.0 licensed with active community contributions

## Table of Contents

- [Development Agents](#development-agents)
- [GitHub Copilot Integration](#github-copilot-integration)
- [CI/CD Automation Agents](#cicd-automation-agents)
- [Code Quality Agents](#code-quality-agents)
- [Documentation Agents](#documentation-agents)
- [Testing Agents](#testing-agents)
- [Platform-Specific Considerations](#platform-specific-considerations)
- [Best Practices](#best-practices)
- [Contributing Guidelines](#contributing-guidelines)

---

## Development Agents

### Supported AI Coding Assistants

The Linux Command Library project benefits from various AI coding assistants that can help with Kotlin Multiplatform development:

#### **GitHub Copilot**
- **Purpose**: Real-time code suggestions and completion
- **Integration**: Works natively in JetBrains IDEs (IntelliJ IDEA, Android Studio), VS Code
- **Use Cases**:
  - Kotlin code generation for multiplatform modules
  - Compose UI component creation
  - Unit test generation
  - Documentation comments
  - Gradle build script assistance

#### **Aider**
- **Purpose**: AI pair programming in the terminal
- **Use Cases**:
  - Automated refactoring across multiple files
  - Large-scale code transformations
  - Git integration for atomic commits
- **Installation**: `pip install aider-chat`

#### **Cursor IDE**
- **Purpose**: AI-first code editor
- **Use Cases**:
  - Codebase-wide understanding
  - Multi-file edits
  - Natural language to code conversion

#### **JetBrains AI Assistant**
- **Purpose**: Native AI support in JetBrains IDEs
- **Use Cases**:
  - Kotlin-specific suggestions
  - Refactoring assistance
  - Test generation
  - Documentation generation

### Command Reference

Many AI coding agents are documented in our command library under the "Coding agents" category:

- `claude` - Anthropic's Claude CLI
- `opencode` - OpenAI Codex command-line tool
- `copilot` - GitHub Copilot CLI
- `grok` - xAI's Grok assistant
- `gemini` - Google's Gemini Code Assist
- `interpreter` - Open Interpreter
- `aider` - AI pair programming tool
- `forge` - AI code generation framework
- `plandex` - AI-powered project planner
- `bondai` - AI agent framework

---

## GitHub Copilot Integration

### Setup for Kotlin Multiplatform

1. **Install GitHub Copilot Extension**
   - IntelliJ IDEA/Android Studio: Settings → Plugins → GitHub Copilot
   - VS Code: Install "GitHub Copilot" extension

2. **Configure for Kotlin**
   - Ensure Kotlin plugin is up to date
   - Enable Copilot for `.kt`, `.kts`, and `.gradle.kts` files

3. **Optimize for Multiplatform Code**
   - Use consistent naming conventions across platforms
   - Leverage `expect`/`actual` declarations properly
   - Comment platform-specific requirements clearly

### Effective Prompts

When working with AI agents on this project, use descriptive comments to guide code generation:

```kotlin
// Create a composable function that displays a command card
// with title, description, and category
// Support dark mode and material 3 design
fun CommandCard(command: Command) { ... }
```

### Copilot Chat Usage

Use Copilot Chat for:
- **Code Explanations**: Understanding existing multiplatform code
- **Refactoring**: "Refactor this ViewModel to use Kotlin Flows"
- **Testing**: "Generate unit tests for this CommandRepository"
- **Documentation**: "Add KDoc comments to this public API"
- **Platform Migration**: "Convert this Android-specific code to common multiplatform code"

---

## CI/CD Automation Agents

### GitHub Actions Workflow

Our CI/CD pipeline (`.github/workflows/android.yml`) uses automated agents for:

1. **Build Agents**
   - APK generation (Android)
   - CLI binary compilation (Linux x64/arm64, macOS x64/arm64, Windows x64)
   - Desktop installers (DMG, MSI, DEB, RPM, AppImage)

2. **Testing Agents**
   - Common unit tests: `./gradlew :common:testDebugUnitTest`
   - Desktop tests: `./gradlew :common:desktopTest`
   - Android instrumentation tests

3. **Release Automation**
   - Multi-platform artifact generation
   - Automatic GitHub release creation
   - Version extraction and tagging

### Dependabot Integration

Dependabot acts as an automated dependency management agent:

```yaml
# .github/dependabot.yml (recommended)
version: 2
updates:
  - package-ecosystem: "gradle"
    directory: "/"
    schedule:
      interval: "weekly"
```

### Spotless Agent

Code formatting is automated via Spotless:

```bash
# Check formatting
./gradlew spotlessCheck

# Apply formatting
./gradlew spotlessApply
```

**Configuration**: See `build.gradle.kts` for ktlint rules

---

## Code Quality Agents

### Static Analysis

#### **Detekt**
- **Purpose**: Kotlin static code analysis
- **Setup**:
  ```kotlin
  // Add to build.gradle.kts
  plugins {
      id("io.gitlab.arturbosch.detekt") version "1.23.0"
  }
  ```
- **Usage**: `./gradlew detekt`

#### **Android Lint**
- **Purpose**: Android-specific code quality checks
- **Usage**: `./gradlew lint`

#### **Spotless + ktlint**
- **Purpose**: Code formatting enforcement
- **Current Configuration**:
  - No wildcard imports (disabled)
  - Package name validation (disabled)
  - Function naming conventions (disabled)
  - Comment location rules (disabled)

### AI-Assisted Code Review

When creating PRs, consider using AI-powered code review tools:

1. **CodeRabbit**
   - Automated PR reviews
   - Security vulnerability detection
   - Best practice suggestions

2. **Qodana** (JetBrains)
   - Deep code quality analysis
   - Kotlin-specific insights
   - CI/CD integration

3. **SonarCloud**
   - Code quality metrics
   - Technical debt tracking
   - Security hotspot detection

---

## Documentation Agents

### KDoc Generation

Use AI agents to generate comprehensive KDoc comments:

```kotlin
/**
 * Repository for managing Linux command data across all platforms.
 *
 * This multiplatform repository provides access to command information
 * including manual pages, categories, and tips.
 *
 * @property commandDao Data access object for command operations
 * @property categoryDao Data access object for category operations
 * @constructor Creates a CommandRepository with the specified DAOs
 */
class CommandRepository(
    private val commandDao: CommandDao,
    private val categoryDao: CategoryDao
) { ... }
```

### README Generation

AI agents can help maintain comprehensive documentation:

- **Project Structure**: Explain module organization
- **Setup Instructions**: Platform-specific setup guides
- **API Documentation**: Public interface documentation
- **Examples**: Code samples for common use cases

### Markdown Linting

Use AI-powered markdown tools:

```bash
# markdownlint for consistency
npx markdownlint-cli2 "**/*.md"
```

---

## Testing Agents

### Unit Test Generation

AI agents excel at generating comprehensive unit tests:

#### **Example Prompt for Copilot**
```kotlin
// Generate unit tests for CommandRepository.searchCommands()
// Include edge cases: empty query, special characters, no results
```

#### **Test Structure**
Follow existing patterns in:
- `common/src/commonTest/kotlin/CommonTests.kt`
- `common/src/desktopTest/kotlin/DesktopTests.kt`
- `android/src/androidTest/java/com/inspiredandroid/linuxcommandbibliotheca/ComposeDeeplinkTests.kt`

### Automated Testing Tools

1. **Kotest** (Recommended for KMP)
   ```kotlin
   dependencies {
       commonTest {
           implementation("io.kotest:kotest-framework-engine:5.8.0")
           implementation("io.kotest:kotest-assertions-core:5.8.0")
       }
   }
   ```

2. **Turbine** (Flow Testing)
   ```kotlin
   // Test Kotlin Flows with Turbine
   viewModel.commandsFlow.test {
       assertEquals(expected, awaitItem())
   }
   ```

3. **AI Test Generation Services**
   - **Diffblue Cover**: Automated Java/Kotlin test generation
   - **Ponicode**: AI-powered unit test creation
   - **Codium AI**: Test generation and suggestions

---

## Platform-Specific Considerations

### Android Platform

**AI Agent Assistance**:
- Jetpack Compose UI generation
- ViewModel implementation
- Navigation graph setup
- Dependency injection with Koin/Hilt

**Testing**:
- Compose UI testing with semantics
- Deeplink validation (see `ComposeDeeplinkTests.kt`)
- Instrumentation tests

### iOS Platform

**AI Agent Assistance**:
- SwiftUI integration code
- Kotlin/Native interop
- Platform-specific API usage

**Key Files**:
- `common/src/iosMain/` - iOS-specific implementations
- `iosApp/` - iOS application code

### Desktop Platform

**AI Agent Assistance**:
- Compose Multiplatform Desktop UI
- Platform-specific file operations
- Window management

**Key Files**:
- `desktopApp/` - Desktop application
- `common/src/desktopMain/` - Desktop-specific code

### CLI Platform

**AI Agent Assistance**:
- Kotlin/Native CLI implementation
- Cross-platform binary generation
- Command-line argument parsing

**Targets**:
- Linux (x64, arm64)
- macOS (x64, arm64)
- Windows (x64)

**Key Files**:
- `cli/` - CLI application module
- `common/src/nativeCliMain/` - Native CLI implementations

### Web Platform

**AI Agent Assistance**:
- Kotlin/JS or Kotlin/Wasm code
- Website generation
- SEO optimization

**Key Files**:
- `websiteBuilder/` - Website generation module

---

## Best Practices

### 1. Context-Aware Prompting

Provide sufficient context when working with AI agents:

```
Good: "Create a Kotlin Multiplatform ViewModel that manages command search state 
using StateFlow, with debouncing for search input, and error handling"

Poor: "Make a ViewModel"
```

### 2. Review AI-Generated Code

Always review and test AI-generated code:
- ✅ Check for null safety
- ✅ Verify platform compatibility
- ✅ Ensure proper error handling
- ✅ Validate against project conventions
- ✅ Run tests: `./gradlew test`
- ✅ Run lint: `./gradlew spotlessCheck`

### 3. Incremental Development

Use AI agents for incremental changes:
1. Generate small, focused code changes
2. Test immediately
3. Commit frequently
4. Request AI assistance for next step

### 4. Multiplatform Considerations

When using AI agents for multiplatform code:
- ✅ Use `expect`/`actual` declarations correctly
- ✅ Avoid platform-specific APIs in common code
- ✅ Test on multiple platforms
- ✅ Document platform-specific behavior

### 5. Security Best Practices

AI agents should help enforce security:
- ✅ No hardcoded secrets or API keys
- ✅ Proper input validation
- ✅ Secure data storage
- ✅ HTTPS for network calls
- ✅ ProGuard rules for release builds (see `proguard-rules.pro`)

### 6. Performance Optimization

Use AI agents to identify and fix performance issues:
- Memory leak detection
- Unnecessary recompositions (Compose)
- Inefficient algorithms
- Database query optimization

### 7. Accessibility

AI agents can help ensure accessibility:
- Compose semantics for screen readers
- Content descriptions
- Keyboard navigation
- High contrast support

---

## Contributing Guidelines

### Using AI Agents for Contributions

1. **Code Generation**
   - Use AI agents to scaffold new features
   - Follow existing project structure
   - Maintain consistency across platforms

2. **Documentation**
   - Generate comprehensive docs for new features
   - Update existing documentation
   - Add examples and use cases

3. **Testing**
   - Generate unit tests for new code
   - Maintain test coverage
   - Include edge cases

4. **Code Review**
   - Use AI-powered tools to self-review before PR
   - Address suggestions from automated reviews
   - Request human review for complex changes

### Pre-Submission Checklist

Before submitting a PR with AI-assisted code:

- [ ] Code compiles on all platforms
- [ ] Tests pass: `./gradlew test`
- [ ] Formatting is correct: `./gradlew spotlessApply`
- [ ] No new warnings or errors
- [ ] Documentation is updated
- [ ] Changes follow project conventions
- [ ] AI-generated code is reviewed and understood
- [ ] Commit messages are descriptive

### Attribution

When AI agents significantly contribute to your code:
- Add comments explaining complex AI-generated logic
- Mention AI assistance in PR description if substantial
- Ensure you understand and can maintain the code

### Example PR Description

```markdown
## Description
Added new command search functionality with fuzzy matching

## Changes
- Implemented fuzzy search algorithm in CommandRepository
- Added search debouncing (300ms)
- Created comprehensive unit tests
- Updated documentation

## AI Assistance
- Used GitHub Copilot for test generation
- Aider for multi-file refactoring
- Manual review and adjustments applied

## Testing
- [x] Unit tests passing
- [x] Manual testing on Android
- [x] Manual testing on Desktop
- [x] CLI tested on Linux
```

---

## Additional Resources

### Official Documentation

- [Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform.html)
- [Jetpack Compose](https://developer.android.com/jetpack/compose)
- [Compose Multiplatform](https://www.jetbrains.com/lp/compose-multiplatform/)
- [Kotlin/Native](https://kotlinlang.org/docs/native-overview.html)

### AI Agent Documentation

- [GitHub Copilot Docs](https://docs.github.com/en/copilot)
- [Aider Documentation](https://aider.chat/)
- [Cursor IDE](https://cursor.sh/)
- [JetBrains AI Assistant](https://www.jetbrains.com/ai/)

### Project-Specific Links

- [Repository](https://github.com/Rikul/LinuxCommandLibrary)
- [CI/CD Workflow](.github/workflows/android.yml)
- [License](LICENSE)
- [Main README](README.md)

---

## Support and Questions

If you have questions about using AI agents with this project:

1. Check existing documentation
2. Review similar examples in the codebase
3. Open a discussion on GitHub
4. Refer to AI agent-specific documentation

## License

This documentation is part of the Linux Command Library project and is licensed under the Apache 2.0 License. See [LICENSE](LICENSE) for details.

---

**Last Updated**: February 2026  
**Maintainers**: Linux Command Library Contributors
