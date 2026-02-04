# Implementation TODO

## Phase 1: Foundation & Node.js Skills

### 1. Agent & Skill Development

#### 1.1 Universal Skills
- [ ] Create `.github/skills/universal/sample-data/SKILL.md`
  - YAML frontmatter: name, description
  - Available sample databases (sample_mflix, sample_restaurants, etc.)
  - Collection schemas for each database
  - Selection criteria (which database for which example)
  - Sample data modification guidelines
- [ ] Create `.github/skills/universal/bluehawk-syntax/SKILL.md`
  - YAML frontmatter: name, description
  - `:snippet-start:` / `:snippet-end:`
  - `:remove-start:` / `:remove-end:`
  - `:replace-start:` / `:replace-end:`
  - `:uncomment-start:` / `:uncomment-end:`
  - Tag nesting rules
  - Common patterns and examples
- [ ] Create `.github/skills/universal/cleanup-patterns/SKILL.md`
  - YAML frontmatter: name, description
  - Conceptual cleanup strategies (universal)
  - Insert cleanup (delete with markers)
  - Update cleanup (revert changes)
  - Delete cleanup (restore deleted docs)
  - Read-only operations (no cleanup)
  - Naming conventions (run* → cleanup*)
  - Bluehawk wrapping (`:remove:` tags)
- [ ] Create `.github/skills/universal/documentation-principles/SKILL.md`
  - YAML frontmatter: name, description
  - Maintainability guidelines
  - DRY principles
  - Code comment philosophy
  - Writer-friendly API design
  - Documentation validation vs development testing
  - What to test (runs without errors, basic structure)
  - What NOT to test (edge cases, error handling, performance)
  - Minimal validation principle

#### 1.2 Node.js Skills
- [ ] Create `.github/skills/nodejs/nodejs-example-creation/SKILL.md`
  - YAML frontmatter: name, description
  - MongoClient connection patterns
  - Async/await best practices
  - Database and collection access
  - Common operations (find, insert, update, delete, aggregate)
  - Error handling patterns
  - Resource cleanup (client.close())
  - Cleanup implementation (language-specific)
  - Return value design
  - File structure conventions
  - Links to Node.js driver documentation (for translation reference)
- [ ] Create `.github/skills/nodejs/nodejs-example-creation/examples/`
  - example-template.js (single template with TODO comments for agents to fill in)
- [ ] Create `.github/skills/nodejs/nodejs-test-creation/SKILL.md`
  - YAML frontmatter: name, description
  - Jest test structure
  - describeWithSampleData usage
  - itWithSampleData usage
  - afterEach cleanup patterns
  - Expect API patterns
  - Test organization
  - Output validation patterns
  - Idempotency considerations
- [ ] Create `.github/skills/nodejs/nodejs-test-creation/examples/`
  - test-template.test.js

#### 1.3 Repository-Wide Instructions
- [ ] Create `.github/copilot-instructions.md`
  - **Purpose**: Activation logic (works from any file location)
  - **Grove agent namespace**: `@grove-` prefix
  - **Activation keywords**: "code example" + action verb (create/add/test/migrate/update)
  - **High-confidence activation**: "code example" + action verb
  - **Medium-confidence activation**: "example" alone (ask for clarification)
  - **Decline pattern**: Non-code-example requests
  - **Scope**: code-example-tests/ directory only
  - Reference to @grove-planning-agent for vague requests
  - Reference to @grove-orchestrator for specific requests

#### 1.4 Path-Scoped Custom Instructions
- [ ] Create `.github/instructions/code-examples.instructions.md`
  - **Purpose**: Implementation context (when editing files in code-example-tests/)
  - **applyTo section**: `code-example-tests/**`
  - **Language-specific patterns**: Node.js, C#, Python, Go, Java
  - **Required elements**: Example function, cleanup function, Bluehawk markup, return values
  - **Quality standards**: Idempotency, sample data usage, error handling
  - Note: This provides context when editing files, NOT activation logic

