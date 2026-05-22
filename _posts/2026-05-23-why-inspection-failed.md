---
layout: master
title: "Why Inspection Failed"
date: 2026-05-22
---
# Why inspection failed — and what AppSec should do next
A plan for software security in the age of AI-generated code

## The hidden factory
Every software organisation runs two factories. The first is the one leadership sees, feature roadmaps, sprint velocity, deployment frequency, revenue. The second is invisible. It exists solely to fix things that were not done right the first time.

Your developers spend over 40% of their time in this hidden factory — fixing bugs, not building features.[^stripe] Paying down this accumulation of technical debt cost approximately $300,000 per year per million lines of code in 2020 (i.e. before the AI coding agent era — anecdotal evidence suggests this number has worsened significantly since).[^cisq-2020] A defect that costs $1 to prevent in design costs at least $100 to fix in production.[^boehm] The Consortium for Information & Software Quality estimated $2.41 trillion in annual US costs from poor software quality in 2022 — roughly 10% of GDP.[^cisq-2022]

The manufacturing world has a name for this: the Price of Non-Conformance (PONC). Philip Crosby, the quality theorist who coined the term, estimated it at 20-25% of revenue for manufacturers and up to 35% for service organisations.[^crosby] He also demonstrated that preventing defects was costing just 3-4% of sales.[^crosby] Following Crosby's analysis, leading manufacturers shifted their investments in quality, improving profit, quality and velocity.

No per-firm PONC calculator exists for software security. A few track customer, employee and developer facing defects and include security defects in these numbers. This is the single largest gap in how security communicates with leadership. Executive committees and boards do not know how to respond to vulnerability and misconfiguration counts. Many can also be sceptical of risks. But all respond to costs. Until security teams can express their PONC — the rework hours, the remediation costs, the tool licensing, the breach losses — ideally as a percentage of engineering budget, investment decisions will remain anchored to opinion and theoretical metrics.

That gap is about to become a crisis. AI is generating code faster than any organisation can inspect it, and AI is discovering vulnerabilities faster than any organisation can patch them.

---

## Why AI breaks the inspection model
In April 2026, Anthropic demonstrated that its frontier model, Claude Mythos Preview, could autonomously identify and exploit zero-day vulnerabilities in every major operating system and every major web browser — including bugs that had survived 16-27 years of expert human review and automated testing.[^anthropic-mythos] Engineers at Anthropic with no formal security training asked Mythos to find remote code execution vulnerabilities overnight. The cost of an individual successful vulnerability discovery run: under $50.[^anthropic-mythos]

The UK's AI Safety Institute (AISI) has independently evaluated Mythos and confirmed these capabilities.[^aisi-mythos] On expert-level capture-the-flag tasks, Mythos succeeded 73% of the time — the first model ever to complete expert-level CTFs, a milestone that no model could meet before April 2025. On a realistic 32-step corporate network attack simulation (estimated at 20 human hours to complete), Mythos completed the full chain in 3 of 10 attempts and averaged 22 of 32 steps. The previous best model, Claude Opus 4.6, averaged 16. AISI's cyber-range evaluation ran to a 100-million-token budget.

AISI's evaluation provides the timeline context that makes the trajectory unmistakable: two years ago, the best models could barely complete beginner-level cyber tasks. By April 2025, the first expert-level CTF completions were achieved. By April 2026, Mythos was completing full multi-stage corporate network attacks autonomously. AISI's conclusion:
> "Mythos Preview can exploit systems with weak security posture, and it is likely that more models with these capabilities will be developed."[^aisi-mythos]

From an attacker's perspective, AI has not _yet_ reduced the human capacity and skill requirements to exploit well-defended systems. However, for middle-tier attackers who are already very capable at monetising system exploitation, AI has demonstrably lowered the bar for finding and exploiting exposed weaknesses.

Weakly defended organisations, and people responsible for software with large technical debt, must consider themselves immediately at risk as a result of AI — irrespective of the general availability of the frontier models and coding tools. Three reinforcing dynamics are driving this urgency:

