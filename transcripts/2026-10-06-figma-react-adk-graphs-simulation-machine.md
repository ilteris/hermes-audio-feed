Two-host technical explainer. Synthetic voices. Hypothetical checkout example.

# Figma to React: ADK Graphs and the Simulation Machine

Alex: Let's build a concrete picture of a Figma-to-React system. A developer selects a checkout screen in Figma and asks for a working React implementation inside an existing repository, using that repository's components and design tokens.

Marcus: The deliverable is a reviewable code patch, with evidence that it builds, renders, and follows the agreed requirements. A screenshot that looks convincing is useful, but it doesn't establish whether the implementation works.

Alex: We'll explain Google's ADK graph workflows, walk through eight stages of this system, and connect them to Viv Trivedy's simulation-machine idea. Our checkout example is hypothetical; the architecture is a proposal, not a report of a system we've already measured.

Marcus: And there are two different sets of eight points. The design framework asks about outcome, correctness, constraints, possible failures, the hardest uncertainty, workflow, evaluation and recovery, and implementation choices. The graph stages turn those answers into an executable process.

Alex: Start with the outcome: generate the checkout interface as a patch against a particular repository revision. Correctness means agreed visual fidelity, valid component usage, compilation, basic accessibility, and the interactions the developer explicitly requested.

Marcus: Constraints include the existing component library, supported browser sizes, permitted files, latency, and cost. Possible failures include invented imports, wrong token modes, lost interaction states, and a plausible-looking screen that fails at a narrow viewport.

Alex: We don't know which failure will dominate before trying it. Our hardest uncertainty might initially be whether Figma components can be mapped reliably to the actual library, but that's a hypothesis to investigate with representative cases.

Marcus: Then we design a workflow that exposes those failures, establish checks and a recovery path, and choose an implementation. ADK becomes a useful implementation choice once we can describe the orchestration it needs to support.

Alex: So what is ADK two point zero graph engineering? ADK is Google's Agent Development Kit, and the graph workflow feature lets you express a process as nodes connected by explicit transitions, including branching and parallel execution.

Marcus: A node can contain ordinary code, a model-backed agent, a tool, or a nested workflow. The graph tells the runtime how work progresses; the model inside a node still handles interpretation or reasoning where that helps.

Alex: The graph we're discussing is a control-flow graph. It isn't a graph database of design relationships, and it doesn't require every step to become a separate autonomous agent.

Marcus: Google's article shows splitting independent lookups, joining their outputs, using deterministic and model-based routing, and pausing for human input. It also describes dynamic child-node execution when the required follow-up work becomes apparent during a run.

Alex: We'll use the documented Python workflow concepts here. Think of the overall path as request, parallel inspection, mapping, optional review, generation, verification, bounded repair, and delivery.

Marcus: Those stages can be ordinary functions around one or two focused agents. ADK provides a place to run and observe that process; it doesn't supply the knowledge of your repository or the correctness criteria for your product.

# Stage 1 — Define and snapshot the request

Alex: Stage one is to turn the request into a concrete job. Our developer asks for the checkout summary, shipping choices, and payment button from a selected Figma frame, at desktop and mobile sizes.

Marcus: We record the file and node identifiers, the repository commit, the component-library version, and the requested interactions. We also specify that this patch presents a checkout interface; implementing payment processing would require a separate behavioral and security specification.

Alex: A static design can't tell us whether a discount is valid or whether the user is authorized to place an order. If those behaviors matter, the job needs API contracts and product requirements in addition to the design.

Marcus: The job state stores requirements and references to captured evidence. Large screenshots and source files live as artifacts, while structured state keeps their locations, revisions, and the decisions made from them.

Alex: This is also where sessions matter. A durable job identifier lets a developer reopen the request, see the latest patch, and continue a review without reconstructing the entire conversation.

Marcus: ADK has session services, including database-backed persistence. We'd configure the appropriate service and test restart behavior, rather than assuming an in-memory development session is durable production storage.

# Stage 2 — Draw the workflow and contracts

