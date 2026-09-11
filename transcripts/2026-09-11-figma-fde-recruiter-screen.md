A: Today we’re turning the Figma Forward Deployed Engineer recruiter screen prep into a spoken rehearsal. The goal is not to memorize a wall of answers. It is to make the shape feel natural.

B: Exactly. The core frame is simple: Ilteris should come across as a hands-on AI product engineer and forward-deployed builder, not only a design or prototyping person.

A: So the hidden test is credibility.

B: Yes. The recruiter may ask normal screen questions, but underneath that they are checking: can this person earn trust with enterprise customers, diagnose real technical blockers, build the missing path, and bring repeatable learning back into Figma product?

A: And there’s a second layer because this is a founding FDE team.

B: Right. The founding-team angle matters, but it should not sound like he wants to skip into management. The clean version is: hands-on first, learn the field motion, then codify playbooks, examples, onboarding, hiring signals, and customer patterns.

A: Let’s start with the thirty-second version. If Michael says, “walk me through your background,” what’s the posture?

B: Start senior and technical. Something like: “I’m a senior AI product engineer and design technologist with twelve-plus years building user-facing software, developer infrastructure, AI workflows, and design-system-adjacent tools. I spent eleven years at Google, including six years in Google Cloud AI, where my work sat between product engineering, UX engineering, AI prototyping, and internal developer platforms.”

A: That lands the level. What comes next?

B: The through-line: “I make ambiguous AI product ideas usable inside real engineering environments.” Then use the Cloud AI GitHub Enterprise and vibe CLI story: shared repos, CI/CD defaults, onboarding, scaffolding, health checks, preview workflows, and adoption across fifty-plus UX, PM, and engineering partners.

A: And the Figma-specific hook?

B: Mention the Python-based Figma MCP server. But be careful on chronology. Don’t say it was more recent if it came before the later Cloud AI GHE and UXR platform work. Say: “Earlier in that arc, I built a Python-based Figma MCP server that exposed files, nodes, variables, styles, and components to agent workflows.”

A: Why does that matter for this role?

B: Because the role is about making design systems agent-readable and production-connected. The point is not “I played with Figma.” The point is: agents need structured design context, component contracts, token rules, and verification, or they generate plausible but off-system UI.

A: Good. Now the question everyone gets: “Why Figma, and why this role?”

B: The answer should be specific to Figma’s current direction. “Figma is becoming the operating layer for how teams design, build, and now increasingly generate product together. The interesting problem is no longer just producing code from a design. It is making design context, repo context, component systems, and AI workflows meet in a way teams can trust.”

A: Nice. And why FDE specifically?

B: “This role feels like the field version of problems I’ve already been solving. It asks someone to enter real customer environments, understand their design system and codebase, get workflows like Make Local, Code Layers, Code Connect, MCP, and agent skills working against their constraints, and turn repeated friction back into product improvements.”

A: The prep doc also mentions Al Kemner’s post. How do we use that without sounding like we’re quoting LinkedIn too much?

B: Use it as a clarifier. “Al’s post made the role clearer to me: the job is to find the exact blocker in a production workflow, whether it lives in the customer environment or in Figma, and make the change that unlocks the next step.”

A: That phrase is important: whether the blocker lives in the customer environment or in Figma.

B: Yes, because it distinguishes FDE from pure advisory work. This is product engineering in the field. Sometimes the fix is customer-side mapping, examples, setup, or CI. Sometimes the evidence should become a Figma product change, maybe even behind a flag for one customer.

A: Let’s unpack the product surface: Make Local, Code Layers, Code Connect, MCP. How should he talk about those without overclaiming internal roadmap knowledge?

B: Use “the way I understand it.” Then say: MCP gives agents structured access to Figma context: selected frames, nodes, components, variables, and hierarchy. Code Connect maps Figma components to production components: imports, props, variants, examples, and usage rules.

A: And Make Local and Code Layers?

