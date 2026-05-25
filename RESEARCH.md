# Architectural Blueprint for a Language-Aligned, Agentic Kanban Platform

The emergence of agentic software development has exposed a significant gap between traditional project management metaphors and the operational requirements of AI-native workflows. Classic Kanban structures focus on human-centric status tracking, leaving the semantic alignment, boundary stress-testing, and rigorous execution loops of autonomous coding completely unaddressed. To resolve these limitations, this document establishes a comprehensive technical blueprint for an agentic Kanban platform designed to natively execute the language-aligned, test-driven engineering paradigm established in Matt Pocock's skills framework. By taking inspiration from, and directly forking, prominent open-source ecosystems such as Cline Kanban and KanVibe, software engineering teams can build a robust, production-grade terminal-integrated interface without duplicating foundational orchestration labor.

## Paradigm Shift: Language-Aligned Engineering vs. Traditional Task Boards

Traditional board structures organize tasks into simple columns representing flat states of activity, typically progressing from a generic backlog to active development, followed by code review and completion. This linear progression fails to address the unique failure modes of AI-driven coding, notably context drift, misalignment of intent, conversational verbosity, and the rapid accumulation of software entropy. The skills framework addresses these structural vulnerabilities by enforcing a process of progressive disclosure, rigorous interrogation, and systematic specification prior to any code generation.

At the center of this methodology is the concept of a ubiquitous, domain-specific language documented within a repository file. Establishing a common vocabulary compresses conversations between engineers and autonomous agents, directly reducing token utilization, minimizing thinking latency, and enforcing naming consistency across variable declarations, function signatures, and file paths. Instead of initiating development with speculative code, the Pocock workflow mandates a strict separation between planning, vertical slicing, and execution.

To evaluate how these foundational concepts transform the visual and structural requirements of an engineering workspace, one must assess the broader landscape of agent-integrated project boards and frameworks. Projects such as Routa have introduced workspace-first multi-agent coordination environments that utilize a shared specification model to enforce consistent execution contracts across independent agent deployments. Similarly, the pioneering Vibe Kanban project by BloopAI sought to wrap a visual drag-and-drop board around Claude Code and other terminal CLI agents. Although Vibe Kanban has sunsetted, its architectural lineage is preserved and extended in community forks like KanVibe. KanVibe implements a keyboard-first, terminal-centric environment utilizing tmux or zellij multiplexers, React, Next.js, and an embedded SQLite database to orchestrate local worktree workspaces. This contrasts with the parallel, Git-worktree-driven design of Cline Kanban, which serves as a local webserver running CLI agents in parallel, capturing status updates through direct terminal hooks and tRPC endpoints.

| Platform | Tech Stack | Storage Architecture | Execution Context | Custom Workflow Adaptability |
|---|---|---|---|---|
| Cline Kanban | React, Vite, Node-PTY, tRPC | Memory-buffered Local State | Ephemeral Git Worktrees | Moderate; built around CLI execution hooks |
| Vibe Kanban | React, Electron, Node.js | File-based Workspace Local DB | Docker & Local Multiplexed Shells | High; supports multiple independent sessions |
| KanVibe | Next.js 16, SQLite, TypeORM, node-pty | Embedded SQLite Database | tmux / zellij terminal tabs | High; keyboard-first dock shortcuts |
| Routa | Multi-Agent Workspace Platform | Shared Declarative Spec Sheets | Containerized Task Workspaces | High; optimized for multi-agent coordination |
| Kanban MCP | Node.js, Model Context Protocol | Embedded SQLite Database | Client-dependent execution shells | Very High; programmatic card transitions |

## The Five-Stage Pocock Skill Lifecycle as a Core State Machine

An agentic Kanban board designed for this specific workflow must move beyond tracking simple completion states to visually represent and enforce the maturation of a feature. The board columns serve as programmatic gates that execute distinct agentic skills as a task moves across the interface. The configuration of these states relies on a localized, repository-level setup that binds the board to the project's underlying issue tracker, custom labels, and domain document paths.

```
+------------------+     +------------------+     +------------------+     +------------------+     +------------------+
|     Stage 1:     |     |     Stage 2:     |     |     Stage 3:     |     |     Stage 4:     |     |     Stage 5:     |
|    Scaffold      | --> |   Interrogate    | --> |    Synthesize    | --> |     Slice        | --> |    TDD Loop      |
| (setup-skills)   |     |  (grill-docs)    |     |     (to-prd)     |     |   (to-issues)    |     |  (tdd/diagnose)  |
+------------------+     +------------------+     +------------------+     +------------------+     +------------------+
```

### Stage 1: Local Scaffolding and Environmental Initialization

