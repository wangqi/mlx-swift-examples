# MLX-Swift-Examples Update: tag-20260127 → tag-20260203

**Update Date:** February 3, 2026
**Previous Version:** tag-20260127
**Current Version:** tag-20260203
**Total Commits:** 2

---

## 🎯 Executive Summary

This update is a **major cleanup and simplification** of the mlx-swift-examples repository. The primary change is adding a minimal LLMBasic chat example while removing redundant/unmaintained code. The update also brings the examples in line with mlx-swift 0.30.3 and mlx-swift-lm 2.30.3.

### Risk Assessment: **LOW** ✅

- **New LLMBasic App** (Low): Simple, minimal chat example added
- **Code Removal** (Very Low): Redundant VLMEval and ExampleLLM removed
- **llm-tool Refactor** (Low): Now uses ChatSession API (better example)
- **Test Migration** (Very Low): Tests moved to mlx-swift-lm (where they belong)
- **Dependencies Update** (Low): mlx-swift 0.30.3, mlx-swift-lm 2.30.3

---

## 🎯 Key Changes

### 1. **New: LLMBasic Application** ✨

**Impact:** LOW - New minimal chat example added

#### What is LLMBasic?
A **212-line minimal chat application** demonstrating the absolute basics of:
- Loading an LLM model (with automatic HuggingFace downloads)
- Setting up a ChatSession
- Simple SwiftUI chat UI

#### Why This Matters:
- **LLMEval** is feature-rich but complex (good for showcasing capabilities)
- **LLMBasic** is minimal and simple (good for learning/starting point)
- **Your app** might benefit from studying this minimal example

#### Files Added:
```
Applications/LLMBasic/
├── ChatModel.swift         (108 lines - model loading & ChatSession)
├── ContentView.swift       (82 lines - chat UI)
├── LLMBasicApp.swift       (22 lines - app entry point)
├── README.md               (24 lines - documentation)
└── Assets.xcassets/        (app icons)
```

#### Key Features:
- ✅ Downloads models from HuggingFace automatically
- ✅ Uses `ChatSession` API for multi-turn conversations
- ✅ Memory cache limiting: `Memory.cacheLimit = 20 * 1024 * 1024`
- ✅ iOS Increased Memory Limit entitlement for large models
- ✅ Outgoing connections entitlement for HuggingFace downloads

#### iOS-Specific:
```swift
// Memory management for iOS
Memory.cacheLimit = 20 * 1024 * 1024  // 20MB cache limit

// Entitlements:
- Increased Memory Limit (for larger models on capable devices)
- Outgoing Connections (Client) for HuggingFace downloads
```

#### Risk Level: **LOW**
- **Pro:** New example, doesn't affect existing code
- **Con:** None - purely additive
- **Mitigation:** Not applicable

---

### 2. **Removed: VLMEval Application** 🗑️

**Impact:** VERY LOW - Redundant code removed

#### Why Removed?
- Redundant with other VLM examples
- Not actively maintained
- Functionality covered by `MLXChatExample` and `llm-tool chat`

#### Files Deleted:
```
Applications/VLMEval/
├── ContentView.swift       (486 lines)
├── VLMEvalApp.swift        (13 lines)
├── README.md               (43 lines)
└── Assets.xcassets/        (app icons)
```

#### Impact on Your App:
❌ **None** - You don't use VLMEval

#### Risk Level: **NONE**
- Removal only, no impact on other code

---

### 3. **Removed: ExampleLLM Tool** 🗑️

**Impact:** VERY LOW - Basic tool removed

#### Why Removed?
- Functionality covered by `llm-tool` (which is more comprehensive)
- Simple example, not needed anymore

#### Files Deleted:
```
Tools/ExampleLLM/
├── main.swift             (49 lines)
└── README.md              (40 lines)
```

#### Risk Level: **NONE**
- Removal only, no dependencies

---

### 4. **llm-tool: Refactored to Use ChatSession** 🔄

**Impact:** LOW - Better example code

#### What Changed?
The `llm-tool chat` command now uses the `ChatSession` API instead of manual state management.

#### Files Changed:
- `Tools/llm-tool/Chat.swift` - Refactored to use ChatSession (256 lines modified)
- `Tools/llm-tool/LLMTool.swift` - Updated integration (71 lines modified)
- `Tools/llm-tool/README.md` - New documentation (26 lines added)

