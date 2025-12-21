# Logical Layer Plan for Papillon AI

## Executive Summary

This document outlines a comprehensive plan to add a **logical layer** on top of Papillon AI that will control how tasks are executed and provide controlled output. The governance layer will be implemented as a **custom extension** that wraps and orchestrates the existing Papillon AI functionality.

---

## 1. Current Architecture Analysis

### Existing Papillon AI Structure
```
papillon-ai-fork/
├── src/extension.ts              # Main extension entry point
├── core/
│   ├── webview/ClineProvider.ts  # Chat UI provider
│   └── config/ContextProxy.ts    # Configuration management
├── services/
│   ├── mcp/McpServerManager.ts   # MCP server integration
│   ├── code-index/manager.ts     # Code indexing
│   └── mdm/MdmService.ts         # MDM service
├── packages/types/src/mode.ts    # Mode definitions (Wingspan, Flutter, etc.)
└── webview-ui/                   # React-based UI
```

### Current Capabilities
- ✅ Multi-mode AI assistance (Code, Ask, Debug, Architect, Orchestrator)
- ✅ MCP server integration for extensibility
- ✅ Code indexing and context management
- ✅ Terminal integration and command execution
- ✅ File editing and diff viewing
- ✅ Browser automation capabilities

### Current Limitations (Governance Gaps)
- ❌ No centralized task approval workflow
- ❌ No policy enforcement layer
- ❌ No audit trail for AI actions
- ❌ No resource usage controls
- ❌ No output filtering/sanitization
- ❌ No compliance checking
- ❌ No role-based access control

---

## 2. Governance Layer Architecture

### 2.1 Design Approach: **Wrapper Extension Pattern**

**Recommendation**: Implement governance as a **separate VS Code extension** that wraps Papillon AI.

**Rationale**:
- ✅ Non-invasive: Doesn't modify core Papillon AI code
- ✅ Maintainable: Easy to update independently
- ✅ Testable: Can be tested in isolation
- ✅ Flexible: Can be enabled/disabled per workspace
- ✅ Upgradeable: Papillon AI can be updated without breaking governance

### 2.2 Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                    VS Code Extension Host                    │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌───────────────────────────────────────────────────────┐  │
│  │         Papillon Governance Extension                  │  │
│  │  (papillon-governance)                                 │  │
│  ├───────────────────────────────────────────────────────┤  │
│  │                                                         │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │  Policy Engine                                   │  │  │
│  │  │  - Task approval rules                           │  │  │
│  │  │  - Resource limits                               │  │  │
│  │  │  - Compliance checks                             │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  │                                                         │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │  Audit Service                                   │  │  │
│  │  │  - Action logging                                │  │  │
│  │  │  - Output recording                              │  │  │
│  │  │  - Compliance reporting                          │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  │                                                         │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │  Interceptor Layer                               │  │  │
│  │  │  - Command interception                          │  │  │
│  │  │  - Output filtering                              │  │  │
│  │  │  - Context sanitization                          │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  │                                                         │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │  Approval Workflow UI                            │  │  │
│  │  │  - Task review panel                             │  │  │
│  │  │  - Approval queue                                │  │  │
│  │  │  - Policy configuration                          │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  │                                                         │  │
│  └───────────────────────────────────────────────────────┘  │
│                            ↕                                 │
│                   Command Interception                       │
│                            ↕                                 │
│  ┌───────────────────────────────────────────────────────┐  │
│  │         Papillon AI Extension                          │  │
│  │  (papillon-ai-fork)                                    │  │
│  │  - Modes (Code, Ask, Debug, etc.)                     │  │
│  │  - MCP Integration                                     │  │
│  │  - Code Indexing                                       │  │
│  │  - Terminal Integration                                │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Core Components

### 3.1 Policy Engine

**Purpose**: Define and enforce governance rules