Alex: Stage two is to draw the known workflow and define what each node produces. The inspection stage returns design evidence and repository evidence; the mapping stage returns candidate mappings with supporting facts and unresolved questions.

Marcus: Generation returns a patch artifact, and verification returns structured findings with the command, result, and relevant file location. Typed boundaries make it easier to tell whether a failure came from missing evidence, a bad interpretation, or code generation.

Alex: Branches should have explicit reasons. An unresolved high-impact mapping can route to review; a failed build can route to repair; a verified patch can route to delivery.

Marcus: We need a bounded conditional repair loop, because repeating generation forever isn't a recovery strategy. Graph validation can catch structural problems, but we still need to test whether our routing conditions and data contracts mean what we intended.

Alex: Suppose the graph reaches a mapping decision with no component candidates. The route should request more evidence or review, and record why; it shouldn't silently interpret an empty result as successful resolution.

# Stage 3 — Inspect independent evidence in parallel

Marcus: Stage three fans out into independent inspection. One branch gathers Figma design context, component and variable references, and screenshots; another reads the repository, component documentation, package versions, and existing usage examples.

Alex: Figma's MCP server provides tools such as design-context retrieval and screenshots, and Code Connect can provide mappings to existing code components. Repository inspection needs its own file-reading and search tools.

Marcus: MCP is the interface through which those capabilities are exposed. The AI work happens when the model interprets the evidence, proposes a mapping, or generates a patch; merely connecting to MCP doesn't establish that the result is correct.

Alex: The branches can run concurrently because neither initially depends on the other's result. A join brings them together before mapping, so the next stage can compare the design with what actually exists in code.

Marcus: We'd capture both branches' evidence with revisions and source locations. If a library changes halfway through the run, the system should detect that mismatch instead of mixing incompatible snapshots.

Alex: And evidence should include more than the happy screenshot. Selected states, variable modes, responsive requirements, and examples of existing checkout layouts can change the implementation substantially.

# Stage 4 — Resolve components and tokens

Marcus: Stage four resolves mappings. In our example, Figma calls the payment control Primary Action, while the repository exports Button with a solid variant and a large size; the design name alone doesn't prove that they're equivalent.

Alex: The resolver first looks for an existing Code Connect mapping. If there isn't one, it searches exports, component documentation, stories, tests, and real usage, then inspects the candidate's supported properties.

Marcus: This isn't necessarily manual, and it doesn't need a new agent for every missing component. One focused mapping agent can use search and file-reading tools, with ordinary code validating that its proposed export and properties exist.

Alex: It might propose Button, solid variant, large size, and the label as children. The evidence should point to the export, the property definition, and a relevant usage example, rather than presenting the proposal as a fact just because the model sounds confident.

Marcus: Tokens need the same care. Two colors can have identical values but different semantic roles, and a token's meaning can change with light and dark modes; matching hexadecimal values alone is weak evidence.

Alex: For an unmapped component, the policy could allow composition from approved primitives, request a new component proposal, or pause for a developer. The right choice depends on whether this is an isolated screen or a change to the shared design system.

Marcus: The resolver's output separates established mappings, proposed mappings, and unresolved decisions. That distinction gives both generation and review something concrete to work with.

# Stage 5 — Pause for consequential review

Alex: Stage five is human review where it's useful. Imagine the design's shipping selector doesn't match an existing component, and implementing it would create a reusable API; that's a good point to ask the developer to choose between two documented options.

Marcus: ADK's request-input mechanism can suspend a workflow and resume with a response. The implementation needs to put that response into the node's expected input, while preserving the relevant evidence and decisions.

Alex: The request should be specific: here are the candidates, the supported behaviors, and the consequence of each option. A vague instruction to review the whole design creates work without resolving the uncertainty.

Marcus: Resumption semantics matter for debugging and side effects. Some workflow patterns re-enter orchestration code while replaying recorded child results, so we keep orchestration thin and put repeatable units behind explicit nodes.

