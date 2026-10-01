# The Contract Between Canvas and Code

Alex: Let's look at a deceptively difficult systems problem: how do you move from Figma to a working prototype, then to production, and finally back to Figma without losing component identity or letting the artifacts drift apart?

Marcus: The usual framing is that one of those environments needs to become the source of truth. Either the canvas is authoritative, because that's where the system is visually composed, or code is authoritative, because that's where the product actually runs.

Alex: And the more useful answer is that they're authoritative about different things. Figma is strong at visual composition, variants, spatial comparison, and design-system structure. Production code is strong at runtime behavior, data, accessibility, performance, and platform constraints. A prototype is where uncertain interactions become testable.

Marcus: So the architecture shouldn't try to make one artifact own everything. It needs a shared contract that lets those artifacts refer to the same underlying component while preserving their different responsibilities.

Alex: The proposal starts with a registry. Suppose we have a component called a Place Card. Figma knows it by a component key. React knows it as an export from a package. SwiftUI and Compose have their own symbols. A prototype may only expose a rendered DOM subtree.

Marcus: The registry gives all of those one durable identity, something like `maps.place-card`. That identity is intentionally neutral. It isn't a Figma node ID, and it isn't a React import path. Those are environment-specific addresses that can change.

Alex: A registry entry says: this is `maps.place-card`, version three. Here is its Figma component key. Here is the React export. Here are the iOS and Android symbols. It also describes the shared interface: properties, allowed variants, editable slots, required tokens, supported platforms, and deprecation information.

Marcus: It does not contain the component implementation or duplicate Figma geometry. It doesn't own token values or animation code. It's a catalog of identity and shared expectations. The analogy we used was DNS for components. DNS doesn't contain the website; it gives the site a stable name and tells participants how to find it.

Alex: That distinction matters during code-to-canvas capture. Ordinary capture tools see rendered pixels, computed CSS, and DOM boxes. They can reconstruct something visually editable, but a Place Card often comes back as fifteen frames and text layers. The fact that those layers came from one design-system component is gone.

Marcus: With the registry, the development build can expose a stable component ID and serializable properties. A capture agent sees `maps.place-card`, looks it up, finds the linked Figma component, creates a native instance, applies recognized variants, and fills supported slots.

Alex: We should be careful with the word lossless, though. Runtime HTML doesn't preserve every part of application intent. It can't serialize callbacks, network behavior, responsive logic, gesture physics, or the distinction between representative data and meaningful product content.

Marcus: Right. So this isn't universal serialization. It's semantic reconstruction where the contract supports it. If the runtime state maps cleanly to the registered component, the system can reconstruct a native Figma instance. If it doesn't, the system preserves the runtime result as evidence and marks the mismatch.

Alex: That leads to the hardest part: conflict policy. The registry can identify two artifacts as the same component, but identity doesn't tell us which one is right.

Marcus: Imagine the last agreed Place Card used `spacing.5` for padding. Since then, a designer changes the Figma component to `spacing.6`, while a production engineer changes the mobile implementation to `spacing.3`. A direct comparison only sees disagreement.

Alex: To understand the disagreement, we need a three-way comparison. We compare current Figma and current code against the last reconciled baseline. It's the same fundamental idea as a Git merge: base, left side, right side.

Marcus: If only Figma changed, we know the change originated in design. If only code changed, it originated in implementation. If both changed to the same semantic value, they converged independently. If both changed differently, then we have a genuine conflict.

Alex: History still isn't enough by itself. We also need an authority matrix. The registry owns component identity and the public property schema. The token repository owns token definitions and values. Figma owns approved visual composition. Production code owns executable behavior and accessibility semantics. Product data belongs to the application and its data model.

Marcus: Authority doesn't mean that only the owner can propose a change. An engineer can discover a visual problem. A designer can propose a new behavioral state. It means the accepted change has to land in the system that owns that concern.

Alex: Let's make the conflict concrete. Our baseline has four states: collapsed, expanded, navigating, and selected. Figma increases the comfortable padding to `spacing.6`. Production introduces compact mobile padding at `spacing.3` and also adds a loading state.

Marcus: The loading state is a contract gap. It appears in code, but the current registry doesn't recognize it. Code is authoritative about the fact that loading behavior is required. It is not automatically authoritative about the visual language of that state across every platform.