1. **The capability ceiling is rising fast.** Each new generation completes substantially more attack steps than the best model 18 months prior. Mythos outperformed Claude Opus 4.6, GPT-5.4, and every other frontier model AISI tested.[^aisi-mythos]
2. **Running these attacks is getting cheaper.** The same model with more processing time reliably improves results. The limiting factor is increasingly funding, not expertise — and we should expect the economic flywheels of criminal groups to spin up quicker as a result.
3. **The capabilities will proliferate.** AISI and the frontier laboratories themselves note that capabilities developed in frontier models are being transferred into smaller, cheaper, or open-weight models through distillation — meaning advances at the frontier set the direction of travel for the whole ecosystem.[^ncsc-frontier]

This has a direct implication for how we build software. AI-generated codebases are growing at rates that make even AI-augmented inspection structurally insufficient as a primary strategy. When a model can scan an entire operating system kernel for exploitable vulnerabilities at a cost of $50 per run, the economics of finding-and-fixing after the fact collapse. The cost of inspection grows with the volume of code. The cost of prevention — building defects out by construction — does not.

This is an economic argument, not a niche or technical one. And it has been understood in manufacturing for 70 years.

---

## The factory floor lesson
In the early 20th century, manufacturing operated on the Taylorist model: engineers designed the work, managers supervised it, workers performed it, and a separate inspection department caught defects before products shipped. Quality was someone else's problem.

At first, the application security industry replicated this bifurcation exactly. Developers ship features. The application security team inspects afterward. There have been incremental improvements since, including threat modelling and moving the inspection to earlier in the software development lifecycle (variously known as 'shift-left', 'security by design' and 'DevSecOps'). By most measures, the outcomes have been less than stellar:
- 82% of organisations use between 6 and 20 security testing tools[^black-duck-2024]
- 60% report that between 21% and 60% of their security test results are noise — false positives, duplicates, or conflicts[^black-duck-2024]
- 86% report that security issues delay software releases at least occasionally[^sembi-2026]
- 81% of development teams knowingly ship code with vulnerabilities.[^checkmarx]

These are obviously averages and within this data there are high performing exceptions. But the data shows that the inspection apparatus is not just failing to prevent defects. It is generating friction that causes organisations to bypass it entirely. And even in high performing teams the cost of achieving security may be much higher than it needs to be.

In the 1950s, engineers at Japanese manufacturers — with input from Deming, Crosby, and Juran — demonstrated that the root of quality failure was not bad workers but bad systems. Most defects had systemic causes that no amount of end-of-line inspection could fix. The solution was not more inspection. It was redesigning the system to prevent defects from being created. Toyota synthesised this into the Toyota Production System (TPS), and some of the techniques have evolved and been incorporated into modern software management systems including Lean, Agile, and DevOps.

The relevant lesson is the result, not the history: Toyota didn’t abandon inspection. It integrated prevention, inspection, and continuous improvement into a single system owned by the people doing the work. The Taylorist separation — quality is someone else's department — is what it eliminated.

For the full historical context including Deming's variation theory, Crosby's Zero Defects, Juran's Quality Trilogy, and a Crosby-Juran debate, see Appendix A: The Quality Revolution.

---

## The economics of the hidden factory in software
A manufacturing quality framework does not just apply to software. It is amplified by five properties unique to digital systems.

1. **A single defect replicates at internet scale.** In manufacturing, a defective batch affects a bounded number of units. In software, a single vulnerability in a centralised platform is instantly replicated across the entire user base. The PONC is not linear — it is multiplicative. The SolarWinds supply chain compromise affected up to 18,000 organisations from a single point of failure.[^solarwinds]
2. **The tempo mismatch is structural.** Security teams plan on annual budget cycles. They improve reactively, after incidents. Their information about threats diffuses through annual reports and quarterly briefings. The adversary iterates weekly, shares TTPs through forums in days, and exploits the gap between a vulnerability's disclosure and its remediation in hours.
3. **This tempo mismatch cannot be fixed by buying better tools.** An organisation with a $10 million security budget that operates on annual planning cycles is, from the attacker's perspective, slower to adapt than a solo ransomware affiliate running commodity tools who reads the forums every morning.
4. **AI-generated code is outpacing inspection.** Independent tests have found that 40-45% of AI-generated code contains at least one security weakness.[^ai-code] Fewer than a quarter of developers run security analysis on the AI-generated code they accept.[^snyk-ai] The volume of code entering production is growing faster than inspection tooling can absorb it. When code is generated at machine speed, inspection capacity must either scale proportionally — at exponential cost — or the system must prevent a greater proportion of defects from being introduced in the first place.
5. **The adversary has AI too.** Manufacturing quality theory was designed for environments with entropy and variation, but not intelligent adversaries. Toyota's competition was other manufacturers, not actors actively trying to break its cars. In 2024, 512,847 malicious packages were detected in open-source software supply chains — a 156% year-on-year increase.[^sonatype] The UK NCSC and AISI assess that defenders "should assume that at least some attackers already have access to capable AI tools" and that, "in specific cyber tasks, models already exceed what a skilled practitioner could achieve at lower cost."[^ncsc-frontier]