Alex: That doesn't justify assuming every external effect happens exactly once. Publishing a branch or opening a pull request still needs an idempotency strategy and a record of the external operation.

# Stage 6 — Generate, verify, and repair

Marcus: Stage six generates the patch using the resolved evidence. The generation agent receives the component APIs, token rules, target files, and behavioral requirements, rather than having to guess them from a screenshot.

Alex: We apply the patch in an isolated checkout, then run the repository's relevant checks. A type checker can reject an invented import or property, and the build can reveal dependencies or framework conventions the model missed.

Marcus: We also render the component in a browser at the agreed sizes. Screenshots help assess layout, but we inspect interaction states and accessibility separately: focus behavior, labels, disabled states, and the control's actual semantics.

Alex: Suppose it compiles but the order summary overflows on mobile. Verification returns the viewport, screenshot, element location, and the observed overflow; the repair node gets a specific failure and the evidence needed to address it.

Marcus: A repair attempt modifies the relevant layout, then reruns the affected checks and any broader checks needed for the changed scope. After a small, explicit attempt budget, unresolved findings go back to the developer with the current patch and trace.

Alex: Delivery includes the diff, preview, verification results, and unresolved assumptions. That lets the developer assess the work, and keeps a passed visual check from being mistaken for proof of unrequested business behavior.

Marcus: For debugging, record node transitions, tool arguments and results, artifact references, durations, model configuration, and check outcomes. We'd also capture cost where available and protect sensitive repository content in logs.

Alex: A session answers which job we're continuing. A trace explains what happened during an execution, and an evaluation case lets us ask whether a later version handles the same challenge better.

# Stage 7 — Turn observed work into evaluations

Marcus: Stage seven turns representative requests and observed failures into evaluation cases. We can start with a small baseline set before deployment, and enrich it as real traces reveal cases we hadn't anticipated.

Alex: Each case needs an input, an environment, and a verifier. For our checkout, the environment includes the repository snapshot, Figma evidence, component documentation, and any required interaction fixtures.

Marcus: The verifier combines checks appropriate to that task. Valid imports and compilation are deterministic; whether the layout preserves visual hierarchy might use a rubric with human calibration; accessibility and responsiveness need their own checks.

Alex: ADK supports evaluation of outputs and tool trajectories, and custom metrics can capture application-specific results. But a tool sequence isn't automatically a quality measure; two different sequences can produce equally valid implementations.

Marcus: We'd use trajectory assertions for meaningful requirements, such as consulting the actual API before selecting an unfamiliar component. We wouldn't award success merely because the graph visited every expected node.

Alex: This is where Viv's simulation-machine post fits. He describes starting from real trajectories, building evaluation environments and verifiers, then testing changes to the model, harness, context, tools, or infrastructure before shipping a winner.

Marcus: His Real-to-Sim point is about making the test environment faithfully reflect the work. The Sim-to-Real question is whether an improvement measured there survives when the system encounters actual requests and tools.

Alex: So the simulation machine isn't an extra reasoning agent. It's the experimental machinery around the application, making runs repeatable enough that we can compare versions and inspect why results changed.

# Stage 8 — Build controlled simulation experiments

Marcus: Stage eight builds that machinery. First, capture a real or representative checkout run, including the request, source snapshots, tool schemas, important responses, and expected outcomes; remove sensitive data before reusing it.

Alex: Then restore the repository into an isolated environment and supply captured Figma context. Keep the real compiler and browser checks running where possible, because those are the things that can expose actual React defects.

Marcus: ADK's environment simulation can substitute tool responses and inject errors. That helps test missing mappings, timeouts, malformed results, or a changed API response without relying on a live service for every experiment.

Alex: But mocked tool responses don't create a complete browser, filesystem, and repository simulator. We have to assemble that environment and verify its fidelity; otherwise a simulated pass may only show that the mocks were easy to satisfy.

Marcus: Recorded-response replay also has limits. If a new model makes a different valid tool call, a literal transcript might have no answer for it, so we need argument-aware fixtures or a sufficiently grounded simulation strategy.

