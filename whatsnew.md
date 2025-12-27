# What's New in mlx-swift-examples

---

# Upgrade: tag-20251112 to tag-20251226

**Upgrade Date:** December 26, 2025
**Total Commits:** 2
**Total Changes:** +3,019 / -585 lines across 31 files

## Executive Summary

This upgrade includes **two main changes**:
1. Complete rewrite of the LLMEval example application with modern UI and new features
2. Migration from CircleCI to GitHub Actions for CI/CD

**Impact Level:** LOW - No changes to core Libraries (MLXLLM, MLXLMCommon, etc.)

---

## Major Changes

### 1. LLMEval Example App Rewrite (#452)

**Commit:** `fc3afc7` (December 11, 2025) by Tim Sneath

Complete modernization of the LLMEval example with MVVM architecture and new capabilities.

#### New Features:

| Feature | Description |
|---------|-------------|
| **Tool Calling** | Function calling support with weather, add, and time tools |
| **Thinking Mode** | Chain-of-thought reasoning toggle |
| **Preset Prompts** | Categorized prompt examples with tool/thinking options |
| **Real-time Metrics** | Live tokens/sec, TTFT, memory usage display |
| **Download Progress** | Visual progress for model downloads |

#### New Files:

**ViewModels:**
- `ViewModels/LLMEvaluator.swift` (393 lines) - Main view model

**Views:**
- `Views/ContentView.swift` (125 lines) - Main view
- `Views/HeaderView.swift` - Model info header
- `Views/MetricsView.swift` - Performance metrics panel
- `Views/OutputView.swift` - Generation output display
- `Views/PromptInputView.swift` - Prompt input area
- `Views/LoadingOverlayView.swift` - Download progress overlay
- `Views/MetricCard.swift` - Individual metric card
- `Views/PresetPromptsSheet.swift` - Prompt selection sheet

**Models:**
- `Models/PresetPrompts.swift` - Preset prompt definitions
- `Models/ToolDefinitions.swift` - Tool input/output types
- `Models/CarKeysStory.md` - Long prompt example
- `Models/LongPrompt.md` - Long prompt example

**Services:**
- `Services/ToolExecutor.swift` (77 lines) - Tool execution management
- `Services/FormatUtilities.swift` - Formatting helpers

#### Tool Calling Example:

```swift
// New tool definition pattern
let currentWeatherTool = Tool<WeatherInput, WeatherOutput>(
    name: "get_current_weather",
    description: "Get the current weather in a given location",
    parameters: [
        .required("location", type: .string, description: "The city and state"),
        .optional("unit", type: .string, description: "Temperature unit")
    ]
) { input in
    WeatherOutput(temperature: 20.0, conditions: "Sunny")
}
```

#### Thinking Mode Example:

```swift
var enableThinking = false
// ...
let userInput = UserInput(
    chat: chat,
    tools: includeWeatherTool ? toolExecutor.allToolSchemas : nil,
    additionalContext: ["enable_thinking": enableThinking]
)
```

#### Default Model Change:

```swift
// Before (tag-20251112)
let modelConfiguration = ModelConfiguration.phi4bit

// After (tag-20251226)
var modelConfiguration = LLMRegistry.qwen3_8b_4bit
```

---

### 2. CI/CD Migration to GitHub Actions (#446)

**Commit:** `7e2e757` (December 2, 2025) by David Koski

Migrated continuous integration from CircleCI to GitHub Actions.

**Changes:**
- Removed `.circleci/config.yml` (74 lines)
- Added `.github/workflows/pull_request.yml` (104 lines)
- Added `.github/ISSUE_TEMPLATE/bug_report.md`
- Added `.github/pull_request_template.md`

---

## Platform Compatibility

The updated LLMEval example supports:
- iOS
- macOS
- visionOS

```swift
#if os(visionOS)
    .padding(40)
#else
    .padding()
#endif
```

---

## iOS-Specific Impact Assessment

### Positive Changes for iOS

1. **Tool Calling Reference**: New pattern for implementing function calling
2. **Thinking Mode**: Demonstrates enabling chain-of-thought reasoning
3. **Metrics Display**: Reference implementation for performance monitoring
4. **MVVM Architecture**: Modern SwiftUI patterns for iOS apps

