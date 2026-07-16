# Charlie Hipps Product + Code Interview Prep — Two-Host Rehearsal

A: This episode is a rehearsal for the Charlie Hipps product and code interview at Confido.

B: The goal is not to memorize forty answers. The goal is to make the through-line automatic: start from the product workflow, identify the design-system pattern, cover states and accessibility, then explain the React and MUI implementation choices.

A: So if the interviewer asks anything specific, the answer should still route through that same structure.

B: Exactly. The strongest framing is: I turn design intent into reusable product systems in code. That means workflow patterns, React and TypeScript components, docs, examples, and guardrails that make the right frontend path easier for designers, engineers, and AI-assisted workflows.

A: Let's start with calibration. Who is Charlie likely to be, and what is this round probably testing?

B: Assume the round tests product-system judgment through an engineering lens. The title says Design Engineer, Design Systems. That means the signal is not just visual taste, and not just raw coding speed. It is whether you can turn messy operational workflows into durable implementation patterns without abstracting too early.

A: What's the danger to avoid?

B: Two dangers. First, sounding like a generic component-library person. Second, sounding like an AI-tooling person who drifted away from product quality. The better message is: I use AI aggressively, but I own the workflow shape, component boundaries, state model, accessibility, and code quality.

A: Give me the thirty-second opening.

B: My work sits between design intent and shipped product. At Google Cloud AI, I built reusable Material 3-based implementation systems, product prototypes, Figma-to-code context, and tooling that helped designers and engineers start from shared patterns instead of one-off UI. What stood out from Janice's description is that Confido needs a design-system engineer close to product and code: React and MUI implementation, AI-assisted frontend guardrails, workflow patterns, and adoption. That maps well to the systems work I have done.

A: That sounds polished, but why Confido specifically?

B: The short version is that Confido's design-system work seems tied to real product workflows, not just UI cleanup. The Deep Indian Kitchen Annual Operating Plan case study is a useful anchor: yearly planning across spreadsheets, store counts, velocities, trade assumptions, SKU-level planning, forecasts, and weighted opportunities. That is the kind of complexity where the design system has to expose decisions, evidence, assumptions, states, and actions, not just draw nice cards.

A: So mention the case study once, but don't pretend to be a CPG expert.

B: Right. Say it as a signal of how you reason: I would validate the exact workflows with the team, but the public material suggests reusable patterns around scenario comparison, assumption editing, evidence review, approval flows, and AI recommendation review.

A: Let's rehearse the role fit. Why this role?

B: The part that resonates is the engineering center of gravity. I am strongest where product patterns become implementation infrastructure: components, state coverage, examples, docs, review conventions, scaffolding, and adoption. I can partner in Figma and I care about craft, but my highest leverage is making the system durable in React and TypeScript and useful enough that engineers actually use it.

A: There's a Figma boundary there. How do you say it without sounding dismissive?

B: Be precise. I was not on the Google Material team defining the canonical Figma system. My work was translating design intent into reusable implementation systems: component libraries, product patterns, docs, and tooling. I am comfortable in Figma and understand tokens, variants, state coverage, and system structure. The place I add the most leverage is making those decisions durable in code and easy for engineers to adopt.

A: Good. Now walk me through the main operating principle.

B: Start from the user decision, then explain the component. Treat repeated UI as workflow pressure, not just visual duplication. Make state coverage concrete: loading, empty, error, permission, stale, low-confidence, accepted, rejected, and audit states. Tie AI back to shared context, review quality, state coverage, and human judgment. Use MUI pragmatically: keep it, wrap it, or replace it based on product and codebase signals.

A: Let's make that interview-ready. Charlie asks: how would you approach Confido's design system from scratch?

B: I would not start by declaring a new system in the abstract. I would start by auditing product surfaces, MUI usage, repeated workflow patterns, AI-generated frontend drift, and the places where engineers are losing time or creating inconsistency. Then I would pick a small number of high-pressure primitives and make them excellent: API, states, accessibility, examples, docs, and migration guidance. I would keep MUI where it gives leverage, wrap or extend it where Confido needs more control, and only propose replacement where the product need is clear.

A: What's the tone there?

B: Practical, not platform-y. The goal is to help the team ship faster immediately while gradually creating a more durable foundation.

A: Charlie asks: how do you decide what becomes a component?

B: I look for repeated workflow pressure, not just visual similarity. If two surfaces share the same job, states, data dependencies, and variation points, they may want a shared primitive. If they only look similar but mean different things, abstracting too early creates friction. My default is product-specific implementation first, then extraction once the pattern has proven itself in real surfaces.

