# What's New in mlx-swift-examples (1.0.0 to HEAD)

## Upgrade Date
November 12, 2025

## Executive Summary

This upgrade represents a **major architectural reorganization** of the mlx-swift-examples repository. The primary change is the extraction of core LLM/VLM libraries into a separate repository ([mlx-swift-lm](https://github.com/ml-explore/mlx-swift-lm)), transforming this repository into a focused collection of examples and applications.

**Impact Level**: 🔴 **HIGH** - Major breaking changes for library consumers

---

## Major Changes

### 1. 🔄 Repository Restructuring (#441)

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

### 2. ✨ New Features

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

**iOS Impact**: ✅ **Positive** - Makes it easier to implement context-aware chat on iOS

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

**iOS Impact**: ✅ **Positive** - Better control over generation performance, useful for optimizing iOS memory usage

---

### 3. 🐛 Bug Fixes

#### 3.1 Qwen3VL Sanitize Fix (#432)
**Commit**: `b071763` (November 4, 2025)

Fixed input sanitization issues in the Qwen3VL vision-language model.

**iOS Impact**: ✅ **Positive** - Improves reliability of vision models on iOS devices

#### 3.2 LoRA Layer Key Support (#424)
**Commit**: `881ad5a` (October 29, 2025)

Improved LoRA adapter configuration support across 47 model files.

**Changes**:
- Better support for loading LoRA adapters from config files
- Simplified LoRA API usage
- Reduced code duplication (~100 lines removed)

**iOS Impact**: ✅ **Positive** - Makes it easier to use fine-tuned models on iOS

---

### 4. 📚 Documentation Updates

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

### ✅ Positive Changes for iOS

1. **Cleaner Dependency Management**: Separation of libraries makes it easier to manage dependencies
2. **Better Performance Controls**: New parameters for prefill step size and generation control
3. **Enhanced Chat API**: Additional context support improves RAG implementations
4. **Bug Fixes**: Vision model improvements and LoRA fixes benefit iOS apps

### ⚠️ Breaking Changes for iOS

1. **Package Dependencies**: Must update Package.swift to reference mlx-swift-lm
2. **Import Statements**: Import paths remain the same, but package source changes
3. **Version Tags**: Need to coordinate versions between mlx-swift-examples and mlx-swift-lm

### 📱 iOS Application Compatibility

**MLXChatExample** (iOS 17.0+ / macOS 14.0+):
- ✅ Still works with updated dependencies
- ✅ Benefits from API improvements
- ⚠️ Requires updating to mlx-swift-lm package

**Other iOS Apps**:
- No new iOS-specific features added
- No iOS version requirement changes
- All changes are backward compatible at the API level

---

## Risk Assessment

### 🔴 High Risk Areas

1. **Dependency Breaking Changes**
   - **Risk**: Existing projects will fail to build without Package.swift updates
   - **Mitigation**: Clear migration documentation, backward compatibility for old tags
   - **Affected**: All projects using MLXLLM, MLXVLM, or MLXEmbedders

2. **Repository Split Confusion**
   - **Risk**: Developers may not realize code moved to new repository
   - **Mitigation**: Prominent README notices, redirect documentation
   - **Affected**: All library consumers

### 🟡 Medium Risk Areas

1. **API Changes**
   - **Risk**: New parameters might break existing code
   - **Mitigation**: All new parameters are optional with defaults
   - **Affected**: Code using `ChatSession.respond()` or `GenerateParameters`

2. **Version Synchronization**
   - **Risk**: Mismatch between mlx-swift-examples and mlx-swift-lm versions
   - **Mitigation**: Coordinated releases, version pinning
   - **Affected**: Projects using both repositories

### 🟢 Low Risk Areas

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

## Overall Risk Rating

### Overall: 🟡 **MEDIUM-HIGH**

**Justification**:
- **Breaking Changes**: The repository split is a major architectural change requiring all consumers to update dependencies
- **Well-Mitigated**: Changes are well-documented, backward compatibility is maintained for old tags
- **Minimal API Impact**: Actual code changes are minimal and mostly additive
- **Clear Migration Path**: Documentation provides clear guidance

**Recommendation**:
- ✅ **Safe to upgrade** for new projects
- ⚠️ **Plan migration** for existing projects (1-2 hours for typical project)
- ✅ **Upgrade encouraged** to benefit from bug fixes and improvements

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
