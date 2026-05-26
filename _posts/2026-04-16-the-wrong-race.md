---
id: 02
title: 'The wrong race'
date: '2026-04-16T00:00:00+00:00'
author: 'Simon Goldsmith'
layout: post
categories:
    - Articles, Cybersecurity, Software, Resilience
---
# The wrong race

![the-wrong-race](/assets/img/2026-04-16/the-wrong-race.png)

## Two Numbers
In January 2026, two security researchers pointed AI agent swarms at Windows kernel drivers from AMD, Intel, NVIDIA, Dell, Lenovo, and IBM. In thirty days, for $600, they found over 100 exploitable vulnerabilities. Cost per bug: four dollars. 

In the same month, CrowdStrike published a number that should concern anyone who has approved a security budget in the past five years: 82% of their detections in 2025 were malware-free. The majority of attackers do not deploy malware. It's a cliché (and frustrating) but attackers really are logging in.

As Anthropic's Mythos hype floods the media and dominates our industry discourse put those numbers side by side. The first tells you where cyber attacks are going. The second tells you where it already is. And the gap between where security investment is and where the threat actually lives is the subject of this article.

---

## The Industry That Used to Be a Technique
In 2010, stealing credentials at scale required you to build your own phishing kit, host your own infrastructure, and go after the large number of targets who hadn't heard of two-factor authentication or considered it 'too hard'. It was artisanal work. In the incident data, despite the rich pickings, credential theft accounted for about 1% of observed attack methods.

Today, you can subscribe. Initial Access Brokers sell network footholds for $439 - down 69% in three years. Phishing-as-a-Service platforms push 30 million malicious emails a month. The infostealer ecosystem processed 53 million stolen credentials last year, available from a dollar per log. Token theft bypasses MFA in 69% of business email compromise cases. Seventy-nine percent of those victims had MFA deployed.

![The cost of buying initial access continues to fall](/assets/img/2026-04-16/network-foothold.png)
*Figure 1 - The cost of buying initial access continues to fall*

Credential theft is not a technique any more. It is an industry with specialised suppliers, commodity infrastructure, and falling input costs. The economics look like any other mature market.

The instinct, when confronted with numbers like these, is to ask "how do we stop credential theft?" Organisations have been asking that for a decade. It is the wrong question. The right one: 

> are we spending at the right level for the threat we're actually facing?

---

