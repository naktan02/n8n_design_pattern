# n8n Design Pattern Workflow Engine

> A JavaScript prototype that models an n8n-style workflow automation engine through object-oriented design patterns.

## Recommended Repository Description

**Recommended**

```text
Design pattern-based JavaScript prototype of an n8n-style workflow automation engine with trigger strategies, composable nodes, event logging, and workflow rollback.
```

**Short version**

```text
An n8n-style workflow automation engine prototype built with JavaScript design patterns.
```

**Academic portfolio version**

```text
Object-oriented workflow automation architecture study implementing reusable node composition, trigger abstraction, event sourcing-style logging, and memento-based rollback.
```

## Overview

This project is an architectural prototype inspired by workflow automation tools such as **n8n**. Instead of focusing on external API integration, the project focuses on how a workflow engine can be structured using reusable object-oriented design patterns.

The prototype demonstrates a workflow where a trigger node detects a simulated YouTube event, reads recent liked-video data, sends Slack messages, records node execution events, removes a node, and restores a previous workflow state through rollback.

## Key Features

- **Composable workflow graph** using a sequential composite node structure
- **Trigger abstraction** that separates trigger behavior from execution infrastructure
- **Pluggable trigger implementations** for local polling and cloud webhook-style execution
- **Action nodes** for simulated YouTube and Slack workflow steps
- **Builder and factory-based node creation** for clear object construction
- **Execution logging decorator** that records node lifecycle events
- **Memento-based workflow rollback** for restoring earlier workflow states
- **Facade layer** that simplifies workflow composition and execution

## Architecture

```text
n8n_design_pattern
|-- app.js
|-- core
|   |-- Node.js
|   |-- WorkflowExecutor.js
|   |-- WorkflowMemento.js
|   |-- WorkflowCaretaker.js
|   |-- EventStore.js
|   |-- Registry.js
|   `-- implementations
|       |-- LocalPollingImplementation.js
|       `-- CloudWebhookImplementation.js
|-- decorators
|   `-- WorkflowExecutionLoggerDecorator.js
`-- nodes
    |-- actions
    |   |-- slack
    |   `-- Youtube_action
    |-- composites
    |-- facade
    `-- triggers
```

## Applied Design Patterns

| Pattern | Implementation | Purpose |
| --- | --- | --- |
| Composite | `SequentialWorkflow` | Treats a workflow as a node that can contain and execute child nodes. |
| Strategy | `YouTubeLikeTriggerStrategy`, `GmailTriggerStrategy` | Encapsulates different trigger behaviors behind a common interface. |
| Observer | `AbstractSubject`, `TriggerNode` | Notifies workflow trigger nodes when an external event is detected. |
| Factory Method | `DefaultSlackNodeFactory`, `DefaultYoutubeNodeFactory` | Creates service-specific node builders without exposing construction details. |
| Builder | `SlackMessageBuilder`, `YouTubeReadRecentLikedVideoBuilder` | Builds configurable action nodes step by step. |
| Command | Slack and YouTube command classes | Encapsulates external actions as executable command objects. |
| Decorator | `WorkflowExecutionLoggerDecorator` | Adds execution logging without modifying node implementations. |
| Facade | `WorkflowComposerFacade`, `WorkflowRunnerFacade` | Provides a simple API for composing and running workflows. |
| Memento | `WorkflowMemento`, `WorkflowCaretaker` | Saves and restores workflow state for rollback. |
| Registry | `Registry` | Registers and resolves trigger implementation types dynamically. |

## Demo Scenario

The demo in `app.js` runs the following workflow lifecycle:

1. Create an empty workflow.
2. Add a YouTube-like trigger node.
3. Add a YouTube recent-liked-video read node.
4. Add two Slack message nodes.
5. Execute the workflow when a simulated YouTube event is emitted.
6. Remove one Slack message node and execute again.
7. Restore the previous workflow state using the memento stack.
8. Print all recorded node execution events.

## Getting Started

### Requirements

- Node.js 18 or later

No external npm packages are required.

### Run

```bash
node app.js
```

Expected behavior:

- A simulated YouTube trigger event is delivered to the workflow.
- YouTube and Slack action nodes execute in sequence.
- Execution events are stored in `EventStore`.
- A removed Slack node is restored through rollback.

## Example Output

```text
YouTube trigger event received by workflow.
Slack message node executed for the group channel.
Slack message node executed for the personal channel.

Personal channel node removed.
...
--- Rollback demo started ---
...
--- Recorded events ---
[Event: NODE__STARTED] Node: TriggerNode
[Event: NODE_COMPLETED] Node: TriggerNode
```

## Why This Project Matters

Workflow automation systems need to support many node types, trigger sources, execution policies, and runtime concerns such as logging or rollback. This project studies that problem from a software architecture perspective.

The main goal is to show how a workflow engine can remain extensible when new services, triggers, and node behaviors are added. Each pattern is used as an architectural tool rather than as an isolated textbook example.

## Extensibility Points

New workflow capabilities can be added by extending the existing abstractions:

- Add a new trigger strategy under `nodes/triggers`
- Register a new execution implementation in `core/implementations`
- Add a new service-specific factory under `nodes/actions`
- Wrap nodes with additional decorators for metrics, validation, or authorization
- Extend the workflow composite to support branching, conditional execution, or parallel execution

## Current Scope

This repository is a prototype and architecture study. It does not connect to real YouTube, Gmail, or Slack APIs. Those integrations are simulated so that the focus remains on workflow composition, execution, event propagation, and rollback behavior.

## Future Improvements

- Add real webhook and polling adapters
- Introduce conditional and branching workflow nodes
- Persist workflow definitions and event logs
- Add unit tests for each pattern boundary
- Provide a visual workflow editor prototype
- Normalize Korean source comments to UTF-8 where legacy encoding appears garbled
