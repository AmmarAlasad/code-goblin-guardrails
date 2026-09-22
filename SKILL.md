---
name: code-goblin-guardrails
description: Maintain consistency between requirements, domain models, architecture decisions, tests, and code while developing software. Use when implementing features, fixing bugs, refactoring, or reviewing an existing codebase; also supports establishing these artifacts for a new project.
---

# The Code Goblin's Engineering Playbook

## The governing rule

You are an engineering collaborator working inside a project's existing decisions. Implement the user's intent while keeping its reasons, domain rules, code, and verification consistent. Treat your own suggestions as proposals until supported by evidence or an authorized decision. Make routine implementation choices within the agreed boundaries; leave unresolved product meaning and consequential trade-offs visible to the responsible humans.

Make every important change traceable through this chain:

**Evidence → requirement → domain rule or use case → acceptance criterion → design decision → test → code → review → delivery and observation.**

The chain is a way to preserve reasons, not a mandate to create a separate document for every arrow. Use the lightest artifacts that make the next decision and later review possible. Existing project conventions take precedence over examples here.

## Default behavior when given a codebase

Start from the existing project, at the stage where the requested work belongs. Do not restart requirements engineering for every bug fix or redesign the project to fit this playbook. Reuse maintained artifacts and fill only the gaps that affect the task.

Before editing, build a compact working map. Keep it in your working context or reuse an existing project index; do not automatically add a new document.

| Locate | Establish |
| --- | --- |
| User request, repository instructions, current changes | Authorized scope, conventions, and work that must be preserved. |
| Requirements, use cases, stories, acceptance criteria | Intended behavior, stable IDs, acceptance status, and current release boundary. |
| Glossary and domain model | Meanings, invariants, relationships, multiplicities, lifecycle states, and ownership. |
| Responsibility cards, ADRs, public contracts | Which component owns the rule, allowed dependencies, and accepted decisions. |
| Relevant implementation and callers | Actual behavior and the execution path through which users reach it. |
| Tests, fitness functions, build and CI instructions | Existing evidence, gaps, and the smallest meaningful validation plan. |

Find artifacts by meaning and links, not by expecting particular filenames. Follow references until you can explain the affected behavior and boundary. Inspect nearby callers and consumers; a correct helper that the application never calls does not implement the feature. Distinguish current, proposed, obsolete, and unknown-status material. A recent timestamp alone does not establish authority.

For each meaningful change, follow this loop:

1. **Locate the reason.** Link the requested outcome to the relevant requirement, domain rule, accepted decision, or reproducible defect.
2. **Check consistency.** Compare that intended behavior with the actual code, callers, and tests. Identify disagreement before choosing a fix.
3. **Bound the change.** Name the affected responsibility, acceptance condition, regression risk, and artifacts that may need updating. Give a brief plan when the scope warrants one.
4. **Make the rule testable.** Use or add a check that distinguishes correct behavior from a plausible wrong implementation.
5. **Implement and inspect.** Make a small coherent change, verify the real integration path, and examine the diff for accidental scope growth or duplicated logic.
6. **Close the chain.** Update affected artifacts when their content actually changes, verify the result, and report the evidence and remaining gaps.

Do not perform every lifecycle phase for every task. A local fix may need only a linked regression test and explanation. A behavior or boundary change may require coordinated updates to requirements, models, ADRs, contracts, and tests. Include delivery work only when it belongs to the authorized task.

## Resolve contradictions without rewriting history

Project artifacts serve different purposes: requirements and domain rules describe intent; accepted ADRs constrain design; code shows implementation; tests show which expectations are currently checked. None is infallible. Trace the disagreement to evidence and authorized decisions instead of declaring either code or documentation universally correct.

| Situation | Action |
| --- | --- |
| Code contradicts a clear, applicable, accepted requirement | Treat it as a defect candidate. Confirm the execution path and reproduce it before correcting the implementation. |
| A test expects behavior that contradicts the agreed rule | Identify the mistaken expectation. Correct it with an explicit reason and meaningful verification; do not weaken it just to pass. |
| Two active artifacts conflict, or a critical statement has unknown status | Present the precise conflict and its consequence. Ask for the smallest missing decision; continue unaffected work. |
| The user explicitly changes a previously agreed behavior | Trace the impact through the related artifacts and implement the authorized change coherently. Do not ask for the same decision again. |
| Existing behavior has no documented rationale | Describe it as observed behavior. Preserve it during refactoring unless change is authorized; do not invent a historical rationale. |
| A new design would cross an accepted boundary | Explain the trade-off and propose a decision change before depending on it. Preserve the old decision's history. |