**Key Features**:
```typescript
interface PolicyEngine {
  // Task approval policies
  requireApprovalFor(taskType: TaskType): boolean
  canExecuteCommand(command: string): boolean
  canAccessFile(filePath: string): boolean
  canMakeNetworkRequest(url: string): boolean
  
  // Resource policies
  getMaxTokensPerRequest(): number
  getMaxConcurrentTasks(): number
  getMaxFileSize(): number
  
  // Compliance policies
  requiresDataRedaction(): boolean
  getAllowedDataCategories(): string[]
  getRetentionPolicy(): RetentionPolicy
}
```

**Configuration Example**:
```json
{
  "papillon.governance.policies": {
    "taskApproval": {
      "requireApprovalForCodeChanges": true,
      "requireApprovalForCommandExecution": true,
      "requireApprovalForNetworkRequests": true,
      "autoApproveReadOnlyOperations": true
    },
    "resourceLimits": {
      "maxTokensPerRequest": 100000,
      "maxConcurrentTasks": 3,
      "maxFileSize": 10485760,
      "maxFilesPerOperation": 50
    },
    "compliance": {
      "redactSecrets": true,
      "redactPII": true,
      "allowedDataCategories": ["code", "documentation"],
      "retentionDays": 90
    }
  }
}
```

### 3.2 Audit Service

**Purpose**: Track all AI actions and outputs

**Key Features**:
```typescript
interface AuditService {
  // Logging
  logTaskStart(task: Task): AuditEntry
  logTaskComplete(task: Task, result: TaskResult): void
  logTaskError(task: Task, error: Error): void
  logApproval(task: Task, approved: boolean, reason?: string): void
  
  // Querying
  getAuditLog(filters: AuditFilters): AuditEntry[]
  exportAuditLog(format: 'json' | 'csv'): string
  
  // Compliance
  generateComplianceReport(period: DateRange): ComplianceReport
}
```

**Audit Entry Schema**:
```typescript
interface AuditEntry {
  id: string
  timestamp: Date
  taskId: string
  taskType: TaskType
  mode: string
  user: string
  workspace: string
  
  // Action details
  action: {
    type: 'read' | 'write' | 'execute' | 'network'
    target: string
    parameters: Record<string, any>
  }
  
  // Approval
  approvalRequired: boolean
  approvalStatus: 'pending' | 'approved' | 'rejected'
  approver?: string
  approvalReason?: string
  
  // Resources
  tokensUsed: number
  filesAccessed: string[]
  commandsExecuted: string[]
  
  // Output
  outputSummary: string
  outputRedacted: boolean
  
  // Compliance
  complianceFlags: string[]
  dataCategories: string[]
}
```

### 3.3 Interceptor Layer

**Purpose**: Intercept and control Papillon AI commands

**Implementation Strategy**:
```typescript
class GovernanceInterceptor {
  // Command interception
  async interceptCommand(
    command: string,
    args: any[]
  ): Promise<InterceptionResult> {
    // 1. Check policy
    const policy = await this.policyEngine.evaluate(command, args)
    
    // 2. Require approval if needed
    if (policy.requiresApproval) {
      const approved = await this.approvalWorkflow.requestApproval({
        command,
        args,
        policy
      })
      
      if (!approved) {
        return { allowed: false, reason: 'User rejected' }
      }
    }
    
    // 3. Sanitize inputs
    const sanitizedArgs = await this.sanitizer.sanitize(args)
    
    // 4. Log action
    await this.auditService.logAction({
      command,
      args: sanitizedArgs,
      approved: true
    })
    
    return { allowed: true, args: sanitizedArgs }
  }
  
  // Output filtering
  async filterOutput(output: any): Promise<any> {
    // 1. Redact sensitive data
    const redacted = await this.redactor.redact(output)
    
    // 2. Apply output policies
    const filtered = await this.outputFilter.filter(redacted)
    
    // 3. Log output
    await this.auditService.logOutput(filtered)
    
    return filtered
  }
}
```