A: That's a core answer. Now make it concrete with Confido-like workflows.

B: Representative candidates could be deduction review, scenario comparison, assumption editing, evidence drawers, AI recommendation review, approvals, audit history, and bulk actions. I would validate the exact patterns with the team, but those are the kinds of repeated decision surfaces I would look for.

A: Now let's handle AI-generated frontend code. This might come up because engineers are already using AI tools.

B: The answer is: AI makes the easy path much faster, but it also makes drift faster. The response is not to block AI. It is to give it better rails: canonical examples, component APIs, docs, prompts, context, and checks that guide generated code toward the system. I use AI as a multiplier for exploration, scaffolding, edge-case discovery, and review. I still own the product judgment, architecture, component API, state model, accessibility, tests, and final code quality.

A: What does that mean as an artifact, not just a philosophy?

B: At the artifact level, I would want workflow-based examples, state matrices, do and don't examples, code snippets, and typed component APIs. If the team is using Cursor, Claude Code, or similar tools, I would make the design-system context available as markdown pattern files, repo instructions, snippets, and maybe an MCP-style context server if that became useful. But the basic principle is enough: when someone asks AI to build a surface, the model should see Confido's primitives and examples before it invents its own.

A: Let's unpack workflow-based examples. What do you mean by that?

B: I would not document the system only as isolated components like Button, Modal, Table, or Drawer. Those matter, but product work happens in workflows. A workflow-based example says: here is an Evidence Drawer used when a user needs to understand why a recommendation was made. It explains when it opens, what evidence it shows, what loading or error states exist, how it relates to the parent surface, what actions are allowed, and what needs to be auditable.

A: So if Confido has an AI-generated plan review, what should the example include?

B: It should include the plan summary, the recommendation, assumptions, supporting evidence, confidence or uncertainty signals, and the delta from the current plan. The user should answer: what changed, why did the system recommend it, what evidence supports it, and what happens if I accept it. Then the example should cover states: loading, empty, partial evidence, stale data, low confidence, validation error, permission denied, edited recommendation, accepted, rejected, and audit history.

A: That sounds like product design, but the role also asks for implementation. How do you bridge into code?

B: By drawing the ownership boundary. The shared design-system layer owns the reusable decision interface: recommendation summary, confidence treatment, evidence entry points, comparison display, review actions, status states, accessibility behavior, and supported layout variations. The product surface owns data fetching, permissions, mutations, routing, domain-specific validation, and business side effects.

A: Say the component name is AIRecommendationReview. Where does domain data mapping live?

B: In an adapter close to the product surface. The workflow component should have a typed contract in product-system terms: recommendation, evidence, confidence, delta, status, available actions, and callbacks. A forecast recommendation or deduction recommendation may have very different domain fields. The adapter maps those fields into the component contract. The component should not learn forecast, deduction, or trade-promotion business rules directly.

A: Why not make the component generic enough to handle everything?

B: Because that is how a reusable component becomes a hidden product engine. If the shared component absorbs business rules from every domain, it becomes hard to understand, hard to test, and hard to evolve. The cleaner split is: product page owns data and side effects, adapter maps domain data, workflow component owns the reusable interaction model, and system primitives handle common UI states.

A: Let's rehearse a whiteboard answer. Charlie asks: design an AI recommendation review card.

B: I would start by asking what workflow it belongs to: who uses it, what decision they are making, what data is required, what can go wrong, and what action follows. Then I would define the contract. The component needs a title, recommendation, rationale, confidence, evidence, delta, status, available actions, and callbacks for accept, edit, reject, view evidence, compare, and add note.

A: Keep going. What states would you draw?

B: Loading, ready, partial evidence, low confidence, stale, accepted, rejected, edited, error, and permission denied. Each state should map to what the user needs to know and what the interface does. Loading uses a skeleton and disabled actions. Partial evidence shows a warning and inspect path. Low confidence asks for careful review and comparison. Stale data offers refresh or revalidate. Permission denied explains why the action is unavailable. Error gives a recovery path.

A: What about accessibility?

B: The evidence drawer, comparison table, and action area should be keyboard reachable. Status should not be color-only. Focus should move predictably when the drawer opens and returns when it closes. Buttons should have clear labels, disabled states should explain why when needed, and the action order should keep destructive or irreversible actions distinct from primary confirmation.

A: Now implementation quality: React, MUI, TypeScript.

B: Good React and MUI implementation means clean component boundaries, explicit state modeling, type-safe props, controlled variation points, thoughtful composition, accessible interactions, and restrained styling. I would avoid a rewrite posture. MUI can remain the foundation while Confido builds more bespoke primitives through wrappers, theme extensions, slots, and well-documented patterns.

