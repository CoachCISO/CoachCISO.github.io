# Why quantifying cost is our best defence
A plan for software security in the age of AI-generated code

## The hidden factory
Every software organisation runs two factories. The first is the one leadership sees: feature roadmaps, sprint velocity, deployment frequency, revenue. The second is invisible. It exists solely to fix things that were not done right the first time.

Your developers spend over 40% of their time in this hidden factory — fixing bugs, not building features.[^stripe] In 2020, paying down accumulated technical debt cost approximately $300,000 per year per million lines of code;[^cisq-2020] anecdotal evidence since the AI coding era began suggests the number has worsened. A defect that costs $1 to prevent in design has historically been quoted as costing $10-$100 to fix in production; the order of magnitude holds across decades of industry data, though modern iterative environments may have been able to compress the ratio towards 1:6–1:15.[^boehm-jones]

The Consortium for Information & Software Quality estimated $2.41 trillion in annual US costs from poor software quality in 2022. That figure spans operational defects, downtime, and accumulated technical debt as well as security defects.[^cisq-2022] I have yet to see an organisation that has analysed their own cost of poor quality, yet alone one where the security share has been cleanly disaggregated.

The manufacturing world has a name for the overall cost: the Price of Non-Conformance (PONC). Philip Crosby, the quality theorist who coined the term, estimated it at 20–25% of revenue for manufacturers and up to 35% for service organisations.[^crosby] He also demonstrated that preventing defects was costing just 3–4% of sales.[^crosby] Following Crosby's analysis, leading manufacturers shifted their investments in quality, improving profit, quality and velocity.

No per-firm PONC calculator exists for software security. Some organisations track the arrival of customer-facing defects and a few of those include security defects in these numbers. But the true cost of security defects is the single largest gap in how security communicates with leadership. Faced with this challenge, some security leaders may say:
> "we are quantifying the risk".

Security leaders rightly recognise that executives and boards do not generally know how to act on vulnerability and misconfiguration reporting. But quantified risk reporting is also not the magic bullet that frameworks and standards promise. There are a lot of reasons for this, but a leading cause is that if decision-makers are faced with a guaranteed loss (e.g. investing in a security control or slowing down releases) and a chance to lose nothing, they will often gamble on the uncertain, larger loss to try and avoid losing anything at all. Ultimately, humans are loss-averse and risk-averse when it comes to securing gains or taking guaranteed minor hits, *but* they become risk-seeking when trying to avoid a sure loss completely.

Therefore, until security teams can express their PONC — the rework hours, the remediation costs, the tool licensing, in addition to the potential and actual breach losses — ideally as a percentage of engineering budget, investment decisions will remain anchored in debates of opinion and theoretical metrics and security will remain an inspection-focused activity.

Except now, the gap is about to become a crisis. AI is generating code faster than any organisation can inspect it, and AI is discovering vulnerabilities faster than any organisation can patch them.

---

## The losses this plan prevents
Before discussing controls, let's name the losses. This plan exists to reduce, in priority order:

1. **Loss of customer data at scale** — material PII or financial-data exfiltration that triggers regulatory reporting and contractual breach.
2. **Loss of service availability beyond agreed thresholds** — let's assume the 4-hour mark for revenue-critical services.
3. **Loss of regulatory authorisation** — operating licence withdrawal, certification suspension, or material reporting obligations under SEC, EU DORA, UK NIS, or sector regulators.
4. **Loss of payment or transaction integrity** — unauthorised movement of money or assets, or repudiable transactions at scale.
5. **Loss of intellectual property** or strategic information to exfiltration.

PONC is the *cost measurement*. These five are the *outcomes* the rebalancing must reduce. The prevention-ratio KPI proposed below fails its purpose if it improves while these losses do not.

---

## Why AI breaks the inspection model
In April 2026, Anthropic demonstrated that its frontier model, Claude Mythos Preview, could autonomously identify and exploit zero-day vulnerabilities in every major operating system and every major web browser — including bugs that had survived 16–27 years of expert human review and automated testing.[^anthropic-mythos] Engineers at Anthropic with no formal security training asked Mythos to find remote code execution vulnerabilities overnight. The cost of an individual successful vulnerability discovery run: under $50.[^anthropic-mythos]

Anthropic is a vendor with a direct commercial interest in selling frontier-model access; their own capability disclosure should be read accordingly. The UK's AI Safety Institute (AISI) independently evaluated Mythos and confirmed the headline capabilities under controlled cyber-range conditions.[^aisi-mythos] On expert-level capture-the-flag tasks, Mythos succeeded 73% of the time — the first model ever to complete expert-level CTFs, a milestone that no model could meet before April 2025. On a realistic 32-step corporate network attack simulation (estimated at 20 human hours to complete), Mythos completed the full chain in 3 of 10 attempts and averaged 22 of 32 steps. The previous best model, Claude Opus 4.6, averaged 16. AISI's cyber-range evaluation ran to a 100-million-token budget.

There is not yet published in-the-wild breach data showing Mythos-class capability being used against production targets. The structural argument here therefore does not depend on Mythos specifically — it depends on the trajectory across multiple frontier labs documented by NCSC and AISI: two years ago, the best models could barely complete beginner-level cyber tasks; by April 2025, the first expert-level CTF completions arrived; by April 2026, multi-stage corporate network attacks ran end-to-end. AISI's conclusion:
> "Mythos Preview can exploit systems with weak security posture, and it is likely that more models with these capabilities will be developed."[^aisi-mythos]

From an attacker's perspective, AI has not yet reduced the human capacity and skill requirements to exploit well-defended systems. However, for middle-tier attackers already capable of monetising system exploitation, AI has demonstrably lowered the bar for finding and exploiting exposed weaknesses.

Weakly defended organisations, and people responsible for software with large technical debt, must consider themselves immediately at risk — irrespective of the general availability of the frontier models and coding tools. Three reinforcing dynamics drive the urgency:

1. **The capability ceiling is rising fast.** Each new generation completes substantially more attack steps than the best model 18 months prior. Mythos outperformed Claude Opus 4.6, GPT-5.4, and every other frontier model AISI tested.[^aisi-mythos]
2. **Running these attacks is getting cheaper.** The same model with more processing time reliably improves results. The limiting factor is increasingly funding, not expertise — and we should expect the economic flywheels of criminal groups to spin up faster as a result.
3. **The capabilities will proliferate.** AISI and the frontier laboratories themselves note that capabilities developed in frontier models are being transferred into smaller, cheaper, or open-weight models through distillation — meaning advances at the frontier set the direction of travel for the whole ecosystem.[^ncsc-frontier]

This has a direct implication for how we build software. AI-generated codebases are growing at rates that make even AI-augmented inspection structurally insufficient as a *primary* strategy. The standard rebuttal is that defenders can run that same AI-augmented inspection at $50 a time. That is true, and it misses where the money goes. Inspection is a bill you pay again every time the code changes: every commit, every new repository, every model that writes more code adds to what must be re-checked, and the bill never stops growing. Prevention is a bill you pay once. An organisation that moves to memory-safe defaults, or wires a guardrail into the pipeline, pays at the point of adoption — and every line of code written afterwards inherits the protection at no extra cost. As code volume climbs, the inspection bill climbs with it; the prevention bill does not.

This is an economic argument, not a niche or technical one. And it has been understood in manufacturing for 70 years.

---

## The factory floor lesson
In the early 20th century, manufacturing operated on the Taylorist model: engineers designed the work, managers supervised it, workers performed it, and a separate inspection department caught defects before products shipped. Quality was someone else's problem.

At first, the application security industry replicated this bifurcation exactly. Developers ship features. The application security team inspects afterwards. There have been incremental improvements since, including threat modelling and moving the inspection to earlier in the software development lifecycle (variously known as 'shift-left', 'security by design' and 'DevSecOps'). By most measures, the outcomes have been disappointing:
- 82% of organisations use between 6 and 20 security testing tools[^black-duck-2024]
- 60% report that between 21% and 60% of their security test results are noise — false positives, duplicates, or conflicts[^black-duck-2024]
- 86% report that security issues delay software releases at least occasionally[^sembi-2026]
- 81% of development teams knowingly ship code with vulnerabilities[^checkmarx]

These figures come from vendor surveys with predictable response bias and should be triangulated against breach-data sources. The Verizon DBIR, Mandiant M-Trends, and the NCSC Annual Review point in the same direction.[^dbir][^mandiant][^ncsc-review] The top exploited vulnerability classes in 2025 are substantially the same as in 2005. That is not a technical failure — it is a market failure that no amount of inspection will fix.

These are averages and within this data there are high-performing exceptions. But the data shows that the inspection apparatus is not just failing to prevent defects. It is generating friction that causes organisations to bypass it entirely. And even in high-performing teams the cost of achieving security may be much higher than it needs to be.

In the 1950s, engineers at Japanese manufacturers — with input from Deming, Crosby, and Juran — demonstrated that the root of quality failure was not bad workers but bad systems. Most defects had systemic causes that no amount of end-of-line inspection could fix. The solution was not more inspection. It was redesigning the system to prevent defects from being created. Toyota synthesised this into the Toyota Production System (TPS), and some of the techniques have evolved and been incorporated into modern software management systems including Lean, Agile, and DevOps.

The relevant lesson is the result, not the history: Toyota didn't abandon inspection. It integrated prevention, inspection, and continuous improvement into a single system owned by the people doing the work. The Taylorist separation — quality is someone else's department — is what it eliminated.

For the full historical context including Deming's variation theory, Crosby's Zero Defects, Juran's Quality Trilogy, and a Crosby-Juran debate, see Appendix A: The Quality Revolution.
![The Quality Revolution](/assets/img/2026-06-20/Quality Revolution.png)

---

## The economics of the hidden factory in software
A manufacturing quality framework does not just apply to software. It is amplified by five properties unique to digital systems.

1. **A single defect replicates at internet scale.** In manufacturing, a defective batch affects a bounded number of units. In software, a single vulnerability in a centralised platform is instantly replicated across the entire user base. The PONC does not add up unit by unit — it multiplies. The SolarWinds supply chain compromise affected up to 18,000 organisations from a single point of failure.[^solarwinds]
2. **The tempo mismatch is structural.** Security teams plan on annual budget cycles. They improve reactively, after incidents. Their information about threats diffuses through annual reports and quarterly briefings. The adversary iterates weekly, shares TTPs through forums in days, and exploits the gap between a vulnerability's disclosure and its remediation in hours.
3. **This tempo mismatch cannot be fixed by buying better tools.** An organisation with a $10 million security budget that operates on annual planning cycles is, from the attacker's perspective, slower to adapt than a solo ransomware affiliate running commodity tools who reads the forums every morning.
4. **AI-generated code is outpacing inspection.** Independent tests have found that 40–45% of AI-generated code contains at least one security weakness.[^ai-code] Fewer than a quarter of developers run security analysis on the AI-generated code they accept.[^snyk-ai] The volume of code entering production is growing faster than inspection tooling can absorb it. When code is generated at machine speed, inspection has to keep pace with all of it, and that bill grows with every line. Prevention does not work that way: once the substrate is in place, new code inherits it at no extra cost. You pay for inspection by the line; you pay for prevention once.
5. **The adversary has AI too — and the manufacturing analogy partially breaks here.** Toyota's competition was other manufacturers, not actors actively trying to break its cars. Toyota's poka-yoke removed variance from a system that did not fight back. Software prevention controls operate against adversaries who target them specifically. The honest argument is therefore not that prevention eliminates risk — it is that prevention raises the adversary's marginal cost of finding exploitable weakness, and shifts the hazard surface to classes that are harder to monetise at scale. In 2024, 512,847 malicious packages were detected in open-source software supply chains — a 156% year-on-year increase[^sonatype] — evidence that adversaries will move to whichever attack surface remains exposed. The UK NCSC and AISI assess that defenders "should assume that at least some attackers already have access to capable AI tools" and that, "in specific cyber tasks, models already exceed what a skilled practitioner could achieve at lower cost."[^ncsc-frontier]

This means inspection alone is insufficient. The inspection-dominant system must be rebuilt around prevention, detection and correction — not as the Taylorist inspection department of old, but as an integrated feedback loop wired into all levels of the organisational hierarchy. Prevention reduces what needs detecting. Detection informs what needs preventing, not just what needs to be fixed. Correction both limits damage and ensures the system learns and improves.