Alex: So the agent creates a proposal. The proposal says that `maps.place-card` needs a loading state, shows where it appeared, attaches a runtime capture and prototype, and identifies the unresolved questions: visual treatment, accessibility announcement, reduced-motion behavior, failure transitions, and support on iOS and Android.

Marcus: The padding difference is a true conflict because both sides changed the same field after the baseline. But review reveals something more interesting than one side being wrong. The designer was improving the comfortable composition, while the engineer was solving a narrow-screen constraint.

Alex: The right resolution is to improve the contract. Version four adds a density property with comfortable and compact values. Comfortable maps to `spacing.6`; compact maps to `spacing.3`. Production chooses density according to an explicit responsive rule, and Figma represents both configurations.

Marcus: That is a good example of what a conflict system should do. It shouldn't merely select a winner. It should expose when two changes reveal a missing dimension in the shared model.

Alex: Now let's place prototyping into this architecture. Figma can show the loading state, but it can't fully answer how it feels under a three-second network delay, how an interrupted gesture behaves, or whether rapidly changing content causes layout instability.

Marcus: A tool like Antigravity becomes the experimental laboratory between design intent and production commitment. An agent uses the registry to pull the correct components and tokens, then builds an interactive spike with real gestures, simulated latency, and representative data.

Alex: The team interacts with that prototype. They may discover that loading is durable enough to become a public component state, while dragging is only a transient implementation state. Both are meaningful, but they don't necessarily belong at the same level of the contract.

Marcus: The agent's job is to make that discovery legible. It compares the prototype with the current registry, classifies differences, gathers evidence, and proposes changes. It does not silently make those changes canonical.

Alex: This gives us a clean division. The prototype discovers what the contract might need to become. The agent translates evidence between systems. Human owners decide what becomes canonical. Production then hardens the approved decision.

Marcus: And production hardening is not copying the prototype folder. The prototype may contain fake data, raw values, simplified error handling, and incomplete accessibility. The approved contract says what must exist. The prototype shows how it should feel. The production repository determines how it has to be engineered.

Alex: So where does approval happen? Figma is a good place to inspect visual consequences, and Antigravity is a good place to experience behavior, but neither should be the canonical decision log.

Marcus: The practical control plane is a pull request against the Git-based registry. The PR contains the proposed contract change and links outward to the evidence: Figma frames, prototype deployments, runtime captures, visual diffs, accessibility notes, and affected implementation repositories.

Alex: The proposal has a lifecycle: draft, ready for review, approved or rejected, implementing, verified, and reconciled. Different categories of change require different owners. A visual token change needs design-system approval. A behavioral state needs a component or platform owner. An accessibility change needs accessibility review. A breaking public change may need approval from every supported platform.

Marcus: This is where GitHub branch protection and `CODEOWNERS` become useful. They aren't the whole policy engine, but they provide durable review, history, comments, and a clear merge event. The agent can open and revise the proposal, request reviewers, and implement approved changes. It cannot satisfy the human approval requirement itself.

Alex: The merge event has precise meaning: this proposal is now part of the shared contract. Before merge, it remains an experiment with evidence. After merge, agents are authorized to propagate the decision across targets.

Marcus: The registry repository might contain component contracts, schemas, proposals, reconciliation baselines, ownership policies, generated platform types, and a small command-line tool. Keeping these in Git makes the decisions branchable, reviewable, and reproducible.

Alex: The CLI is the deterministic backbone. Agents interpret and propose; the CLI parses, validates, classifies, and enforces. We don't want a language model deciding whether a schema is valid or whether a required verification is present.

Marcus: The first command is `registry validate`. It reads every component contract and checks basic structure: IDs, versions, property types, enum values, slots, and platform mappings. Then it checks relationships: referenced tokens exist, Figma keys aren't duplicated, required targets are present, and deprecated components identify replacements.

Alex: The second command is `registry diff --base origin/main`. This isn't a textual YAML diff. It compares canonical models and reports semantic change: a state was added, a property became required, a token mapping changed, or a variant was removed.

Marcus: From that, it can classify compatibility. Adding an optional state may be additive. Removing a public variant is breaking. Changing documentation may be a patch. The repository's policy files define those classifications, so CI and local development reach the same answer.

Alex: Then there's `registry impact`. It asks what work the change creates. If Place Card adds loading, the affected targets might be Figma, React, SwiftUI, Compose, Code Connect, and accessibility verification. Components that compose Place Card may also be indirectly affected.