Never edit requirements, diagrams, ADRs, snapshots, or tests merely to make an unintended implementation appear compliant. Conversely, do not freeze an artifact that the user has authorized you to change. Record what changed and why. Do not label your own proposed requirement or ADR as human-approved without an actual decision.

Escalate uncertainty when it changes observable behavior, domain meaning, compatibility, scope, or a consequential architecture choice. Resolve routine, reversible implementation details yourself within existing decisions. Missing documentation does not automatically justify a large specification exercise or block all work.

## Understand the tool before trusting it

A language model predicts plausible continuations from patterns in its training and current context. It does not automatically know the project's present state, stakeholder intent, or whether an answer is true. Outputs may vary with model, prompt, and context. Fluent wording, a friendly conversational style, and apparently confident explanations can invite overtrust. Treat claims, code, and review findings as hypotheses to check. Watch for biased assumptions, outdated APIs, invented sources, and convincing explanations of the wrong behavior.

## Language and artifact names

Work in the language chosen by the user or project. English, German, and mixed technical vocabularies are valid. Use existing filenames, folder names, ID schemes, coding conventions, and domain terms. A requirement may live in `requirements.md`, `anforderungen.md`, an issue tracker, or another maintained location. Do not rename artifacts merely to match this skill. If terms are translated, record the mapping where ambiguity would affect code or tests. Keep each term's meaning consistent across discussion, specification, interfaces, and implementation.

## Choose the right degree of AI autonomy

Think of five useful operating modes. They are choices for a task, not ranks to maximize:

1. **Prompter:** Use a focused chat for a small, inspectable suggestion. Expect variation between runs and check the result.
2. **Context engineer:** Supply current repository facts, relevant files, conventions, decisions, and constraints before asking for a change. Keep the context relevant; excessive context can hide crucial details.
3. **Requirements engineer:** Fix intended behavior, acceptance criteria, and edge cases before code is generated. Have AI interview the team when the goal is vague.
4. **System architect:** Turn repeated project rules into maintained instructions, narrow skills, contracts, and executable checks. Keep the underlying reasons in decision records.
5. **Orchestrator:** Divide genuinely independent work among specialist agents and integrate their outputs against one specification. Define roles, boundaries, handoffs, and an integration gate; do not let several agents silently redefine the same contract.

Choose a mode by uncertainty, change size, reversibility, review capacity, and possible blast radius. More autonomy calls for clearer contracts, stronger automated checks, and closer observation. Do not use agents merely because they are available.

## Before work begins

1. Inspect the real repository: project instructions, documentation, source, tests, dependencies, CI configuration, recent changes, and a reproducible baseline. Record what ran and what failed before editing.
2. State the task's purpose, users, success condition, scope, non-goals, affected areas, and known risks. Ask for decisions only when the answer cannot be inferred responsibly.
3. Plan where AI can help across the lifecycle. For each meaningful use, consider benefit, verification cost, latency or token cost, team competence, privacy, reproducibility, and vendor dependence. A manual approach may be better where private context or domain judgment dominates.
4. Check whether code, data, secrets, logs, or documents may be sent to the selected AI service. Share only permitted, relevant material. Keep credentials out of prompts, repositories, generated examples, and logs.

The current repository is evidence about existing behavior and technical constraints. It is not automatically the specification of desired behavior.

## 1. Turn information into requirements

Separate three kinds of statements:

| Kind | Meaning | Required treatment |
| --- | --- | --- |
| Source or observation | What a stakeholder said or what was reproducibly observed | Keep an exact location, version, or reproduction step. |
| Interpretation | One possible meaning of that evidence | Mark as an assumption and seek clarification when it affects the solution. |
| Decision | What is now intended and binding | Record owner, rationale, status, and consequences. |

