# Agent Architecture for MongoDB Documentation Code Examples

## Overview

This document defines the agent architecture for assisting technical writers
in creating tested code examples for MongoDB documentation. The architecture
uses specialized agents with focused context to improve output quality and
maintainability.

## Design Principles

### 1. Specialized Agents Over General Purpose
- **Rationale**: Specialized agents consistently outperform general-purpose agents in narrow domains (2026 best practices)
- **Implementation**: Separate agents for different languages and tasks (example creation vs test creation)
- **Benefit**: Reduced hallucination, better understanding of domain-specific nuances

### 2. Deterministic Scaffolding + AI Generation
- **Rationale**: Hybrid approaches combining deterministic tooling with AI generation produce more reliable outputs
- **Implementation**: CLI tool generates file structure and metadata; agents fill in implementation
- **Benefit**: Reduces structural decisions, improves consistency, minimizes hallucination

### 3. Focused Context Management
- **Rationale**: Smaller, focused context windows with relevant information outperform large, unfocused context
- **Implementation**: Each agent receives only the context needed for its specific task
- **Benefit**: Better comprehension, faster processing, more accurate outputs

### 4. Separation of Concerns
- **Example Agent**: Knows database operations, writes examples and cleanup
- **Test Agent**: Knows testing patterns, calls examples and validates output
- **Reviewer Agent**: Knows quality criteria, validates and suggests improvements

## Agent Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                     ORCHESTRATOR AGENT                          │
│  • Analyzes writer requests                                     │
│  • Executes CLI commands (discovery, scaffolding)               │
│  • Routes to specialized agents                                 │
│  • Manages workflow state                                       │
│  • Presents results                                             │
└────────────┬────────────────────────────────────────────────────┘
             │
             │ Calls CLI commands for deterministic operations
             │ (discovery, file creation, metadata extraction)
             │
    ┌────────┼────────┬────────────────┬──────────────┐
    ▼        ▼        ▼                ▼              ▼
┌─────────┐ ┌──────────────────────┐ ┌─────────┐ ┌──────────┐
│ Audit   │ │  Example Agents      │ │  Test   │ │ Reviewer │
│  CLI    │ │                      │ │  Agent  │ │  Agent   │
│ (Tool)  │ │  • Net New           │ └─────────┘ └──────────┘
└─────────┘ │  • Translation       │
            │  • Migration         │
            └──────────────────────┘

WORKFLOW EXAMPLE: "Create Node.js $lookup example"
═══════════════════════════════════════════════════

1. Writer Request
   │
   ▼
2. Orchestrator Agent
   ├─> Analyzes: "net new example, nodejs, $lookup aggregation"
   └─> Calls CLI: grove-cli list examples javascript/driver aggregation
   │
   ▼
3. CLI Returns: Similar examples found
   │
   ▼
4. Orchestrator Agent
   ├─> Presents to writer: "Found aggregation/lookup-join.js. Use this or create new?"
   └─> Writer responds: "Create new with different collections"
   │
   ▼
5. Orchestrator Agent
   └─> Calls CLI: grove-cli scaffold javascript/driver lookup-example --topic aggregation
   │
   ▼
6. CLI Creates Scaffolds
   ├─> Creates: examples/aggregation/lookup-example.js (with TODOs)
   ├─> Creates: tests/aggregation/lookup-example.test.js (with TODOs)
   └─> Returns: metadata JSON
   │
   ▼
7. Orchestrator Agent
   └─> Routes to: nodejs-net-new-example-agent (with metadata)
   │
   ▼
8. nodejs-net-new-example-agent
   ├─> Reads: example scaffold
   ├─> Fills: runLookupExample() implementation
   ├─> Adds: Bluehawk markup
   ├─> Adds: cleanupLookupExample() (if needed)
   └─> Returns: completed example
   │
   ▼
9. Orchestrator Agent
   └─> Routes to: nodejs-test-agent (with metadata)
   │
   ▼
10. nodejs-test-agent
    ├─> Reads: test scaffold
    ├─> Fills: test implementation
    ├─> Imports: runLookupExample, cleanupLookupExample
    ├─> Adds: describeWithSampleData
    └─> Adds: afterEach cleanup call
    │
    ▼
11. Orchestrator Agent
    └─> Routes to: reviewer-agent
    │
    ▼
12. Reviewer Agent
    ├─> Runs: npm test (1st run)
    ├─> Runs: npm test (2nd run - idempotency)
    ├─> Runs: npm run snip (Bluehawk)
    └─> Reports: ✅ All checks passed
    │
    ▼
13. Orchestrator Agent
    └─> Presents: Summary to writer
```

## CLI Integration

Grove uses two CLI tools for different purposes:

### Grove CLI (Test Suite Operations)

**Purpose**: Discovery and scaffolding within `code-example-tests/` directory

**Location**: `code-example-tests/grove-meta/cli/`

#### Discovery Commands
```bash
# List available test suites
grove-cli list suites
# Output: javascript/driver, python/pymongo, csharp/driver, etc.

# List directory structure within a suite
grove-cli list topics <suite>
# Output: List of topic directories

# List examples in a topic
grove-cli list examples <suite> <topic>
# Output: List of example files

# List test files in a topic (for test strategy decisions)
grove-cli list tests <suite> <topic>
# Output: List of test files with examples they test

