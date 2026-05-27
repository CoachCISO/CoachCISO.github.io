---
id: 04
title: 'Cyber security is trapped in tactics'
date: '2026-05-27T00:00:00+00:00'
author: 'Simon Goldsmith'
layout: post
excalidraw: true
categories:
    - Articles, Cybersecurity, Strategy
---
# Cyber security is trapped in tactics

<div data-excalidraw="/assets/img/2026-05-27/enterprise-security-control-hierarchy.excalidraw" data-alt="enterprise-security-control-hierarchy"></div>

*When security teams treat security as a control problem rather than a failure problem, they can shift from a tactical inspection and response team to architects and engineers dynamically making the right things easy and the wrong things hard.*

We are spending more money, hiring more people, and deploying more tools than ever before. Yet those rising costs are themselves a sign that we are not making satisfactory progress.

The problem isn't our funding; it's our philosophy. By relying on the market for solutions, we have fundamentally mistaken tactics for strategy. We have adopted a strange hybrid of a military combat mentality — focused almost entirely on threats and lines of defence — and 1920s-style factory quality inspection, rather than focusing on the systemic complexity embedded within our organisational structures and technology stacks.

Manufacturing and safety engineering settled this argument: you cannot inspect quality and safety into a product, you have to build it in to the organisational wiring at every layer down to machine-level observability.  The quality movement moved from catching defects at the end of the line to processes that made defects rare in the first place. Security is relearning the same lesson the hard way — and most of us are still standing at the end of the line, clipboard in hand.

Tactics focus on *how* to defeat a specific action; strategy focuses on *outcomes* — specifically, in security's case, preventing losses due to intentional actions by malicious actors while simultaneously achieving the organisation's reasons for existing. When you audit what most security teams actually do day to day, it is an endless loop of tactical firefighting masquerading as a strategic roadmap.

---

## Example 1: GRC automation vs hierarchical control

Governance, Risk, and Compliance (GRC) is a prime example of tactical substitution.

* **The tactical reality:** Too often, GRC functions drift into administrative hubs — automating existing, siloed compliance checklists, chasing evidence logs, and managing static risk registers. This is fundamentally a bottom-up, asset-centric approach that treats risk as a linear accounting problem.

* **The strategic imperative:** Strategy would define a top-down, hierarchical control structure that explicitly wires system constraints from the board down to the codebase. Security risk needs to stop being static entries in a register reviewed in committees; it should be a measurable emergent property from how an organisation decides and acts. A strategic approach establishes continuous control loops between organisational layers. The board enforces risk appetite and policy; executives translate this into architectural constraints; and engineering builds mechanisms to enforce those constraints. Automating a broken, fragmented process in a compliance or risk workflow automation tool only scales the security theatre. 

**Where to start:** take one line of your board's risk appetite and trace it, on a single page, to the architectural constraint that expresses it, the engineering control that enforces it, and the feedback loops that prove the control is implemented and works. If you can't draw that loop for even one risk, your control structure is still aspirational.

---

## Example 2: Vulnerability management vs systemic architecture

Our industry’s obsession with the vulnerability management lifecycle is another tactical trap — the _Mythos Vulnerability Storm_ is a prime example.

* **The tactical reality:** Security engineering teams pour millions into scanners, pentesting and red teaming to find, prioritise, and patch vulnerabilities faster. We treat the software flaw as the proximate cause of a potential loss. This is an increasingly losing game of trying to inspect security into technology. It implicitly relies on the assumption that we can predict, find and remediate flaws before adversaries exploit them. It also ignores that many attacks exploit the interactions between components rather than a weakness in an individual component lacking a security control.

* **The strategic imperative:** True strategy embraces 'assumed exposure'. It shifts the engineering focus from the unwinnable goal of finding and fixing flaws and towards building a resilient system architecture. Strategy dictates that we design systems where systemic weaknesses are structurally hard to produce, reach and exploit. We do this by eliminating [unforgivable vulnerabilities](https://www.ncsc.gov.uk/report/a-method-to-assess-forgivable-vs-unforgivable-vulnerabilities), minimising the blast radius through strict architectural decoupling, implementing immutable infrastructure so recovery is fast and predictable, and ensuring core services persist even when individual components fail. Decoupling matters most where recovery isn't possible at all: you can rebuild a server, but you cannot un-leak data — so the real goal is to limit what any single compromise can ever reach.

Good architecture makes exploits and component failures operational nuisances, not an existential crisis.

**Where to start:** pick your highest-value data store and determine if authorisation is designed *in*, or is it left to audits and  scanners to find the absence of it. 

## Summary

To move from tactics to strategy, we should stop focusing on the adversary's next move and start focusing on our system’s organisation, operations and technology architecture — and on enforcing the behavioural constraints required to keep organisations secure.

It also changes how we judge security we don't own — a vendor, an acquisition, a portfolio company. Stop ticking off controls and counting  open findings; ask whether their and your architecture makes failure survivable, create incentives in contracts and turn assumptions into written agreements to reinforce desired behaviours. While it is harder to scale operator judgement than inspector checklists - it is a more useful question to answer.

<a href="https://www.linkedin.com/pulse/cyber-security-trapped-tactics-simon-goldsmith-nuqne/" target="_blank" rel="noopener" aria-label="Read this article on LinkedIn"><i class="fab fa-linkedin"></i></a> This article was published on [LinkedIn](https://www.linkedin.com/pulse/cyber-security-trapped-tactics-simon-goldsmith-nuqne/), please give it a like or share there, or even better let me know what you think.