Marcus: A developer or agent can run `registry proposal create` to represent a possible change without editing the active contract. The proposal contains the semantic operation, origin, rationale, affected targets, required reviewers, and evidence references.

Alex: Evidence can be attached with stable revisions and checksums. A runtime capture should refer to the source commit that produced it. A prototype should identify its deployment revision. A Figma reference should identify the component and version under review. Otherwise the evidence could change after approval.

Marcus: CI runs the same validate, diff, and impact commands. It also evaluates ownership rules and creates a target verification matrix. For our loading state, the matrix might show that Figma and React have passed, Android is in progress, and iOS plus accessibility are still pending.

Alex: Separate repositories introduce a sequencing problem. The contract can't become active until implementations pass, but implementations need the proposed contract in order to compile and test.

Marcus: The solution is a preview registry package for the pull request, something like `component-registry@pr-142`. Each platform branch consumes that preview, implements the proposal, runs its tests, and reports an attestation containing the component, proposal, repository, commit, and verification results.

Alex: Once all required targets and reviewers are satisfied, `registry proposal promote` produces the deterministic patch that updates the active component contract, increments its version, updates generated artifacts, and records migration notes. It still doesn't bypass the pull request or merge itself.

Marcus: After release, `registry baseline record` captures the new point of agreement: registry commit, Figma version, web commit, iOS commit, Android commit, and token version. Future reconciliation runs compare against that exact point.

Alex: Let's walk through the entire scenario once, from beginning to end. We start with Place Card version three. The registry validates cleanly, and all current targets are mapped.

Marcus: An agent builds an interactive prototype and discovers that slow place data needs a loading state. The agent creates a proposal rather than modifying the contract, attaches the prototype and runtime evidence, and runs semantic diff and impact analysis.

Alex: CI confirms that the change is additive but cross-platform. It publishes a preview contract. Figma adds the proposed visual state. React, iOS, and Android implement against the preview. Accessibility review specifies the announcement and focus behavior.

Marcus: Each target submits verification. Human owners approve the intent. Promotion updates the active contract to version four, the PR merges, and a stable registry package is published. After deployment, the baseline records the versions that now agree.

Alex: Later, Figma and production diverge on padding. A runtime capture returns to the canvas, but it doesn't overwrite the design library. The reconciliation command compares both sides to the baseline, identifies the true conflict, and opens a new proposal with both sets of evidence.

Marcus: Review determines that the disagreement represents comfortable and compact density rather than a single winner. The contract becomes more expressive, both targets update, verification passes, and a new baseline is recorded.

Alex: There may eventually be a dedicated control-panel application showing pending proposals, breaking changes, drift, target verification, and stale baselines. But it should remain a view over Git and CI, not a second database of truth.

Marcus: If someone clicks approve in that dashboard, the action should create a GitHub review. If they edit a proposal, it should commit to the proposal branch. If they promote a contract, it should merge the protected pull request after policy checks pass.

Alex: That keeps the architecture honest. The control panel improves usability, but the durable record remains contracts, commits, reviews, attestations, and baselines.

Marcus: The smallest viable implementation is therefore modest: a Git repository, component schemas, a registry CLI, GitHub Actions, protected pull requests, and agent-generated evidence. Start with one component and one Figma-to-React path before attempting every platform.

Alex: Add complexity only when the earlier layer proves useful. First validate identity and the shared property contract. Then add semantic diff and impact analysis. Then proposals and evidence. Then target verification and preview packages. Cross-repository attestations and a custom dashboard can come later.

Marcus: The governing principle stays stable throughout: automatically reconcile only changes already authorized by the shared contract. Preserve everything else as attributed evidence, a proposal, or an explicit conflict.

Alex: And that makes the system more than a design handoff pipeline. It's a controlled learning loop. Figma expresses visual intent. Prototypes test hypotheses. Production exposes operational reality. Agents carry structured evidence between them. The registry records what the organization has actually agreed to.

Marcus: The result isn't perfect synchronization, because perfect synchronization between unequal representations isn't possible. What you get instead is traceable convergence: every important difference has an origin, an owner, a review path, and a verified resolution.

Alex: That's the real infrastructure. Not a magical converter between pixels and code, but a disciplined protocol for turning design and runtime disagreement into better shared contracts.