---

## The missing pillar — incentives
Mechanism changes do not, on their own, fix incentive misalignment. Ross Anderson's foundational claim in *Security Engineering*: "if Alice guards a system and Bob pays the cost of failure, you can expect trouble."[^anderson] Cybercrime has remained structurally stable through three decades of mechanism shifts — anti-virus, firewalls, SAST, DAST, EDR, zero-trust, SBOMs — because none of those changed the underlying incentive structure. A fourth mechanism, sold the same way, should not be expected to either.

But the argument of this paper is not another mechanism. It is a measurement — and measurement is itself an incentive intervention, because the largest single piece of the misalignment is not Alice-and-Bob at all. It is that Alice cannot see what failure is already costing *her*. The hidden factory is not an externality: the 40%-plus of developer time lost to rework, the remediation hours, the tool sprawl, the bypassed releases — those costs fall on the firm today, in full, and are tolerated only because nobody totals them. That was Crosby's entire insight: quality looks expensive and waste looks free, right up until someone adds the waste up. Quantifying PONC closes that gap unilaterally — it turns a cost the firm already bears but cannot see into one it can act on, and once it is visible, prevention is the self-interested choice. No external party has to make it so.

What measurement cannot reach is the residual — the share of failure cost the firm genuinely offloads onto customers, partners, and the wider ecosystem. That is the true Alice/Bob externality, and no internal accounting corrects it, because by definition the firm does not pay it. Here, and only here, external correction does the work the economics cannot — and it is arriving regardless of whether any individual organisation reallocates voluntarily:

- The **EU Cyber Resilience Act** imposes design-time security obligations on manufacturers of digital products and components, with penalties scaled to global turnover.[^eu-cra]
- The **NCSC Cyber Assessment Framework v4.0** introduced a new principle (B4.a — Secure Software Development) requiring organisations to demonstrate prevention-by-design as evidence, not assurance-by-test as evidence.[^caf-b4a]
- **US Executive Order 14028** and successor secure-by-default mandates require federal suppliers to demonstrate prevention-by-construction practices.[^eo14028]
- **SEC cyber disclosure rules** require listed companies to report material incidents on a four-business-day timeline, internalising breach cost in equity value rather than absorbing it through reputation.
- **Insurance underwriting** is beginning to price PONC directly. Coalition's continuous external scanning lowered the share of policyholders carrying critical vulnerabilities from 17% to 9.7% through 2022 — a 43% relative reduction.[^coalition-2022]

These mechanisms do not replace the economic argument; they complete it. Prevention is already the cheaper option on internal waste alone — the case a firm can act on this quarter, without waiting for a regulator. What the externality-correction mechanisms add is a price on the residual that firms used to push onto others, moving the private optimum further towards prevention and putting a deadline on reaching it. So read the rest of this paper in that order: prevention pays first because it shrinks waste you are already funding, and pays again because the failures you currently externalise are about to land back on your own balance sheet.

There is also an internal principal-agent problem to solve. Engineering pays the migration cost (language adoption, IaC tooling, hardened images, IDE re-tooling). Security captures the visible benefit (vulnerability counts down, breach probability reduced). Customers and the broader ecosystem capture the rest. This is the Alice/Bob misalignment in miniature, inside one organisation. The Coasian fix is some combination of (a) internal transfer pricing — the security cost centre charges back to product teams for shared platform investments; (b) joint OKR ownership — the prevention-ratio KPI is co-owned by the CISO and the CTO/VP Engineering; (c) executive-level mediation — the CFO recuts cost centres so the budget moves with the responsibility. Pick one explicitly before launching the programme. The action plan returns to this below.

---

## The Andon Cord for code
The quality revolution is not something the software industry must invent. Significant elements are already operational — unevenly distributed, but proven.