# Read specific example file
grove-cli read <suite> <example-path>
# Output: File content + Bluehawk snippets
```

#### Scaffolding Commands
```bash
# Create new example scaffold (test files created by Test Agent)
grove-cli scaffold <suite> <example-name> [--topic <topic>]
# Creates:
#   - examples/<topic>/<example-name>.js (with TODOs)
# Returns: JSON with file path
```

### Audit CLI (Documentation Extraction)

**Purpose**: Extract code examples from documentation source files (RST)

**Location**: Separate repository

**Used by**: Translation Agent, Migration Agent, Reviewer Agent

```bash
# Extract code examples from documentation page
audit-cli extract code-examples <page-path> --output /tmp/extracted/
# Output: Extracted code files (no RST markup)
```

### CLI Output Format

When creating scaffolds, CLI outputs file paths:

```json
{
  "suite": "javascript/driver",
  "example_file": "examples/aggregation/group-example.js"
}
```

**Note**: Agents infer context from suite name, file paths, and auto-loaded skills. No additional metadata needed.

## Monorepo Scoping

### Problem

This repository is a monorepo containing multiple concerns:
- **code-example-tests/** - Code example test infrastructure (our domain)
- **documentation-content/** - Documentation source files (not our domain)
- **build-tooling/** - Build and deployment tools (not our domain)
- **external-repos/** - External repository content (not our domain)

**Agents and skills must ONLY activate for code example operations in `code-example-tests/`.**

### Agent Naming Convention

**All Grove agents use `grove-` prefix** to avoid conflicts with other agents:

```
@grove-planning-agent
@grove-orchestrator
@grove-net-new-example-agent
@grove-translation-example-agent
@grove-migration-example-agent
@grove-test-agent
@grove-reviewer-agent
```

**Benefits**:
- ✅ Clear namespace separation
- ✅ No conflicts with future agents (e.g., `@docs-planning-agent`, `@build-orchestrator`)
- ✅ Self-documenting (writers know it's Grove-related)
- ✅ Consistent with CLI naming (`grove-cli`)

### Scoping Strategy: Layered Defense

#### Layer 1: Agent Scope Declaration

Every agent includes explicit scope declaration in instructions:

```markdown
## Scope and Activation

**This agent is ONLY for code example operations in the `code-example-tests/` directory.**

### High-Confidence Activation (Proceed immediately)

Activate when request contains **BOTH**:
1. "code example" (or "code examples")
2. AND one of: "create", "add", "generate", "write", "test", "migrate", "update"

OR when explicitly mentioned:
- "@grove-planning-agent" (or other @grove-* agent)
- "grove agent"

### Medium-Confidence Activation (Ask for clarification)

If request contains ONLY:
- "example" (without "code") → Ask: "Did you mean code examples for documentation?"
- "test" (without "code example") → Ask: "Are you referring to code example tests?"
- "migration" (without "code example") → Ask: "Are you referring to code example migration?"

### Do NOT Activate

When request is clearly about:
- Build tooling tests (e.g., "test the build script")
- Data migration (e.g., "migrate the database")
- Documentation content editing (e.g., "update this documentation page")
- Utility development (e.g., "add tests for the comparison library")

### Decline Pattern

If activation criteria not met, respond:
"This request doesn't appear to be about code examples for documentation.
 I'm @grove-planning-agent, specialized for code example operations in code-example-tests/.

 Did you mean to:
 - Create/test/migrate code examples? (I can help with that)
 - Work on something else? (Try a different agent or standard Copilot)"
```

**Key insight**: Agents activate based on **keywords in the request**, not the current file location. Writers often request code examples while viewing documentation pages, not while in `code-example-tests/`.

#### Layer 2: CLI Directory Validation

CLI commands validate working directory before execution:

```javascript
function validateWorkingDirectory() {
  const cwd = process.cwd();

  if (!cwd.includes('code-example-tests')) {
    console.error(`
Error: grove-agent commands must be run from code-example-tests/ directory.
Current: ${cwd}
Please navigate there first: cd code-example-tests
    `);
    process.exit(1);
  }
}
```

#### Layer 3: Skill Scoping

Skills declare their domain in frontmatter:

```markdown
---
name: nodejs-example-creation
description: Node.js MongoDB driver code example patterns for code-example-tests
scope: code-example-tests
filePatterns:
  - "code-example-tests/**/*.js"
  - "code-example-tests/**/*.test.js"