A: Charlie asks: when do you keep MUI, wrap it, or introduce a custom primitive?

B: Keep MUI when the need is standard and the team benefits from built-in behavior: buttons, menus, dialogs, form controls, tabs, simpler tables, focus behavior, accessibility primitives, and theme integration. Wrap MUI when the product has a repeated Confido-specific pattern but MUI is still useful underneath: review panel, status chip, evidence drawer, assumption editor, approval action bar. Move to a custom primitive when MUI is fighting the product model: too many style overrides, copy-pasted patches, state behavior outside the component, or interactions that are domain-specific enough that the MUI abstraction creates more work than it saves.

A: What codebase signals would you look for?

B: Duplication, override density, inconsistent props, repeated loading and error states, divergent spacing, and whether engineers need too much local knowledge to get the product behavior right. If the system requires too much local knowledge, that is a sign we need a Confido-level primitive.

A: Let's rehearse the deductions workflow challenge. Dense table, retailer metadata, invoice references, AI classifications, confidence, evidence links, comments, and approval actions. Current UI works, but every variation takes too long. What do you say?

B: I would start from the workflow, not the component inventory. For a deductions review surface, the user is trying to identify the deduction, understand the claim, evaluate evidence, compare it against invoices or expected terms, accept or change the AI classification, leave an audit trail, and take an approval action. Then I would separate what is truly reusable from what is coincidentally similar. Reusable pieces might include an evidence drawer, confidence indicator, classification selector, exception state, approval action bar, audit note pattern, and review table pattern with expandable detail.

A: Do you abstract the whole page?

B: No. The first pass would probably be a reference implementation in the real product: one high-pressure workflow made clean, with clear state coverage, API boundaries, accessibility behavior, and examples. Once that proves useful, I would pull stable pieces into system primitives or workflow patterns. The system should preserve product nuance. A deduction review is not just a table plus a drawer. It has decision states, evidence, confidence, auditability, and risk.

A: Now the classic small-team objection: why spend extra time on reference implementation, states, and docs?

B: I would frame it around where velocity is already being lost, not around design-system purity. If engineers lose time to repeated component decisions, inconsistent MUI usage, AI-generated one-off UI, missing states, unclear review expectations, weak examples, or no migration path, the design-system work should create leverage exactly there. The reference implementation solves today's workflow and makes the next similar workflow cheaper.

A: How do you keep a reusable workflow component from becoming a giant configuration object?

B: Watch for too many booleans, props that only apply to one surface, conditionals that are really separate workflows, and teams asking for escape hatches instead of using the component as intended. The fix is composition over configuration. Keep the core primitive small and opinionated, expose slots for legitimate variation, and split the component when product semantics diverge. Shared components should make the common path obvious, not make every path technically possible.

A: Testing. How would you test a shared workflow component without making tests brittle?

B: Start from the contract. Test state coverage: loading, empty, error, partial data, permission denied, disabled actions, optimistic updates, accepted and rejected states, and audit states. At the component level, test stable behavior: actions fire, evidence drawer opens, focus moves correctly, disabled states are respected, and status is not color-only. At the workflow level, use fixtures to verify that a user can understand what changed, inspect evidence, accept, reject, override when allowed, and see the correct follow-up state. Avoid brittle tests that freeze exact DOM structure or spacing.

A: That is strong for a code interview because it shows product quality and implementation maturity.

B: Exactly. It says testing is part of the design-system contract, not just a coverage number.

A: Let's cover performance and reliability.

B: For complex product UI, performance matters where it affects trust, comprehension, or flow. In operational products, a slow table, laggy comparison view, or delayed review action can make the interface feel unreliable. I would watch dense tables, scenario modeling, large payloads, expensive rerenders, and client-side state churn. Tools include virtualization, TanStack Query caching, stable props, lazy loading, and memoization when profiling shows it protects real work. But the design-system angle is that performance states need design too: skeletons, progressive loading, stale-data indicators, disabled actions, and recovery paths.

A: What about reliability and edge states?

B: Operational products need reliable state communication. I would explicitly design loading, empty, partial data, error, stale data, permission denied, validation, optimistic update, conflict, and recovery states. For AI-assisted workflows, I would add provenance, confidence, override paths, human review, and audit trails. The user should know what the system did, why it did it, and what control they still have.

A: How do you partner with product, design, and engineering?