### No Breaking Changes for iOS

- All changes are confined to `Applications/LLMEval/`
- Core libraries (MLXLLM, MLXLMCommon) are unchanged
- No API modifications
- Package.swift unchanged

---

## Risk Assessment

### Overall Risk Level: LOW

| Category | Risk | Reason |
|----------|------|--------|
| Core Libraries | None | No changes to MLXLLM, MLXLMCommon, or other libraries |
| API Compatibility | None | No breaking API changes |
| Package.swift | None | No dependency version changes |
| Build System | None | No structural changes affecting libraries |
| iOS Compatibility | None | No iOS version requirement changes |

### Why This Upgrade is Safe

1. **Library Directory Unchanged**: `Libraries/` has zero modifications
2. **Package.swift Unchanged**: No dependency changes
3. **Example App Only**: Changes confined to `Applications/LLMEval/`
4. **CI/CD Only**: GitHub Actions migration doesn't affect library code

---

## Files Changed Summary

### Added (22 files)
```
.github/ISSUE_TEMPLATE/bug_report.md
.github/pull_request_template.md
.github/workflows/pull_request.yml
Applications/LLMEval/AppIcon.icon/*
Applications/LLMEval/Models/CarKeysStory.md
Applications/LLMEval/Models/LongPrompt.md
Applications/LLMEval/Models/PresetPrompts.swift
Applications/LLMEval/Models/ToolDefinitions.swift
Applications/LLMEval/Services/FormatUtilities.swift
Applications/LLMEval/Services/ToolExecutor.swift
Applications/LLMEval/ViewModels/LLMEvaluator.swift
Applications/LLMEval/Views/*.swift (7 files)
```

### Modified (3 files)
```
Applications/LLMEval/README.md
Applications/LLMEval/ViewModels/DeviceStat.swift (+2 lines)
mlx-swift-examples.xcodeproj/project.pbxproj
```

### Deleted (6 files)
```
.circleci/config.yml
Applications/LLMEval/ContentView.swift (old - 386 lines)
Applications/LLMEval/LLMEval.entitlements (2 lines removed)
Applications/LLMEval/Assets.xcassets/* (preview assets)
```

---

## Recommendations

1. **Safe to Upgrade**: No code migration required for existing integrations
2. **Review Tool Calling Patterns**: Consider adopting the new `ToolExecutor` pattern for your app
3. **Reference Implementation**: Use the new metrics display as reference for implementing performance monitoring
4. **Thinking Mode**: Test `enable_thinking` context flag with supported models

---

## Conclusion

This is a **safe, low-risk upgrade** focused entirely on improving the LLMEval example application and modernizing CI/CD. The core libraries that your iOS app depends on remain completely unchanged. The new tool calling and thinking mode demonstrations serve as useful reference implementations.

---
---

# Previous Upgrade: 1.0.0 to tag-20251112

**Upgrade Date:** November 12, 2025
**Impact Level:** HIGH - Major breaking changes for library consumers

## Executive Summary