This means inspection alone is insufficient. The inspection-dominant system must be uplifted to prevention, detection and correction — not as the Taylorist inspection department of old, but as an integrated feedback loop wired into all levels of the organisational hierarchy. Prevention reduces what needs detecting. Detection informs what needs preventing, not just what needs to be fixed. Correction both limits damage and ensures the system learns and improves.

---

## The Andon Cord for code
The quality revolution is not something the software industry must invent. Significant elements are already operational — unevenly distributed, but proven.

### Mistake-proofing the pipeline
**Poka-Yoke.** Toyota's principle of making it structurally impossible to do a task incorrectly already exists in software development:
- **Memory-safe languages (Rust, Go)** eliminate buffer overflows and use-after-free vulnerabilities by construction. Google reported a 1,000x reduction in memory-safety vulnerability density when moving from C/C++ to Rust. Android memory-safety bugs now account for less than 20% of all vulnerabilities, down from 76% six years earlier.[^google-rust]
- **Parameterised queries** eliminate SQL injection. Content security policies prevent XSS. These are not inspection tools — they are constraints that make entire vulnerability classes impossible.
- **Infrastructure-as-Code (IaC) and Hardened images** prevent system configuration mistakes, excessive access and inherited vulnerabilities by default. The IaC template implements, enforces and provides evidence of policy natively in the code and other pre-verified infrastructure such as hardened images ensure only the minimum required, secure artefacts are built and loaded into runtime. As a useful by-product they also make fixes for discovered defects more efficient and automatable.
- **Reasoning in the IDE** is a rapidly developing capability where developers and their AI agents are being given the context and understanding to prevent vulnerabilities and misconfigurations in their preferred coding environment before they commit. This collapses the time and interactions previously required to develop security requirements from threat modelling and organisational policies, check they've been met, and provide evidence that this work has happened.

These are not incremental improvements to the inspection process. They are defect elimination by construction.

### Stop the line
**Jidoka.** Toyota's principle of stopping the assembly line when a defect is found maps directly to IDE, Repo and CI/CD security control planes. When a merge request is blocked by a failing security policy, that is 'stop the line.' Teams that embed automated security scans inside CI/CD pipelines detect 60% more critical issues before deployment than teams running ad-hoc or late-lifecycle checks.[^gitlab-2024] On the broader monitoring layer, Coalition reported a 43% year-on-year reduction in the share of insurance policyholders carrying critical vulnerabilities after deploying continuous scanning and remediation guidance.[^coalition-2022] The distinction between inspection and prevention + inspection measures matters here: a CI/CD gate that runs a scanner and flags findings for a human to triage later is inspection moved earlier. An IDE that reasons about the context of the code and security policies and adds friction to a commit that fails an agreed conformance standard, followed by a security CI/CD gate that enforces a policy constraint and halts deployment if violated, is Jidoka.

### Continuous improvement
**Kaizen.** Toyota's culture of small, incremental, systematic improvement maps to structured DevSecOps maturity models. The Building Security In Maturity Model (BSIMM) tracks more than 120 activities across 12 practices.[^bsimm] Organisations with mature DevSecOps practices saw an average data-breach cost roughly $1.7M lower than organisations with little or no DevSecOps adoption.[^ibm-breach]

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