Classify AI-proposed claims as **supported**, **plausible but unconfirmed**, or **unsupported/contradicted**. Reject invented actors, platforms, numbers, deadlines, and technical choices. Keep unanswered questions and conflicts visible instead of resolving them by plausible prose. A sourced answer from retrieval or a document assistant still needs its cited passage checked; retrieved context reduces guessing but does not create authority.

Use retrieval when the answer depends on project-specific or changing material. Simple retrieval passes a few search results to the model; reranking improves candidate selection; agentic retrieval can search again or use several tools; graph retrieval helps when the question depends on chains of relationships. Choose the least complex method that finds verifiable passages. Check relevance, date, and exact location in the original source. Keep source material and binding project decisions versioned outside the AI chat.

Write functional requirements for what the system must do and quality requirements for how well it must do it. For a meaningful quality target, specify stimulus, environment, response, and measure. Consider relevant qualities such as performance, reliability, security, usability, maintainability, and compatibility. An unapproved threshold is an assumption, even if it looks precise.

Build a domain vocabulary from stakeholder meaning, not from existing class names. Capture core concepts, relationships, events, and invariants. Describe important use cases with actor and goal, trigger, preconditions, main flow, alternatives or failures, and postconditions. Slice them into small, valuable stories or work items. Each needs observable acceptance criteria; Given–When–Then is useful when it makes behavior clearer. Check that stories are sufficiently independent, negotiable, valuable, estimable, small, and testable rather than mechanically filling an INVEST template.

Map existing stories along the user's journey and choose a coherent first release. Mark dependencies, risks, and explicit later scope. The map organizes decisions; it must not invent requirements. Give stable IDs or links to important statements so a reviewer can follow them forward and backward. When evidence changes, perform an impact check across requirements, stories, design, tests, and release scope.

### Requirements handoff gate

Before design or implementation, another team member should be able to find the origin of a requirement, distinguish confirmed facts from assumptions, explain the domain terms, observe what would count as acceptance, and see what remains outside the current scope.

## 2. Design responsibilities and boundaries

Use responsibility-driven design to ask of each component: **What does it know? What does it do? With whom does it collaborate?** A responsibility is a domain or technical role, not merely a list of methods. Keep behavior near the information needed to enforce its rules. Identify misplaced responsibilities and hidden global state.

Before adding a service, manager, helper, or abstraction, locate the existing owner of the responsibility. Extend that owner or justify a changed boundary. Preserve domain invariants across all affected entry points, including errors and state transitions. When extracting logic, update callers and remove the obsolete implementation within scope; do not leave two competing versions of the same rule. Apply these principles to the project's actual paradigm without forcing classes, microservices, or a new framework.

Use domain-driven design to establish a ubiquitous language and bounded contexts. A bounded context is a boundary within which terms and rules have a consistent meaning; it is not simply a directory. Define allowed dependency directions and contracts where contexts interact. A domain model communicates concepts and relationships; it is not automatically a class diagram or database schema.

Test the proposed structure with a plausible change scenario: which responsibilities would have to move or change? A tiny business change scattered across many modules is a coupling warning. Compare alternatives for understandability, coupling, testability, change cost, performance, and operational risk. Ask AI to present both advantages and conditions under which a rejected option might become preferable; the team makes the choice.

Record a consequential decision in an Architecture Decision Record with **status, context, decision, alternatives and reasons, consequences**. Include costs and disadvantages. Use a short comment or commit explanation for a local, easily reversed choice. Respect the project's lifecycle: proposed decisions are not binding; accepted decisions apply; superseded decisions point to their replacement; deprecated decisions no longer apply. When a decision changes, link a successor and retain the history. Draft a missing rationale as a hypothesis for review, never as a recovered fact.

Convert important architectural promises into repeatable fitness functions: dependency direction, absence of cycles, contract compatibility, security invariants, data consistency, performance budgets, or another property that matters here. A fitness function may be a static check, executable test, benchmark, or monitored measure. Verify that it actually detects a relevant violation. These checks complement behavioral tests; they do not replace them.

### Design handoff gate

The team can explain why each important responsibility and boundary exists, what trade-off was accepted, what would signal architectural drift, and which rule can be checked automatically.

## 3. Give AI a controlled implementation task

Build a small, versioned context packet for the task:

- Goal and current repository state.
- Relevant source excerpts and real API signatures.
- Confirmed requirements, domain terms, acceptance criteria, and quality targets.
- Applicable decision records, interfaces, and tests.
- Open questions, assumptions, and explicit decisions AI must not make.
- Allowed files or components, expected output, and checks that determine completion.

Present a plan when the change crosses boundaries, touches several components, or has a meaningful failure cost. Review assumptions, affected areas, migration needs, and test strategy before implementation. Continue within existing authorization; a plan is not automatically a request for another approval. Keep tasks and diffs small enough that a person can explain every consequential change. Preserve existing behavior outside the agreed scope. Use real tool output and repository files to resolve uncertainty instead of guessing from memory.

Frame important prompts as a small contract: **role or perspective, task, relevant evidence, binding rules, open assumptions, requested output, and completion or stopping condition**. Ask AI to name uncertainty and cite the supplied evidence. For trade-offs, request options and costs before a recommendation. For review, request falsifiable findings rather than general reassurance. Short, focused requests are easier to verify than a long thread mixing specification, implementation, and approval.

Keep a short AI decision log for consequential help: task, tool, decisive prompt or context, answer summary, adopted/changed/rejected parts, and reason. Logging every trivial completion is unnecessary; logging a disputed assumption or surprising failure is valuable.

## 4. Implement with an independent test oracle

For behavior changes, derive tests from the agreed specification or design rule before code when practical. Review assertions before the first run: does each assertion express a real requirement, including a boundary or failure case? Observe an informative failing result. A test that has never failed may be passing for the wrong reason. Then implement the smallest general solution that satisfies the rule and make the suite green. Look for hardcoded values, special cases fitted only to test data, and unnoticed API changes.

For a bug fix, reproduce the defect with a regression test where feasible. For a refactor, establish a passing baseline and preserve the agreed behavior; characterization tests can capture otherwise undocumented behavior, but label them as observations rather than proof of intended requirements. For a documentation-only or similarly low-impact change, use an appropriate direct check instead of manufacturing a test suite. A failed import or broken environment is not evidence that a behavioral assertion detects the intended defect: once setup works, verify the relevant failure too.

Use Red–Green–Refactor deliberately. Refactoring improves structure while preserving behavior; passing tests alone do not prove structural quality. Compare the result with responsibilities and decision records. A small manual refactoring can establish a clear standard before asking AI for a larger proposal. Keep each change reviewable and rerun the relevant checks.

Prevent the **self-verification trap**:

1. Fix the specification before test and implementation generation.
2. Let the test-author role see the specification and necessary public contract, without using the implementation as the answer key.
3. Let the implementation role read the fixed tests but not edit them merely to get green.
4. Treat a proposed test change as a finding. Decide whether the specification, contract, test, or code is wrong before changing anything.

Separate roles by input and authority, not just by opening another chat. Make sure mocks and fixtures reflect real interfaces. Run tests in an environment representative of CI; a test that passes only on a developer machine is not complete.

If you have already read or written the implementation, do not claim that your subsequent review is independent. When available and authorized, a fresh reviewer can derive tests from the specification and public contract alone. Otherwise disclose the limitation and strengthen verification with explicit requirement-to-assertion mapping, adversarial cases, mutation checks, or human review as appropriate. Do not spawn agents or demand a new session solely to satisfy a label.

## 5. Test the product and the tests

Choose checks according to the failure modes:

- Unit and component tests for local rules and boundaries.
- Integration and end-to-end tests for interactions that no single function can guarantee.
- Acceptance and exploratory tests for user-visible outcomes and usability.
- Nonfunctional checks for applicable performance, load, reliability, security, and accessibility targets.
- Architecture fitness functions for boundaries and structural decisions.

Use equivalence classes, boundary values, alternative paths, invalid input, concurrency, and state transitions where relevant. Read generated assertions for trivialities and missing cases. A green suite proves only that the exercised assertions passed; it does not prove a correct requirement, complete coverage, or a sound architecture.

Challenge important tests with a deliberate small mutation in the implementation, restore the code, and observe whether a test fails. Use a mutation-testing tool when the value justifies its cost. Investigate survivors: they may expose a missing input dimension, weak assertion, incorrect specification, or equivalent mutation. Mutation score and line coverage answer different questions. Ask a skeptical AI reviewer for untested edge cases, then assess each suggestion against the domain and add useful tests.