### 3.4 Approval Workflow UI

**Purpose**: Provide user interface for task approval

**Components**:

1. **Approval Queue Panel**
   - Shows pending tasks requiring approval
   - Displays task details, risks, and policy violations
   - Provides approve/reject buttons

2. **Task Review Dialog**
   - Detailed view of task to be executed
   - Shows files to be modified
   - Shows commands to be executed
   - Displays risk assessment

3. **Policy Configuration Panel**
   - Configure governance policies
   - Set approval rules
   - Configure resource limits

4. **Audit Log Viewer**
   - Browse historical actions
   - Filter by date, user, task type
   - Export audit logs

---

## 4. Implementation Plan

### Phase 1: Foundation (Week 1-2)

**Goal**: Create basic governance extension structure

**Tasks**:
1. ✅ Create new extension: `papillon-governance`
2. ✅ Set up extension manifest and activation
3. ✅ Implement basic command interception
4. ✅ Create policy configuration schema
5. ✅ Implement basic audit logging

**Deliverables**:
- Extension skeleton with activation
- Basic command interception working
- Simple audit log to file

### Phase 2: Policy Engine (Week 3-4)

**Goal**: Implement policy evaluation and enforcement

**Tasks**:
1. ✅ Implement PolicyEngine class
2. ✅ Create policy configuration UI
3. ✅ Implement resource limit checks
4. ✅ Add compliance rule evaluation
5. ✅ Create policy testing framework

**Deliverables**:
- Working policy engine
- Configuration UI
- Unit tests for policies

### Phase 3: Approval Workflow (Week 5-6)

**Goal**: Build user approval interface

**Tasks**:
1. ✅ Create approval queue panel
2. ✅ Implement task review dialog
3. ✅ Add approval/rejection logic
4. ✅ Implement approval notifications
5. ✅ Add approval history tracking

**Deliverables**:
- Approval UI components
- Working approval workflow
- Approval history view

### Phase 4: Audit & Compliance (Week 7-8)

**Goal**: Complete audit and compliance features

**Tasks**:
1. ✅ Implement comprehensive audit logging
2. ✅ Create audit log viewer UI
3. ✅ Add compliance reporting
4. ✅ Implement data redaction
5. ✅ Add export functionality

**Deliverables**:
- Complete audit system
- Audit log viewer
- Compliance reports

### Phase 5: Integration & Testing (Week 9-10)

**Goal**: Integrate with Papillon AI and test

**Tasks**:
1. ✅ Full integration with Papillon AI
2. ✅ End-to-end testing
3. ✅ Performance optimization
4. ✅ Documentation
5. ✅ User acceptance testing

**Deliverables**:
- Fully integrated system
- Test suite
- User documentation

---

## 5. Technical Implementation Details

### 5.1 Extension Structure

```
papillon-governance/
├── package.json                    # Extension manifest
├── src/
│   ├── extension.ts               # Main entry point
│   ├── core/
│   │   ├── PolicyEngine.ts        # Policy evaluation
│   │   ├── AuditService.ts        # Audit logging
│   │   ├── Interceptor.ts         # Command interception
│   │   └── Sanitizer.ts           # Data sanitization
│   ├── ui/
│   │   ├── ApprovalPanel.ts       # Approval queue UI
│   │   ├── TaskReviewDialog.ts    # Task review UI
│   │   ├── AuditLogViewer.ts      # Audit log UI
│   │   └── PolicyConfigPanel.ts   # Policy config UI
│   ├── storage/
│   │   ├── AuditStore.ts          # Audit log storage
│   │   └── PolicyStore.ts         # Policy storage
│   └── utils/
│       ├── Redactor.ts            # Secret/PII redaction
│       └── RiskAssessor.ts        # Risk assessment
├── webview-ui/                    # React UI components
│   ├── src/
│   │   ├── components/
│   │   │   ├── ApprovalQueue.tsx
│   │   │   ├── TaskReview.tsx
│   │   │   └── AuditLog.tsx
│   │   └── App.tsx
│   └── package.json
└── test/
    ├── unit/
    └── integration/
```