B: Keep it broader: they sit closer to the workflow where Figma artifacts become local implementation work or code. The enterprise challenge is that tools like this are only as good as the customer’s system: token hygiene, component naming, Storybook quality, repo structure, CI checks, auth constraints, and review gates.

A: So the answer shouldn’t be a feature tour.

B: Exactly. It should become the FDE plan: make the design system legible to the toolchain, wire the right contracts, prove the workflow on a narrow screen or component set, and feed recurring gaps back to product.

A: There’s a big risk area: customer-facing experience. The prep doc says not to frame it as “I do not have customer-facing experience.”

B: That’s crucial. The better frame is: “I haven’t held the formal Forward Deployed Engineer title, and I don’t want to overstate that. But customer-facing technical work is not unfamiliar.” Then cite Fox Sports, public infrastructure, Finance Risk, and Cloud AI internal enterprise customers.

A: What’s the bridge sentence?

B: “I’ve been in customer-facing technical rooms where the work was to listen for constraints, translate them into a technical path, build enough to make the solution real, and bring the learning back into the product or platform.”

A: Then immediately move to evidence.

B: Right. The GHE and vibe story is the strongest scalable proof. It shows embedded discovery, implementation, onboarding, governance, and turning repeated friction into reusable infrastructure.

A: Let’s rehearse that story in a conversational way.

B: “At Google Cloud AI, designers and product teams were building AI prototypes in fragmented local tools and one-off environments. The work was valuable, but it was hard to share, review, branch, deploy, or hand off to engineering. I saw an opportunity in Google’s GitHub Enterprise effort and pushed Cloud AI UX to become an early adopter.”

A: Then the build.

B: “I led the migration pattern: repo structure, CI/CD defaults, branch protection, onboarding, deployment playbooks, and later a Node.js CLI called vibe that handled scaffolding, health checks, project defaults, and IDE commands.”

A: And the adoption result.

B: “The important part was not only the tooling. Designers needed low-friction setup, PMs needed shareable demos, engineers needed reviewable code, and platform teams needed governance. Direct adoption reached fifty-plus UX, PM, and engineering partners, with broader activity around three thousand commits and fifteen hundred PRs.”

A: Then tie it back to FDE.

B: “That maps closely to FDE work: enter a messy workflow, make the path real, reduce setup friction, and turn the solution into reusable defaults.”

A: Good. Now let’s handle the Figma MCP server question. What are they testing?

B: They’re testing if the artifact is real and whether he understands why it matters. The answer should focus on agent-readable design context, not just API plumbing.

A: Give the answer.

B: “The problem I was exploring was that AI coding agents can generate plausible UI, but they hallucinate design-system structure when Figma context is not queryable. A screenshot or flattened description doesn’t tell the agent enough about component identity, variants, variables, styles, or cross-file library references.”

A: Then what did it do?

B: “So I built a Python-based Figma MCP server that made Figma files more agent-readable. It could fetch targeted nodes, expose hierarchy, resolve components, extract variables and styles, and compress the result into context an agent could use.”

A: And connect to Code Connect.

B: “For this FDE role, MCP gives the agent what is in the Figma file. Code Connect or an equivalent contract tells the agent which production component and props that Figma component maps to. Then evals or verifiers catch drift.”

A: Avoid overselling scale.

B: Yes. The strength is technical relevance and timing, not massive enterprise adoption.

A: Let’s move to the design-system-agent-ready question. It sounds abstract. How do we make it concrete?

B: Start small. “I would pick one representative workflow, not a full-system rewrite. I’d inspect the Figma library, token structure, component naming, variant properties, docs, Storybook or equivalent examples, and the production component API. Then I’d choose one high-value screen where AI-generated code currently fails review.”

A: Three layers?

B: Context, contract, verification. First: Figma MCP or API data so the agent can see frames, components, variables, and hierarchy. Second: production contract mapping Figma components to React components, imports, props, tokens, and required states. Third: checks that reject hardcoded colors, raw controls, missing states, wrong imports, or off-system component use.