The investment question for every organisation: what percentage of your security budget targets vulnerability classes that could be eliminated by construction, vs classes that genuinely require detection? For most organisations, the answer will currently be that the vast majority of spend is on inspection of preventable defects.

---

## Why we cling to inspection 
If the economics are this clear, why does the security industry remain obsessed with inspection? Because checking a box feels like security — even when it isn't.

### The placebo of compliance
The illusion of control is a cognitive distortion where individuals believe they have more influence over unpredictable events than they actually do. In software security, this manifests as security theatre: passing a certification, increasing dashboard coverage, discovering a vulnerability, writing a policy. These activities provide comfort. They do not provide control.

Organisations believe that if they aggregate enough observability tools into a single platform, they achieve security. In practice, this produces point-in-time snapshots of a system that is changing continuously — passing a checklist while missing the entropic shifts underneath.

### The confidence trap
There is also a related cognitive bias: the belief that technical expertise protects against uncertainty. Security experts with deep knowledge are often the most qualified to assess risk - but their confidence can lead them to treat genuine unpredictability as manageable risk. Unmeasurable hard to execute threats get classified as 'low probability' rather than 'unknown.' Weakness reduction gets deferred as a risk trade-off rather than operationalised as a security imperative.

The result is that the industry defaults to activities that feel rigorous (scanning, testing, auditing) over activities that are economically superior (preventing defects by construction) because the former produce visible artefacts and the latter produce the absence of problems - which is harder to measure and easier to resist or cut from the budget.

### Shift 15-20% of inspection budget to prevention
A 90-day action plan for Security Leaders
1. **Measure your PONC.** Catalogue rework costs, vulnerability remediation hours, breach-related losses, and tool licensing for inspection. Express as a percentage of the engineering budget. 
2. **Map your top 3 vulnerability classes by volume and cost.** For each class, identify whether a prevention intervention exists (memory-safe language, parameterised queries, IaC policy-as-code, hardened images, security control points in IDE, Repo and CI/CD).
3. **Propose a rebalancing.** Shift 15-20% of inspection tool budget toward prevention investment: language migration, secure-by-default platform adoption, hardened images, IDE and CI/CD guardrails.
4. **Establish a prevention ratio KPI.** Track percentage of security spend on prevention vs inspection vs rework and response, quarterly.
5. **Measure and iterate on the new performance standards.** Set a Zero Defects policy for vulnerability classes eliminable by construction. 
6. **Share your experience with peers.** As a profession we often rely on consultants and suppliers to collate and share good practice. While these are welcome contributions, defenders are being out-collaborated by attackers. When operators get together to share playbooks and tactics, we see noticeable maturity improvements across the community. Within the best communities you invariably get more out than the effort you and your teams put in. If you struggle to find the right community, start one.

The board conversation you want on this plan: 
>"We currently spend [X]% of our engineering budget fixing defects that could be prevented by construction. We recommend reallocating [Y]% toward prevention investments that eliminate these defect classes entirely, reducing our failure costs by an estimated [Z]%."

Boards do not respond to vulnerability counts. They might accept risks. But they respond to costs.

## From compliance to execution intelligence
The next generation of software security is a convergence of manufacturing quality principles and AI-powered execution. It moves beyond 'shift left' (testing earlier) and even 'shift down' (embedding security in platforms) to security as an integrated property of the tools and processes that build and execute code.

When AI agents write code, review code, deploy code, and discover vulnerabilities in code, security cannot remain a human-operated inspection department bolted onto the side. It must be wired into the execution path — the constraints that prevent defects, the gates that enforce policy, and the feedback loops that inform the next generation of preventive design.

The race is not between prevention and inspection. It is between organisations that integrate both into a coherent security system — where prevention reduces the attack surface, intelligent inspection verifies the residual, and rapid response limits damage while informing the next iteration — and organisations that remain trapped in the Taylorist bifurcation, running faster on the inspection treadmill while AI generates code and attackers discover weaknesses at machine speed around them.

## How to test this thesis
A framework that cannot be disproved is advocacy, not analysis. The following conditions would challenge the core claims:

1. If high-maturity DevSecOps organisations sustainably outperform prevention-focused organisations on breach rates and vulnerability escape rates, then the inspection-improvement path is sufficient and the rebalancing recommendation is unnecessary.
2. If AI-powered inspection scales proportionally with AI-generated code — maintaining coverage and accuracy as code volume grows — then the claim that inspection cannot keep pace is empirically refuted.
3. If the systemic-to-individual ratio for software security defects is measured below 70%, the directional application of Deming's principle is weaker than argued, and individual-focused interventions retain more value than this framework grants.
4. If platform-level default-on security controls fail to reduce vulnerability rates at organisations that adopt them, the Poka-Yoke prescription is invalidated.

These are testable propositions. The research community is invited to test them.

## Appendix A: the quality revolution
### Frederick Winslow Taylor and scientific management
Taylor's principles (early 1900s) rationalised mass production by breaking complex tasks into simple steps. Planning was separated from execution. Independent inspection departments caught defects after manufacture. This created a reactive culture where quality belonged to the inspector. The security industry replicated this model: developers ship features, infosec inspects afterward.
### W. Edwards Deming and variation theory
Deming demonstrated that most quality failures are systemic (common causes) rather than individual (special causes). His work in post-war Japan enabled Toyota and other manufacturers to leapfrog American competitors within five years. Deming estimated that workers cause approximately 6% of quality problems while 94% are inherent in the system.[^deming] Though this specific ratio has not been validated for software security, the directional insight is well supported — the persistence of buffer overflows for 35 years, and the proportion of codebases shipping with known critical vulnerabilities, both point to systemic roots.
### Philip Crosby: *Quality is Free*
Crosby argued that the cost of preventing defects is always lower than the cost of finding and fixing them. His four principles:

1. Definition: Quality is conformance to requirements.
2. System: Prevention, not appraisal.
3. Performance standard: Zero Defects (not "close enough").
4. Measurement: The Price of Non-Conformance (PONC).

PONC includes rework, scrap, warranty claims, inspection costs, and administrative overhead. Crosby estimated PONC at 20-25% of revenue for manufacturing, up to 35% for services. The Price of Conformance (prevention investment) he estimated at 3-4% of sales.[^crosby] In software: $2.41T annual cost of poor quality against ~$215B in global cyber security spend suggests the ratio is similarly lopsided.[^cisq-2022][^gartner-spend]

### Joseph Juran: the Quality Trilogy
Juran defined quality as fitness for use rather than conformance to specification. His Quality Trilogy — Quality Planning, Quality Control, Quality Improvement — mirrors financial management (budgeting, control, improvement). His Cost of Quality framework divides costs into: Prevention Costs (investments to stop defects), Appraisal Costs (testing and verification), Internal Failure Costs (catching defects before shipping), and External Failure Costs (breach, recall, reputation loss).

Juran and Crosby disagreed on the feasibility of zero: Juran argued that approaching zero defects produces exponentially increasing costs, and advocated for Pareto-style prioritisation. Crosby countered that accepting any defect level was a trap that allowed managers to tolerate failure. In security, both are right: apply Crosby's Zero Defects to classes eliminable by construction (memory safety, injection, misconfiguration) and Juran's optimisation to classes requiring ongoing detection (logic flaws, adversarial techniques).

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

## Appendix B: concept mapping

| Quality concept | Software security equivalent | Prevention or inspection? |
| --- | --- | --- |
| Jidoka (stop the line) | CI/CD gates enforcing security policy | Prevention (when enforcing constraints) / inspection (when running scanners) |
| Poka-Yoke (mistake-proofing) | Memory-safe languages; parameterised queries; pre-hardened images and IaC; reasoning in IDE | Prevention |
| Go Gemba (observe real work) | Chaos engineering; runtime observability; developer workflow analysis | Verification |
| Kaizen (continuous improvement) | BSIMM maturity; blameless post-mortems; sprint-allocated security debt reduction | Improvement |
| Muda (waste) | False positives; duplicated scanning; compliance rework; manual audit overhead | Waste identification |
| PONC (price of non-conformance) | Bug fix costs (100x in production); breach costs; remediation hours; tool licensing | Economic measurement |
| POC (price of conformance) | Safe language investment; secure platform adoption; developer guardrails; policy-as-code | Economic measurement |
| Zero Defects | Elimination of preventable vulnerability classes by construction | Performance standard |
| Quality Control (Juran) | AI-augmented detection; behavioural analytics; continuous monitoring; adversarial testing | Intelligent inspection |
| Quality by Design | Threat modelling in design; security architecture review; secure-by-default frameworks | Prevention |