#### 1.5 Language-Agnostic Agents
- [ ] Create `.github/agents/grove-planning-agent.md`
  - **Agent name**: `@grove-planning-agent`
  - **Scope declaration** (code-example-tests/ only)
  - **Activation logic**: "code example" + action verb OR explicit mention
  - **Decline pattern** (non-code-example requests with helpful message)
  - Documentation page structure analysis
  - Section identification heuristics
  - Operation type inference rules
  - Example naming conventions
  - MongoDB operation catalog
  - CLI discovery command execution
  - Deduplication logic (check existing examples)
  - Plan generation (structured JSON output)
  - Clarification request generation (when scope unclear)
  - Escalation to writer patterns
- [ ] Create `.github/agents/grove-orchestrator.md`
  - **Agent name**: `@grove-orchestrator`
  - **Scope declaration** (code-example-tests/ only)
  - **Activation logic**: "code example" + action verb OR explicit mention
  - **Decline pattern** (non-code-example requests with helpful message)
  - Scope detection (specific vs vague)
  - @grove-planning-agent routing (when to use)
  - CLI scaffolding command execution patterns
  - CLI output parsing
  - Routing decision tree (language-agnostic)
  - Agent capability matrix
  - Workflow coordination
  - Fix loop coordination (max 3 iterations)
  - Error handling and escalation
  - Result presentation to writers
  - Plan approval workflow
- [ ] Create `.github/agents/grove-net-new-example-agent.md`
  - **Agent name**: `@grove-net-new-example-agent`
  - **Activation**: Invoked by @grove-orchestrator only (not directly by writers)
  - Universal task logic (language-agnostic)
  - MongoDB operation selection criteria
  - Sample data selection logic (uses sample-data skill)
  - Bluehawk snippet design (uses bluehawk-syntax skill)
  - Cleanup function creation (uses cleanup-patterns + language skill)
  - Return value design (uses language skill)
  - Note: Language-specific implementation from auto-loaded skills
- [ ] Create `.github/agents/grove-translation-example-agent.md`
  - **Agent name**: `@grove-translation-example-agent`
  - **Activation**: Invoked by @grove-orchestrator only (not directly by writers)
  - Universal task logic (language-agnostic)
  - Reference example analysis
  - API translation approach (refer to driver documentation for equivalent APIs)
  - Structure preservation
  - Cleanup matching (uses cleanup-patterns + language skill)
  - Note: Language-specific implementation from auto-loaded skills
- [ ] Create `.github/agents/grove-migration-example-agent.md`
  - **Agent name**: `@grove-migration-example-agent`
  - **Activation**: Invoked by @grove-orchestrator only (not directly by writers)
  - Universal task logic (language-agnostic)
  - Anti-pattern detection (agent-specific, not in skills)
  - Refactoring strategies
  - Testable structure extraction
  - Cleanup addition (uses cleanup-patterns + language skill)
  - **Discrepancy documentation** (structured metadata)
    - Document each discrepancy as it's made
    - Explain *why* each change was necessary
    - Flag areas requiring writer review
    - Return metadata to @grove-orchestrator
  - Note: Language-specific implementation from auto-loaded skills
- [ ] Create `.github/agents/grove-test-agent.md`
  - **Agent name**: `@grove-test-agent`
  - **Activation**: Invoked by @grove-orchestrator only (not directly by writers)
  - Universal task logic (language-agnostic)
  - Test file structure (uses language skill)
  - Sample data wrapper usage (uses language skill)
  - Cleanup function invocation (uses cleanup-patterns + language skill)
  - Output validation patterns (uses language skill)
  - Idempotency considerations
  - Note: Language-specific implementation from auto-loaded skills
- [ ] Create `.github/agents/grove-reviewer-agent.md`
  - **Agent name**: `@grove-reviewer-agent`
  - **Activation**: Invoked by @grove-orchestrator only (not directly by writers)
  - Quality checklist
  - Test execution commands (embedded table: npm test, dotnet test, pytest, etc.)
  - Idempotency verification (run tests twice)
  - Bluehawk validation (npm run snip)
  - Regression detection
  - Common issues and fixes
  - Escalation criteria
  - **Migration-specific validation**:
    - Validate discrepancies are documented in metadata
    - Do NOT diagnose undocumented discrepancies (escalate to Orchestrator)
    - Pass metadata + validation results to Orchestrator

### 2. CLI Tools for Grove Agents

**Context**: Grove agents need two types of CLI operations:
1. **Test suite operations** (discovery, scaffolding in `code-example-tests/`)
2. **Documentation extraction** (extracting code from RST source files)