#### New Capabilities:
```bash
# Multi-turn chat with image support
./mlx-run llm-tool chat --model mlx-community/gemma-3-12b-it-qat-4bit

> /image support/test.jpg
> what type of creature is in the image?
The creature in the image is a **dog**, specifically a **Poodle**.

> where is the dog sitting?
The dog is sitting on someone's **lap**.
```

#### Chat Commands Available:
- `/help` - Show available commands
- `/image <path>` - Add image to context
- `/reset` - Reset conversation
- And more...

#### Why This Matters:
- **Better example** of how to use ChatSession API
- **Your app uses similar patterns** - this validates the approach
- Shows best practices for multi-turn conversations

#### Risk Level: **LOW**
- **Pro:** Better example code, more maintainable
- **Con:** Changes tool behavior slightly
- **Mitigation:** Tool only, doesn't affect library code

---

### 5. **Test Migration** 📦

**Impact:** VERY LOW - Code reorganization

#### What Moved?
All MLX-LM tests moved from mlx-swift-examples to mlx-swift-lm repository:

#### Tests Deleted (moved to mlx-swift-lm):
```
Tests/MLXLMTests/
├── BaseConfigurationTests.swift    (73 lines)
├── EvalTests.swift                 (299 lines)
├── StreamlinedTests.swift          (64 lines)
├── ToolTests.swift                 (88 lines)
├── UserInputTests.swift            (143 lines)
└── README.md
Tests/mlx-libraries-Package.xctestplan (24 lines)
```

#### Why This Matters:
- Tests belong with the library code (mlx-swift-lm)
- Reduces duplication
- Better CI/CD organization

#### Impact on Your App:
❌ **None** - Tests are for library development

#### Risk Level: **NONE**
- Organization change only

---

### 6. **Dependency Updates** 📦

**Impact:** LOW - Framework updates

#### Updated Dependencies:

| Dependency | Old Version | New Version |
|------------|-------------|-------------|
| **mlx-swift** | 0.30.2 | **0.30.3** |
| **mlx-swift-lm** | (via git) | **2.30.3** |
| **MLXRandom** | Not included | **Added** |

#### Package.swift Changes:
```swift
// Before
.package(url: "mlx-swift", .upToNextMinor(from: "0.30.2"))

// After
.package(url: "mlx-swift", .upToNextMinor(from: "0.30.3"))
.product(name: "MLXRandom", package: "mlx-swift"),  // NEW
```

#### Why MLXRandom Added?
- Needed for proper random number generation in examples
- Already part of mlx-swift, just not imported before

#### Risk Level: **LOW**
- **Pro:** Bug fixes and improvements from 0.30.3
- **Con:** Dependency update always carries minimal risk
- **Mitigation:** Incremental update, well-tested

---

### 7. **Xcode Project Cleanup** 🧹

**Impact:** VERY LOW - Project organization

#### Changes:
- Removed VLMEval.xcscheme → Added LLMBasic.xcscheme
- Removed ExampleLLM.xcscheme → Renamed to image-tool.xcscheme
- Added MLXChatExample.xcscheme
- Cleaned up project.pbxproj (986 lines modified for organization)

#### Build Schemes After Update:
```
✅ LLMBasic          (NEW - minimal chat)
✅ LLMEval           (existing - full-featured)
✅ MLXChatExample    (scheme now visible)
✅ llm-tool          (updated)
✅ image-tool        (renamed from ExampleLLM)
❌ VLMEval           (removed)
❌ ExampleLLM        (removed)
```

#### Risk Level: **VERY LOW**
- Project organization only
- No code changes

---

## 📊 File Statistics

### Lines Added/Removed:
```
Total changes: 6,368 insertions(+), 2,622 deletions(-)
Net change: +3,746 lines (mostly commit.log and whatsnew.md)
Code net change: ~-1,196 lines (cleanup!)
```

### New Files Added:
- ✅ LLMBasic app (5 files, 212 lines)
- ✅ Tools/llm-tool/README.md (259 lines)
- ✅ MLXChatExample.xcscheme

### Files Removed:
- ❌ VLMEval app (4 files, 542 lines)
- ❌ ExampleLLM tool (2 files, 89 lines)
- ❌ MLXLMTests (6 files, 667 lines)