---

## References

[^stripe]: Stripe and Harris Poll, *The Developer Coefficient* (September 2018). Developers spend approximately 17.3 hours per week on maintenance and bad-code remediation — roughly 42% of a 40-hour week. https://stripe.com/files/reports/the-developer-coefficient.pdf

[^cisq-2020]: Consortium for Information & Software Quality, *The Cost of Poor Software Quality in the US: A 2020 Report* (Herb Krasner). Includes the cost-of-technical-debt-per-million-LOC framing. https://www.it-cisq.org/cisq-files/pdf/CPSQ-2020-report.pdf

[^boehm]: The 1:100 prevention-to-production cost ratio is most commonly traced to Barry Boehm, *Software Engineering Economics* (Prentice Hall, 1981), and is corroborated in Capers Jones' subsequent industry data. The ratio varies by defect type but the order of magnitude is consistently observed across decades of empirical work.

[^cisq-2022]: Consortium for Information & Software Quality, *The Cost of Poor Software Quality in the US: A 2022 Report* (Herb Krasner, sponsored by Synopsys, December 2022). Headline figures: $2.41 trillion in cost of poor software quality, $1.52 trillion in accumulated technical debt. https://www.it-cisq.org/wp-content/uploads/sites/6/2022/11/CPSQ-Report-Nov-22-2.pdf

[^crosby]: Philip B. Crosby, *Quality is Free: The Art of Making Quality Certain* (McGraw-Hill, 1979). PONC and POC ratios are stated throughout, particularly Chapter 1 ("Making Quality Certain") and Chapter 6 ("Quality Cost Calculations").

[^black-duck-2024]: Black Duck Software, *Global State of DevSecOps 2024* (October 2024). Based on a Censuswide survey of more than 1,000 IT, security, and software development professionals across multiple countries and industries. Headline findings: 82% of organisations use between 6 and 20 security testing tools; 60% report that 21-60% of security test results are noise (false positives, duplicates, or conflicts); only 24% are "very confident" in their testing of AI-generated code. https://www.blackduck.com/blog/black-duck-devsecops-report.html

[^sembi-2026]: Sembi, *Inaugural Software Quality Pulse Report on Integrated Quality Operations* (May 2026). Global study of nearly 4,000 software development and delivery leaders. 86% of organisations report that security issues delay software releases at least occasionally; on average 57% of tests are automated; 53% of code is AI-generated or AI-assisted. https://www.businesswire.com/news/home/20260512679522/en/Sembi-Publishes-Inaugural-Software-Quality-Pulse-Report-on-Integrated-Quality-Operations

[^checkmarx]: Checkmarx, *Future of Application Security 2025* (August 2025), based on a survey of 1,500 security and development leaders. 81% of development teams knowingly ship code with vulnerabilities, up from 66% in 2024. https://checkmarx.com/

[^solarwinds]: SolarWinds Orion supply chain compromise (disclosed December 2020). SolarWinds reported approximately 18,000 customers downloaded the compromised SUNBURST update; CISA later confirmed a smaller subset (~100 organisations and ~9 federal agencies) were actively exploited. https://www.cisa.gov/news-events/cybersecurity-advisories/aa20-352a

[^ai-code]: Pearce et al., *Asleep at the Keyboard? Assessing the Security of GitHub Copilot's Code Contributions* (NYU / arXiv:2108.09293, 2021) found 40% of Copilot-generated code samples were vulnerable across 89 scenarios. Veracode's *2025 GenAI Code Security Report* tested >100 LLMs and found 45% of AI-generated code contained at least one exploitable weakness. Stanford HAI (Perry, Srivastava et al., 2022) found developers using AI assistants wrote measurably less secure code while being more confident in its security.

[^snyk-ai]: Snyk, *2024 AI Code Security Report*. Fewer than 25% of developers report using software composition analysis (SCA) on AI-generated code suggestions before accepting them, despite 56% acknowledging that AI tools sometimes or frequently introduce security issues. https://snyk.io/