This upgrade represents a **major architectural reorganization** of the mlx-swift-examples repository. The primary change is the extraction of core LLM/VLM libraries into a separate repository ([mlx-swift-lm](https://github.com/ml-explore/mlx-swift-lm)), transforming this repository into a focused collection of examples and applications.

---

## Major Changes

### 1. Repository Restructuring (#441)

**Commit**: `0db7c5d` (November 11, 2025)

The most significant change in this upgrade is the split of language model libraries into a separate repository.

#### What Moved to mlx-swift-lm:
- **MLXLLMCommon** - Common utilities and base classes for language models (~8,000 lines)
- **MLXLLM** - Large Language Model implementations (~15,000 lines)
- **MLXVLM** - Vision Language Model implementations (~7,000 lines)
- **MLXEmbedders** - Embedding model implementations (~2,000 lines)

**Total code removed**: ~31,300 lines across 111 files

#### What Remains in mlx-swift-examples:
- **Applications**: Example iOS/macOS apps
- **Tools**: Command-line utilities
- **MLXMNIST**: MNIST training example
- **StableDiffusion**: Image generation models

#### Migration Path:
```swift
// OLD (no longer works)
.package(url: "https://github.com/ml-explore/mlx-swift-examples/", branch: "main")
dependencies: [.product(name: "MLXLLM", package: "mlx-swift-examples")]

// NEW (required for LLM/VLM support)
.package(url: "https://github.com/ml-explore/mlx-swift-lm/", branch: "main")
dependencies: [.product(name: "MLXLLM", package: "mlx-swift-lm")]
```

**iOS/macOS Impact**:
- All applications (MLXChatExample, etc.) now depend on the external mlx-swift-lm package
- Existing projects using MLXLLM/MLXVLM/MLXEmbedders must update their Package.swift
- Previous tags and URLs continue to work for backward compatibility

---

### 2. New Features

#### 2.1 Embedder Tool (#408)
**Commit**: `f156eda` (October 29, 2025)

A new command-line tool for document indexing and semantic search capabilities.

**Features**:
- Document corpus indexing with embedding models
- Semantic search using cosine similarity
- Support for BERT and Nomic-BERT embedding models
- REPL interface for interactive search
- Memory-efficient batch processing

**Files Added**: 21 new files (~2,000 lines)

**iOS Relevance**: Low - This is a macOS command-line tool, but the underlying embedding functionality could be useful for iOS search features.

#### 2.2 LoRA Training Example (#436)
**Commit**: `db9b51a` (November 4, 2025)

Added documentation and example for LoRA (Low-Rank Adaptation) fine-tuning.

**iOS Relevance**: Medium - LoRA training is computationally intensive, primarily targeting macOS, but the resulting fine-tuned models can be used on iOS.

#### 2.3 Additional Context API (#433)
**Commit**: `fa76d9a` (November 4, 2025)

Enhanced the streamlined API to support additional context in chat sessions.

**Changes**:
- Added `additionalContext` parameter to `ChatSession.respond()`
- Allows providing extra context without modifying message history
- Useful for RAG (Retrieval-Augmented Generation) patterns

**iOS Impact**: Positive - Makes it easier to implement context-aware chat on iOS

**Example**:
```swift
let response = try await session.respond(
    to: "What is this about?",
    additionalContext: "Document: [retrieved content here]"
)
```

#### 2.4 Prefill Step Size Configuration (#439)
**Commit**: `5651f0b` (November 4, 2025)

Added `prefillStepSize` to `GenerateParameters` initialization.

**iOS Impact**: Positive - Better control over generation performance, useful for optimizing iOS memory usage

---

### 3. Bug Fixes

#### 3.1 Qwen3VL Sanitize Fix (#432)
**Commit**: `b071763` (November 4, 2025)

Fixed input sanitization issues in the Qwen3VL vision-language model.

**iOS Impact**: Positive - Improves reliability of vision models on iOS devices

#### 3.2 LoRA Layer Key Support (#424)
**Commit**: `881ad5a` (October 29, 2025)

Improved LoRA adapter configuration support across 47 model files.

**Changes**:
- Better support for loading LoRA adapters from config files
- Simplified LoRA API usage
- Reduced code duplication (~100 lines removed)

**iOS Impact**: Positive - Makes it easier to use fine-tuned models on iOS

---

### 4. Documentation Updates

#### 4.1 README Reorganization (#448, #447)
**Commits**: `d8af319`, `c78998c` (November 11, 2025)

Major README restructuring to reflect the new repository organization.

**Changes**:
- Clarified the repository split
- Updated installation instructions
- Fixed broken links to mlx-swift-lm documentation
- Added migration guide

---

## iOS-Specific Impact Assessment

### Positive Changes for iOS

1. **Cleaner Dependency Management**: Separation of libraries makes it easier to manage dependencies
2. **Better Performance Controls**: New parameters for prefill step size and generation control
3. **Enhanced Chat API**: Additional context support improves RAG implementations
4. **Bug Fixes**: Vision model improvements and LoRA fixes benefit iOS apps

### Breaking Changes for iOS

1. **Package Dependencies**: Must update Package.swift to reference mlx-swift-lm
2. **Import Statements**: Import paths remain the same, but package source changes
3. **Version Tags**: Need to coordinate versions between mlx-swift-examples and mlx-swift-lm

### iOS Application Compatibility

**MLXChatExample** (iOS 17.0+ / macOS 14.0+):
- Still works with updated dependencies
- Benefits from API improvements
- Requires updating to mlx-swift-lm package

**Other iOS Apps**:
- No new iOS-specific features added
- No iOS version requirement changes
- All changes are backward compatible at the API level

---

## Risk Assessment

### High Risk Areas

1. **Dependency Breaking Changes**
   - **Risk**: Existing projects will fail to build without Package.swift updates
   - **Mitigation**: Clear migration documentation, backward compatibility for old tags
   - **Affected**: All projects using MLXLLM, MLXVLM, or MLXEmbedders

2. **Repository Split Confusion**
   - **Risk**: Developers may not realize code moved to new repository
   - **Mitigation**: Prominent README notices, redirect documentation
   - **Affected**: All library consumers

### Medium Risk Areas

1. **API Changes**
   - **Risk**: New parameters might break existing code
   - **Mitigation**: All new parameters are optional with defaults
   - **Affected**: Code using `ChatSession.respond()` or `GenerateParameters`

2. **Version Synchronization**
   - **Risk**: Mismatch between mlx-swift-examples and mlx-swift-lm versions
   - **Mitigation**: Coordinated releases, version pinning
   - **Affected**: Projects using both repositories

### Low Risk Areas

1. **Bug Fixes**: All fixes are backward compatible
2. **Documentation**: Only improves clarity
3. **New Tools**: Optional, don't affect existing code
4. **iOS Compatibility**: No changes to minimum iOS version

---

## Upgrade Checklist for iOS Projects

### Required Actions

- [ ] Update `Package.swift` to reference `mlx-swift-lm` instead of `mlx-swift-examples` for LLM/VLM dependencies
- [ ] Update package dependencies in Xcode
- [ ] Verify all imports still work (`import MLXLLM`, `import MLXVLM`, etc.)
- [ ] Test build on both iOS and macOS targets
- [ ] Review and update any pinned version constraints

### Optional Improvements

- [ ] Adopt `additionalContext` parameter for RAG implementations
- [ ] Configure `prefillStepSize` for better memory management on iOS
- [ ] Review LoRA adapter loading if using fine-tuned models
- [ ] Update documentation references to point to mlx-swift-lm

### Recommended Testing

- [ ] Test LLM text generation on iOS devices
- [ ] Test VLM image processing on iOS devices
- [ ] Verify LoRA adapter loading (if applicable)
- [ ] Test memory usage with new prefill settings
- [ ] Validate chat session context handling

---

## Overall Risk Rating: MEDIUM-HIGH

**Justification**:
- **Breaking Changes**: The repository split is a major architectural change requiring all consumers to update dependencies
- **Well-Mitigated**: Changes are well-documented, backward compatibility is maintained for old tags
- **Minimal API Impact**: Actual code changes are minimal and mostly additive
- **Clear Migration Path**: Documentation provides clear guidance

**Recommendation**:
- Safe to upgrade for new projects
- Plan migration for existing projects (1-2 hours for typical project)
- Upgrade encouraged to benefit from bug fixes and improvements

---

## Dependencies Version Changes

| Package | Version | Notes |
|---------|---------|-------|
| mlx-swift | 0.29.1 | No change |
| swift-transformers | 1.1.1 | No change |
| swift-jinja | 2.1.0 | No change |
| swift-collections | 1.2.0 | No change |
| GzipSwift | 6.0.1 | No change |

**Note**: mlx-swift-lm package will need to be added as a new dependency.

---

## Conclusion

This upgrade represents a strategic reorganization rather than feature development. The split into mlx-swift-lm creates a cleaner separation of concerns and will likely improve long-term maintainability. For iOS developers, the migration effort is moderate but worthwhile given the bug fixes and API improvements included.

The changes are well-executed with clear documentation and backward compatibility considerations. Teams should plan for the migration but can proceed with confidence.