---
```

#### Layer 4: Path-Scoped Custom Instructions (Verified)

**GitHub Copilot officially supports path-scoped custom instructions** using `*.instructions.md` files.

**Purpose**: Provide **additional context** when editing files in `code-example-tests/`, NOT for activation.

**Key insight**: Path-scoped instructions activate based on the **current file being edited**, not based on chat requests about other files. Writers often request code examples while viewing documentation pages, so we can't rely on path-scoping for activation.

**How it works**:
- Create files in `.github/instructions/` directory (repository root)
- Name files descriptively: `code-examples.instructions.md`
- Use `applyTo:` section with path globs to target specific directories

**Example**: `.github/instructions/code-examples.instructions.md`

```markdown
applyTo:
  - code-example-tests/**
---

# Code Example Implementation Context

When working on files in code-example-tests/:

## Language-Specific Patterns
- Node.js: async/await, MongoClient patterns
- C#: async Task, IDisposable cleanup
- Python: context managers, pytest fixtures
- Go: defer cleanup, error handling
- Java: try-with-resources, JUnit patterns

## Required Elements
- Executable example function
- Cleanup function (if data modified)
- Bluehawk markup for snippets
- Return values for test validation
- Sample data usage (sample_mflix, sample_restaurants)

## Quality Standards
- Idempotent tests (pass twice in a row)
- No hardcoded values (use sample data)
- Proper error handling
- Clear variable names for documentation

---

**Note**: This context applies when editing files in code-example-tests/.
For planning/orchestration, use @grove-planning-agent or @grove-orchestrator.
```

**Benefits**:
- ✅ Officially supported by GitHub Copilot (as of Sept 2025)
- ✅ Deterministic path matching using glob patterns
- ✅ Multiple instruction files for different domains
- ✅ Works with Copilot Chat, Code Review, and Coding Agent
- ✅ Provides rich context when actually working in code-example-tests/

**Limitations**:
- ❌ Cannot use nested `.github/` directories (not supported)
- ✅ Must use root `.github/instructions/` directory
- ⚠️ Only activates when editing files matching the path pattern
- ⚠️ Does NOT activate for chat requests about other files

#### Layer 5: Repository-Wide Instructions (For Activation)

**GitHub Copilot also supports repository-wide instructions** in `.github/copilot-instructions.md`.

**Purpose**: Provide **activation logic** that works regardless of current file location.

**Use case**: Since writers often request code examples while viewing documentation pages (not in `code-example-tests/`), we need repository-wide instructions to handle activation based on keywords.

**Example**: `.github/copilot-instructions.md`

```markdown
# Grove Code Example Agents

For code example operations, use Grove agents with `@grove-` prefix:
- **@grove-planning-agent** - Analyze pages, suggest examples
- **@grove-orchestrator** - Execute example creation workflows

**Activation keywords**: "code example" (or "code examples") + action verb (create/add/test/migrate/update)

**Examples that activate Grove agents**:
- ✅ "Create code examples for this page"
- ✅ "Add code examples showing insertOne"
- ✅ "Test the code examples on this page"
- ✅ "Migrate these code examples to the test suite"

**Examples that do NOT activate Grove agents**:
- ❌ "Run tests for the build tooling" (no "code example")
- ❌ "Migrate the database schema" (no "code example")
- ❌ "Add tests for the comparison utility" (no "code example")

**Scope**: Code examples in `code-example-tests/` directory only

---

For other operations (documentation editing, build tooling, utilities):
- Use standard GitHub Copilot features
- Do NOT use Grove agents
```

**Alternative: AGENTS.md files**

GitHub Copilot also supports `AGENTS.md` files (inspired by the agents.md standard):
- Can be placed anywhere in the repository
- Nearest `AGENTS.md` in directory tree takes precedence
- Useful for directory-specific agent instructions

**For our monorepo**: We could use `code-example-tests/AGENTS.md` as an alternative to path-scoped instructions, but path-scoped instructions are more explicit and better supported.

### Recommended Scoping Implementation

**Hybrid strategy**: Use **Layer 5 (Repository-Wide) for activation** + **Layer 4 (Path-Scoped) for context**.

**File structure**:
```
grove-agent-exploration/
├── .github/
│   ├── copilot-instructions.md              # Repository-wide: activation logic
│   └── instructions/
│       └── code-examples.instructions.md    # Path-scoped: implementation context
└── code-example-tests/
    ├── node/
    ├── csharp/
    └── python/
```

**Why this approach**:
- ✅ Works when writers request code examples from documentation pages
- ✅ Keyword-based activation (deterministic patterns)
- ✅ Path-scoped instructions provide rich context when editing in code-example-tests/
- ✅ Clear separation from other monorepo concerns
- ✅ Multiple activation methods (keywords, explicit mention, path)

**Backup layers**: Layers 1-3 (agent declarations, CLI validation, skill scoping) provide additional defense.

### Writer Guidance

**To activate code example agents**:
1. Use keywords: "code example" + action verb (create/add/test/migrate)
2. Or mention agent explicitly: "@grove-planning-agent" or "@grove-orchestrator"
3. Works from any file location (documentation pages, code-example-tests/, etc.)

**Examples**:
- ✅ "Create code examples for this page"
- ✅ "@grove-planning-agent analyze this page"
- ✅ "Add Node.js code examples for insertOne"

**Agents will decline requests** that don't contain "code example" keywords or explicit agent mentions.

**Path-scoped instructions will automatically provide additional context** when working on files in `code-example-tests/`.

## Agent Tiers

### Tier 0: Planning and Scope Determination

#### @grove-planning-agent

**Purpose**: Analyze requests, check for duplicates, decide test strategy, suggest examples

**Scope**: ONLY code example operations in `code-example-tests/` directory

**Activation**: Invoked by @grove-orchestrator for **all** code example requests (not directly by writers)

**Responsibilities**:
- Receive **all** writer requests from @grove-orchestrator (simple and complex)
- **For simple requests** (e.g., "create a Node.js insertOne code example"):
  - Check for duplicate examples using CLI discovery
  - Analyze existing test files and decide test file strategy
  - Generate single-example plan with test strategy
  - Return plan quickly to Orchestrator
- **For complex requests** (e.g., "create examples for this page"):
  - Read and analyze documentation page content
  - Identify sections that need examples
  - Infer operation types from page content
  - Suggest example names (following naming conventions)
  - Execute CLI discovery commands to check for existing examples
  - Analyze existing test files and decide test file strategy for each example
  - Generate multi-example plan with test strategies
  - Return plan to Orchestrator for writer approval
- **For all requests**:
  - Prevent duplicate examples
  - Prevent test file explosion through intelligent test file strategy
  - If scope cannot be determined, escalate to writer with clarification questions

**CLI Integration**:
- Calls `grove-cli list topics <suite>` to see directory structure
- Calls `grove-cli list examples <suite> <topic>` to find existing examples in a topic
- Calls `grove-cli list tests <suite> <topic>` to analyze existing test files
- Calls `grove-cli read <suite> <example-path>` to examine specific examples
- Uses CLI output to avoid duplicate suggestions and decide test file strategy
- **Note**: grove-cli provides scoped discovery within `code-example-tests/` to avoid contamination from documentation content in monorepo (codebase-retrieval searches entire repository)

**Input**:
- Writer request (natural language)
- Page URL or directory path
- Language/framework (if specified)

**Output**: Structured plan (JSON)
```json
{
  "pageUrl": "https://docs.mongodb.com/drivers/node/current/usage-examples/insertOne/",
  "pageTitle": "Insert a Document",
  "suggestedExamples": [
    {
      "name": "insert-one-basic",
      "operation": "insert",
      "section": "Insert a Single Document",
      "rationale": "Section shows insertOne syntax, needs executable example",
      "language": "nodejs",
      "taskType": "net-new",
      "priority": "high",
      "testStrategy": {
        "action": "create_new_file",
        "testFile": "tests/crud/insert-one-basic.test.js",
        "rationale": "No existing test file for insert operations in crud/"
      }
    },
    {
      "name": "insert-one-with-options",
      "operation": "insert",
      "section": "Insert Options",
      "rationale": "Section covers writeConcern and other options",
      "language": "nodejs",
      "taskType": "net-new",
      "priority": "medium",
      "testStrategy": {
        "action": "add_to_existing",
        "testFile": "tests/crud/insert-one-basic.test.js",
        "rationale": "Group related insert examples in same test file"
      }
    }
  ],
  "existingExamples": [
    {
      "file": "examples/insert/insert-one.js",
      "tested": true,
      "testFile": "tests/insert/insert-one.test.js",
      "note": "Already covers basic insertOne"
    }
  ],
  "requiresWriterApproval": true,
  "clarificationNeeded": null
}
```

**Escalation to Writer** (if scope unclear):
```json
{
  "clarificationNeeded": true,
  "question": "This page covers both insertOne and insertMany. Should I create examples for:\n  a) insertOne only\n  b) insertMany only\n  c) Both\n  d) Let me specify",
  "context": "Page has 2 main sections but unclear which need examples"
}
```

**Does NOT**:
- Execute scaffolding commands (Orchestrator does this)
- Route to example/test agents (Orchestrator does this)
- Create files (CLI does this)
- Write code (example agents do this)

**Discovery Tasks**:
- "Find untested examples on this page"
- "Analyze test coverage for this directory"
- "Suggest examples missing from this page"
- "Find all examples that need migration"

**Context Size**: ~2,500 tokens
- Documentation structure patterns
- Section identification heuristics
- Operation type inference rules
- Example naming conventions
- MongoDB operation catalog
- Driver API mappings
- Deduplication logic

### Tier 1: Orchestration

#### @grove-orchestrator

**Purpose**: Coordinate agents, execute CLI scaffolding commands, manage workflow

**Scope**: ONLY code example operations in `code-example-tests/` directory

**Activation**: See Layer 1 (Agent Scope Declaration) for keyword-based activation logic

**Responsibilities**:
- Receive writer requests
- **Always route to @grove-planning-agent first** (for duplicate checking and test strategy decisions)
- Present plans to writer for approval
- Execute grove-cli scaffolding commands (after plan approval)
- Route to appropriate example agent based on task type (@grove-net-new-example-agent, @grove-translation-example-agent, @grove-migration-example-agent)
- Route to @grove-test-agent with test strategy from plan
- Route to @grove-reviewer-agent
- Coordinate fix loops (max 3 iterations)
- Consolidate results and present to writer
- Handle escalations and writer interventions

**Simplified Workflow** (always starts with Planning Agent):
```
Writer Request
    ↓
@grove-orchestrator
    ↓
@grove-planning-agent (checks duplicates, decides test strategy)
    ↓
@grove-orchestrator (presents plan, gets approval)
    ↓
Execute scaffold command
    ↓
Route to example agent → test agent → reviewer agent
```

**Why Always Call Planning Agent**:
- Planning Agent checks for duplicate examples (prevents redundant work)
- Planning Agent decides test file strategy (prevents test file explosion)
- Consistent workflow regardless of request specificity
- Planning Agent is fast for simple requests (single example, clear scope)
- Keeps Orchestrator focused on coordination, not decision-making

**CLI Integration**:
- Calls `grove-cli scaffold` to create example file scaffold (after plan approval)
- Passes file paths and Planning Agent's test strategy to agents
- **Note**: grove-cli only scaffolds example files, not test files (test file creation/modification is handled by @grove-test-agent)

**Does NOT**:
- Analyze page content (@grove-planning-agent does this)
- Suggest examples (@grove-planning-agent does this)
- Decide test file strategy (@grove-planning-agent does this)
- Execute discovery commands (@grove-planning-agent does this)
- Write code (example agents do this)
- Run tests (@grove-reviewer-agent does this)
- Load language-specific skills (agents do this via auto-loading)

**Context Size**: ~3,500 tokens
- Task routing logic
- CLI command patterns
- Agent capability matrix
- Workflow patterns
- Error handling strategies
- Iteration tracking

### Tier 2: Task-Specific Agents (Language-Agnostic)

**Purpose**: Execute task-specific logic using universal patterns and language-specific skills.

#### Example Creation Agents

**Three Specialized Types**:
- `@grove-net-new-example-agent` - Create examples from scratch
- `@grove-translation-example-agent` - Translate examples between languages/products
- `@grove-migration-example-agent` - Migrate untested code to testable state

**Scope**: ONLY code example operations in `code-example-tests/` directory

**Activation**: Invoked by @grove-orchestrator (not directly by writers)

**Responsibilities** (all agents):
- Receive file path and context from @grove-orchestrator (suite determines language)
- Write executable example function (using language-specific skill)
- Write cleanup function (using language-specific skill + cleanup-patterns)
- Add Bluehawk markup (using bluehawk-syntax skill)
- Return file paths to @grove-orchestrator

**Skills Auto-Loaded** (example: Node.js request):
- `sample-data` - Database selection
- `bluehawk-syntax` - Markup syntax
- `cleanup-patterns` - Cleanup strategies
- `documentation-principles` - Doc code best practices
- `nodejs-example-creation` - Node.js implementation patterns

**Task-Specific Differences**:
- **@grove-net-new-example-agent**: Selects sample database, infers structure from requirements
- **@grove-translation-example-agent**: Maps APIs from source to target language, preserves structure
- **@grove-migration-example-agent**:
  - Detects anti-patterns, refactors to testable structure
  - **Documents discrepancies** as changes are made (structured output)
  - Explains *why* each discrepancy exists
  - Flags areas requiring writer review
  - Returns discrepancy documentation to @grove-orchestrator for writer presentation

#### Test Creation Agent

**Agent**: `@grove-test-agent` (language-agnostic)

**Scope**: ONLY code example operations in `code-example-tests/` directory

**Activation**: Invoked by @grove-orchestrator (not directly by writers)

**Responsibilities**:
- Receive example file path and test strategy from @grove-orchestrator
- **Test strategy determines action**:
  - **create_new_file**: Create new test file with test structure
  - **add_to_existing**: Add test case to existing test file
- Create or modify test file in appropriate location (using language-specific skill)
- Import example and cleanup functions (using language-specific skill)
- Write test cases that validate return values (using language-specific skill)
- Call cleanup in afterEach hook (using language-specific skill + cleanup-patterns)
- Return test file path to @grove-orchestrator

**Test Strategy Input** (from Planning Agent):
```json
{
  "testStrategy": {
    "action": "create_new_file",  // or "add_to_existing"
    "testFile": "tests/crud/insert-one-basic.test.js",
    "rationale": "No existing test file for insert operations in crud/"
  }
}
```

**Skills Auto-Loaded** (example: Node.js request):
- `sample-data` - Database selection
- `cleanup-patterns` - Cleanup strategies
- `nodejs-test-creation` - Jest patterns, describeWithSampleData, assertions, adding test cases to existing files

### Tier 3: Quality Control

#### @grove-reviewer-agent

**Purpose**: Validate outputs, run tests, check compliance

**Scope**: ONLY code example operations in `code-example-tests/` directory

**Activation**: Invoked by @grove-orchestrator (not directly by writers)

**Responsibilities**:
- Receive example and test file paths from @grove-orchestrator
- Run tests twice to verify idempotency (using language-specific test command)
- Check for common issues (missing cleanup, hardcoded values, etc.)
- Validate Bluehawk markup
- **If tests fail**: Classify failure type and route to appropriate agent
- Report results to @grove-orchestrator
- If migration task:
  - Confirm Bluehawked example matches original example exactly, or at least matches logic
  - Validate that discrepancies are documented (from @grove-migration-example-agent)
  - If undocumented discrepancies found, escalate to @grove-orchestrator
  - Pass discrepancy documentation + validation results to @grove-orchestrator for writer review

**Failure Classification (Triager Pattern)**:

Reviewer acts as a **triager**, not a diagnostician. It classifies failures using pattern matching and routes to the appropriate specialist.

**What Reviewer CAN do**:
- ✅ Run tests and capture output
- ✅ Pattern match on error messages
- ✅ Classify failure type (compilation, runtime, assertion, idempotency, etc.)
- ✅ Extract error context (line numbers, error messages, stack traces)
- ✅ Route to appropriate agent with context

**What Reviewer CANNOT do**:
- ❌ Understand code semantics
- ❌ Diagnose complex logic errors
- ❌ Suggest specific code fixes
- ❌ Modify code directly

**Classification Patterns**:
- **Compilation/Syntax Error** → Route to Example Agent
  - Pattern: `SyntaxError`, `cannot find module`, `is not defined`
- **Runtime Error** → Route to Example Agent
  - Pattern: `TypeError`, `ReferenceError`, `null is not an object`
- **Assertion Failure** → Route to Orchestrator (needs review)
  - Pattern: `Expected X but received Y`
  - Could be wrong example output OR wrong test expectation
- **Idempotency Failure** → Route to Example Agent
  - Pattern: First run passes, second run fails
  - Hint: "Check cleanup function implementation"
- **Connection Error** → Route to Example Agent
  - Pattern: `ECONNREFUSED`, `MongoServerError`
- **Environment Error** → Escalate to Writer
  - Pattern: `sample_mflix not found`, `database not available`

**Failure Report Structure**:
```json
{
  "failureType": "idempotency_failure",
  "routeTo": "example-agent",
  "context": "Second run failed at line 45: duplicate key error",
  "hint": "Check cleanup function removes test data",
  "testOutput": "...",
  "firstRunPassed": true,
  "secondRunPassed": false,
  "iteration": 1
}
```

**Fix Loop**:
1. Reviewer classifies failure
2. Reviewer routes to Orchestrator with classification
3. Orchestrator routes to appropriate agent (Example or Test)
4. Agent fixes issue
5. Orchestrator routes back to Reviewer
6. Reviewer runs tests again
7. Max 3 iterations, then escalate to writer

**Note on Responsibility Boundary**:
- Reviewer **classifies** failures using pattern matching
- Reviewer **does NOT diagnose** root causes
- Example/Test Agents are responsible for understanding code and fixing
- Orchestrator coordinates the fix loop and enforces iteration limits

**Test Commands** (embedded in agent, not skills):
- Node.js: `npm test`
- C#: `dotnet test`
- Python: `pytest`
- Go: `go test`
- Java: `mvn test`

**Success Criteria**:
- ✅ Tests pass twice in a row
- ✅ Cleanup functions exist
- ✅ Bluehawk markup is valid
- ✅ No hardcoded connection strings
- ✅ Uses sample data appropriately

## Task Type Differentiation

### Why Three Example Agent Types?

The three task types have fundamentally different requirements:

| Aspect | Net New | Translation | Migration |
|--------|---------|-------------|-----------|
| **Input** | Requirements/spec | Working example in different language | Existing untested code |
| **Structure Decisions** | Must infer from requirements | Mirror source structure | Structure exists |
| **API Knowledge** | Must know MongoDB + language | Must map APIs across products | Already has API calls |
| **Risk** | HIGH - no reference | MEDIUM - has pattern | LOW - has working code |
| **Context Needs** | MongoDB concepts, sample data catalog | API mappings, idiomatic patterns | Anti-patterns, refactoring rules |

**Decision**: Separate agents for each task type to maintain focused context and reduce cognitive load.

## Agent Communication Patterns

### Information Flow and Responsibility Boundaries

Agents communicate through the **Orchestrator** using structured outputs. Each agent is responsible for specific information:

#### 1. Example Agents → Orchestrator

**All Example Agents Return**:
- File paths (example file, cleanup file if applicable)

**Migration Agent Additionally Returns**:
- Discrepancy documentation (structured output)
- Reason for each discrepancy
- Writer action items

**Migration Discrepancy Documentation Structure**:
```json
{
  "taskType": "migration",
  "language": "nodejs",
  "exampleFile": "examples/aggregation/lookup.js",
  "originalFile": "legacy/lookup-example.js",
  "discrepancies": [
    {
      "type": "data_source_change",
      "location": "line 15-20",
      "original": "hardcoded array of movies",
      "migrated": "sample_mflix.movies collection",
      "reason": "Documentation examples require sample data for testability",
      "writerAction": "Review page text for references to hardcoded data structure",
      "semanticImpact": "none"
    },
    {
      "type": "error_handling_added",
      "location": "line 25-30",
      "original": "no error handling",
      "migrated": "try-finally with client.close()",
      "reason": "Required for testable, production-ready examples",
      "writerAction": "Consider if page text should mention error handling",
      "semanticImpact": "minor"
    }
  ],
  "semanticEquivalence": "partial",
  "requiresWriterReview": true
}
```

**Rationale**:
- ✅ Migration agent has context while making changes
- ✅ Captures "why" before context is lost
- ✅ Reviewer validates, doesn't diagnose
- ✅ Orchestrator consolidates for writer

#### 2. Test Agent → Orchestrator

**Returns**:
- Test file path
- Test framework used
- Sample data requirements
- Cleanup invocation confirmed

#### 3. Reviewer Agent → Orchestrator

**Returns**:
- Test results (pass/fail, first run)
- Idempotency results (pass/fail, second run)
- Bluehawk validation results
- Quality check results
- **For migration**: Validation that discrepancies are documented
- Suggested fixes (if tests fail)

**Does NOT**:
- Diagnose undocumented discrepancies (escalates instead)
- Modify code directly (routes through Orchestrator)

#### 4. Orchestrator → Writer

**Presents**:
- Consolidated results from all agents
- Migration discrepancies (if applicable) with reasons
- Test results and idempotency status
- Writer action items
- Next steps or required interventions

### Error Handling and Escalation

**If tests fail** (Triager Pattern):
1. Reviewer runs tests and captures output
2. Reviewer classifies failure using pattern matching
3. Reviewer creates failure report with classification
4. Reviewer routes to Orchestrator with failure report
5. Orchestrator routes to appropriate agent:
   - **Compilation/Runtime/Idempotency/Connection** → Example Agent
   - **Assertion Failure** → Orchestrator reviews (could be Example or Test Agent)
   - **Environment Error** → Escalate to Writer
6. Agent receives failure report with context
7. Agent diagnoses and fixes (using code understanding)
8. Agent returns fixed code to Orchestrator
9. Orchestrator routes back to Reviewer
10. Reviewer runs tests again (iteration++)
11. **Max 3 iterations**, then escalate to writer

**Iteration Tracking**:
```json
{
  "iteration": 1,
  "maxIterations": 3,
  "history": [
    {
      "iteration": 1,
      "failureType": "idempotency_failure",
      "routedTo": "example-agent",
      "fixAttempted": "Added cleanup function",
      "result": "still_failing"
    }
  ]
}
```

**If undocumented discrepancies found (migration)**:
1. Reviewer detects discrepancy not in metadata
2. Reviewer escalates to Orchestrator
3. Orchestrator routes back to Migration Agent
4. Migration Agent documents discrepancy
5. Returns to Reviewer for validation

**If writer intervention needed**:
1. Any agent can flag `requiresWriterReview: true`
2. Orchestrator consolidates all flags
3. Orchestrator presents to writer with context
4. Writer makes decision
5. Orchestrator routes follow-up actions

**Escalation Triggers**:
- ✅ 3 failed fix iterations
- ✅ Environment errors (missing sample data, connection issues)
- ✅ Ambiguous failures (Orchestrator can't determine if Example or Test issue)
- ✅ Migration discrepancies requiring writer judgment
- ✅ Any agent explicitly flags `requiresWriterReview: true`

## Writer Workflows

### Workflow A: Specific Request (Single Example)

**Writer Request**: "Create a Node.js code example for insertOne"

**Activation**: ✅ Contains "code example" + "create" (high-confidence)

**Flow**:
```
Writer (viewing documentation page)
  ↓
@grove-orchestrator (activates via keywords)
  ├─> Detects: Specific scope (one example, clear operation)
  ├─> Determines: net-new task, nodejs language, insert operation
  ├─> Calls CLI: grove-cli scaffold javascript/driver insert-one-basic --topic crud/insert
  ├─> Routes to: @grove-net-new-example-agent
  └─> Routes to: @grove-test-agent → @grove-reviewer-agent
```

**No @grove-planning-agent needed** - Scope is clear from request

**Result**: One example created, tested, and validated

**Note**: Works from any file location (documentation page, code-example-tests/, etc.)

---

### Workflow B: Page-Level Request (Multiple Examples)

**Writer Request**: "Create code examples for this documentation page: <filepath>"

**Activation**: ✅ Contains "code examples" + "create" (high-confidence)

**Flow**:
```
Writer (viewing documentation page)
  ↓
@grove-orchestrator (activates via keywords)
  ├─> Detects: Vague scope (page-level, unclear how many examples)
  └─> Routes to: @grove-planning-agent
      ↓
@grove-planning-agent
  ├─> Reads page content
  ├─> Calls CLI: grove-cli list examples javascript/driver crud/insert
  ├─> Analyzes sections: "Insert a Single Document", "Insert Options"
  ├─> Checks existing examples (via CLI output)
  ├─> Generates plan:
  │   {
  │     "suggestedExamples": [
  │       {"name": "insert-one-basic", "operation": "insert", "priority": "high"},
  │       {"name": "insert-one-with-options", "operation": "insert", "priority": "medium"}
  │     ],
  │     "existingExamples": [],
  │     "requiresWriterApproval": true
  │   }
  └─> Returns plan to: @grove-orchestrator
      ↓
@grove-orchestrator
  ├─> Presents plan to writer:
  │   "@grove-planning-agent suggests 2 examples:
  │    1. insert-one-basic (high priority)
  │    2. insert-one-with-options (medium priority)
  │    Approve all / Choose specific / Cancel?"
  └─> Waits for writer approval
      ↓
Writer: "Approve all"
      ↓
@grove-orchestrator
  ├─> Calls CLI: grove-cli scaffold javascript/driver insert-one-basic --topic crud/insert
  ├─> Routes to: @grove-net-new-example-agent → @grove-test-agent → @grove-reviewer-agent
  ├─> Calls CLI: grove-cli scaffold javascript/driver insert-one-with-options --topic crud/insert
  └─> Routes to: @grove-net-new-example-agent → @grove-test-agent → @grove-reviewer-agent
```

**@grove-planning-agent determines scope** - @grove-orchestrator executes approved plan

**Result**: Multiple examples created based on page analysis

**Note**: Works from any file location (documentation page, code-example-tests/, etc.)

---

### Workflow C: Migration Request (Discovery Needed)

**Writer Request**: "Add tests for the examples on this page: <filepath>"

**Flow**:
```
Writer
  ↓
Orchestrator
  ├─> Detects: Migration task (add tests = migrate to testable state)
  └─> Routes to: Planning Agent
      ↓
Planning Agent
  ├─> Calls CLI: grove-cli list examples javascript/driver legacy
  ├─> CLI returns:
  │   {
  │     "suite": "javascript/driver",
  │     "topic": "legacy",
  │     "examples": [
  │       "basic-insert.js",
  │       "insert-with-id.js"
  │     ]
  │   }
  ├─> Generates plan:
  │   {
  │     "taskType": "migration",
  │     "suggestedMigrations": [
  │       {"source": "legacy/basic-insert.js", "target": "insert-one-basic"},
  │       {"source": "legacy/insert-with-id.js", "target": "insert-one-with-id"}
  │     ],
  │     "requiresWriterApproval": true
  │   }
  └─> Returns plan to: Orchestrator
      ↓
Orchestrator
  ├─> Presents plan to writer:
  │   "Found 2 untested examples:
  │    1. legacy/basic-insert.js → migrate to insert-one-basic
  │    2. legacy/insert-with-id.js → migrate to insert-one-with-id
  │    Approve all / Choose specific / Cancel?"
  └─> Waits for writer approval
      ↓
Writer: "Approve all"
      ↓
Orchestrator
  ├─> Calls CLI: grove-cli scaffold javascript/driver insert-one-basic --topic crud/insert
  ├─> Routes to: migration-example-agent → test-agent → reviewer-agent
  ├─> Calls CLI: grove-cli scaffold javascript/driver insert-one-with-id --topic crud/insert
  └─> Routes to: migration-example-agent → test-agent → reviewer-agent
```

**Planning Agent discovers existing examples** - Orchestrator executes migration

**Result**: Untested examples migrated to testable state

---

### Workflow D: Unclear Scope (Escalation to Writer)

**Writer Request**: "Create examples for this page: <filepath>"

**Flow**:
```
Writer
  ↓
Orchestrator
  ├─> Detects: Vague scope
  └─> Routes to: Planning Agent
      ↓
Planning Agent
  ├─> Reads page content
  ├─> Finds: 15 different sections covering many topics
  ├─> Cannot determine: Which sections need examples
  ├─> Generates clarification request:
  │   {
  │     "clarificationNeeded": true,
  │     "question": "This page covers 15 different topics (Connection, CRUD, Aggregation, etc.).
  │                  Which topics should I create examples for?
  │                  a) All topics
  │                  b) Let me choose specific topics
  │                  c) Cancel",
  │     "context": "Page is a fundamentals overview with many sections"
  │   }
  └─> Returns to: Orchestrator
      ↓
Orchestrator
  └─> Presents clarification to writer
      ↓
Writer: "Just CRUD operations"
      ↓
Orchestrator
  └─> Routes back to: Planning Agent with clarification
      ↓
Planning Agent
  ├─> Focuses on CRUD sections only
  ├─> Generates plan for CRUD examples
  └─> Returns plan to: Orchestrator
      ↓
[Continues as Workflow B]
```

**Planning Agent escalates when scope unclear** - Writer provides clarification

**Result**: Focused examples based on writer's clarification

## Cleanup Responsibility Model

### Problem
Tests that modify sample data must clean up to ensure idempotency. Tests run twice must pass both times.

### Solution: Example Agent Writes Cleanup

**Rationale**:
- Example agent knows exactly what database operations it performed
- Test agent only sees return values, not internal operations
- Example agent can write cleanup while operation is fresh in context
- Bluehawk `:remove:` tags hide cleanup from documentation

**Pattern**:
```javascript
// Example agent creates both example and cleanup
export async function runInsertExample() {
  // ... insert operation with test_marker ...
}

// :remove-start:
export async function cleanupInsertExample() {
  // ... delete documents with test_marker ...
}
// :remove-end:
```

```javascript
// Test agent simply calls both
afterEach(async () => {
  await cleanupInsertExample();
});

it('should insert', async () => {
  const result = await runInsertExample();
  Expect.that(result).shouldMatch(expectedOutput);
});
```

**Benefits**:
- ✅ Example agent has full context of operations
- ✅ Test agent has simple, consistent pattern
- ✅ Cleanup hidden from documentation via Bluehawk
- ✅ Easier to maintain (cleanup next to operation)

**Naming Convention**:
- Example function: `run<Operation>Example()`
- Cleanup function: `cleanup<Operation>Example()`
- Must match exactly for test agent to find

**Cleanup Strategies**:
- **Inserts**: Delete documents with `test_marker: 'cleanup_needed'`
- **Updates**: Revert changes using `$unset` or restore original state
- **Deletes**: Snapshot before delete, restore in cleanup
- **Read-only**: No cleanup needed
- **Indexes**: Drop or revert index in cleanup

## Agent Skills Architecture

This system uses **GitHub Copilot Agent Skills** for composable, language-specific knowledge. Skills are automatically loaded based on context, enabling agents to be language-agnostic while accessing specialized knowledge when needed.

### Skill Structure

```
.github/
├── agents/
│   ├── planning-agent.md               # Scope analysis, example suggestions
│   ├── orchestrator-agent.md           # Routes tasks, executes CLI
│   ├── net-new-example-agent.md        # Task logic (language-agnostic)
│   ├── translation-example-agent.md    # Task logic (language-agnostic)
│   ├── migration-example-agent.md      # Task logic (language-agnostic)
│   ├── test-agent.md                   # Task logic (language-agnostic)
│   └── reviewer-agent.md               # Validation logic
│
└── skills/
    ├── universal/
    │   ├── sample-data/
    │   │   └── SKILL.md                # MongoDB sample databases
    │   ├── bluehawk-syntax/
    │   │   └── SKILL.md                # Bluehawk markup
    │   ├── cleanup-patterns/
    │   │   └── SKILL.md                # Cleanup strategies
    │   └── documentation-principles/
    │       └── SKILL.md                # Doc code best practices
    │
    ├── nodejs/
    │   ├── nodejs-example-creation/
    │   │   ├── SKILL.md                # Node.js example patterns
    │   │   └── examples/
    │   │       ├── connection-template.js
    │   │       ├── crud-template.js
    │   │       └── aggregation-template.js
    │   └── nodejs-test-creation/
    │       ├── SKILL.md                # Jest test patterns
    │       └── examples/
    │           └── test-template.test.js
    │
    ├── csharp/
    │   ├── csharp-example-creation/
    │   │   └── SKILL.md                # C# example patterns
    │   └── csharp-test-creation/
    │       └── SKILL.md                # xUnit test patterns
    │
    └── python/
        ├── python-example-creation/
        │   └── SKILL.md                # Python example patterns
        └── python-test-creation/
            └── SKILL.md                # pytest patterns
```

### How Skills Work

**Progressive Disclosure (3-level loading)**:
1. **Discovery**: Copilot knows all available skills via YAML frontmatter (`name` and `description`)
2. **Instructions**: Loads `SKILL.md` body when request matches description
3. **Resources**: Accesses additional files (templates, examples) only as needed

**Automatic Activation**: Skills are loaded automatically based on the agent's task and the writer's request. No manual selection required.

**Example**: "Create a Node.js insert example"
- Agent: `net-new-example-agent` (task logic)
- Auto-loaded skills:
  - `sample-data` (needs database selection)
  - `bluehawk-syntax` (needs markup)
  - `cleanup-patterns` (insert modifies data)
  - `documentation-principles` (writing doc code)
  - `nodejs-example-creation` (Node.js implementation)

### Skill Categories

**Universal Skills** (4 skills):
- `sample-data` - MongoDB sample databases, schemas, selection criteria
- `bluehawk-syntax` - Markup syntax for documentation snippets
- `cleanup-patterns` - Conceptual cleanup strategies (insert/update/delete)
- `documentation-principles` - What makes good documentation code

**Language-Specific Skills** (2 per language):
- `{language}-example-creation` - Connection patterns, idioms, cleanup implementation
- `{language}-test-creation` - Test framework patterns, assertions, utilities

**Total**: 4 universal + (2 × 5 languages) = **14 skills**

vs. 45 agents in a language-specific agent approach

### Maintenance Strategy

**Update universal skill** → All languages benefit
- Example: Update `cleanup-patterns` → affects all example agents

**Update language skill** → All task types benefit
- Example: Update `nodejs-example-creation` → affects net-new, translation, migration agents

**Update agent** → All languages benefit
- Example: Update `net-new-example-agent` → affects Node.js, C#, Python, etc.

**Portability**: Skills work across GitHub Copilot in VS Code, Copilot CLI, and Copilot coding agent

## Workflow Example

**Writer Request**: "Create a Node.js example showing $lookup aggregation"

```
1. Orchestrator Agent
   ├─> Analyzes: "net new example, nodejs, $lookup aggregation"
   └─> Executes CLI: grove-cli list examples javascript/driver aggregation

2. CLI Returns
   └─> Found: aggregation/lookup-join.js (similar example)

3. Orchestrator Agent
   ├─> Presents to writer: "Found aggregation/lookup-join.js. Use this or create new?"
   └─> Writer responds: "Create new with different collections"

4. Orchestrator Agent
   └─> Executes CLI: grove-cli scaffold javascript/driver lookup-example --topic aggregation

5. CLI Creates Scaffold
   ├─> Creates: examples/aggregation/lookup-example.js (with TODOs)
   └─> Returns: { "suite": "javascript/driver", "example_file": "examples/aggregation/lookup-example.js" }

6. Orchestrator Agent
   ├─> Parses: suite from CLI output (determines language: "nodejs")
   └─> Routes to: net-new-example-agent (with file path and test strategy)

7. net-new-example-agent
   ├─> Copilot auto-loads skills:
   │   ├─> sample-data
   │   ├─> bluehawk-syntax
   │   ├─> cleanup-patterns
   │   ├─> documentation-principles
   │   └─> nodejs-example-creation
   ├─> Reads: example scaffold
   ├─> Writes: runLookupExample() implementation
   ├─> Adds: Bluehawk markup for snippets
   ├─> Writes: cleanupLookupExample() (if needed)
   └─> Returns: completed example file

8. Orchestrator Agent
   └─> Routes to: test-agent (with file path and test strategy)

9. test-agent
   ├─> Copilot auto-loads skills:
   │   ├─> sample-data
   │   ├─> cleanup-patterns
   │   └─> nodejs-test-creation
   ├─> Reads: test scaffold
   ├─> Writes: Jest test with describeWithSampleData
   ├─> Adds: afterEach with cleanup call
   └─> Returns: completed test file

10. Orchestrator Agent
    └─> Routes to: reviewer-agent

11. reviewer-agent
    ├─> Runs: npm test (first run)
    ├─> Runs: npm test (second run - idempotency check)
    ├─> Runs: npm run snip (Bluehawk generation)
    ├─> Validates: snippets generated correctly
    └─> Reports: success or issues to Orchestrator

12. Orchestrator Agent
    └─> Presents: summary to writer with next steps
```

## Initial Implementation Scope

**Phase 1: Node.js Foundation**
- Grove CLI (test suite operations: discovery, scaffolding)
- Audit CLI (documentation extraction - existing tool)
- 1 Planning Agent (language-agnostic)
- 1 Orchestrator Agent (language-agnostic)
- 3 Example Agents (net-new, translation, migration) - language-agnostic
- 1 Test Agent (language-agnostic)
- 1 Reviewer Agent (language-agnostic)
- 4 Universal Skills (sample-data, bluehawk-syntax, cleanup-patterns, documentation-principles)
- 2 Node.js Skills (nodejs-example-creation, nodejs-test-creation)

**Total**: 7 agents + 6 skills + 2 CLIs

**Rationale**: Start with one language to validate architecture before scaling. Agents are language-agnostic from the start, so adding new languages only requires adding 2 skills per language.

**Success Criteria**:
- Agent can create net new Node.js example from requirements
- Tests pass on first and second run (idempotent)
- Bluehawk generates correct snippets
- Writer intervention < 20% of cases

**Future Phases**: Add 2 skills per language
- Phase 2: +2 C# skills (csharp-example-creation, csharp-test-creation)
- Phase 3: +2 Python skills (python-example-creation, python-test-creation)
- Phase 4: +2 Go skills (go-example-creation, go-test-creation)
- Phase 5: +2 Java skills (java-example-creation, java-test-creation)

**Final Total**: 6 agents + 14 skills (4 universal + 10 language-specific)

## Evaluation Criteria

### Agent Performance Metrics

**Success Rate**:
- % of examples that compile/run without errors
- % of tests that pass on first run
- % of tests that pass on second run (idempotency)
- % of Bluehawk snippets that generate correctly

**Quality Metrics**:
- Code follows language idioms
- Appropriate sample data selection
- Correct Bluehawk markup
- Proper cleanup implementation
- Test coverage of example functionality

**Efficiency Metrics**:
- Writer intervention required (% of cases)
- Time to complete task
- Number of iterations needed
- Context size vs output quality

**Maintenance Metrics**:
- Shared base module reuse
- Agent instruction clarity
- Common failure patterns
- Improvement velocity

### Quality Gates

**Example Agent Output**:
- [ ] Code compiles/runs without errors
- [ ] Uses environment variables for connection strings
- [ ] Includes appropriate Bluehawk markup
- [ ] Exports cleanup function if data modified
- [ ] Returns value suitable for test validation
- [ ] Follows language-specific best practices

**Test Agent Output**:
- [ ] Imports example and cleanup functions
- [ ] Uses appropriate test wrapper (describeWithSampleData)
- [ ] Calls cleanup in afterEach
- [ ] Validates output using Expect API
- [ ] Passes on first run
- [ ] Passes on second run (idempotent)

**Reviewer Agent Validation**:
- [ ] All tests pass
- [ ] Idempotency verified (two consecutive runs)
- [ ] Bluehawk snippets generated
- [ ] No regressions in existing tests
- [ ] Cleanup functions present for data modifications
- [ ] Code follows project conventions

## Next Steps

See [todo.md](./todo.md) for implementation tasks and priorities.