**Solution**: Use two CLIs for different purposes:
- **`grove-cli`** (NEW): Test suite discovery and scaffolding
- **`audit-cli`** (EXISTING): Documentation content extraction

#### 2.0 Grove CLI Implementation

**Purpose**: Scoped operations within `code-example-tests/` directory

**Location**: `code-example-tests/grove-meta/cli/`

**Why separate from audit-cli?**
- Different domain: test suite structure vs. documentation files
- Different users: agents (automated) vs. writers (manual)
- Simpler scope: only `code-example-tests/` directory
- Independent evolution: won't affect existing audit workflows

**Implementation**:
- [ ] Create `code-example-tests/grove-meta/cli/` directory
- [ ] Initialize Node.js project (`package.json`)
- [ ] Create CLI entry point (`src/index.js`)
- [ ] Add working directory validation (all commands)
  - Check current directory is within `code-example-tests/`
  - Error message: "grove-cli must be run from code-example-tests/ directory"
  - Exit with code 1 if validation fails

#### 2.1 Grove CLI: Directory Traversal Commands (Phase 1)

**Used by**: Planning Agent (for discovery and deduplication)

- [ ] `grove-cli list suites`
  - List available test suites (e.g., javascript/driver, python/pymongo, csharp/driver)
  - Return suite paths and basic info (language, framework)
  - Example output:
    ```json
    {
      "suites": [
        {"path": "javascript/driver", "language": "javascript", "framework": "jest"},
        {"path": "python/pymongo", "language": "python", "framework": "pytest"},
        {"path": "csharp/driver", "language": "csharp", "framework": "xunit"}
      ]
    }
    ```
- [ ] `grove-cli list topics <suite>`
  - List directory tree structure under `<suite>/examples/`
  - Show nested directories (e.g., aggregation/pipelines/group/)
  - Return tree structure for progressive disclosure
  - Example: `grove-cli list topics javascript/driver`
  - Example output:
    ```json
    {
      "suite": "javascript/driver",
      "topics": [
        "aggregation/pipelines/filter/",
        "aggregation/pipelines/group/",
        "aggregation/pipelines/join-multi-field/",
        "crud/read/",
        "get-started/",
        "time-series/"
      ]
    }
    ```
- [ ] `grove-cli list examples <suite> <topic>`
  - List files in a specific topic directory
  - Return file paths relative to suite
  - Example: `grove-cli list examples javascript/driver aggregation/pipelines/group`
  - Example output:
    ```json
    {
      "suite": "javascript/driver",
      "topic": "aggregation/pipelines/group",
      "examples": [
        "tutorial.js",
        "tutorial-setup.js",
        "tutorial-output.sh"
      ]
    }
    ```
- [ ] `grove-cli read <suite> <example-path>`
  - Read specific example file (scoped to code-example-tests, not entire monorepo)
  - Return file content
  - Parse existing Bluehawk markup (if present)
  - Example: `grove-cli read javascript/driver aggregation/pipelines/group/tutorial.js`
  - Example output:
    ```json
    {
      "suite": "javascript/driver",
      "path": "aggregation/pipelines/group/tutorial.js",
      "content": "...",
      "bluehawk_snippets": ["connection", "pipeline", "results"]
    }
    ```

- [ ] `grove-cli list tests <suite> <topic>`
  - List existing test files in a topic directory
  - Help Planning Agent decide test file strategy
  - Example: `grove-cli list tests javascript/driver crud`
  - Example output:
    ```json
    {
      "suite": "javascript/driver",
      "topic": "crud",
      "test_files": [
        {
          "path": "tests/crud/read/match-embedded-document.test.js",
          "examples_tested": ["crud/read/match-embedded-document.js"]
        }
      ]
    }
    ```

#### 2.2 Grove CLI: Search Commands (Phase 2 - Deferred)

**Note**: Deferred until metadata schema is defined or content indexing is implemented.

- [ ] Define metadata schema for examples (operation types, stages, topics)
- [ ] Implement metadata extraction or manual tagging
- [ ] `grove-cli search <suite> [--operation <op>] [--stage <stage>]`
  - Search for examples matching criteria
  - Support operation types: find, insert, update, delete, aggregate
  - Support aggregation stages: lookup, group, match, etc.
  - Return matching examples with relevance scores