The initial stage of the workspace is governed by the setup-matt-pocock-skills command. Before any planning or code changes occur, the platform executes a repository-level scan to detect the issue tracker convention, triage label vocabulary, and context file layout. The board visualizes this phase under a configuration state, prompting the user to confirm whether issues are tracked via GitHub, GitLab, local markdown files, or external interfaces. It establishes a strict five-role triage state machine consisting of needs-triage, needs-info, ready-for-agent, ready-for-human, and wontfix to organize the backlog efficiently.

### Stage 2: Interrogation and Ubiquitous Domain Grounding

Once a card is selected from the backlog, it moves to the grilling stage. This phase is driven by the /grill-with-docs skill, which prevents developer-agent misalignment by executing an active interrogation. Rather than generating code immediately, the agent interrogates the operator on edge cases, interface designs, and architectural dependencies, proposing recommended paths one question at a time.

During this interactive phase, the board hosts a split-pane interface: one pane for the active conversational terminal and another displaying real-time updates to CONTEXT.md and ADRs. As terms are clarified and decisions are resolved, the background agent updates the local domain glossary and commits Architecture Decision Records only when changes are highly irreversible, surprising, or represent significant technical trade-offs. This ensures that the code remains anchored to a strict domain model.

### Stage 3: Non-Interactive Specification and PRD Synthesis

Upon resolving all ambiguities, the conversation context is synthesized directly into a comprehensive Product Requirement Document (PRD) using the /to-prd skill. This process is entirely non-interactive and focuses on extracting deep, highly encapsulated modules that can be verified in isolation. The board displays the generated PRD structure within a designated column, organizing the document into distinct panels for user stories, testing strategies, out-of-scope boundaries, and architectural decisions, then publishing it directly to the designated issue tracker.

### Stage 4: Vertical Tracer-Bullet Slicing and Dependency Resolution

Once the PRD is published, the task card progresses to the slicing column. Here, the /to-issues skill executes to decompose the monolithic PRD into a series of tracer bullets: thin, independent, vertical slices that cut through every architectural layer including schemas, APIs, and the user interface.

The visual board must support a parent-child relationship at this juncture. The parent PRD card remains anchored in the specification column, while a nested, dependency-linked tree of vertical slice cards is populated in the ready column, marked as either Human-In-The-Loop (HITL) or Away-From-Keyboard (AFK) based on implementation autonomy. This ensures that the agent works in an optimal sequence, starting only when preceding blocker tickets are fully resolved.

### Stage 5: High-Assurance Test-Driven Execution

When an AFK-ready vertical slice is initiated, the board launches the /tdd execution engine. This stage implements a strict red-green-refactor iteration cycle. The agent is programmatically restricted from writing any production code before producing a failing integration-style test that exercises public APIs rather than mocked internal components.

The board must visually reflect the active phase of this loop: displaying a red indicator while the initial test fails, a green indicator once minimal code satisfies the test, and a refactoring phase where the agent is allowed to optimize duplication and deepen module boundaries only while remaining in a passing state. If execution encounters misleading runtime failures, the board triggers the /diagnose skill, enforcing a disciplined loop of reproducing, minimizing, hypothesizing, instrumenting, and fixing the regression.

| Operational State | Primary Trigger Command | Underlying Skill Executed | Visual Board Event / State Transition | File Artifact Created or Updated |
|---|---|---|---|---|
| Backlog Triage | /setup-matt-pocock-skills | setup-matt-pocock-skills | Initializes issue tracker columns and triage roles | docs/agents/issue-tracker.md, CLAUDE.md |
| Context Grilling | /grill-with-docs | grill-with-docs | Launches active terminal split-pane interrogation | CONTEXT.md, docs/adr/00XX-decision.md |
| PRD Generation | /to-prd | to-prd | Renders formatted PRD panel; pushes to tracker | Generated Markdown Issue body, pushed via GitHub CLI |
| Vertical Slicing | /to-issues | to-issues | Populates nested card dependency tree; labels HITL/AFK | Decomposed GitHub/GitLab issue records |
| TDD Execution | /tdd | tdd | Renders Red-Green loop; displays isolated terminal TUI | Integrated Test Files and production code changes |
| Error Diagnosis | /diagnose | diagnose | Initiates isolated bug investigation in workspace | Local instrumentation logs and test assertions |

## Evaluating Existing Foundations for Custom Forking

To construct this platform efficiently, the engineering team should avoid building core terminal interfaces and worktree management engines from scratch. Instead, developers can fork and adapt established open-source repositories to support Matt Pocock's skills framework. Analyzing the structural layouts of cline/kanban and rookedsysc/kanvibe reveals where modification and integration points must be introduced.

### The Cline Kanban Foundation