### Mistake-proofing the pipeline
**Poka-Yoke.** Toyota's principle of making it structurally impossible to do a task incorrectly already exists in software development:
- **Memory-safe languages (Rust, Go)** eliminate buffer overflows and use-after-free vulnerabilities by construction *for new code in those languages*. Google reported a 1,000x reduction in memory-safety vulnerability density when moving from C/C++ to Rust. Android memory-safety bugs now account for less than 20% of all vulnerabilities, down from 76% six years earlier.[^google-rust] Legacy code requires a different play — see "Brownfield vs greenfield" below.
- **Parameterised queries** eliminate SQL injection. Content security policies prevent XSS. These are not inspection tools — they are constraints that make entire vulnerability classes impossible to express.
- **Infrastructure-as-Code (IaC) and hardened images** displace system-configuration mistakes upward to template authoring, where they can be audited once and applied many times. The IaC template implements, enforces, and provides evidence of policy natively in code; hardened base images ensure only the minimum required, pre-verified artefacts are built and loaded into runtime. As a useful by-product they make fixes for discovered defects more efficient and automatable.
- **Reasoning in the IDE (emerging — track, don't yet rely on).** IDE-level agents that reason about security policy at commit time are the most promising emerging Poka-Yoke control, but they are not yet a dependable prevention mechanism. Black Hat USA 2025 audits of AI-powered developer tooling found exploitable vulnerabilities in every tool tested, including a configuration-injection issue in the most-installed AI code-review app on GitHub that leaked its GitHub App private key for over a million repositories.[^kudelski] Until governance, policy-corpus ownership, and supply-chain assurance for these tools mature, the 90-day plan depends only on the currently shippable mechanisms above. Treat IDE-level reasoning as a 2027–2028 prevention pillar, not a 2026 one.

These are not incremental improvements to the inspection process. They are defect elimination by construction — for the construction substrate you control.

A reasonable objection: the Stanford HAI study found developers using AI assistants wrote measurably *less* secure code while being *more* confident in its security.[^ai-code] That result directly cuts against *advisory* prevention (the AI tells the developer something is wrong; the developer ignores or misinterprets). It does not cut against *structural* prevention (parameterised queries, memory-safe languages, hardened images, policy-as-code). The case made here is the structural one — change the construction primitives so the defect cannot be expressed — not the advisory one.

### Stop the line
**Jidoka.** Toyota's principle of stopping the assembly line when a defect is found maps directly to repo and CI/CD security control planes. When a merge request is blocked by a failing security policy, that is 'stop the line.' Teams that embed automated security scans inside CI/CD pipelines detect 60% more critical issues before deployment than teams running ad-hoc or late-lifecycle checks[^gitlab-2024] — though that is a result for better inspection, not for prevention specifically. Coalition's 43% year-on-year reduction in critical-vulnerability share is also a better-inspection result.[^coalition-2022] The distinction matters: a CI/CD gate that runs a scanner and flags findings for human triage is inspection moved earlier. A CI/CD gate that enforces a policy constraint and halts deployment if violated is Jidoka. Both improve outcomes; only the latter raises the adversary's marginal cost.

### Continuous improvement
**Kaizen.** Toyota's culture of small, incremental, systematic improvement maps to structured DevSecOps maturity models. The Building Security In Maturity Model (BSIMM) tracks more than 120 activities across 12 practices.[^bsimm] Organisations with mature DevSecOps practices saw average breach costs roughly $1.7m lower than organisations with little or no DevSecOps adoption.[^ibm-breach] This is evidence that inspection-led DevSecOps is working — and should be read carefully: the argument here is not that inspection has failed. It is that the marginal returns on additional inspection are now diminishing as AI-generated code volume rises, while the marginal returns on prevention have not yet been captured.

### Observe the real work
**Go Gemba.** Toyota's principle of observing work as done, not as imagined, maps to chaos engineering, runtime observability, and developer workflow analysis. You cannot improve a process you have not seen operate in reality.

### Zero defects for what you can automate away
The debate between Crosby (Zero Defects — prevent everything) and Juran (optimise — focus on the 20% of causes creating 80% of problems) resolves differently for different vulnerability classes.

Apply Crosby's zero defects to vulnerability classes eliminable by construction:

- Memory safety → Rust, Go (eliminates roughly 70% of serious vulnerabilities in legacy C/C++ codebases such as Chromium)[^google-rust]
- Injection → parameterised queries, type-safe ORMs
- Misconfiguration → Infrastructure-as-Code with policy-as-code
- Known-bad dependencies → automated supply chain verification, reproducible builds

Apply Juran's optimisation to classes requiring ongoing detection:

- Logic vulnerabilities and authentication bypasses
- Business logic flaws
- Novel adversarial techniques
- Supply chain attacks using legitimate packages

The investment question for every organisation: what percentage of your security budget targets vulnerability classes that could be eliminated by construction, vs classes that genuinely require detection? For most organisations, the answer is that the vast majority of spend is on inspection of preventable defects.

---

## Brownfield vs greenfield — where each strategy applies
The Google Android Rust migration (76% → <20% memory-safety bugs over six years) is the cleanest empirical anchor for prevention-by-construction. It is also a hyperscaler case: a vertically integrated platform owner, a monorepo, and effectively unlimited platform-engineering capital. Most enterprise PONC sits in code that does not look like Android. A useful tiering:

- **Greenfield new code.** Zero Defects line in the sand: non-memory-safe languages off by default; IaC + policy-as-code mandatory; hardened base images required. This is where the construction substrate is changed. Cost: in line with Crosby's Price of Conformance (POC) estimate of 3–4% of engineering investment for greenfield discipline.
- **Brownfield active code — the top 20%.** Pareto targeting. The 20% of legacy services carrying 80% of the security-relevant defect mass get scheduled rewrites onto the new substrate. Expect 5–10 FTE-years per major service, sustained over multi-year horizons. This is the long curve and it cannot be compressed into a 90-day action.
- **Brownfield active code — the long tail.** For services where a rewrite is uneconomic, the prevention play is **structural containment**: segmentation (east-west and identity-tiered), sandboxing, hardware-assisted mitigations (ARM MTE, CFI), aggressive blast-radius reduction, and rebuildable disposable infrastructure. This is closer to Anderson's "fail safe" and "recoverability" principles than to Toyota's poka-yoke, and aligns with the Day-Zero Normal CISO playbook[^day-zero-normal]: accept the exploit-to-patch gap as permanent and engineer to contain its consequences.
- **Brownfield deprecated code.** Asset-truth-driven decommissioning with a named sunset date. Continue to inspect; do not migrate.

The full prescription is: **prevention-by-construction for new code; targeted migration for the legacy hot spots; structural containment for the legacy long tail; sunset for the rest.** Treating "memory-safe migration" as the universal answer overshoots; treating "segmentation" as the universal answer undershoots. The two strategies are complementary across the portfolio.

---

## Why we cling to inspection
If the economics are this clear, why does the security industry remain obsessed with inspection? Because checking a box feels like security — even when it isn't.

### The placebo of compliance
The illusion of control is a cognitive distortion in which individuals believe they have more influence over unpredictable events than they actually do. In software security, this manifests as security theatre: passing a certification, increasing dashboard coverage, discovering a vulnerability, writing a policy. These activities provide comfort. They do not provide control.

Organisations believe that if they aggregate enough observability tools into a single platform, they achieve security. In practice, this produces point-in-time snapshots of a system that is changing continuously — passing a checklist while missing the changes underneath.

### The confidence trap
There is also a related cognitive bias: the belief that technical expertise protects against uncertainty. Security experts with deep knowledge are often the most qualified to assess risk — but their confidence can lead them to treat genuine unpredictability as manageable risk. Unmeasurable, hard-to-execute threats get classified as 'low probability' rather than 'unknown'. Weakness reduction gets deferred as a risk trade-off rather than operationalised as a security imperative.

### The prevention apparatus has its own failure modes
To pre-empt the symmetric objection: the proposed prevention apparatus will produce its own friction-and-bypass dynamics if not designed and governed carefully.

- A hardened image that omits a required runtime binary breaks production. The prevention control becomes the outage.
- An IaC default-deny rule applied during an active incident can take down the service the responder is trying to recover.
- A CI/CD policy gate can block deployment of a security patch because the patch trips an unrelated rule.
- An IDE auto-fix can substitute one defect class for another.
- A Zero Defects policy applied to a legacy codebase before a migration path exists freezes development without reducing the defect.

Prevention controls therefore need their own override authority, exception process, and review cadence — exactly the governance machinery the inspection apparatus has spent twenty years failing to build. The argument here is not that prevention is monotonically good; it is that prevention shifts the failure mode from "defects shipped" to "controls misapplied," and the second is a more tractable problem because the controls are under your authority and the defects are not.

The result of these dynamics is that the industry defaults to activities that feel rigorous (scanning, testing, auditing) over activities that are economically superior (preventing defects by construction) because the former produce visible artefacts and the latter produce the absence of problems — which is harder to measure and easier to resist or cut from the budget.

---

## A 90-day action plan for security and engineering leaders
The reallocation question is the wrong place to start. Start with measurement, then let the rebalancing fall out of the data.

1. **Measure your PONC, narrowed and scoped.** A defensible enterprise-wide PONC number takes 9–18 months of activity-based costing across fragmented finance data. Do not try to produce it in 30 days. Instead, in 90 days, produce a credible PONC estimate for **one product line or one business unit**, using: (a) developer time on bug fixes from issue tracking; (b) AppSec headcount and tool licensing; (c) incident response hours from ticketing; (d) breach-related costs from finance. Express as a percentage of that unit's engineering budget. Use it as the worked example for the board conversation and the template for wider rollout.
2. **Map your top three vulnerability classes by volume and cost.** For each class, identify whether a prevention intervention exists (memory-safe language, parameterised queries, IaC policy-as-code, hardened images, repo and CI/CD security control points). Cross-reference against the brownfield-vs-greenfield tiering so each class has a named strategy: Zero Defects, targeted migration, containment, or sunset.
3. **Propose a derived (not asserted) rebalancing.** Anchor the reallocation to expected loss reduction, not to a manufacturing analogy. Gordon-Loeb caps optimal information-security investment at approximately 37% of expected loss for any given loss event;[^gordon-loeb] the prevention share of that ceiling should be derived from the per-class cost-benefit, not picked. Disaggregate by Wardley evolution stage: IaC + policy-as-code (commodity, cheap, fast payback); type-safe data access (commodity); hardened images (commodity); memory-safe language migration (custom, expensive, slow); IDE-level reasoning (genesis, defer). The portfolio mix determines whether the rebalancing is 5%, 15%, or 30% of inspection spend — there is no universal number.
4. **Tier the plan by organisation size and tech stack.** SMB and PE-backed: IaC + hardened images + repo/CI/CD policy gates. Mid-market: add type-safe data access and policy-as-code coverage. Enterprise/regulated: add memory-safe-language migration roadmap on a 5–10-year curve, with structural containment for the long tail. A single recipe does not fit a Google-scale platform owner and a regional bank running 30-year-old C++.
5. **Name the kill list.** A reallocation that does not name what stops becomes a budget addition. Three concrete candidates most organisations should evaluate: (a) one overlapping SAST/DAST vendor — most enterprises run 2–3 and tolerate substantial overlap; consolidate or replace with AI-augmented PR-time review; (b) annual penetration testing as the *primary* assurance line — keep red-team/purple-team exercises but cut the compliance-pentest contract; (c) manual GRC artefact production where IaC + policy-as-code + automated evidence collection can replace it. The principle is that something must give way.
6. **Co-own the prevention-ratio KPI.** Make this a joint CISO–CTO/VP Engineering metric. The spend that moves it sits in engineering; the responsibility for it sits in security; the budget mediation sits with the CFO. Without explicit co-ownership the KPI fails on accountability alone.
7. **Establish a leading indicator at 90 days.** PONC reduction is a lagging metric — 4–6 quarters minimum. The leading indicator that tells you the programme is alive: **percentage of new repositories created on the prevention stack** (memory-safe language by default, IaC + policy-as-code, hardened base image, signed dependencies). This is measurable in week one, requires no PONC baseline, and gives the board an early signal at the first quarterly review.
8. **Set the performance standard and iterate.** Zero Defects policy for vulnerability classes eliminable by construction, applied to new code, with a credible amortisation curve for legacy. Track the prevention-ratio KPI quarterly. Review the override cadence on the prevention controls themselves at the same quarterly rhythm — exceptions granted, exceptions sunset, exceptions converted into policy.

The board conversation you want on this plan:

> "Our PONC for [product line / business unit] is currently [X]% of engineering budget for that unit. Of that, [Y]% is fixing defects that could be prevented by construction. We recommend an initial [Z]% reallocation towards prevention investments — IaC policy-as-code and hardened images first, followed by a multi-year memory-safe-language migration roadmap for our highest-risk legacy services. It also happens to be the lowest-cost path to compliance with the EU Cyber Resilience Act, US EO 14028, and the new NCSC CAF Secure Software Development principle. We will know the programme is working in 90 days by the percentage of new repositories created on the prevention stack, and we will report PONC trend at each quarterly review."

The parallel CTO / VP Engineering conversation:

> "We currently lose [N] FTE-equivalent capacity to the hidden factory — developers fixing bugs rather than building features. We propose reallocating that capacity to prevention investments that compound: IaC and policy-as-code first, then memory-safe-language migration for our top three highest-risk services on a five-year curve. The platform-engineering ask is [headcount and tooling]. Joint OKR ownership of the prevention-ratio KPI with the CISO. The CFO mediates the budget recut."

Boards do not respond to vulnerability counts. They might accept risks. But they respond to costs — and increasingly, to regulatory exposure.

A final note on community. Defenders are being out-collaborated by attackers. Where operators get together to share playbooks and tactics, maturity improves across the community. This is a cultural posture rather than a 90-day action. If you struggle to find the right community, start one.

---

## From compliance to execution intelligence
The next generation of software security is a convergence of manufacturing quality principles, AI-powered execution, and the externality-correction mechanisms that are reshaping software liability. It moves beyond 'shift left' (testing earlier) and even 'shift down' (embedding security in platforms) to security as an integrated property of the tools and processes that build and execute code.

When AI agents write code, review code, deploy code, and discover vulnerabilities in code, security cannot remain a human-operated inspection department bolted onto the side. It must be wired into the execution path — the constraints that prevent defects, the gates that enforce policy, the controls that contain blast radius for the inevitable failures, and the feedback loops that inform the next generation of preventive design.

The race is not between prevention and inspection. It is between organisations that integrate both into a coherent security system — where prevention reduces the attack surface, intelligent inspection verifies the residual, structural containment limits the consequences of what gets through, and rapid response informs the next iteration — and organisations that remain trapped in the Taylorist bifurcation, running faster on the inspection treadmill while AI generates code and attackers discover weaknesses at machine speed around them.

---

## How to test this thesis
A framework that cannot be disproved is advocacy, not analysis. The following testable conditions would challenge the core claims. Each names a metric, a threshold, and a measurement window, so that any organisation, researcher, or competitor can run the test.

1. **Existing DevSecOps is sufficient.** *If* high-maturity DevSecOps organisations (BSIMM Level 3+) sustainably outperform prevention-by-construction adopters on major-incidents-per-$bn-revenue-per-year, measured on a three-year rolling window with consistent severity criteria, *then* the inspection-improvement path is sufficient and the rebalancing recommendation is unnecessary.
2. **AI inspection keeps pace with AI generation.** *If* the total cost of AI-augmented inspection grows more slowly than the volume of code it must cover across a 24-month window — so the cost per line inspected falls rather than holds or rises — *then* the claim that inspection cannot keep pace is empirically refuted and the cost argument collapses.
3. **Platform-level prevention controls work in practice.** *If* the rate of CVSS-≥7 vulnerabilities per million lines of code, in services that have adopted memory-safe languages and IaC policy-as-code, has not declined by at least 50% over a 24-month window post-adoption, the Poka-Yoke prescription is invalidated.
4. **Prevention controls don't just shift the hazard surface.** *If* organisations adopting prevention controls see compensating new defect classes (e.g. supply-chain compromise rates, logic-flaw rates, IDE-agent abuse) emerge at comparable cost-of-non-conformance to the classes the prevention controls displaced, *then* prevention is shifting the hazard surface rather than eliminating it, and the framework needs to be reweighted towards structural containment.
5. **The systemic-vs-individual ratio generalises from manufacturing.** *If* causal analysis of major software-security incidents over a 24-month window shows that individual-actor causes (untrained developer, ignored alert, deliberate bypass) account for the majority of root causes — rather than systemic causes (substrate, defaults, incentives) — *then* Deming's directional insight applies less strongly to software security than this paper assumes, and individual-focused interventions retain more value than the framework grants.

These are testable propositions. The research community is invited to test them, and the author commits to revising the framework against published results.

---

## Appendix A: the quality revolution
### Frederick Winslow Taylor and scientific management
Taylor's principles (early 1900s) rationalised mass production by breaking complex tasks into simple steps. Planning was separated from execution. Independent inspection departments caught defects after manufacture. This created a reactive culture where quality belonged to the inspector. The security industry replicated this model: developers ship features, infosec inspects afterwards.

### W. Edwards Deming and variation theory
Deming demonstrated that most quality failures are systemic (common causes) rather than individual (special causes). His work in post-war Japan enabled Toyota and other manufacturers to leapfrog American competitors within five years. Deming estimated that workers cause approximately 6% of quality problems while 94% are inherent in the system.[^deming] Though this specific ratio has not been validated for software security, the directional insight is well supported — the persistence of buffer overflows for 35 years, and the proportion of codebases shipping with known critical vulnerabilities, both point to systemic roots.

### Philip Crosby: *Quality is Free*
Crosby argued that the cost of preventing defects is always lower than the cost of finding and fixing them. His four principles:

1. Definition: Quality is conformance to requirements.
2. System: Prevention, not appraisal.
3. Performance standard: Zero Defects (not 'close enough').
4. Measurement: The Price of Non-Conformance (PONC).

PONC includes rework, scrap, warranty claims, inspection costs, and administrative overhead. Crosby estimated PONC at 20–25% of revenue for manufacturing, up to 35% for services. The Price of Conformance (prevention investment) he estimated at 3–4% of sales[^crosby] — a figure that applies to greenfield manufacturing with mature quality discipline, not to retrofit migration of decades-old systems. In software the directional argument is similar but the brownfield migration cost dominates the early years of any adoption curve.

### Joseph Juran: the Quality Trilogy
Juran defined quality as fitness for use rather than conformance to specification. His Quality Trilogy — Quality Planning, Quality Control, Quality Improvement — mirrors financial management (budgeting, control, improvement). His Cost of Quality framework divides costs into: Prevention Costs (investments to stop defects), Appraisal Costs (testing and verification), Internal Failure Costs (catching defects before shipping), and External Failure Costs (breach, recall, reputation loss).

Juran and Crosby disagreed on the feasibility of zero: Juran argued that approaching zero defects produces exponentially increasing costs and advocated for Pareto-style prioritisation. Crosby countered that accepting any defect level was a trap that allowed managers to tolerate failure. In security, both are right: apply Crosby's Zero Defects to classes eliminable by construction (memory safety, injection, misconfiguration) and Juran's optimisation to classes requiring ongoing detection (logic flaws, adversarial techniques).

### The Toyota Production System
Toyota synthesised Deming, Crosby, and Juran into an integrated system:

- **Jidoka (autonomation):** Stop the line when a defect is found — empowering any worker to halt production.
- **Poka-Yoke (mistake-proofing):** Physical constraints making incorrect execution impossible.
- **Go Gemba (actual place):** Observe and model work as done, not as imagined.
- **Kaizen (continuous improvement):** Small, incremental, systematic improvements.
- **Muda (waste):** Eliminate activities that consume resources without adding value.
- **Heijunka and Kanban:** Level production and signal based on actual demand.

Toyota never abandoned inspection. It eliminated the Taylorist separation of inspection from production. Quality control remained integral — but it happened at the source, by the people doing the work, as part of the flow.

### Gene Kim and *The Phoenix Project*
Kim, Spafford, and Behr's *The Phoenix Project* (2013) translated TPS into software through the Three Ways of DevOps: optimising global flow (identify and remove the constraint), shortening feedback loops (the Andon Cord for code), and continuous learning (chaos engineering, retrospectives, deliberate practice). Security, when it operates as a manual gate, becomes the constraint that throttles the entire system.

---

## Appendix B: concept mapping

| Quality concept | Software security equivalent | Prevention or inspection? |
| --- | --- | --- |
| Jidoka (stop the line) | CI/CD gates enforcing security policy | Prevention (when enforcing constraints) / inspection (when running scanners) |
| Poka-Yoke (mistake-proofing) | Memory-safe languages; parameterised queries; pre-hardened images and IaC; structural containment for legacy | Prevention |
| Go Gemba (observe real work) | Chaos engineering; runtime observability; developer workflow analysis | Verification |
| Kaizen (continuous improvement) | BSIMM maturity; blameless post-mortems; sprint-allocated security debt reduction | Improvement |
| Muda (waste) | False positives; duplicated scanning; compliance rework; manual audit overhead | Waste identification |
| PONC (price of non-conformance) | Bug fix costs; breach costs; remediation hours; tool licensing | Economic measurement |
| POC (price of conformance) | Safe language investment; secure platform adoption; developer guardrails; policy-as-code | Economic measurement |
| Zero Defects | Elimination of preventable vulnerability classes by construction (new code; legacy is contained) | Performance standard |
| Quality Control (Juran) | AI-augmented detection; behavioural analytics; continuous monitoring; adversarial testing | Intelligent inspection |
| Quality by Design | Threat modelling in design; security architecture review; secure-by-default frameworks | Prevention |
| Incentive alignment (Anderson) | Liability shift (EU CRA), regulation (CAF B4.a, EO 14028), insurance underwriting, joint CISO/CTO KPI | Structural correction |

---

## References

[^stripe]: Stripe and Harris Poll, *The Developer Coefficient* (September 2018). Developers spend approximately 17.3 hours per week on maintenance and bad-code remediation — roughly 42% of a 40-hour week. https://stripe.com/files/reports/the-developer-coefficient.pdf

[^cisq-2020]: Consortium for Information & Software Quality, *The Cost of Poor Software Quality in the US: A 2020 Report* (Herb Krasner). Includes the cost-of-technical-debt-per-million-LOC framing. https://www.it-cisq.org/cisq-files/pdf/CPSQ-2020-report.pdf

[^boehm-jones]: The 1:100 prevention-to-production cost ratio is most commonly traced to Barry Boehm, *Software Engineering Economics* (Prentice Hall, 1981). Capers Jones' subsequent industry data and NIST's *The Economic Impacts of Inadequate Infrastructure for Software Testing* (2002) corroborate the order of magnitude but compress the ratio to roughly 1:6–1:15 in modern iterative/agile environments. The directional point — defects fixed late cost dramatically more than defects prevented early — holds consistently across decades of empirical work.

[^cisq-2022]: Consortium for Information & Software Quality, *The Cost of Poor Software Quality in the US: A 2022 Report* (Herb Krasner, sponsored by Synopsys, December 2022). Headline $2.41 trillion figure spans operational defects, downtime, schedule overruns, and accumulated technical debt — the security-specific share is not disaggregated. Use directionally, not as a security-specific anchor. https://www.it-cisq.org/wp-content/uploads/sites/6/2022/11/CPSQ-Report-Nov-22-2.pdf

[^crosby]: Philip B. Crosby, *Quality is Free: The Art of Making Quality Certain* (McGraw-Hill, 1979). PONC and POC ratios stated throughout, particularly Chapter 1 ("Making Quality Certain") and Chapter 6 ("Quality Cost Calculations").

[^black-duck-2024]: Black Duck Software, *Global State of DevSecOps 2024* (October 2024). Based on a Censuswide survey of more than 1,000 IT, security, and software development professionals across multiple countries and industries. Headline findings: 82% of organisations use between 6 and 20 security testing tools; 60% report that 21–60% of security test results are noise (false positives, duplicates, or conflicts); only 24% are "very confident" in their testing of AI-generated code. https://www.blackduck.com/blog/black-duck-devsecops-report.html

[^sembi-2026]: Sembi, *Inaugural Software Quality Pulse Report on Integrated Quality Operations* (May 2026). Global study of nearly 4,000 software development and delivery leaders. 86% of organisations report that security issues delay software releases at least occasionally; on average 57% of tests are automated; 53% of code is AI-generated or AI-assisted. https://www.businesswire.com/news/home/20260512679522/en/Sembi-Publishes-Inaugural-Software-Quality-Pulse-Report-on-Integrated-Quality-Operations

[^checkmarx]: Checkmarx, *Future of Application Security 2025* (August 2025), based on a survey of 1,500 security and development leaders. 81% of development teams knowingly ship code with vulnerabilities, up from 66% in 2024. https://checkmarx.com/

[^dbir]: Verizon, *Data Breach Investigations Report* (annual). Multi-year incident dataset spanning thousands of confirmed breaches across sectors; cited here as the canonical non-vendor-survey breach-data anchor. https://www.verizon.com/business/resources/reports/dbir/

[^mandiant]: Mandiant, *M-Trends* (annual). Incident-response telemetry from Mandiant Consulting engagements; provides dwell time, initial-access vector, and threat-actor attribution data independent of vendor-tool surveys. https://www.mandiant.com/m-trends

[^ncsc-review]: UK National Cyber Security Centre, *Annual Review* (annual). UK government assessment of the threat landscape and incident response, drawing on national telemetry. https://www.ncsc.gov.uk/section/about-this-website/annual-review

[^solarwinds]: SolarWinds Orion supply chain compromise (disclosed December 2020). SolarWinds reported approximately 18,000 customers downloaded the compromised SUNBURST update; CISA later confirmed a smaller subset (~100 organisations and ~9 federal agencies) were actively exploited. https://www.cisa.gov/news-events/cybersecurity-advisories/aa20-352a

[^ai-code]: Pearce et al., *Asleep at the Keyboard? Assessing the Security of GitHub Copilot's Code Contributions* (NYU / arXiv:2108.09293, 2021) found 40% of Copilot-generated code samples were vulnerable across 89 scenarios. Veracode's *2025 GenAI Code Security Report* tested >100 LLMs and found 45% of AI-generated code contained at least one exploitable weakness. Stanford HAI (Perry, Srivastava et al., 2022) found developers using AI assistants wrote measurably less secure code while being more confident in its security.

[^snyk-ai]: Snyk, *2024 AI Code Security Report*. Fewer than 25% of developers report using software composition analysis (SCA) on AI-generated code suggestions before accepting them, despite 56% acknowledging that AI tools sometimes or frequently introduce security issues. https://snyk.io/

[^sonatype]: Sonatype, *10th Annual State of the Software Supply Chain Report* (October 2024). 512,847 malicious packages logged year-on-year, a 156% increase. https://www.sonatype.com/state-of-the-software-supply-chain/

[^anderson]: Ross Anderson, *Security Engineering: A Guide to Building Dependable Distributed Systems*, 3rd edition (Wiley, 2020). Ch. 7 and Ch. 29 develop the Policy/Mechanism/Assurance/Incentive framework and the foundational argument that security failures are predominantly incentive failures.

[^eu-cra]: Regulation (EU) 2024/2847 — the Cyber Resilience Act. Establishes design-time security obligations on manufacturers of digital products and components placed on the EU market, with non-compliance penalties scaled to global turnover. Applies progressively from late 2024 through 2027. https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act

[^caf-b4a]: UK NCSC, *Cyber Assessment Framework v4.0* (2025). Principle B4.a — Secure Software Development — introduces a design-as-evidence requirement, distinct from the v3.x assurance-by-test posture. https://www.ncsc.gov.uk/collection/cyber-assessment-framework

[^eo14028]: White House, *Executive Order 14028: Improving the Nation's Cybersecurity* (12 May 2021), and successor secure-by-default mandates including the *National Cybersecurity Strategy* (March 2023). Federal-supplier obligations include SBOM provision and demonstrable secure-development practices. https://www.federalregister.gov/documents/2021/05/17/2021-10460/improving-the-nations-cybersecurity

[^gordon-loeb]: Lawrence A. Gordon and Martin P. Loeb, *The economics of information security investment*, ACM Transactions on Information and System Security 5(4), 2002. Establishes that optimal information-security investment is bounded by approximately 37% of expected loss for any given loss event — the canonical reference for sizing security spend against quantified risk.

[^google-rust]: Jeff Vander Stoep and Alex Rebert (Google), *Rust in Android: move fast and fix things*, Google Online Security Blog (November 2025). Memory-safety vulnerabilities in Android fell from 76% to under 20% of all vulnerabilities over six years; Rust components show a 1,000x lower memory-safety vulnerability density than the equivalent C/C++ code. https://security.googleblog.com/2025/11/rust-in-android-move-fast-fix-things.html. The "70% of serious bugs are memory safety" figure for legacy C/C++ codebases such as Chromium is reported in the same source.

[^kudelski]: Neal Samier and Nathan Hammel (Kudelski Security), *Hack to the Future: Owning AI-Powered Tools with Old School Vulns*, Black Hat USA 2025. Audit of over a dozen AI-powered developer tools — code review agents, AI coding agents, data-analytics assistants — found exploitable vulnerabilities in every tool tested. CodeRabbit (the most-installed AI code review app on GitHub) leaked its GitHub App private key via a RuboCop configuration-injection issue, granting researchers write access to over one million repositories. https://www.blackhat.com/us-25/briefings/schedule/

[^gitlab-2024]: GitLab, *2024 Global DevSecOps Report*. Teams that embed automated security scanning into their CI/CD pipelines detect approximately 60% more critical security issues before deployment than teams relying on ad-hoc or late-lifecycle assessments. https://about.gitlab.com/developer-survey/

[^coalition-2022]: Coalition, *Active Insurance: a year in review* (2022). Coalition lowered the share of insurance policyholders carrying critical vulnerabilities from 17% to 9.7% through 2022 — a 43% relative reduction — by combining continuous external scanning with notifications and self-service remediation guidance. Cited here as evidence both of better inspection and of insurance-underwriting-priced PONC. https://www.coalitioninc.com/blog/active-insurance-year-review

[^bsimm]: Synopsys, *BSIMM14 Foundations Report* (2023). The model tracks 125+ activities across 12 practices, applied to security programmes at 130+ participating organisations. https://www.synopsys.com/software-integrity/resources/analyst-reports/bsimm.html

[^ibm-breach]: IBM Security and Ponemon Institute, *Cost of a Data Breach Report 2023*. Organisations with high DevSecOps maturity reported average breach costs $1.68 million lower than those with low or no DevSecOps adoption. The 2025 edition has different methodology and figures. https://www.ibm.com/reports/data-breach

[^day-zero-normal]: Rob Fuller, *The Day-Zero Normal — A Practical Reprioritization Guide for CISOs Entering the AI Vulnerability Era* (April 2026). Argues for governance velocity, segmentation, runtime asset truth, and recovery speed as the structural response to permanent exploit-to-patch gaps.

[^deming]: W. Edwards Deming, *Out of the Crisis* (MIT Press, 1986). The 94%/6% systemic-vs-special-cause ratio is reproduced verbatim by the W. Edwards Deming Institute: https://deming.org/quotes/

[^anthropic-mythos]: Anthropic, *Assessing Claude Mythos Preview's cybersecurity capabilities* (7 April 2026). Mythos Preview autonomously identified and exploited zero-day vulnerabilities across major operating systems and browsers, including a 27-year-old OpenBSD bug, a 17-year-old FreeBSD NFS remote code execution flaw, and a 16-year-old FFmpeg vulnerability. Individual successful runs cost under $50. Vendor-published; should be read alongside [^aisi-mythos] for independent confirmation. https://red.anthropic.com/2026/mythos-preview/

[^aisi-mythos]: UK AI Safety Institute, *Our evaluation of Claude Mythos Preview's cyber capabilities* (13 April 2026). 73% success rate on expert-level CTF tasks; 3 of 10 full-chain completions and an average of 22 of 32 steps on the "Last Ones" corporate network attack simulation (Claude Opus 4.6 averaged 16); cyber-range evaluation budget of 100 million tokens. https://www.aisi.gov.uk/blog/our-evaluation-of-claude-mythos-previews-cyber-capabilities

[^ncsc-frontier]: Paul J and Alan Steer, *Why cyber defenders need to be ready for frontier AI*, UK National Cyber Security Centre (30 March 2026). Joint NCSC and AISI assessment that "defenders should assume that at least some attackers already have access to capable AI tools" and that "in specific cyber tasks, models already exceed what a skilled practitioner could achieve at lower cost." https://www.ncsc.gov.uk/blogs/why-cyber-defenders-need-to-be-ready-for-frontier-ai
