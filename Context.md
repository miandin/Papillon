# Papillon AI Extension - Features, Epics & User Stories

**Version:** 1.1.0  
**Generated:** December 24, 2024  
**Source:** papillon-ai-v1.1.0.vsix

---

## Executive Summary

This document captures all implemented features in the Papillon AI VS Code extension, organized as Agile Epics with nested User Stories. Features were automatically identified from source code analysis including:
- Extension manifest (`package.json`)
- Service implementations (`/src/services/`)
- API handlers (`/src/api/`)
- UI components (`/webview-ui/`)
- Configuration settings

---

## Epic 1: AI-Powered Code Assistance

**Description:** Core AI capabilities for code generation, explanation, and improvement using multiple LLM providers.



### User Stories

#### 1.1 Multi-Provider AI Integration
**As a** developer  
**I want** to connect to multiple AI providers (Anthropic, OpenAI, OpenRouter, etc.)  
**So that** I can choose the best model for my coding tasks

**Acceptance Criteria:**
- ✅ Support for 40+ AI providers including:
  - Anthropic (Claude), OpenAI (GPT), OpenRouter
  - AWS Bedrock, Google Vertex AI, Gemini
  - Local models: Ollama, LM Studio
  - Specialized: DeepSeek, Mistral, Groq, XAI, HuggingFace
  - Enterprise: Cerebras, SambaNova, Fireworks
- ✅ Provider-specific configuration (API keys, base URLs, models)
- ✅ Token counting per provider
- ✅ Streaming responses support



---

#### 1.2 Code Explanation
**As a** developer  
**I want** to select code and get AI-powered explanations  
**So that** I can understand complex code quickly

**Acceptance Criteria:**
- ✅ Right-click context menu: "Papillon AI: Explain Selection"
- ✅ Keyboard shortcut support
- ✅ Explanation appears in chat interface
- ✅ Supports multiple programming languages


---

#### 1.3 Code Improvement Suggestions
**As a** developer  
**I want** AI to suggest improvements to my code  
**So that** I can write better, more efficient code

**Acceptance Criteria:**
- ✅ Context menu: "Papillon AI: Improve Code"
- ✅ Analyzes selected code for improvements
- ✅ Provides refactoring suggestions
- ✅ Maintains code functionality



---

#### 1.4 Code Fixing
**As a** developer  
**I want** AI to identify and fix bugs in my code  
**So that** I can resolve issues faster

**Acceptance Criteria:**
- ✅ Command: "Papillon AI: Fix Code"
- ✅ Analyzes code for errors
- ✅ Suggests fixes with explanations
- ✅ Preserves code structure



---

## Epic 2: Interactive Chat Interface

**Description:** Conversational AI interface for natural language coding assistance.



### User Stories

#### 2.1 Chat-Based Task Management
**As a** developer  
**I want** to interact with AI through a chat interface  
**So that** I can describe tasks in natural language

**Acceptance Criteria:**
- ✅ Sidebar chat panel with message history
- ✅ Text input with markdown support
- ✅ Image attachment support (up to 20 images per message)
- ✅ Message streaming with real-time updates
- ✅ Task history preservation



---

#### 2.2 New Task Creation
**As a** developer  
**I want** to start new AI-assisted tasks  
**So that** I can work on multiple coding problems

**Acceptance Criteria:**
- ✅ "+" button in sidebar for new tasks
- ✅ Command: "Papillon AI: New Task"
- ✅ Task isolation (separate conversation contexts)
- ✅ Task naming and organization



---

#### 2.3 Context Management
**As a** developer  
**I want** to add files and code to AI context  
**So that** AI has relevant information for tasks

**Acceptance Criteria:**
- ✅ Command: "Papillon AI: Add to Context"
- ✅ Keyboard shortcut: Cmd/Ctrl+Y
- ✅ Supports file selection, code selection
- ✅ Context size management



---

#### 2.4 Chat History & Search
**As a** developer  
**I want** to search and review past conversations  
**So that** I can reference previous solutions

**Acceptance Criteria:**
- ✅ History button in toolbar
- ✅ Task search functionality
- ✅ Task preview with metadata
- ✅ Task restoration