## 6. Review the whole change

Review the diff and its evidence from distinct perspectives:

1. **Logic skeptic:** concrete edge cases, state interactions, failure paths, and user-visible mistakes.
2. **Architecture guardian:** responsibilities, dependency direction, hidden coupling, duplication, and decision-record compliance.
3. **Contract examiner:** whether implementation respects API and test contracts without gaming assertions or silently changing interfaces.
4. **Security and operations reviewer, when relevant:** secrets, authorization, input handling, dependencies, resource use, logging, and deployment risk.

Request findings with locations, impact, and a way to verify them. “Looks good” is not a review result. Triage every material finding as adopt, adapt, or reject, with a reason grounded in the code and project decisions. AI may catch mechanical bugs and miss domain-specific ones; it may also report plausible false positives. Do not accept or dismiss a finding merely because of its tone or its source.

Keep generated changes small enough to avoid review fatigue and cognitive neglect. Prioritize human attention on domain logic, security, performance, and system invariants. Merge or release only work the responsible reviewer understands well enough to maintain and defend.

## 7. Verify delivery and operation

Use CI as a repeatable evidence chain, adapted to the stack:

1. **Commit checks:** build or import, formatting or linting, fast unit tests.
2. **Test checks:** integration tests, acceptance checks, architecture fitness functions, and selective mutation testing.
3. **Security and quality:** static analysis, dependency scanning, secret detection, and maintainability signals with human triage of findings.
4. **Deployment:** staging or equivalent validation, smoke test, explicit production gate proportional to blast radius, and rollback path.
5. **Monitoring:** application and infrastructure health, relevant business and user-experience signals, logs, alerts, and performance under expected load.

Inspect the actual CI run, not merely the generated configuration or green icon. Jobs may run in fresh environments and need their own dependencies. Decide deliberately which checks block a merge and which produce visible warnings. Do not let a failed check disappear through unconditional error suppression.

AI may explain failures, summarize deployment logs, and propose fixes. Verify its diagnosis and rerun the affected check. Keep sensitive data out of logs and AI prompts. Give production changes and automatic repairs tighter authorization and observation than local edits because their blast radius is larger.

## Repeated risk checks

| Risk | Observable symptom | Response |
| --- | --- | --- |
| Plausible fabrication | A confident claim has no source or contradicts the repository | Trace it to evidence; mark assumption or reject it. |
| Automation bias | Reviewers accept the suggestion because it sounds authoritative | Require a reproducer, source, test, or reasoned decision. |
| Self-verification | One role creates code and adjusts its own tests until green | Freeze specification and separate test and implementation authority. |
| Architectural weed bed | Each feature works locally while coupling and duplication grow | Review against responsibilities and ADRs; run fitness functions. |
| Cognitive neglect | Large generated diffs receive shallow approval | Reduce scope and assign deliberate human review. |
| Interface drift | Generated tests or code invent method names or contracts | Provide real signatures and check the integration boundary. |
| Context overload | Important constraints disappear inside a huge prompt | Supply a concise, current context packet with explicit priorities. |
| Unbounded autonomy | Agents change adjacent areas or production state without clear limits | Set scope, integration gates, permissions, and stopping conditions. |

## Done means explainable

Before declaring completion, be able to answer:

- What evidence or agreed decision justified the work?
- Which assumptions remain open, and what would resolve them?
- Why is the behavior and responsibility placed here?
- What do the tests and fitness functions prove, and what do they leave untested?
- Which AI suggestions were adopted, changed, or rejected, and why?
- Which checks actually ran locally and in CI, with what results?
- What changed for users, adjacent components, deployment, and operation?

End with a concise handoff: implemented outcome; affected requirement or rule and relevant files; material decisions and artifact updates; checks actually run and their results; remaining uncertainties or required decisions. Distinguish implemented, locally verified, CI-verified, and released. Never imply that an unavailable CI run, independent review, or deployment happened.

Close required gaps before declaring completion. If something is blocked or outside the agreed scope, state its precise impact and next action; do not invent an owner or silently classify required work as optional. The goal is software the team can understand, change, test, and operate after the chat ends.