A: And then compare before and after.

B: Exactly. “Run the same generation task before and after. If the second attempt passes the customer’s review gate and a token change propagates correctly, we have a repeatable adoption pattern rather than a demo.”

A: This is where the Acme demo can show up.

B: Yes, but keep it short unless asked. “I built a tiny Acme field lab around this: same Figma frame, same agent, first attempt fails because it invents raw controls and hardcoded colors; second attempt passes after Code Connect-style mappings and a verifier; then a token change flows through.”

A: That’s a clean one-minute demo talk track.

B: The final point is: the value is not AI-generated UI by itself. The value is trusted, system-faithful generation inside real customer workflows.

A: Now agentic tools and workflow orchestration. What’s the best proof?

B: The nine-pass UXR pipeline. It shows practical LLM workflow architecture: structured context, evidence quotes, timestamps, trace logs, human checkpoints, and judge passes.

A: Say the result carefully.

B: “In pilots, it reduced per-study analysis from roughly four hours to five minutes while keeping researchers in control.” That’s a strong metric, and it also shows the right AI posture: speed plus trust.

A: If they probe model depth?

B: Use the boundary: “I’m not an ML researcher; I’m strongest where model behavior becomes product and workflow architecture.” That’s honest and strong.

A: Next risk: production software. This is where a recruiter might worry he is too prototype-heavy.

B: The answer needs to be precise without being defensive. “I have shipped production software and built production-grade internal systems, but I want to be precise about claim boundaries.” Then cite Jamboard/Meet experiences, Vertex AI Studio Model Picker shaping a GA Google Cloud Console feature, and end-to-end systems with frontend, backend, model/API integration, persistence, Docker, Cloud Build, and Cloud Run.

A: What’s the final capability sentence?

B: “I’m hands-on across the stack and have shipped or materially shaped production product work, while my recent Google Cloud AI role sometimes separated UX engineering from final production deploy ownership. For this role, the relevant strength is that I can build the connective tissue between product intent, code, AI workflows, and adoption.”

A: That does two things: it tells the truth and ends on the relevant strength.

B: Exactly. Don’t let the answer trail off into apology.

A: What about unfamiliar customer codebases, CI, and non-standard stacks?

B: Answer like an operator. “I would first map the repo structure, package manager, design-system package, build path, test path, CI checks, deploy path, auth/access model, and where generated code is supposed to land. Then I would reproduce the failure locally or in the customer’s approved environment before changing anything.”

A: Nice. That sounds calm.

B: It also matches the job: customer environments are messy. The right signal is not “I know every stack.” It is “I know how to map constraints, reproduce failures, and narrow the problem.”

A: Let’s talk founding team and leadership.

B: The answer should be: yes, motivating, but hands-on first. “I would start by solving customer problems directly, learning where deployments break, and understanding what excellent FDE work looks like at Figma.”

A: Then systematize.

B: “Then I’d help turn that into playbooks, onboarding, customer-discovery templates, example mappings, verifier patterns, hiring questions, and a clear bar for technical judgment plus customer communication.”

A: Mention mentoring and interview panels?

B: Yes, briefly. “I’ve mentored engineers and designers, served on UXE interview panels, and taught AI-augmented UX patterns to a two-hundred-plus person audience.” Then return to the key idea: leadership has to stay grounded in real customer and engineering work.

A: Let’s cover logistics quickly.

B: “I’m based in Brooklyn/New York; the posting lists New York, US hubs, and remote US, so that works. I’m comfortable with customer-facing work and periodic travel for key customer engagements. I’m authorized to work in the United States.”

A: Any caveat?

B: Confirm exact travel comfort and work authorization wording before the call. If compensation comes up, the posting lists a wide base range, so unless he wants to anchor, it’s reasonable to defer and say fit, scope, and level are the first priority.