The cline/kanban repository provides a powerful framework for running CLI agents in parallel. It is optimized for spawning isolated Git worktrees and symlinking directories like node_modules to minimize overhead. It handles tRPC hook state ingestion, enabling the platform to update card states based on agent telemetry.

### The KanVibe Foundation

The rookedsysc/kanvibe repository is an evolution of Vibe Kanban, offering a keyboard-first local workspace manager. It uses Next.js, TypeORM, and SQLite to persist board states, and coordinates local sessions through terminal multiplexers like tmux and zellij. It is highly suited for local-first developer environments where persistence across restarts is critical.

| Target Repository | Key File Path | File Responsibility | Necessary Modifications for Pocock Skills Integration |
|---|---|---|---|
| cline/kanban | src/core/agent-catalog.ts | Declares available coding agents and startup arguments | Add custom arguments for loading Matt Pocock's skills via --plugin-dir |
| cline/kanban | src/core/agent-session-adapters.ts | Coordinates process spawning and intercepts terminal output | Change default flags from --dangerously-skip-permissions to --permission-mode auto |
| cline/kanban | src/prompts/append-system-prompt.ts | Injects instructions into active agent sessions | Inject custom rules directing agents to use /grill-with-docs and /to-issues on the board |
| cline/kanban | src/core/task-worktree.ts | Provisions and cleans isolated Git worktrees | Integrate the Worktree Slot Pool to recycle directories and minimize clone latency |
| rookedsysc/kanvibe | src/app/api/tasks/route.ts | Handles task creation, state transitions, and deletions | Map Next.js routes to recognize parent-child structures and dependency blocks |
| rookedsysc/kanvibe | electron/runtime.ts | Manages the background SQLite and node-pty systems | Implement tRPC hook listeners to capture /tdd output and transition cards |

## Integrating Matt Pocock's Skills into the Claude Code Ecosystem

To leverage the skills within a visual workspace, the platform must interface directly with the plugin architecture of Claude Code. Claude Code manages custom behaviors by reading plugin directories located in the configuration path ~/.claude/plugins/. Each skill requires a manifest file (plugin.json) specifying its namespaced identifier, version, description, and execution path.

When Matt Pocock's skills are installed, they are registered using unique namespaces such as aihero-tdd@skills-by-mattpocock. This namespacing ensures collision resistance when multiple plugins offering similar features (such as alternative TDD frameworks) are active simultaneously. The agent discovers these capabilities dynamically, reading the frontmatter description fields in each skill's SKILL.md file to determine when to call the tools.

Beyond local files, the platform can use Model Context Protocol (MCP) servers to maintain the Kanban board state within the agent's context window. Model Context Protocol servers such as kanban-mcp or knbn-mcp expose structured tools directly to the LLM. By integrating an MCP server into the Claude Code setup, the agent gains direct, programmatic access to the board state.

The agent can query board configurations, append tasks, and transition cards through stdio-based tool calls, ensuring that the visual board and the agent's inner planning state remain in sync across multiple conversation turns.

## Engineering Customizations for a Unified Platform

To turn a standard Cline Kanban fork into a language-aligned workspace, the engineering team must introduce technical overrides to the workspace management and state transition engines. These overrides resolve resource constraints on local machines and establish robust communication loops between the visual interface and running agents.

### Ephemeral Worktree Slot Optimization and Resource Constraints

Cline Kanban's default architecture spawns a distinct Git worktree for every card on the board to avoid merge conflicts. However, continuously provisioning worktrees, cloning submodules, and copying or symlinking dependencies like node_modules introduces severe performance overhead in larger monorepos.

To optimize this, the backend orchestrator must be modified to implement a Worktree Slot Pool. Instead of destroying worktrees on card completion, the server maintains a fixed pool of persistent slots. Let $E_t$ represent the execution time of task $t$, $P$ represent the parallel execution limit, and $T_{\text{reset}}$ represent the time required to clean a slot :

$$T_{\text{reset}} = T_{\text{checkout}} + T_{\text{clean}} + T_{\text{reset\_hard}}$$

We can model the total physical execution time of the worktree slot pooling architecture versus the default ephemeral model as:

$$T_{\text{ephemeral}} = \sum_{t=1}^{N} \left( T_{\text{create\_worktree}} + E_t + T_{\text{destroy\_worktree}} \right)$$

$$T_{\text{pooled}} = T_{\text{init\_pool}} + \frac{1}{P} \sum_{t=1}^{N} \left( E_t + T_{\text{reset}} \right)$$

Because $T_{\text{create\_worktree}} \gg T_{\text{reset}}$, utilizing a slot pool yields an immediate reduction in workspace initialization latency, particularly when executing parallel, iterative test runs on constrained machines.

