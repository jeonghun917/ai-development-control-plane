# AI Development Control Plane

I started this as a personal dashboard on August 18, 2026.

By September 19, the private repository had reached PR #194.

Somewhere in between, it stopped being a dashboard.

It became the control plane I use to manage long-running software projects built with AI coding workers: project state, execution authority, validation, review, recovery, and eventually end-to-end development.

The production system is private. This repository is where I publish the parts of the engineering that are useful outside my own stack.

## What it does

The coding model is not the system.

Models are replaceable workers underneath it.

```text
design / intent
      ↓
current project authority
      ↓
bounded task contract
      ↓
coding worker
      ↓
deterministic validation
      ↓
independent semantic review
      ↓
source-grounded adjudication
      ↓
promotion / merge
      ↓
exact readback
      ↓
durable project state
      ↓
next task
```

The goal is to make that loop increasingly autonomous without letting the model decide, by itself, what it is allowed to change or whether its own work is correct.

## How this happened

The architecture was not designed upfront.

Most of it exists because an earlier version failed in a way that made a new invariant necessary.

### PR #1 — A modular dashboard

The first version was a fairly conventional modular dashboard: plugin registration, integrations, runtime modules, navigation, CI, and provider adapters.

It could display and connect systems.

It could not yet answer the harder questions:

- What is the current task?
- Which record is actually authoritative?
- Is this worker allowed to act?
- Is this state current or just historical?
- Did an external mutation really happen?
- Does this validation belong to this exact candidate?

Those questions ended up driving most of the next 190 pull requests.

### PR #4 onward — Durable control state

The dashboard gained durable events, actions, webhook receipts, cursors, and scheduler records.

Static credentials were replaced with short-lived workload identity. Source fingerprints and change diffs became durable. Inbox actions stopped being ephemeral UI state.

The first major shift was simple:

> Development state should survive the interface that displays it.

### PR #26–#51 — Incidents, lifecycle, and stale state

Continuous observation created another problem: old information remained technically true while no longer describing the current world.

That led to a deterministic Incident engine, explicit lifecycle states, scheduler leases, production E2E fixtures, and durable sanitized summaries.

I also tried an external AI runtime for incident interpretation and removed it one PR later.

That was an early version of a rule that later became much more important:

> AI output can help interpret state without becoming the authority for that state.

### PR #59 — Continuity

The next problem was session loss.

The first Continuity Kernel introduced persistent task contracts, scope guards, evidence references, authority references, and handoff state.

This eventually became a project-level continuity system spanning multiple long-lived projects.

One rule survived every later revision:

> A resume can tell the system where to look. It is not itself execution authority.

Before acting, the system has to resolve and reread the current task and its exact authority.

### PR #67–#76 — Orchestration and bounded workers

Deterministic orchestration came next.

Then the first evidence-bound Coding Worker.

A worker was no longer given a repository and told to "fix it." Its execution was tied to things like:

- exact repository
- exact base commit
- bounded paths
- mutation limits
- task identity
- evidence
- execution contract
- resulting commit identity

A successful coding run produced a candidate, not a completed task.

Execution-plane supervision followed.

At this point, the system was beginning to separate three things that are easy to blur together:

**capability, permission, and correctness.**

They are not the same thing.

### PR #101 onward — Real model execution

The coding path eventually moved from deterministic scaffolding into real model execution.

That exposed a different class of problems: provider behavior, runtime identity, transport, context limits, retries, partial results, and ambiguous external state.

The control system became less about calling a model and more about making sure a model call could not silently expand into authority it had never been granted.

### PR #134–#153 — Exact identity and promotion

Machine authentication, exact compare-and-set behavior, canonical state cleanup, and repository-scoped authority discovery accumulated into a deterministic validation and merge Control Spine.

Candidate promotion became its own bounded machine operation.

A core rule here is:

> Validation is evidence about an exact candidate.

If the candidate changes, old validation does not magically transfer to it.

That sounds trivial until an automated system is simultaneously dealing with branches, PR heads, workflow runs, deployments, and stale local state.

### PR #157–#165 — Review stopped being truth

Semantic review was separated into its own durable three-reviewer path.

More importantly, review output was deliberately classified as hypothesis rather than truth.

The current pattern is approximately:

```text
candidate
   ↓
review A
review B
review C
   ↓
raw semantic evidence
   ↓
separate source-grounded adjudication
   ↓
accept or bounded repair
```

A majority of reviewers agreeing does not grant promotion authority.

PR #163 then introduced the bounded Development Controller: a higher-level controller that derives the next admissible development transition from current task, authority, registry, governance, provider, and repository state.

PR #165 connected semantic evidence, adjudication, and a bounded repair cycle.

That was the point where the system started looking much more like a development control plane than a dashboard.

### PR #168 onward — Running the system against itself

Recent work has mostly been the control plane exposing its own assumptions.

Real controller runs found problems involving:

- stale task projection
- runner identity and ownership chains
- context composition
- oversized source
- models reconstructing source they were supposed to edit
- truncated output
- provider IAM and account-level model availability
- local transport failure before provider inference
- models proposing mutations outside the intended local delta
- architecture context being omitted during compression
- a model treating a bounded extension of existing architecture as forbidden new architecture

Those failures produced more generic mechanisms instead of one-off prompt patches.

For example, the mutation path evolved from model-authored source replacement toward server-owned exact source references, then gained local-delta limits when a model found a different way to reconstruct too much code.

The system is increasingly being developed by exercising the development system itself.

## A few rules I now care about

### History is not current authority

Keep the history.

Do not automatically feed old state back into the next decision.

### Model capability is not permission

A model may be technically capable of changing a repository while having zero authority to do so.

### Review is evidence

A reviewer can be wrong.

Three reviewers can be wrong together.

### Unknown mutation state means reconcile first

If an external request times out after durable intent exists, blindly retrying can duplicate the mutation.

Read the provider state back first.

### Paid execution is also authority

Having enough account credit does not mean every task is authorized to spend it.

### Failing before mutation is often success

An agent run can fail.

A provider can fail.

A model can produce garbage.

The important property is that those failures do not automatically become repository state.

## Why I am building this

The control plane is not currently a product.

It is private infrastructure for my other projects, which include work on persistent AI continuity, provenance-grounded information systems, speech and voice systems, and simulation.

That changes what I optimize for.

I do not need a generic workflow that makes every user happy.

I need a system that can preserve the exact state and constraints of my own projects for years, use whatever coding models are best at the time, and keep moving them forward without requiring me to manually reconstruct the entire development context every session.

If OpenAI, Anthropic, or someone else builds a better coding worker, sandbox, or persistent workspace, I would rather plug it into this system than compete with it.

The layer I care about is what remains above the worker.

## Current direction

The near-term path is:

1. close the full Dashboard development lifecycle end-to-end;
2. run the same controller against a real product repository;
3. introduce persistent development workspaces;
4. prove interrupted work can be resumed, repaired, and reconstructed;
5. reduce the amount of human relay between design and verified implementation.

The question I started with was:

> Can AI write useful code for me?

That stopped being the interesting question fairly quickly.

The one I am working on now is:

> **How much software development can I delegate while still knowing exactly what was authorized, what changed, what was verified, and what state the project is actually in?**

That is what this repository is about.