---

## 🎯 Impact on Your App

### Direct Impact: **NONE** ❌

This update **does not affect your app** because:
1. ✅ No changes to library APIs
2. ✅ Only example applications and tools modified
3. ✅ Tests moved, not deleted
4. ✅ Dependencies updated to same versions you're using

### Learning Opportunities: ✅

You **may want to review**:
1. **LLMBasic app** - Minimal example of ChatSession usage
2. **llm-tool chat** - Multi-turn chat implementation
3. **Memory management** - Cache limiting patterns

---

## 🚨 Breaking Changes & Deprecations

### Breaking Changes: **NONE** ✅

### Deprecations: **NONE** ✅

### Removed Code:
- ❌ VLMEval app (redundant)
- ❌ ExampleLLM tool (redundant)
- ❌ MLXLMTests (moved to mlx-swift-lm)

**None of these affect your app.**

---

## 📈 Overall Risk Assessment

### Risk Breakdown:

| Component | Risk Level | Impact on Your App |
|-----------|-----------|-------------------|
| New LLMBasic | **LOW** | None (new app) |
| VLMEval Removal | **NONE** | None (not used) |
| ExampleLLM Removal | **NONE** | None (not used) |
| llm-tool Refactor | **LOW** | None (tool only) |
| Test Migration | **NONE** | None (tests) |
| Dependency Update | **LOW** | None (same versions) |
| Xcode Cleanup | **VERY LOW** | None (organization) |

### Overall: **LOW** ✅

**Why Low Risk:**
1. ✅ No library API changes
2. ✅ Only examples/tools modified
3. ✅ Removals are redundant code
4. ✅ Additions are new examples
5. ✅ Dependencies already in use

---

## ✅ Testing Checklist

Since this update **doesn't affect your app**, minimal testing needed:

### Optional Testing:

- [ ] **Build Verification**
  - [ ] Verify mlx-swift-examples builds (if you build it)
  - [ ] Check LLMBasic app runs (optional learning)

### Recommended Actions:

- [ ] **Review LLMBasic** - Study minimal ChatSession example
- [ ] **Review llm-tool** - See multi-turn chat implementation
- [ ] **Update references** - If you reference VLMEval/ExampleLLM anywhere, remove them

---

## 📝 Summary

This is a **low-risk cleanup update** that:
- ✨ Adds a minimal chat example (LLMBasic)
- 🗑️ Removes redundant code (VLMEval, ExampleLLM)
- 🔄 Improves llm-tool to use ChatSession
- 📦 Reorganizes tests to proper location
- 🧹 Cleans up Xcode project

**Impact on Your App:** Essentially zero. This is all examples and tools.

**Recommendation:** Safe to update. Consider reviewing LLMBasic for learning purposes.

---

## 🎓 Learning Opportunities

### Study LLMBasic for:
```swift
// Minimal model loading
let modelContainer = try await LLMModelFactory.shared.loadContainer(
    configuration: ModelConfiguration(id: modelID)
)

// ChatSession setup
let session = ChatSession(
    modelContainer: modelContainer,
    systemPrompt: systemPrompt
)

// Simple generation
for try await text in session.generate(prompt: userInput) {
    // Process streaming text
}
```

### Study llm-tool for:
- Multi-turn chat with ChatSession
- Image input handling (`/image` command)
- Conversation history management
- Interactive REPL implementation

---

## 📚 References

- [LLMBasic README](Applications/LLMBasic/README.md)
- [llm-tool README](Tools/llm-tool/README.md)
- [ChatSession API](https://swiftpackageindex.com/ml-explore/mlx-swift-lm/main/documentation/mlxlmcommon/chatsession)
- [MLX Troubleshooting](https://swiftpackageindex.com/ml-explore/mlx-swift/main/documentation/mlx/troubleshooting)

---

## 🔗 Related Updates

This update aligns with:
- **mlx-swift-lm update** (tag-20260111 → tag-20260203) - See separate whatsnew.md
- **mlx-swift 0.30.3** - Thread safety improvements
- **mlx-swift-lm 2.30.3** - Tool call parsing, thread safety

---

**Generated:** 2026-02-03
**Impact on Your App:** None (examples only) ✅
**Action Required:** None (optional review for learning) 📚