B: I try to make the system useful to each function. For product, I help clarify workflow states and reusable product patterns. For design, I translate visual and interaction intent into durable implementation. For engineering, I create components, examples, APIs, docs, and review conventions that reduce ambiguity. The strongest design-system work is not handoff. It is a shared operating layer for shipping product.

A: Let's prepare for Charlie asking about your Google stories. Which story comes first?

B: The vibe CLI and GitHub Enterprise platform story is best for adoption, tooling, governance, and AI frontend drift. The problem was not only reusable UI. The workflow around UI-system work was inconsistent: repo setup, scaffolding, linting, review expectations, templates, onboarding, and conventions. I treated that as a product problem: how do we make the right workflow the default? I built a Node.js CLI for scaffolding, health checks, blueprint templates, onboarding, and IDE commands, and helped define repo patterns, CI defaults, branch protections, review conventions, and docs.

A: What's the lesson?

B: Governance works when it reduces friction. If the system feels like tax, people route around it. If it helps them start faster and review with more confidence, they adopt it. For Confido, if engineers are using AI tools for frontend, the design system has to be visible as examples, docs, component APIs, and checks that make generated code align with the system.

A: Second story?

B: The Material 3 implementation library. I built and maintained a custom eighty-plus component Material 3 implementation library for Google Cloud AI prototypes and enterprise AI demos. To be precise, I was not defining Google's canonical Material system in Figma. I was building the reusable code layer that let teams express design intent consistently. It covered navigation, panels, forms, dense data states, embedded previews, agent surfaces, and interaction feedback. The hard part was deciding what should be reusable, what should remain product-specific, and how to prevent teams from copying one-off UI as products moved quickly.

A: Third story?

B: The Figma MCP server. AI coding agents need structured design-system context, but raw Figma files are too large and noisy. I built a Python and FastAPI server that exposed Figma files, components, variants, and token-like context as lightweight YAML. The goal was not to let AI own implementation. The goal was to give engineers and AI tools better context while keeping humans responsible for architecture, component boundaries, and final code quality.

A: And the dense product UI story?

B: Vertex AI Studio Model Picker. It was a production Cloud Console surface for browsing, comparing, and selecting AI models. The product challenge was dense decision support: users needed to compare capabilities, understand tradeoffs, and choose a model without being overwhelmed. That maps to Confido because operational products often need compact, trustworthy interfaces where state, assumptions, evidence, tradeoffs, and next actions are legible.

A: Let's hit the personal questions. What are you looking for next?

B: I am looking for a role where I can stay close to product, code, and systems work. I do not want to be only a prototype person, and I do not want to be only a Figma-library person. I am most effective when I can understand the product workflow, build the reusable implementation layer, and create the tooling and guardrails that help other engineers ship faster.

A: Why leave Google, or what happened?

B: My role ended in March as part of a leadership-driven restructuring in Cloud AI. I took a short reset afterward, and now I am being selective about roles that keep me close to product, code, and systems work. Confido stood out because the design-system role seems tied directly to product velocity and engineering adoption.

A: Keep that short and neutral.

B: Yes. Do not over-explain it. Return to what you are choosing next.

A: How do you approach a new product domain?

B: I do not pretend to be a domain expert on day one. I get close to the workflow: customer calls, support notes, spreadsheets being replaced, existing product surfaces, and manual reconciliation people still do outside the product. Then I map actors, decisions, data dependencies, permissions, failure states, review points, and repeated patterns. The goal is to learn enough domain structure to know which abstractions are real and which are premature.

A: How would you help product velocity?

B: I would first identify where velocity is being lost: repeated component decisions, inconsistent MUI usage, AI-generated one-off UI, missing states, unclear review expectations, weak examples, or no migration path. Then I would create leverage at those points: canonical implementations, state matrices, docs, examples, scaffolds, health checks, and review guidance. The system should make the correct path faster than inventing a one-off solution.

A: What makes design systems fail?

B: They fail when they are too abstract, too separate from product work, or too expensive to adopt. If engineers have to slow down to use the system, they route around it. If the system does not cover real states and edge cases, teams fork it. The systems that work make the easy path the correct path, stay close to product surfaces, include real examples, and create migration paths instead of demanding a big-bang rewrite.

A: Let's simulate an objection. Charlie says: this sounds like a lot of tooling. Are you more of a tools person than a product person?

B: I would say the tooling matters only because it changes product execution. I build tools when they make the product pattern easier to adopt, review, and maintain. The core judgment is still product-system judgment: what workflow is repeated, what states matter, what abstraction is safe, and what implementation will help the team ship better surfaces faster.

A: Another objection: why not just use MUI directly?

