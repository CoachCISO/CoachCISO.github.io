---
id: 03
title: 'Control Theory for Software Security'
date: '2026-05-24T00:00:00+00:00'
author: 'Simon Goldsmith'
layout: post
excalidraw: true
categories:
    - Articles, Cybersecurity, Software
---
# Control Theory for Software Security

I've been thinking about the application of control theory to software security, and would appreciate feedback on whether I'm adding something or just stating the obvious.

**The problem:** most teams still chase secure software by asking developers two questions — did you build in the security requirements from threat modelling and policy, and did you fix what the scanners found? The trouble is that the same spec, tools, tests and environment, handed to different developers and their AI agents, produce different code — different functions, data structures and call paths. The result is a different, unpredictable attack surface every time.

**The cause:** two things drive this. First, the code is the real architecture — not the diagram in a threat model or spec, which is only a statement of belief. Second, that architecture is the sum of thousands of small choices made by the people and AI models writing the code, and those choices vary with training, context and values (see Simon Wardley's work if you're wondering whether AI can have 'values', and why it matters). So securing software isn't about making each component predictable; it's a control problem. The job is to find the hazardous states and build a control structure — across the developer, the AI, the IDE and the CI/CD pipeline — that keeps the system out of them. The system being controlled is the codebase and its deployment.

**The solution:** make software consistently harder to attack by having the IDE bind the AI's output in real time, and the CI/CD pipeline act as a default-blocking gate — a violated constraint can't reach production except through an exception that is scoped, logged and time-limited.

## Proposal: a hierarchical control structure
Higher levels set constraints on the levels below; lower levels feed information back up.

<div data-excalidraw="/assets/img/2026-05-24/fig1-control-structure.excalidraw" data-alt="The control structure. Each controller constrains the one below and feeds observations back up."></div>

*Figure 1 — The control structure. Each controller constrains the one below and feeds observations back up. The AI harness generates code but can't be trusted to enforce, so the deterministic CI/CD gate is the backstop before the controlled system.*

1. **The controllers**
Every controller, human or machine, works from a process model — its picture of the system's current state — and incidents happen when that picture drifts from reality.

    - **The developer** holds the intent, but their picture can be wrong (e.g. misreading how input is sanitised).
    - **The AI harness** writes and reviews code from its training and prompt context, so a wrong picture means unsafe suggestions. Guardrails and prompt constraints are its first control loop — but it's a black box: its reasoning is opaque and probabilistic, so we can't rely on it to enforce anything. That's exactly why it needs a deterministic supervisor downstream.
    - **The IDE** is the fast feedback loop and the boundary between the developer/AI and the code. As code is written it checks constraints locally, and when something unsafe appears (say, a vulnerable SQL query) it pushes back immediately - correcting the developer's and the AI's picture before anything is committed.
    - **The CI/CD pipeline** is the deterministic gate. It doesn't matter whether the developer misunderstood something or the AI imagined it was safe: the checks run, and a violated constraint fails the build. This isn't a physical, unbreakable barrier — it's software the organisation owns — so the point isn't that it can't be bypassed, but that bypassing it is a deliberate, logged, expiring exception rather than an invisible one.

2. **The constraints they enforce**
Instead of 'we'll scan for vulnerabilities', we state explicit constraints, and the controllers exist only to enforce them.

    - **constraint 1:** untrusted data must never reach a database without parameterisation.
    - **constraint 2:** no hardcoded secrets in the source repository.
    - **constraint 3:** the AI must not commit changes to authentication logic without a second, human sign-off.

3. **A constraint you can't scan for**

The three constraints above are the easy kind — a tool can already check each one. The real test is a constraint no scanner reliably catches. Take broken object-level authorisation (IDOR / BOLA), the most exploited class of web and API bug: one user reading or changing another user's records. No scanner finds it reliably, because the missing authorisation check is the bug, and whether a check is even needed depends on what the data means.

Start with the loss, not the control. The loss is a user reading or changing another user's data; the hazard is any request handler that returns or changes a record without checking that this user is allowed this record; so the constraint is that every access to a user-owned resource must carry an authorisation decision tied to the user, the record and the action.

You can't prove that with a scanner — whether each handler got it right is a question of meaning, not syntax. So don't try to catch the missing check; design it so the check can't be skipped. Route all data access through one gate that won't run a query unless you tell it who's asking and what they're touching. An unscoped query no longer compiles, and 'forgot to authorise' stops being a state you can reach. It's the same trick as constraint 1 — where parameterisation makes injection impossible to express — lifted to a harder problem. Each layer then enforces the part it can check, and CI/CD proves deterministically that no code reaches data outside the gate.

<div data-excalidraw="/assets/img/2026-05-24/fig2-authorisation-gate.excalidraw" data-alt="Designing the hazardous state out. Every data path runs through one authorisation gate that needs the principal, the record and the action."></div>

*Figure 2 — Designing the hazardous state out. Every data path runs through one authorisation gate that needs the principal, the record and the action; an unscoped query has no path to the database — it doesn't compile.*

What no gate can prove is whether the rules themselves are right — 'an order is readable by its owner and by same-region support staff'. That's a human judgement. But the payoff is that the judgement now lives in one small, reviewable place instead of being scattered across every handler. The structure doesn't remove trust in the AI; it shrinks the surface that trust has to cover. Two things keep the gate honest: it must fail closed, so an unrecognised data-access path breaks the build, and it must reject 'system' callers that quietly switch authorisation off.

The general move is repeatable: take a constraint you can't check directly, find a structural version you can, push it down until the unsafe state can't be expressed, and keep human review for the small part that's left.