#### 2.3 Grove CLI: Scaffolding Commands (Phase 1)

**Used by**: Orchestrator (after plan approval)

**Design Decision**: Scaffold command **only creates example file**, not test file.

**Rationale**:
- Test organization varies by suite (one test file per example vs. multiple examples per test file)
- Test file naming isn't standardized across suites
- Can't deterministically decide whether to create new test file or add to existing file
- Planning Agent should analyze existing test files and decide test strategy
- Test Agent should create or modify test files based on Planning Agent's decision

- [ ] `grove-cli scaffold <suite> <example-name> [--topic <topic>]`
  - **Only creates example file scaffold** (not test file)
  - Determine file path based on suite conventions and topic
  - Generate example file scaffold with:
    - Import statements (language-specific)
    - Function signature (run<Name>Example)
    - TODO comments for example agent
    - Cleanup function signature (TODO for agent to implement)
  - Return JSON with file paths
  - Example: `grove-cli scaffold javascript/driver insert-one-basic --topic crud/insert`
  - Example output:
    ```json
    {
      "suite": "javascript/driver",
      "example_file": "examples/crud/insert/insert-one-basic.js"
    }
    ```

#### 2.4 Audit CLI: Documentation Extraction (Existing Tool)

**Purpose**: Extract code examples from documentation source files (RST)

**Location**: Separate repository (`/Users/dachary.carey/workspace/audit-cli`)

**Used by**: Translation Agent, Migration Agent, Reviewer Agent (for migration tasks)

**Why use audit-cli for this?**
- Already understands RST directives and includes
- Handles transcluded content (follows includes)
- Can recursively scan directories
- Extracts code examples as clean files (no RST markup)

**Usage**:
- [ ] Translation Agent: Extract mongosh examples from docs page for translation
  - `audit-cli extract code-examples <page-path> --output /tmp/extracted/`
  - Translate extracted files to driver code
  - Create corresponding testable examples
- [ ] Migration Agent: Extract untested code from docs for migration
  - `audit-cli extract code-examples <page-path> --output /tmp/extracted/`
  - Migrate extracted files to testable structure
  - Document discrepancies between original and migrated
- [ ] Reviewer Agent: Extract original for comparison
  - `audit-cli extract code-examples <page-path> --output /tmp/extracted/`
  - Compare extracted original with migrated example
  - Validate semantic equivalence

**Note**: Net-New Example Agent and Test Agent do NOT use audit-cli (they work with test suite files only).

#### 2.5 Scaffold Templates

**Design Decision**: Single template per language for Phase 1. Operation-specific templates deferred to future phases.

**Rationale**:
- Simpler to implement and maintain
- AI agents can adapt generic template to specific operations
- Can add operation-specific templates later if needed based on actual usage patterns

- [ ] Create language-specific templates (one per language)
  - **Node.js template** - Example structure:
    ```javascript
    // :replace-start: {
    //   "terms": {
    //     "const result = ": ""
    //   }
    // }

    import { MongoClient } from 'mongodb';

    const uri = process.env.CONNECTION_STRING;
    const client = new MongoClient(uri);

    // TODO: Agent fills in example function
    export async function run<Name>Example() {
      try {
        const database = client.db('TODO: database name');
        const collection = database.collection('TODO: collection name');

        // :snippet-start: main-operation
        // TODO: Agent implements main operation here
        // :snippet-end:

        // TODO: Return value for test validation
        return result;
      } finally {
        await client.close();
      }
    }

    // TODO: Agent implements cleanup if data is modified
    // export async function cleanup<Name>Example() {
    //   // TODO: Cleanup implementation
    // }

    // :replace-end:
    ```
  - **C# template** - Similar structure with C# patterns (async Task, using statements, IDisposable)
  - **Python template** - Similar structure with Python patterns (context managers, pymongo)

#### 2.6 Grove CLI: Validation
- [ ] Validate file paths don't conflict with existing files
- [ ] Validate naming conventions
- [ ] Validate suite exists and is valid

### 3. Agent Integration & Testing

#### 3.1 Orchestrator Setup
- [ ] Define CLI command execution patterns
- [ ] Define CLI output parsing logic
- [ ] Define agent routing logic based on task type
- [ ] Create task analysis patterns
- [ ] Implement handoff protocols (passing metadata to agents)
- [ ] Add error handling for CLI failures
- [ ] Add error handling for agent failures
- [ ] Create result aggregation