`

---

## Epic 3: Model Context Protocol (MCP) Integration

**Description:** Integration with MCP servers for extended tool capabilities.

**Reference:** `src/services/mcp/`, `src/package.json` (lines 84-87)

### User Stories

#### 3.1 MCP Server Management
**As a** developer  
**I want** to connect to MCP servers  
**So that** I can extend AI capabilities with custom tools

**Acceptance Criteria:**
- ✅ MCP server configuration interface
- ✅ Server connection management (start/stop/restart)
- ✅ Server status monitoring
- ✅ Singleton pattern (one instance across webviews)



---

#### 3.2 MCP Tool Discovery
**As a** developer  
**I want** AI to discover and use MCP tools  
**So that** I can leverage external services in my workflow

**Acceptance Criteria:**
- ✅ Automatic tool discovery from connected servers
- ✅ Tool schema validation
- ✅ Tool invocation through AI
- ✅ Tool result handling


---

#### 3.3 MCP Resource Access
**As a** developer  
**I want** AI to access MCP resources  
**So that** it can retrieve external data

**Acceptance Criteria:**
- ✅ Resource listing from MCP servers
- ✅ Resource content retrieval
- ✅ Resource caching
- ✅ Error handling for unavailable resources



---

## Epic 4: Code Intelligence & Indexing

**Description:** Semantic code search and understanding using vector embeddings.



### User Stories

#### 4.1 Codebase Indexing
**As a** developer  
**I want** my codebase to be semantically indexed  
**So that** AI can find relevant code quickly

**Acceptance Criteria:**
- ✅ Automatic workspace indexing
- ✅ Incremental updates on file changes
- ✅ Vector embedding generation
- ✅ Qdrant vector store integration
- ✅ Configurable batch size (1-200 embeddings)



---

#### 4.2 Semantic Code Search
**As a** developer  
**I want** to search code by meaning, not just keywords  
**So that** I can find relevant code examples

**Acceptance Criteria:**
- ✅ Natural language code queries
- ✅ Similarity-based search results
- ✅ Result ranking by relevance
- ✅ Search result caching



---

#### 4.3 Index State Management
**As a** developer  
**I want** to monitor indexing progress  
**So that** I know when my codebase is ready

**Acceptance Criteria:**
- ✅ Indexing states: Standby, Indexing, Ready, Error
- ✅ Progress updates with file counts
- ✅ Error recovery mechanisms
- ✅ Manual re-indexing option



---

## Epic 5: Terminal Integration

**Description:** AI assistance for terminal commands and shell interactions.



### User Stories

#### 5.1 Terminal Command Assistance
**As a** developer  
**I want** AI to help with terminal commands  
**So that** I can execute complex shell operations

**Acceptance Criteria:**
- ✅ Terminal context menu integration
- ✅ Command: "Terminal: Add to Context"
- ✅ Command: "Terminal: Fix Command"
- ✅ Command: "Terminal: Explain Command"
- ✅ Terminal output capture



---

#### 5.2 Command Execution Control
**As a** developer  
**I want** to control which commands AI can execute  
**So that** I maintain security and safety

**Acceptance Criteria:**
- ✅ Allowed commands whitelist (default: git log, git diff, git show)
- ✅ Denied commands blacklist
- ✅ Command timeout configuration (0-600 seconds)
- ✅ Timeout allowlist for specific commands
- ✅ Command approval workflow

**Files:** `src/package.json` (lines 340-374), `src/services/command/`

---

## Epic 6: Browser Automation

**Description:** Puppeteer-based browser control for web testing and automation.



### User Stories

#### 6.1 Browser Session Management
**As a** developer  
**I want** AI to control a browser  
**So that** I can test web applications

**Acceptance Criteria:**
- ✅ Launch browser sessions
- ✅ Navigate to URLs
- ✅ Take screenshots
- ✅ Close browser sessions
- ✅ Session state tracking



---

#### 6.2 Browser Interaction
**As a** developer  
**I want** AI to interact with web pages  
**So that** I can automate testing workflows

**Acceptance Criteria:**
- ✅ Click elements
- ✅ Type text into forms
- ✅ Navigate pages
- ✅ Execute JavaScript
- ✅ Capture console logs


---

## Epic 7: File Operations & Editing

**Description:** AI-powered file reading, writing, and modification capabilities.


### User Stories

#### 7.1 File Reading
**As a** developer  
**I want** AI to read project files  
**So that** it understands my codebase

**Acceptance Criteria:**
- ✅ Read single files
- ✅ Read multiple files
- ✅ Support for various file types (text, PDF, DOCX, Excel)
- ✅ Binary file detection
- ✅ Large file handling



---

#### 7.2 File Writing & Editing
**As a** developer  
**I want** AI to create and modify files  
**So that** it can implement code changes

**Acceptance Criteria:**
- ✅ Create new files
- ✅ Modify existing files
- ✅ Apply diffs/patches
- ✅ Preserve file formatting
- ✅ Undo/redo support



---

#### 7.3 Diff Visualization
**As a** developer  
**I want** to see proposed changes before applying  
**So that** I can review AI modifications

**Acceptance Criteria:**
- ✅ Side-by-side diff view
- ✅ Inline diff highlighting
- ✅ Accept/reject changes
- ✅ Diff URI scheme support



---

## Epic 8: Configuration & Settings

**Description:** Comprehensive extension configuration and customization.

**Reference:** `src/package.json` (lines 337-469)

### User Stories

#### 8.1 API Provider Configuration
**As a** developer  
**I want** to configure my AI provider settings  
**So that** I can use my preferred models

**Acceptance Criteria:**
- ✅ Provider selection (40+ options)
- ✅ API key management
- ✅ Model selection per provider
- ✅ Custom base URLs
- ✅ Request timeout configuration (0-3600 seconds)



---

#### 8.2 Governance Layer Settings
**As a** developer  
**I want** to enable governance controls  
**So that** I can ensure compliance and security

**Acceptance Criteria:**
- ✅ Centralized Task Approval toggle
- ✅ Policy Enforcement toggle
- ✅ Audit Trail toggle
- ✅ Resource Usage Controls toggle
- ✅ Output Filtering toggle
- ✅ Compliance Checking toggle
- ✅ Role-Based Access Control toggle



---

#### 8.3 Custom Storage Path
**As a** developer  
**I want** to specify where extension data is stored  
**So that** I can manage storage location

**Acceptance Criteria:**
- ✅ Custom path configuration
- ✅ Path validation
- ✅ Migration support
- ✅ Command: "Set Custom Storage Path"



---

#### 8.4 Settings Import/Export
**As a** developer  
**I want** to import/export settings  
**So that** I can share configurations across machines

**Acceptance Criteria:**
- ✅ Export settings to JSON
- ✅ Import settings from JSON
- ✅ Auto-import on startup (configurable path)
- ✅ Settings validation



---

## Epic 9: Internationalization (i18n)

**Description:** Multi-language support for global accessibility.



### User Stories

#### 9.1 Multi-Language Support
**As a** developer  
**I want** to use the extension in my native language  
**So that** I can work more comfortably

**Acceptance Criteria:**
- ✅ Support for 15+ languages:
  - English, Spanish, French, German, Italian
  - Japanese, Korean, Chinese (Simplified & Traditional)
  - Portuguese (BR), Russian, Polish, Dutch
  - Turkish, Vietnamese, Hindi, Indonesian, Catalan
- ✅ Automatic language detection from VS Code
- ✅ Manual language override
- ✅ Complete UI translation



---

## Epic 10: Marketplace & Extensions

**Description:** Browse and install community-created prompts and extensions.

**Reference:** `src/services/marketplace/`, `src/package.json` (lines 94-97)

### User Stories

#### 10.1 Marketplace Browser
**As a** developer  
**I want** to browse available extensions  
**So that** I can enhance Papillon AI capabilities

**Acceptance Criteria:**
- ✅ Marketplace button in toolbar
- ✅ Extension listing with descriptions
- ✅ Search and filter extensions
- ✅ Extension installation



---

#### 10.2 Custom Prompts
**As a** developer  
**I want** to create and share custom prompts  
**So that** I can reuse common workflows

**Acceptance Criteria:**
- ✅ Prompts button in toolbar
- ✅ Prompt library management
- ✅ Prompt templates
- ✅ Prompt sharing



---

## Epic 11: Cloud Integration

**Description:** Cloud-based features for sync, collaboration, and enhanced capabilities.



### User Stories

#### 11.1 Cloud Authentication
**As a** developer  
**I want** to sign in to Papillon Cloud  
**So that** I can access cloud features

**Acceptance Criteria:**
- ✅ OAuth authentication flow
- ✅ JWT token management
- ✅ Auth state tracking
- ✅ Secure credential storage



---

#### 11.2 Cloud Sync
**As a** developer  
**I want** to sync my settings and history  
**So that** I can work across multiple machines

**Acceptance Criteria:**
- ✅ Settings synchronization
- ✅ Task history sync
- ✅ Conflict resolution
- ✅ Offline support



---

#### 11.3 Cloud Upsell
**As a** user  
**I want** to learn about cloud features  
**So that** I can decide if I want to upgrade

**Acceptance Criteria:**
- ✅ Cloud upsell dialog
- ✅ Feature comparison
- ✅ Dismissible notifications
- ✅ Cloud button in toolbar



---

## Epic 12: Code Actions & Quick Fixes

**Description:** Inline code actions and quick fixes powered by AI.



### User Stories

#### 12.1 Inline Code Actions
**As a** developer  
**I want** AI-powered quick fixes in my editor  
**So that** I can fix issues without leaving my code

**Acceptance Criteria:**
- ✅ Lightbulb actions in editor
- ✅ Context-aware suggestions
- ✅ One-click fixes
- ✅ Enable/disable toggle



---

## Epic 13: Search & Navigation

**Description:** Advanced code search using ripgrep and glob patterns.



### User Stories

#### 13.1 Fast Code Search
**As a** developer  
**I want** to search code quickly  
**So that** I can find relevant files

**Acceptance Criteria:**
- ✅ Ripgrep integration for fast search
- ✅ Regex pattern support
- ✅ Glob pattern filtering
- ✅ Search result ranking



---

#### 13.2 File Pattern Matching
**As a** developer  
**I want** to filter files by patterns  
**So that** I can focus on relevant code

**Acceptance Criteria:**
- ✅ Glob pattern support
- ✅ .gitignore integration
- ✅ Custom ignore patterns (.rooignore)
- ✅ Include/exclude rules



---

## Epic 14: Telemetry & Analytics

**Description:** Usage tracking and error reporting for product improvement.

**Reference:** `packages/telemetry/`, `src/extension.ts` (lines 71-78)

### User Stories

#### 14.1 Usage Analytics
**As a** product team  
**I want** to track feature usage  
**So that** I can improve the product

**Acceptance Criteria:**
- ✅ PostHog integration
- ✅ Event tracking
- ✅ User consent management
- ✅ Privacy-preserving analytics



---

#### 14.2 Error Reporting
**As a** developer  
**I want** errors to be reported automatically  
**So that** issues can be fixed quickly

**Acceptance Criteria:**
- ✅ Automatic error capture
- ✅ Error serialization
- ✅ Stack trace collection
- ✅ User privacy protection



---

## Epic 15: Task Management & Todos

**Description:** Track task progress with todo lists and checkpoints.

**Reference:** `src/core/`, `src/package.json` (lines 375-378, 421-425)

### User Stories

#### 15.1 Todo List Management
**As a** developer  
**I want** to track task progress with todos  
**So that** I can see what's completed

**Acceptance Criteria:**
- ✅ Create todo lists for tasks
- ✅ Mark todos as complete/incomplete
- ✅ Todo progress tracking
- ✅ Prevent completion with open todos (configurable)
- ✅ Require todos for new tasks (configurable)



---

#### 15.2 Task Checkpoints
**As a** developer  
**I want** to save task checkpoints  
**So that** I can restore previous states

**Acceptance Criteria:**
- ✅ Automatic checkpoint creation
- ✅ Manual checkpoint triggers
- ✅ Checkpoint restoration
- ✅ Checkpoint warnings



---

## Epic 16: Auto-Approval & Automation

**Description:** Automated approval of AI actions for streamlined workflows.

**Reference:** `src/package.json` (lines 179-182, 320-325)

### User Stories

#### 16.1 Auto-Approve Mode
**As a** developer  
**I want** to auto-approve AI actions  
**So that** I can work faster on trusted tasks

**Acceptance Criteria:**
- ✅ Toggle auto-approve mode
- ✅ Keyboard shortcut: Cmd/Ctrl+Alt+A
- ✅ Command: "Toggle Auto Approve"
- ✅ Visual indicator when active
- ✅ Granular control per action type



---

## Epic 17: Mobile Device Management (MDM)

**Description:** Enterprise device management and policy enforcement.



### User Stories

#### 17.1 MDM Policy Enforcement
**As an** IT administrator  
**I want** to enforce policies on developer machines  
**So that** I can maintain security standards

**Acceptance Criteria:**
- ✅ MDM service initialization
- ✅ Policy retrieval from MDM server
- ✅ Policy enforcement
- ✅ Compliance reporting



---

## Epic 18: Tree-Sitter Code Parsing

**Description:** Advanced code parsing for syntax-aware operations.



### User Stories

#### 18.1 Syntax-Aware Code Analysis
**As a** developer  
**I want** AI to understand code structure  
**So that** it can make better suggestions

**Acceptance Criteria:**
- ✅ Support for 30+ programming languages
- ✅ AST (Abstract Syntax Tree) generation
- ✅ Syntax node identification
- ✅ Code structure analysis



---

## Epic 19: Audio Feedback

**Description:** Sound notifications for task completion and events.



### User Stories

#### 19.1 Audio Notifications
**As a** developer  
**I want** audio feedback for events  
**So that** I know when tasks complete

**Acceptance Criteria:**
- ✅ Task completion sounds
- ✅ Error notification sounds
- ✅ Configurable audio settings
- ✅ Multiple audio types support



---

## Epic 20: Version Control Integration

**Description:** Git integration for code change tracking.

**Reference:** Dependencies: `simple-git@3.27.0`

### User Stories

#### 20.1 Git Operations
**As a** developer  
**I want** AI to interact with Git  
**So that** it can manage version control

**Acceptance Criteria:**
- ✅ Git status checking
- ✅ Commit creation
- ✅ Branch management
- ✅ Diff generation


---

## Cross-Cutting Concerns

### Security

#### SEC-1: API Key Management
**As a** developer  
**I want** secure API key storage  
**So that** my credentials are protected

**Acceptance Criteria:**
- ✅ VS Code SecretStorage integration
- ✅ Encrypted credential storage
- ✅ No plaintext keys in settings
- ✅ Key rotation support



---

#### SEC-2: Command Validation
**As a** developer  
**I want** dangerous commands blocked  
**So that** AI cannot harm my system

**Acceptance Criteria:**
- ✅ Command whitelist/blacklist
- ✅ Command approval workflow
- ✅ Timeout protection
- ✅ Audit logging



---

### Performance

#### PERF-1: Token Counting
**As a** developer  
**I want** efficient token counting  
**So that** I can manage API costs

**Acceptance Criteria:**
- ✅ Worker-based token counting
- ✅ Tiktoken integration
- ✅ Provider-specific counting
- ✅ Caching for repeated content



---

#### PERF-2: Caching
**As a** developer  
**I want** intelligent caching  
**So that** the extension performs well

**Acceptance Criteria:**
- ✅ LRU cache for search results
- ✅ Vector embedding cache
- ✅ File content cache
- ✅ Configurable cache sizes



---

### Logging & Debugging

#### LOG-1: Output Channel Logging
**As a** developer  
**I want** detailed logs  
**So that** I can debug issues

**Acceptance Criteria:**
- ✅ VS Code Output Channel
- ✅ Structured logging
- ✅ Log levels (info, warn, error)
- ✅ Dual logging (console + channel)



---

#### LOG-2: Audit Trail
**As an** administrator  
**I want** audit logs of AI actions  
**So that** I can track usage

**Acceptance Criteria:**
- ✅ Action logging
- ✅ User identification
- ✅ Timestamp tracking
- ✅ Governance layer integration


---

## Technical Debt & Future Enhancements

### Known Limitations

1. **Governance Layer (v1.1.0)**: UI-only toggles, backend implementation pending
2. **Browser Automation**: Requires Chromium installation
3. **Code Index**: Requires Qdrant server for vector storage
4. **Cloud Features**: Requires cloud subscription

### Proposed Enhancements

1. **Collaborative Editing**: Real-time collaboration on AI tasks
2. **Custom Model Training**: Fine-tune models on project code
3. **Advanced Diff Tools**: 3-way merge support
4. **Plugin System**: Third-party extension support
5. **Mobile Companion**: Mobile app for task monitoring

---


## Appendix A: Dependencies Summary

### Core Dependencies (40.49 MB package)
- **AI SDKs**: @anthropic-ai/sdk, openai, @google/genai, ollama
- **MCP**: @modelcontextprotocol/sdk
- **Vector Store**: @qdrant/js-client-rest
- **Code Parsing**: tree-sitter-wasms, web-tree-sitter
- **Browser**: puppeteer-core, puppeteer-chromium-resolver
- **File Processing**: mammoth, pdf-parse, exceljs, turndown
- **Utilities**: axios, cheerio, diff, yaml, zod

### Development Dependencies
- **Build**: esbuild, tsup, turbo
- **Testing**: vitest, @vscode/test-electron
- **Linting**: eslint, prettier
- **Packaging**: @vscode/vsce

---

## Document Metadata

- **Total Epics**: 20
- **Total User Stories**: 60+
- **Cross-Cutting Concerns**: 4
- **Source Files Analyzed**: 100+
- **Lines of Code**: ~50,000+
- **Supported Languages**: 15+
- **AI Providers**: 40+

---

**End of Document**