Alex: Let's compare two mapping strategies: one gets the screenshot and component names, while the other also receives the relevant property definitions and existing usage examples. Keep the model and verifier fixed to isolate that change.

Marcus: Run both across several tasks and repeated trials, then compare task completion, unsupported component use, visual defects, latency, and cost. We'd report variation and inspect failures, not choose a winner from one successful screenshot.

Alex: Keep some tasks out of the tuning loop, including different layouts and ambiguous mappings. If improvements only appear on the cases we repeatedly edited against, the experiment hasn't established broader reliability.

Marcus: Next, run the candidate on a limited set of real requests and compare its behavior with the simulation. A newly observed mismatch becomes evidence for improving the environment or verifier, as well as potentially improving the application.

Alex: That's the hill-climbing loop: measure, inspect, change a specific part, rerun, and check transfer. It can be useful here, but the measurement system itself needs scrutiny so we don't optimize a score that misses what users care about.

Marcus: There's a practical catch: we need some working machinery before we can discover how that machinery fails. The response is to build a small observable slice, not to pretend the dominant failure is already known.

Alex: Start with one design, one repository, and one patch. Capture the evidence, run real checks, examine what broke, and use those findings to decide which parts deserve more orchestration, retrieval, or evaluation work.

# Choosing the runtime and putting it together

Marcus: Do we need ADK to do this? We could write the model calls with the GenAI SDK and implement the workflow in application code; we'd then own sessions, branching, pause-and-resume, traces, and the surrounding evaluation integration.

Alex: ADK is useful when those workflow concerns are substantial enough to justify a runtime. Its graph gives us explicit control flow, but the SDK still serves the model interface, and the repository tools and verification environment still need engineering.

Marcus: More capable models can reduce how much scaffolding is useful inside a reasoning step. They don't remove the need to identify the repository revision, preserve a review decision, check a build, or measure results.

Alex: The full system therefore has a request interface, durable job state, an ADK workflow, Figma and repository tools, patch artifacts, and a real verification environment. Alongside it sits the evaluation runner, with captured tasks, controlled tool behavior, and result comparison.

Marcus: Keep the first graph small. Parallel evidence gathering, a mapping decision, generation, verification, and bounded repair already provide a useful baseline; add review and dynamic follow-up paths where the work demonstrates a need.

Alex: In the checkout example, success means the developer gets a usable patch built from justified component mappings, with checks and remaining assumptions visible. The simulation machine then helps us learn whether a change improves that outcome across more than one example.

Marcus: That's the relationship between the pieces: the design framework defines the job, the graph runs it, and the evaluation environment tests improvements. We learn their limits by observing actual work, then feed that evidence into the next version.

# Sources and editorial scope

Checked October 6, 2026. The checkout application and proposed architecture are hypothetical. ADK environment simulation is documented as experimental; pin and validate the installed API before implementation.

- [Google: Graph workflows in ADK](https://cloud.google.com/blog/topics/developers-practitioners/graph-workflows-in-adk-everything-you-need-to-know/)
- [ADK workflow guide](https://github.com/google/adk-python/blob/main/docs/guides/workflow/workflow/index.md)
- [ADK graph routes](https://github.com/google/adk-python/blob/main/docs/guides/workflow/graph/index.md)
- [ADK dynamic nodes and replay](https://github.com/google/adk-python/blob/main/docs/guides/workflow/dynamic_nodes/index.md)
- [ADK sessions](https://adk.dev/sessions/session/)
- [ADK evaluation](https://adk.dev/evaluate/)
- [ADK environment simulation](https://adk.dev/evaluate/environment_simulation/)
- [ADK custom metrics](https://adk.dev/evaluate/custom_metrics/)
- [Figma MCP tools](https://developers.figma.com/docs/figma-mcp-server/tools-and-prompts/)
- [Figma Code Connect integration](https://developers.figma.com/docs/figma-mcp-server/code-connect-integration/)
- [Viv Trivedy: Evals and Building a Simulation Machine](https://x.com/Vtrivedy10/status/2107300627617337801)