#### 3.2 Grove CLI Testing (Automated Unit Tests)

**Location**: `code-example-tests/grove-meta/cli/__tests__/`

**Approach**: Traditional unit tests for the CLI tool itself

**Test Framework**: Jest (Node.js)

**What We Test**:
- ✅ CLI commands execute successfully
- ✅ JSON output has correct structure
- ✅ File paths are valid
- ✅ Discovery commands return expected data
- ✅ Scaffold command creates files with correct structure
- ✅ Error handling works correctly

**Implementation**:
- [ ] Create `grove-meta/cli/__tests__/` directory
- [ ] Test `list suites` command
  - Returns valid JSON
  - Includes all expected suites
  - Suite objects have required fields (path, language, framework)
- [ ] Test `list topics` command
  - Returns valid JSON
  - Lists directories correctly
  - Handles nested directories
- [ ] Test `list examples` command
  - Returns valid JSON
  - Lists files in topic directory
  - Filters non-example files
- [ ] Test `list tests` command
  - Returns valid JSON
  - Lists test files correctly
  - Parses test file metadata
- [ ] Test `read` command
  - Returns file content
  - Parses Bluehawk markers
  - Handles missing files gracefully
- [ ] Test `scaffold` command
  - Creates example file
  - File has correct template structure
  - Returns correct file path
  - Validates suite exists
  - Prevents overwriting existing files

#### 3.3 Agent Testing (Manual + Automated Validation)

**Location**: `code-example-tests/grove-meta/tests/validation/`

**Approach**: Manual agent invocation + automated validation of outputs

**How It Works**:
1. **Manual Step**: Developer invokes agent through normal GitHub Copilot Chat
2. **Automated Step**: Run validation script on agent's output files
3. **Result**: Pass/fail report with specific issues

**Test Framework**: Jest (Node.js) for validation scripts

**Validation Scripts Test**:
- ✅ File creation and structure
- ✅ Syntax validity (code compiles/parses)
- ✅ Test execution (tests pass/fail)
- ✅ Idempotency (tests pass twice in a row)
- ✅ Required elements present (Bluehawk markers, cleanup functions, etc.)
- ✅ Naming conventions followed
- ✅ File paths correct

**Implementation**:
- [ ] Create `grove-meta/tests/validation/` directory structure
  - `validators/` - Validation functions
  - `test-cases/` - Manual test case documentation
  - `package.json` - Test dependencies

- [ ] Create validation utilities (`validation/validators/`)
  - `syntax-validator.js` - Check if code compiles/parses
  - `structure-validator.js` - Check file structure (imports, functions, etc.)
  - `bluehawk-validator.js` - Check Bluehawk markers present and valid
  - `test-runner.js` - Run tests and capture results
  - `idempotency-checker.js` - Run tests twice and compare
  - `naming-validator.js` - Check naming conventions

- [ ] Create validation test suite (`validation/validate-example.test.js`)
  - Takes example file path as input
  - Discovers test file that imports the example
  - Parses imported function names from example file
  - Finds and runs test(s) that use those functions
  - Runs tests twice to check idempotency
  - Generates pass/fail report
  - Example usage:
    ```bash
    npm test -- --example=javascript/driver/examples/crud/insert/insert-one.js
    ```
  - Discovery process:
    1. Search suite's `tests/` directory for files importing the example
    2. Parse import statement to extract function names (e.g., `runInsertOneExample`, `cleanupInsertOneExample`)
    3. Find test blocks that reference those function names
    4. Run specific test using framework-specific filtering:
       - Jest: `npm test -- --testNamePattern="test name"`
       - pytest: `pytest -k "test_name"`
       - NUnit: `dotnet test --filter "FullyQualifiedName~TestName"`
       - Go: `go test -run TestName/SubtestName`
    5. Run same test again to verify idempotency
  - Fallback: If specific test isolation fails, run entire test file twice

- [ ] Create manual test case documentation (`validation/test-cases/`)
  - `planning-agent.md` - How to test Planning Agent manually
  - `net-new-example-agent.md` - How to test Net-New Example Agent manually
  - `translation-agent.md` - How to test Translation Agent manually
  - `migration-agent.md` - How to test Migration Agent manually
  - `test-agent.md` - How to test Test Agent manually
  - `reviewer-agent.md` - How to test Reviewer Agent manually