A: What should he ask Michael?

B: Pick two or three depending on time. The best first one: “How is Figma defining success for the founding FDE team in the first six to twelve months?”

A: Second?

B: “How much of the role is direct customer implementation versus product engineering back inside Figma?”

A: Third?

B: “What are the most common blockers customers hit with Make Local, Code Layers, Code Connect, or agent-assisted design-to-code today?”

A: There’s also the Al Kemner product-change-behind-a-flag question.

B: Good if the conversation is already technical. “Al’s post mentions that sometimes the answer may be a Figma product change behind a flag for one customer. How often do you expect early FDEs to ship into Figma product code versus customer-side integrations, templates, or playbooks?”

A: Let’s do a compressed answer bank. Question: “Tell me about yourself.”

B: “I’m a senior AI product engineer and design technologist with twelve-plus years across user-facing software, developer infrastructure, AI workflows, and design-system-adjacent tooling. At Google Cloud AI, I built the connective tissue that made AI product work usable in real engineering environments: shared repos, CI defaults, onboarding, a CLI, and agentic workflow infrastructure. I also built a Python-based Figma MCP server earlier in that arc, which is directly relevant because this role is about making Figma context and production systems legible to agents and engineers.”

A: Question: “Why this role?”

B: “Because it combines field diagnosis and product engineering. I’m excited by the version of FDE where you go into a customer’s real workflow, find the exact blocker, build or change the path, and turn what repeats into product, docs, examples, evals, or team playbooks. That is the kind of adoption work I’ve done inside Google-scale environments, and Figma’s design-to-code and AI direction makes the problem especially timely.”

A: Question: “Are you customer-facing enough?”

B: “I haven’t held the exact FDE title, but I have been in customer-facing technical rooms and I know the pattern: listen for the constraint, translate it into a technical path, build enough to make it real, and bring the learning back to the product or platform. Fox Sports is a clean external example, and my later Cloud AI work was the internal enterprise version of that motion across UX, PM, engineering, platform, and executive stakeholders.”

A: Question: “What would you do for a customer whose AI-generated UI keeps failing design-system review?”

B: “I’d choose one narrow workflow first. Map their Figma components, tokens, variants, production component APIs, Storybook examples, repo structure, CI checks, and review gates. Then I’d add the missing contract layer between Figma and production code, likely through Code Connect-style mappings, plus verifiers that catch hardcoded colors, raw controls, wrong imports, or missing states. Then I’d rerun the same task and measure whether it passes review.”

A: Question: “Do you want to manage?”

B: “I’m open to leadership responsibility, especially on a founding team, but I would not want to skip the hands-on credibility step. For this role, I think the right sequence is: be excellent in the field, codify the patterns, help hire and onboard, and only then decide what formal management shape the team needs.”

A: Let’s end with the mental model for the call.

B: Three anchors: hands-on engineer, field diagnostic loop, product feedback loop. Every answer should come back to one of those.

A: And what should he avoid?

B: Avoid sounding like a design-only candidate. Avoid saying he lacks customer-facing experience. Avoid overclaiming Figma’s internal roadmap. Avoid making the Google/Figma adoption angle sound like insider knowledge. And avoid getting defensive about production ownership; be precise, then bridge to capability.

A: Final rehearsal version: what is the thesis of Ilteris for this Figma FDE role?

B: He has seen the problem from inside a Google-scale environment: AI and code-first workflows can move faster than design-system integration. He has built the kinds of tools that connect product intent, code, AI workflows, and adoption. And he can operate in the FDE loop: diagnose the customer blocker, build the missing path, verify it works, and turn the learning into a repeatable product or team pattern.

A: That’s the story. Not “I like Figma.” Not “I make prototypes.”

B: Right. The story is: “I help complex teams make AI product workflows real, reviewable, and adoptable. Figma is where that work can become part of how product teams build.”