### 5.2 Command Interception Mechanism

**Approach**: Use VS Code's command registration to wrap Papillon AI commands

```typescript
// In extension.ts
export async function activate(context: vscode.ExtensionContext) {
  const governance = new GovernanceController(context)
  
  // Intercept Papillon AI commands
  const papillonCommands = [
    'papillon.chat',
    'papillon.explainSelection',
    'papillon.refactorSelection',
    'papillon.generateTests',
    'papillon.runAgentTask'
  ]
  
  for (const command of papillonCommands) {
    // Register wrapper command
    context.subscriptions.push(
      vscode.commands.registerCommand(
        `${command}.governed`,
        async (...args) => {
          // 1. Intercept
          const result = await governance.interceptCommand(command, args)
          
          if (!result.allowed) {
            vscode.window.showWarningMessage(
              `Action blocked: ${result.reason}`
            )
            return
          }
          
          // 2. Execute original command
          const output = await vscode.commands.executeCommand(
            command,
            ...result.args
          )
          
          // 3. Filter output
          return await governance.filterOutput(output)
        }
      )
    )
  }
}
```

### 5.3 Storage Strategy

**Audit Logs**: SQLite database
- Structured queries
- Efficient indexing
- Easy export

**Policies**: VS Code settings + workspace settings
- User-level defaults
- Workspace-level overrides
- Easy configuration

**Approval Queue**: In-memory + persistent backup
- Fast access
- Survives restarts

---

## 6. Configuration Examples

### 6.1 Workspace Settings

```json
{
  "papillon.governance.enabled": true,
  "papillon.governance.policies": {
    "taskApproval": {
      "requireApprovalForCodeChanges": true,
      "requireApprovalForCommandExecution": true,
      "autoApproveReadOnlyOperations": true
    },
    "resourceLimits": {
      "maxTokensPerRequest": 100000,
      "maxConcurrentTasks": 3
    },
    "compliance": {
      "redactSecrets": true,
      "redactPII": true,
      "retentionDays": 90
    }
  }
}
```

### 6.2 Policy Templates

**Strict Mode** (High Security):
```json
{
  "requireApprovalForCodeChanges": true,
  "requireApprovalForCommandExecution": true,
  "requireApprovalForNetworkRequests": true,
  "autoApproveReadOnlyOperations": false,
  "maxTokensPerRequest": 50000,
  "redactSecrets": true,
  "redactPII": true
}
```

**Balanced Mode** (Default):
```json
{
  "requireApprovalForCodeChanges": true,
  "requireApprovalForCommandExecution": true,
  "requireApprovalForNetworkRequests": false,
  "autoApproveReadOnlyOperations": true,
  "maxTokensPerRequest": 100000,
  "redactSecrets": true,
  "redactPII": false
}
```

**Permissive Mode** (Low Security):
```json
{
  "requireApprovalForCodeChanges": false,
  "requireApprovalForCommandExecution": false,
  "requireApprovalForNetworkRequests": false,
  "autoApproveReadOnlyOperations": true,
  "maxTokensPerRequest": 200000,
  "redactSecrets": true,
  "redactPII": false
}
```

---

## 7. User Experience

### 7.1 Typical Workflow

1. **User initiates task** via Papillon AI
2. **Governance intercepts** the command
3. **Policy evaluation** determines if approval needed
4. **Approval dialog** shows if required:
   - Task description
   - Files to be modified
   - Commands to execute
   - Risk assessment
5. **User approves/rejects**
6. **Task executes** if approved
7. **Output filtered** and sanitized
8. **Audit logged** automatically

### 7.2 UI Mockups