**Example Manual Test Case** (`net-new-example-agent.md`):
```markdown
# Testing Net-New Example Agent

## Setup
1. Ensure grove-cli is installed
2. Ensure MongoDB connection string is set
3. Navigate to code-example-tests/

## Test Case 1: Simple Insert Example

### Manual Steps
1. Open GitHub Copilot Chat
2. Invoke: "@grove-orchestrator Create a Node.js insertOne example"
3. Review and approve the plan
4. Wait for agent to complete
5. Note the file path created (e.g., `javascript/driver/examples/crud/insert/insert-one-basic.js`)

### Automated Validation
```bash
# From grove-meta/tests/validation/
npm test -- --example=javascript/driver/examples/crud/insert/insert-one-basic.js
```

**What the validation script does:**
1. **Syntax validation**: Parses the example file to check for syntax errors
2. **Structure validation**: Checks for required imports, functions, Bluehawk markers
3. **Test discovery**:
   - Searches `javascript/driver/tests/` for files importing `insert-one-basic.js`
   - Parses import to find function names (e.g., `runInsertOneBasicExample`, `cleanupInsertOneBasicExample`)
   - Finds test that calls those functions
4. **Test execution**: Runs the discovered test using Jest
5. **Idempotency check**: Runs the same test again to verify cleanup works

### Expected Results
- ✅ File exists at expected path
- ✅ Syntax is valid (no parse errors)
- ✅ Has required imports (MongoClient, etc.)
- ✅ Has runInsertOneBasicExample function
- ✅ Has cleanupInsertOneBasicExample function
- ✅ Has Bluehawk :snippet-start: and :snippet-end: markers
- ✅ Has Bluehawk :replace-start: and :replace-end: markers for connection string
- ✅ Test file exists and imports the example
- ✅ Test passes on first run
- ✅ Test passes on second run (idempotency verified)

### Manual Review (not automated)
- Code is readable and follows best practices
- Uses appropriate sample database
- Cleanup logic is correct and complete
- Example is pedagogically sound
- Comments are helpful and accurate

#### 3.3 End-to-End Workflow Testing (Manual Acceptance Tests)

**Location**: `code-example-tests/grove-meta/tests/e2e/`

**Approach**: Manual testing with documented acceptance criteria

**Why Manual?**
- Workflow involves human interaction (plan approval, clarifications)
- Quality assessment requires human judgment
- Success criteria include usability and writer experience
- Can be partially automated later if patterns emerge

**Test Documentation Format**:
Each workflow test should document:
1. **Setup**: Initial state and prerequisites
2. **Action**: What the writer requests
3. **Expected Agent Behavior**: What agents should do
4. **Acceptance Criteria**: Checklist of requirements
5. **Success Metrics**: Time, iterations, intervention needed

**Implementation**:
- [ ] Create workflow test documentation (`grove-meta/tests/e2e/README.md`)
  - Test execution instructions
  - Acceptance criteria checklists
  - Success metrics tracking

- [ ] Test Workflow A: Simple Request (Single Example)
  - **Action**: Writer: "Create a Node.js insertOne example"
  - **Expected Behavior**:
    - Orchestrator routes to Planning Agent
    - Planning Agent checks for duplicates (grove-cli list examples)
    - Planning Agent decides test strategy (grove-cli list tests)
    - Planning Agent returns single-example plan
    - Orchestrator presents plan to writer
    - Writer approves
    - Orchestrator executes grove-cli scaffold
    - Orchestrator routes to Net-New Example Agent → Test Agent → Reviewer Agent
  - **Acceptance Criteria**:
    - [ ] Example file created with correct structure
    - [ ] Test file created (new or added to existing based on strategy)
    - [ ] Tests pass on first run
    - [ ] Tests pass on second run (idempotency)
    - [ ] Bluehawk generates valid snippets
    - [ ] Code is readable and follows best practices
    - [ ] Total time < 3 minutes
    - [ ] Writer intervention < 20% of cases

- [ ] Test Workflow B: Page-Level Request (Multiple Examples)
  - **Action**: Writer: "Create examples for this page: [URL]"
  - **Expected Behavior**:
    - Orchestrator routes to Planning Agent
    - Planning Agent reads page content
    - Planning Agent identifies sections needing examples
    - Planning Agent calls grove-cli discovery for each
    - Planning Agent decides test strategies
    - Planning Agent returns multi-example plan
    - Orchestrator presents plan to writer
    - Writer approves
    - Orchestrator executes grove-cli scaffold for each
    - Orchestrator routes each to appropriate agents
  - **Acceptance Criteria**:
    - [ ] All examples created
    - [ ] All tests pass (first and second run)
    - [ ] Test file strategy prevents explosion
    - [ ] No duplicate examples created
    - [ ] Total time < 10 minutes for 3-5 examples
    - [ ] Writer intervention < 30% of cases

- [ ] Test Workflow C: Translation Request
  - **Action**: Writer: "Translate the mongosh examples from this page to Node.js"
  - **Expected Behavior**:
    - Orchestrator routes to Planning Agent
    - Planning Agent uses audit-cli to extract mongosh examples
    - Planning Agent generates translation plan
    - Orchestrator routes to Translation Agent
    - Translation Agent translates each example
    - Test Agent creates tests
    - Reviewer Agent validates
  - **Acceptance Criteria**:
    - [ ] Translated examples preserve logic
    - [ ] API translations correct (mongosh → driver)
    - [ ] Tests pass (first and second run)
    - [ ] Code follows driver best practices

- [ ] Test Workflow D: Migration Request
  - **Action**: Writer: "Migrate the untested examples from this page to testable format"
  - **Expected Behavior**:
    - Orchestrator routes to Planning Agent
    - Planning Agent uses audit-cli to extract examples
    - Planning Agent generates migration plan
    - Orchestrator routes to Migration Agent
    - Migration Agent migrates and documents discrepancies
    - Test Agent creates tests
    - Reviewer Agent validates and confirms discrepancies documented
  - **Acceptance Criteria**:
    - [ ] Migrated examples are testable
    - [ ] Tests pass (first and second run)
    - [ ] Discrepancies documented with reasons
    - [ ] Writer action items clear
    - [ ] Semantic equivalence maintained (or documented)

- [ ] Test Workflow E: Unclear Scope (Escalation)
  - **Action**: Writer: "Create examples for this page: [complex page URL]"
  - **Expected Behavior**:
    - Orchestrator routes to Planning Agent
    - Planning Agent cannot determine scope
    - Planning Agent generates clarification request
    - Orchestrator presents clarification to writer
    - Writer provides clarification
    - Planning Agent generates focused plan
  - **Acceptance Criteria**:
    - [ ] Clarification request is clear and specific
    - [ ] Plan matches writer's clarification
    - [ ] No unnecessary examples created

- [ ] Measure and track success metrics
  - Success rate (% working examples on first try)
  - Idempotency rate (% passing twice without fixes)
  - Writer intervention rate (% requiring manual fixes)
  - Time to completion (average per example)
  - Test file explosion prevention (avg examples per test file)

### 4. Documentation & Guidelines

- [ ] Create writer-facing documentation
  - How to request new examples
  - How to interpret agent output
  - When to intervene manually
  - Common issues and fixes
- [ ] Create agent maintenance guide
  - How to update universal skills
  - How to add new language skills
  - How to update language-agnostic agents
  - How to measure agent performance
  - How to debug agent failures

## Phase 2: C# Skills

**Note**: Agents are already language-agnostic. Only need to add C# skills.

### 2.1 C# Example Creation Skill
- [ ] Create `.github/skills/csharp/csharp-example-creation/SKILL.md`
  - YAML frontmatter: name, description
  - MongoClient connection patterns (C# driver)
  - Async/await best practices (Task-based)
  - Database and collection access
  - Common operations (Find, InsertOne, UpdateOne, DeleteOne, Aggregate)
  - Error handling patterns (try-catch)
  - Resource cleanup (using statements, IDisposable)
  - Cleanup implementation (C#-specific)
  - Return value design
  - File structure conventions
  - Links to C# driver documentation (for translation reference)
- [ ] Create `.github/skills/csharp/csharp-example-creation/examples/`
  - ExampleTemplate.cs (single template with TODO comments for agents to fill in)

### 2.2 C# Test Creation Skill
- [ ] Create `.github/skills/csharp/csharp-test-creation/SKILL.md`
  - YAML frontmatter: name, description
  - xUnit test structure
  - Sample data fixture patterns
  - IDisposable cleanup patterns
  - Assert patterns
  - Test organization
  - Output validation patterns
  - Idempotency considerations
- [ ] Create `.github/skills/csharp/csharp-test-creation/examples/`
  - test-template.cs

### 2.3 Testing
- [ ] Test agents with C# skills
  - Test net-new-example-agent creates C# examples
  - Test translation-example-agent translates to C#
  - Test migration-example-agent migrates C# code
  - Test test-agent creates xUnit tests
  - Test reviewer-agent runs dotnet test
  - Verify csharp-example-creation skill loaded correctly
  - Verify csharp-test-creation skill loaded correctly

## Phase 3: Python Skills

**Note**: Agents are already language-agnostic. Only need to add Python skills.

### 3.1 Python Example Creation Skill
- [ ] Create `.github/skills/python/python-example-creation/SKILL.md`
  - YAML frontmatter: name, description
  - MongoClient connection patterns (PyMongo)
  - Context manager patterns (with statements)
  - Database and collection access
  - Common operations (find, insert_one, update_one, delete_one, aggregate)
  - Error handling patterns (try-except)
  - Resource cleanup (context managers)
  - Cleanup implementation (Python-specific)
  - Return value design
  - File structure conventions
  - Links to PyMongo documentation (for translation reference)
- [ ] Create `.github/skills/python/python-example-creation/examples/`
  - example_template.py (single template with TODO comments for agents to fill in)

### 3.2 Python Test Creation Skill
- [ ] Create `.github/skills/python/python-test-creation/SKILL.md`
  - YAML frontmatter: name, description
  - pytest test structure
  - Fixture patterns
  - Cleanup patterns (teardown, fixtures)
  - Assert patterns
  - Test organization
  - Output validation patterns
  - Idempotency considerations
- [ ] Create `.github/skills/python/python-test-creation/examples/`
  - test_template.py

### 3.3 Testing
- [ ] Test agents with Python skills
  - Test net-new-example-agent creates Python examples
  - Test translation-example-agent translates to Python
  - Test migration-example-agent migrates Python code
  - Test test-agent creates pytest tests
  - Test reviewer-agent runs pytest
  - Verify python-example-creation skill loaded correctly
  - Verify python-test-creation skill loaded correctly

## Phase 4: Go & Java Skills

**Note**: Agents are already language-agnostic. Only need to add Go and Java skills.

### 4.1 Go Skills
- [ ] Create `.github/skills/go/go-example-creation/SKILL.md`
  - Connection patterns (mongo-go-driver)
  - Context patterns
  - Error handling (multiple return values)
  - Cleanup implementation (defer)
  - Links to Go driver documentation (for translation reference)
- [ ] Create `.github/skills/go/go-example-creation/examples/`
  - example_template.go (single template with TODO comments for agents to fill in)
- [ ] Create `.github/skills/go/go-test-creation/SKILL.md`
  - testing package patterns
  - Cleanup patterns (defer, t.Cleanup)
  - Assertion patterns
- [ ] Create `.github/skills/go/go-test-creation/examples/`
  - test_template_test.go

### 4.2 Java Skills
- [ ] Create `.github/skills/java/java-example-creation/SKILL.md`
  - Connection patterns (driver-sync, driver-reactive)
  - Try-with-resources patterns
  - Error handling
  - Cleanup implementation
  - Links to Java driver documentation (for translation reference)
- [ ] Create `.github/skills/java/java-example-creation/examples/`
  - ExampleTemplate.java (single template with TODO comments for agents to fill in)
- [ ] Create `.github/skills/java/java-test-creation/SKILL.md`
  - JUnit patterns
  - Cleanup patterns (@AfterEach)
  - Assertion patterns
- [ ] Create `.github/skills/java/java-test-creation/examples/`
  - ExampleTemplateTest.java

## Notes

- Start with nodejs-net-new-example-agent as proof of concept
- Validate architecture before scaling to other languages
- Prioritize idempotency and cleanup correctness
- Keep writer intervention as escape hatch
- Measure everything to enable data-driven improvements