[^sonatype]: Sonatype, *10th Annual State of the Software Supply Chain Report* (October 2024). 512,847 malicious packages logged year-on-year, a 156% increase. https://www.sonatype.com/state-of-the-software-supply-chain/

[^google-rust]: Jeff Vander Stoep and Alex Rebert (Google), *Rust in Android: move fast and fix things*, Google Online Security Blog (November 2025). Memory-safety vulnerabilities in Android fell from 76% to under 20% of all vulnerabilities over six years; Rust components show a 1,000x lower memory-safety vulnerability density than the equivalent C/C++ code. https://security.googleblog.com/2025/11/rust-in-android-move-fast-fix-things.html. The "70% of serious bugs are memory safety" figure for legacy C/C++ codebases such as Chromium is reported in the same source.

[^gitlab-2024]: GitLab, *2024 Global DevSecOps Report*. Teams that embed automated security scanning into their CI/CD pipelines detect approximately 60% more critical security issues before deployment than teams relying on ad-hoc or late-lifecycle assessments. https://about.gitlab.com/developer-survey/

[^coalition-2022]: Coalition, *Active Insurance: a year in review* (2022). Coalition lowered the share of insurance policyholders carrying critical vulnerabilities from 17% to 9.7% through 2022 — a 43% relative reduction — by combining continuous external scanning with notifications and self-service remediation guidance. Source intervention is continuous monitoring, not strictly CI/CD gating; cited here to evidence the broader Jidoka pattern of automated alert-and-fix flows. https://www.coalitioninc.com/blog/active-insurance-year-review

[^bsimm]: Synopsys, *BSIMM14 Foundations Report* (2023). The model tracks 125+ activities across 12 practices, applied to security programmes at 130+ participating organisations. https://www.synopsys.com/software-integrity/resources/analyst-reports/bsimm.html

[^ibm-breach]: IBM Security and Ponemon Institute, *Cost of a Data Breach Report 2023*. Organisations with high DevSecOps maturity reported average breach costs $1.68 million lower than those with low or no DevSecOps adoption. The 2025 edition has different methodology and figures. https://www.ibm.com/reports/data-breach

[^deming]: W. Edwards Deming, *Out of the Crisis* (MIT Press, 1986). The 94%/6% systemic-vs-special-cause ratio is reproduced verbatim by the W. Edwards Deming Institute: https://deming.org/quotes/

[^gartner-spend]: Gartner, *Forecast Analysis: Information Security, Worldwide*. Global cyber security and risk management end-user spending was forecast at $215 billion for 2024 and projected to exceed $260 billion by 2026. https://www.gartner.com/en/newsroom/press-releases

[^anthropic-mythos]: Anthropic, *Assessing Claude Mythos Preview's cybersecurity capabilities* (7 April 2026). Mythos Preview autonomously identified and exploited zero-day vulnerabilities across major operating systems and browsers, including a 27-year-old OpenBSD bug, a 17-year-old FreeBSD NFS remote code execution flaw, and a 16-year-old FFmpeg vulnerability. Individual successful runs cost under $50. https://red.anthropic.com/2026/mythos-preview/

[^aisi-mythos]: UK AI Safety Institute, *Our evaluation of Claude Mythos Preview's cyber capabilities* (13 April 2026). 73% success rate on expert-level CTF tasks; 3 of 10 full-chain completions and an average of 22 of 32 steps on the "Last Ones" corporate network attack simulation (Claude Opus 4.6 averaged 16); cyber-range evaluation budget of 100 million tokens. https://www.aisi.gov.uk/blog/our-evaluation-of-claude-mythos-previews-cyber-capabilities

[^ncsc-frontier]: Paul J and Alan Steer, *Why cyber defenders need to be ready for frontier AI*, UK National Cyber Security Centre (30 March 2026). Joint NCSC and AISI assessment that "defenders should assume that at least some attackers already have access to capable AI tools" and that "in specific cyber tasks, models already exceed what a skilled practitioner could achieve at lower cost." https://www.ncsc.gov.uk/blogs/why-cyber-defenders-need-to-be-ready-for-frontier-ai