B: If MUI directly solves the problem and produces consistent product UI, I would use it directly. I would not wrap for the sake of wrapping. But if Confido has repeated operational patterns that MUI does not describe at the product level, then a Confido primitive can encode the domain-neutral workflow: evidence review, confidence, audit note, action bar, state coverage, and examples. MUI can still provide the lower-level mechanics.

A: Another objection: how do you avoid premature abstraction?

B: Start with a real surface, not an invented platform. Let the product pressure prove the pattern. Extract stable pieces only after you understand the shared user decision, state model, and variation points. Keep adapters close to product pages so domain logic does not leak into the shared component. And be willing to split components when semantics diverge.

A: First thirty, sixty, ninety days. What would you say?

B: First thirty days: learn product workflows, audit current MUI usage and repeated patterns, understand where AI-generated frontend drift is happening, and build trust with design and engineering. Sixty days: ship one or two reference implementations tied to real product pressure, with state coverage, examples, docs, and migration guidance. Ninety days: turn the proven patterns into a small operating layer: component APIs, workflow examples, review conventions, and AI-readable context that makes future frontend work more consistent.

A: Good. Now let's cover the coding interview approach.

B: I would first clarify the target user workflow and success criteria. Then I would define the state model and component shape before writing much code. Since Confido allows preferred tooling for the solo coding session, I would use AI for scaffolding and edge-case checking, but I would keep architecture, API, state model, accessibility, and final review under my control. As I build, I would prioritize a polished complete slice: clear layout, typed data, loading, empty, and error states, accessible controls, reusable components where justified, and a final walkthrough explaining tradeoffs.

A: What should the final walkthrough mention?

B: Requirements clarified, state model chosen, component boundaries, MUI or custom styling strategy, what I would add with more time, tests I would write, accessibility and edge states, and how AI helped while I overrode it where product judgment mattered.

A: How do you get from ninety percent to one hundred percent polish?

B: The last ten percent is usually state coverage, spacing, affordance clarity, interaction feedback, accessibility, copy precision, and consistency with the surrounding system. I try to make polish concrete: does the user know what changed, is the next action clear, do disabled, loading, and error states behave correctly, and does the layout survive real data?

A: Let's end with questions for Charlie. Pick a few that show the right taste.

B: First: what product area is creating the most pressure on the design system right now: deductions, trade, demand planning, supply planning, cash application, or something else? Second: where do engineers lose the most time today: figuring out MUI usage, recreating patterns, handling edge states, visual polish, PR review, or AI-generated frontend cleanup? Third: when a new workflow is being built, how do product, design, and engineering decide whether a pattern should become reusable or stay product-specific?

A: Add one about contribution shape.

B: What does a strong design-system contribution look like in your codebase today: a component, an example, a doc, a migration, a review convention, or all of the above?

A: Add one about outcome.

B: What would make you say after six months that this hire changed how Confido ships frontend?

A: Now give me the pressure phrases. These are the short lines to remember if the conversation gets dense.

B: First: I start from the workflow and the user decision, then decide which component abstraction is real.

A: Second.

B: Repeated UI is signal, but repeated workflow pressure is what justifies a shared pattern.

A: Third.

B: The shared layer owns the reusable decision interface. The product surface owns data, permissions, mutations, and business meaning.

A: Fourth.

B: AI can accelerate frontend work, but the design system has to give it better rails and humans still own the review bar.

A: Fifth.

B: MUI can stay the foundation. The question is where Confido needs product-level primitives on top of it.

A: Sixth.

B: Shared components should make the common path obvious, not make every path technically possible.

A: Seventh.

B: The system should make the correct path faster than inventing a one-off solution.

A: And the closing line?

B: The role is interesting to me because the design system seems directly tied to product velocity. Confido is not just asking for a component library; you need a reusable implementation layer for complex operational workflows, AI-assisted product behavior, and a fast engineering team. That is the kind of systems work I want to do.

A: Final meta-advice before the interview?

B: Keep every answer anchored in design-system value first, then implementation detail. Don't over-index on AI. Don't over-explain Google. Don't pitch a rewrite. Don't apologize for not being a pure Figma-library designer. You are strongest where product workflow, component architecture, React implementation, and adoption meet.

A: And if Charlie goes very engineering-heavy?

B: Welcome it. Answer through the Design Engineer lens: product pattern, craft, state coverage, accessibility, adoption, then code. That is the role.

A: Crisp. That's the rehearsal.

B: Exactly. The interview thesis is simple: product complexity becomes reusable implementation infrastructure when the design-system engineer stays close to the workflow, the code, and the team adopting it.