```typescript
// Implementing the Worktree Slot Pool in src/core/task-worktree.ts
import { exec } from "child_process";
import { promisify } from "util";
const execAsync = promisify(exec);

export class WorktreePool {
  private slots: Array<{ id: number; path: string; busy: boolean }> = [];
  private maxSlots = 4; // Capped to physical core density to prevent thrashing 

  constructor(private repoRoot: string) {}

  public async initializePool() {
    for (let i = 0; i < this.maxSlots; i++) {
      const slotPath = `.kanban/worktrees/slot-${i}`;
      await execAsync(`git worktree add ${slotPath} --detach`, { cwd: this.repoRoot });
      this.slots.push({ id: i, path: slotPath, busy: false });
    }
  }

  public async acquireSlot(): Promise<string> {
    const slot = this.slots.find(s =>!s.busy);
    if (!slot) {
      throw new Error("All execution slots are busy. Task queued.");
    }
    slot.busy = true;
    return slot.path;
  }

  public async releaseSlot(slotPath: string, baseBranch: string) {
    const slot = this.slots.find(s => s.path === slotPath);
    if (slot) {
      // Execute fast reset to prevent lingering state or untracked changes 
      await execAsync(`git reset --hard && git clean -fdx && git checkout ${baseBranch}`, { cwd: slotPath });
      slot.busy = false;
    }
  }
}
```

### Event-Driven Ingestion and State Hooks

The platform maintains real-time status synchronization by capturing agent execution telemetry. The CLI agent is configured with wrapper commands that report event payloads to the runtime's tRPC server via standard command line hooks.

As the agent executes, the runtime captures these transitions, applying guarded logic to update the board interface.

```typescript
// tRPC Ingestion Handler in src/core/hooks.ts
import { z } from "zod";
import { publicProcedure, router } from "./trpc";

export const hooksRouter = router({
  ingest: publicProcedure
    .input(
      z.object({
        eventId: z.string(),
        taskId: z.string(),
        event: z.enum(["to_in_progress", "to_review", "test_failed", "test_passed"]),
        details: z.string().optional(),
      })
    )
    .mutation(async ({ input }) => {
      const { taskId, event, details } = input;
      
      // Enforce state transitions based on Pocock execution rules 
      switch (event) {
        case "to_in_progress":
          await db.task.update(taskId, { status: "PROGRESS", subState: "RUNNING" });
          break;
        case "to_review":
          await db.task.update(taskId, { status: "REVIEW", subState: "AWAITING_REVIEW" });
          break;
        case "test_failed":
          // Highlight card border red; block automated git commit 
          await db.task.update(taskId, { subState: "RED_LIGHT", logs: details });
          break;
        case "test_passed":
          // Highlight card border green; enable auto-commit/PR option 
          await db.task.update(taskId, { subState: "GREEN_LIGHT", logs: details });
          break;
      }
      
      return { success: true };
    }),
});
```

To support this event loop, we can also model the token overhead reduction of an aligned agent session. Let $C_{\text{turn}}$ represent the token cost per execution turn. If $T_p$ is the prompt token count and $T_y$ is the generation token count, the total token cost is defined by the vocabulary compression factor $\gamma$ achieved through domain alignment, and the cognitive reduction factor $\delta$ :

$$C_{\text{turn}} = T_p \cdot (1 - \gamma) + T_y \cdot (1 - \delta)$$

By establishing a strict domain glossary (CONTEXT.md) and past decisions (docs/adr/) in the early stages of the board, the agent avoids expensive cognitive evaluation overhead, reducing overall API costs and accelerating loop completion times.

## Conclusions and Strategic Recommendations

To deploy a language-aligned, test-driven Kanban environment without duplicating foundational software development, the following strategic actions are recommended:

1. Fork the Cline Kanban Repository: Fork the active cline/kanban project to inherit its robust tRPC server, Vite web interface, visual diff viewer, and native Git worktree isolation.

2. Implement Secure Claude Integration: Modify agent-session-adapters.ts to replace the insecure --dangerously-skip-permissions bypass with --permission-mode auto. This preserves the automated experience while using Claude Code's background safety classifier.

3. Mount Pocock's Skills Framework: Configure the agent catalog to automatically append the --plugin-dir argument pointing to the localized user installation of Pocock's skills (~/.claude/skills).

4. Enforce Programmatic Architectural Gates: Customize append-system-prompt.ts to instruct active agents to use /grill-with-docs and /to-issues on the board. The columns on the visual board serve as behavioral gates, restricting agents from writing production code before establishing domain glosseries and writing failing tests.

5. Deploy the Worktree Slot Pool: Implement a worktree slot pool in task-worktree.ts to recycle and reset directories. This avoids expensive worktree reconstruction and dependency linking on local developer machines.

By adopting this architecture, software engineering teams can transition their AI assisted workflows from speculative generation into a structured, highly reliable engineering discipline.