## Diffusion and Evolution
[Simon Wardley](https://www.linkedin.com/in/simonwardley/) drew a distinction in 2015 that the cybersecurity industry has still not absorbed. Diffusion is the adoption of a specific product - an S-curve. Evolution is the maturation of an activity through successive diffusion curves, from genesis to commodity. A thing can become a commodity at 5% adoption, or remain custom-built at 95%.

![Evolution and Diffusion of Ransomware](/assets/img/2026-04-16/evolution-diffusion.png)
*Figure 2 - Evolution and Diffusion of Ransomware*

When security teams talk about "TTP evolution," they are usually describing diffusion: the spread of a particular ransomware variant, the adoption of a specific exploit kit. But the question that should determine investment is different: 

> How mature is the activity itself? Is attack capability still artisanal, or has it become a utility?

The answer determines what kind of defence makes economic sense. A genesis-stage threat - novel, expert-dependent, rare - warrants expensive, highly bespoke, multi-faceted defences. A commodity-stage threat - standardised, widely available, low-skill - warrants highly automated and standardised practices which neutralise as many attacker TTPs as possible for lowest investment. Both benefit from advances in AI but in different ways.

More importantly, the mismatch occurs when we keep paying genesis prices for a commodity problem. And when we are hand-crafting defences for threats that arrive by subscription.

To understand where the landscape actually sits, I pulled over 10,000 incidents from the [VCDB](https://github.com/vz-risk/VCDB) and cross-referenced them against 800+ reports from over six years of annual threat reporting including the likes of CrowdStrike, IBM, Mandiant, and Verizon. 

---

## What Defenders Got Right
Before I describe the mismatch, something important about where defenders succeeded. Because the success is real, and it sets up the failure.

Ransomware followed a clean Wardley arc. It appeared in the incident data around 2013. By 2016 it had jumped five-fold. By 2019 it was commodity: Ransomware-as-a-Service with affiliate programmes, dashboards, and customer support. The defensive ecosystem responded. EDR matured. Automated backup recovery reached the point where 97% of encrypted organisations now recover without paying. Payment rates dropped to 28%. Phishing click-through rates halved in two years.

Commodity attack, met by commodity defence. That is what success looks like: the defensive investment matched the maturity of the threat. SQL injection followed the same pattern a decade earlier — parameterised queries and ORMs eliminated the entire attack class at the platform level. It barely registers in the data any more.

The security industry should take genuine credit for these outcomes. It rarely does, because the credit disappears into the next crisis. But these are proof that the mechanism works. When defenders automate the right thing at the right level, the attacker is forced to go elsewhere.

Which is exactly what happened.

---

## Where the Attacker Went
As EDR and backup automation made extortion based on malware attacks less profitable, the attack landscape migrated to identity. 

CrowdStrike's malware-free detection rate — the proportion of intrusions that use no malware at all, relying instead on stolen credentials, session tokens, and legitimate tools — rose from 40% in 2019 to 82% in 2025. Credential-based breaches take 292 days to resolve, the longest lifecycle of any attack vector. Only half of breaches are detected by the organisation's own security teams. The rest are discovered by outsiders: government agencies, researchers, the attackers themselves.

Identity threat detection and response, conditional access, behavioural analytics, phishing-resistant authentication, non-human identity discovery and management, least privilege — the tools and practices that address this exist albeit at early evolution. But mainstream adoption is early-diffusion. The identity defence ecosystem is roughly where endpoint security was in 2015: the market is forming, the products work but are tricky to orchestrate in organisations where speed is a competitive advantage, the access already granted is prolific and the majority of organisations playing catch up.

Meanwhile, the threat they address is already commodity. Infostealers feed Initial Access Brokers feed ransomware operators in a self-sustaining flywheel. Token theft has made conventional MFA irrelevant against the specific attack chains it targets. The attacker's cost of entry is approaching zero.

> The core mismatch: defenders built commodity-grade automation for now minority threat category , and maintain disproportionately manual prevention and detection operations for the threat category that is dominant.

![AI accelerates the patching treadmill but it's not the source of attacks right now](/assets/img/2026-04-16/patching-treadmill.png)
*Figure 3 - AI accelerates the patching treadmill but it's not the source of attacks right now*

This is not a failure of intelligence. Security leaders know identity is the dominant threat. The lag is structural. Budgets are annual and have been built around the malware paradigm. Security workflows were designed for vulnerability discovery in third party managed software and network and endpoint alerts. Skills, tools, and procurement cycles are all optimised for a threat model that now accounts for less than 30% of detections.

---

## The Amplification Problem
The identity mismatch would be serious enough on its own. What makes it urgent is the amplification chain that sits underneath every vulnerability in every piece of software an organisation runs. 

Every defect that ships in production code gets amplified. First by the number of systems it deploys to. Then by the time it sits unpatched. Then by the number of threat actors who discover it. Then by the incident response cost when one succeeds. A flaw that costs minutes to prevent at the point of authoring costs hours in code review, days in triage and backlog refinement, weeks in patch deployment, and months in breach response. Each stage is multiplicative, not additive.

![The price of \[in\]security is multiplicative](/assets/img/2026-04-16/security-bugs.png)
*Figure 4 - The price of [in]security is multiplicative*

The security industry has spent two decades investing overwhelmingly at the amplified end of that chain. Better detection. Faster response. More scanners. More dashboards. The economics are brutal: 82% of organisations use six to twenty security testing tools. Sixty percent report false positive rates above 20%. Eighty-one percent knowingly ship vulnerable code under deadline pressure. We built an enormous apparatus for finding problems after they've been amplified.

And then AI arrived and attacked the point of origin.

The median time from vulnerability disclosure to first observed exploit collapsed from 771 days in 2018 to 6 days in 2023 to 4 hours in 2024. By 2025, the majority of exploited vulnerabilities were weaponised on or before the day they were disclosed. Check out the data behind [Sergej Epp's](https://www.linkedin.com/in/sergejepp/) [zero day clock](https://zerodayclock.com/). AI exploit generation runs at $8.80 per exploit with an 87% success rate. The UK's [AI Security Institute](https://www.linkedin.com/company/ai-security-institute/) found that Anthropic’s Claude Mythos Preview could autonomously complete a 32-step, end-to-end corporate network attack simulation. This followed Anthropics announcement that it had found 500 high-severity vulnerabilities in widely-used open-source software in a matter of days — bugs that had survived decades of expert human review.

![The zero day clock measures the gap between CVE disclosure and confirmed exploitation](/assets/img/2026-04-16/zero-day-clock.png)
*Figure 5 - The zero day clock measures the gap between CVE disclosure and confirmed exploitation*

The detection-response-patch cycle that underpins virtually every security programme is operating against arithmetic it cannot win. The volume of discoverable weaknesses is growing faster than any inspection apparatus can process them. However, the better, faster, cheaper solution is in prevention, not inspection.

---

## A Spectrum, Not a Switch
To quote Sergej in his excellent blog post, [Winning the AI Cyber Race: Verifiability is All You Need:](https://sergejepp.substack.com/p/winning-the-ai-cyber-race-verifiability) 

> AI will win where verification is easy. Defense will lose where verification is hard.

Currently verification asymmetry states that AI favours attack evolution over defence due to the speed and quality of verification. 

![The Verifier's Law](/assets/img/2026-04-16/verifiers-law.png)
*Figure 6 - The Verifier's Law*

It is tempting to frame all of this as a binary — defenders losing, attackers winning. That framing is wrong, and the correction matters for investment decisions.

The best security teams are disrupting ransomware in an average of three minutes through  hygiene controls and autonomous AI agents. One identity platform blocked 15 billion malicious login attempts last year and automatically remediated 81% of flagged high-risk users. Organisations with AI and automation are achieving 30% faster identification and containment of security events. Mandiant claims their clients average ten-day dwell times. 

The top 20-30% of enterprises are closing the evolution gap. They are doing it by combining prevention strategies and faster and more accurate detection and containment (including using AI). For the rest — organisations where 42% of alerts go uninvestigated, where staffing shortages are growing at 26% per year, where security teams lose a fifth of their capacity to manual tasks — the mismatch is widening. This widening bifurcation of defender capability is where we should be concerned.

The strategic question is not whether defenders can catch up. The evidence suggests they can, for the same reason they caught up to malware-based extortion: when the economic case becomes undeniable, adoption follows. FIDO2 and passkeys are cryptographically resistant to the phishing and token theft that drive the commodity parts of the identity attack chain. Google, Apple, and Microsoft are pushing them simultaneously. The mechanisms that eliminated SQL injection - baked-in security that make the insecure path structurally difficult - is now being applied to identity.

---

## What Actually Matters
In 2001, Ross Anderson at Cambridge published a paper that should have changed the industry more than it did: information insecurity is at least as much due to perverse incentives as to technical problems. The people who build insecure software don't pay when it gets hacked. The costs land on the wrong people - a textbook economic externality.

Twenty-five years later, nothing structural has changed about that incentive landscape. What has changed is the speed at which the externality compounds. When AI can find and exploit vulnerabilities at four dollars each, the amplification funnel runs from discovery to exploitation in hours, not months. Neither the operational remediation nor the inspection apparatus - scanners, dashboards, analysts triaging thousands of alerts per day - scale to meet that volume.

Every organisation faces a choice. Every pound spent improving the vulnerability inspection cycle is a pound not spent on architectural prevention. Every hour a security team spends triaging AI-generated findings or building AI to triage AI-generated findings is an hour not spent rebuilding systems to limit loss and be secure by design. Every quarter spent trying to inspect security into technology and processes is a quarter not spent on the prevention paradigm shift.

Three things warrant investment right now, in roughly this order.

1. **Identity defence commoditisation.** This is the single highest-leverage move available. Not because identity is the newest threat, but because it is the only major category where the attacker has reached commodity while the defender has not. Phishing-resistant authentication, ITDR, conditional access, continuous session validation, non-human identity discovery and management. The tools exist. The economic case is proven. The window between "early adopter advantage" and "table stakes" is closing. Every other major threat category - ransomware, phishing, SQL injection, supply chain, AI-driven vulnerability exploitation - is either matched or not yet mature enough to warrant commodity-scale investment.
2. **Prevention at the point of origin.** If the amplification funnel is the structural problem and the Verifier’s Law is the accelerant. The complimentary to verification is to make the IDE and the CI/CD pipeline the primary locus of security - not through scanners that flag problems after they're written, but through context-aware assembly tooling that makes insecure construction structurally difficult. An IDE that understands the security context of what a developer is building - the threat model of the service, the sensitivity of the data it handles, the trust boundaries it crosses - can surface the right requirements at the moment of authoring. Not a list of generic best practices. The specific constraints that matter for this code, in this service, right now. A CI/CD pipeline that enforces security properties as build-time invariants - type-safe data access, verified authentication flows, policy-as-code for infrastructure configuration - prevents categories of weakness from ever entering the deployable artifact.
3. **Honest accounting of what security is actually doing.** There is a hidden software factory in every organisation building and shipping code. Not just the SOC — the entire security and engineering operation. The scanning tools, the triage queues, the penetration test reports that sit unactioned, the compliance audits that verify process rather than outcome, the six-to-twenty security tools generating alerts at a 60% false positive rate, the developer context-switching back to code they half-remember to fix findings they suspect are noise. All of it is a cost incurred because the defect was not prevented at origin. The deeper question for security leaders is not "how do I staff my security team?" nor "which AI Agents should I buy or build?" It should be: 

> what is the total price of non-conformance across our entire engineering and security operation, and what proportion of that cost would disappear if defects and unnecessary access were prevented at origin rather than discovered after amplification?

In 20th century manufacturing, companies that measured the price of non-conformance discovered that 20-35% of revenue was being consumed by the cost of doing things wrong and then fixing them. The insight was that management only prioritised quality when someone expressed the waste in financial terms.

![Manufacturing discovered the economic argument for building quality in rather than bolting it on](/assets/img/2026-04-16/old-new-model.png)
*Figure 7 - Manufacturing discovered the economic argument for building quality in rather than bolting it on*

Nobody has done this calculation for cybersecurity - not properly, not per-firm. But the aggregate numbers are available and are staggering. A 2022 report from CISQ estimated that poor software quality cost the US $2.41 trillion annually. Developers were spending 42% of their time fixing bugs rather than building features. Eighty-one percent of organisations knowingly ship vulnerable code under deadline pressure, then pay fifteen to a hundred times more to fix the same defect in production that would have cost minutes to prevent at the point of authoring. The multiples of course start getting a lot bigger post-exploitation.

Whether cybersecurity breaks the malware vs identity or inspection vs prevention patterns or confirms them, the investment decisions that matter are being made right now. And they will be made with or without a clear picture of where the actual mismatch sits.

<a href="https://www.linkedin.com/pulse/wrong-race-simon-goldsmith-y2nqe/" target="_blank" rel="noopener" aria-label="Read this article on LinkedIn"><i class="fab fa-linkedin"></i></a> This article was published on [LinkedIn](https://www.linkedin.com/pulse/wrong-race-simon-goldsmith-y2nqe/), please give it a like or share there, or even better let me know what you think.