**Approval Dialog**:
```
┌─────────────────────────────────────────────────┐
│ Task Approval Required                          │
├─────────────────────────────────────────────────┤
│                                                 │
│ Task: Refactor authentication module           │
│ Mode: Code                                      │
│ Risk: Medium                                    │
│                                                 │
│ Files to be modified:                           │
│ ✓ src/auth/login.ts                            │
│ ✓ src/auth/session.ts                          │
│ ✓ tests/auth.test.ts                           │
│                                                 │
│ Commands to execute:                            │
│ ✓ npm test                                      │
│                                                 │
│ Policy violations: None                         │
│                                                 │
│ [View Details] [Approve] [Reject]              │
└─────────────────────────────────────────────────┘
```

---

## 8. Benefits

### 8.1 For Organizations

- ✅ **Compliance**: Meet regulatory requirements
- ✅ **Auditability**: Complete action history
- ✅ **Control**: Enforce organizational policies
- ✅ **Risk Management**: Prevent unauthorized actions
- ✅ **Accountability**: Track who did what

### 8.2 For Users

- ✅ **Transparency**: See what AI will do before it acts
- ✅ **Safety**: Prevent accidental destructive actions
- ✅ **Confidence**: Know actions are logged and reviewable
- ✅ **Flexibility**: Configure policies per workspace

---

## 9. Alternatives Considered

### Alternative 1: Modify Papillon AI Core
**Pros**: Tighter integration
**Cons**: 
- Harder to maintain
- Breaks on Papillon AI updates
- More complex testing
**Decision**: ❌ Rejected

### Alternative 2: Proxy Service
**Pros**: Language-agnostic
**Cons**:
- Additional infrastructure
- Network latency
- More complex deployment
**Decision**: ❌ Rejected

### Alternative 3: Wrapper Extension (Selected)
**Pros**:
- Non-invasive
- Easy to maintain
- Independent updates
- Testable in isolation
**Cons**:
- Requires command interception
**Decision**: ✅ **Selected**

---

## 10. Success Metrics

### Technical Metrics
- Command interception success rate: >99%
- Approval workflow latency: <500ms
- Audit log write performance: <100ms
- Policy evaluation time: <50ms

### User Metrics
- User approval rate: Track approve/reject ratio
- False positive rate: <5% (unnecessary approvals)
- User satisfaction: >4/5 rating

### Compliance Metrics
- Audit log completeness: 100%
- Policy violation detection: >95%
- Data redaction accuracy: >99%

---

## 11. Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| Performance overhead | Medium | Async operations, caching |
| User friction | High | Smart auto-approval, clear UX |
| Papillon AI updates break integration | High | Loose coupling, version checks |
| Audit log storage growth | Medium | Retention policies, compression |
| Policy misconfiguration | High | Templates, validation, testing |

---

## 12. Next Steps

### Immediate Actions
1. ✅ Review and approve this plan
2. ✅ Set up development environment
3. ✅ Create `papillon-governance` extension skeleton
4. ✅ Implement basic command interception POC
5. ✅ Design policy configuration schema

### Week 1 Goals
- Working extension that can intercept one Papillon AI command
- Basic audit logging to file
- Simple approval dialog

---

## 13. Conclusion

The governance layer will be implemented as a **separate VS Code extension** that wraps Papillon AI, providing:

- ✅ **Policy enforcement** through command interception
- ✅ **Approval workflows** for sensitive operations
- ✅ **Comprehensive auditing** of all AI actions
- ✅ **Compliance features** including data redaction
- ✅ **Flexible configuration** per workspace

This approach is **non-invasive**, **maintainable**, and provides the control needed for enterprise use while preserving Papillon AI's functionality.

**Form**: Custom code application (VS Code extension)
**Timeline**: 10 weeks to full implementation
**Effort**: ~400-500 hours of development

---

**Document Version**: 1.0  
**Date**: December 21, 2024  
**Author**: Papillon AI Planning Team
