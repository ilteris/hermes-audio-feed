A: So the tweet we’re unpacking says OpenAI’s new Agents API is basically “Harness-as-a-Service.” What does that mean in plain English?

B: It means the API boundary is moving outward. Instead of sending a prompt to a model and getting text back, you send work to a managed agent runtime. The runtime handles the loop: model calls, tool use, context, long-running sessions, sandboxes, artifacts, and progress events.

A: So it is not just “better ChatGPT through an API.”

B: Right. The old shape was: prompt plus model gives you a response. The new shape is: task plus tools plus environment plus state gives you completed work, or at least a work attempt with artifacts and a trace.

A: Viv’s point was that OpenAI is turning Codex into an endpoint. Is that fair?

B: Directionally, yes. OpenAI’s own post says: build and run cloud agents with the Codex harness, fully managed by OpenAI. They handle orchestration, long-running sessions, and context management. That is the harness being productized.

A: But “Codex as an endpoint” sounds narrower than what they actually announced.

B: Exactly. The docs make it more general. There is an agent, an environment, a session, events, items, tools, MCP servers, and different execution options. The agent can run with no environment, an OpenAI-hosted sandbox, or an external environment. So the important thing is not only Codex. It is a managed work runtime.

A: Why does that matter for Ilteris’s local work system?

B: Because it validates the direction without replacing the local layer. OpenAI is saying: we can run the work loop. Ilteris’s system is asking a different question: what is the work, why does it exist, what project does it belong to, what evidence proves it, who reviewed it, and when is it safe to merge or close?

A: So one layer executes work. The other governs work.

B: Exactly. The managed harness is the worker. The local control plane is the memory, policy, evidence, review, and handoff layer around the worker.

A: That sounds like an important distinction. If OpenAI owns the harness, should Ilteris stop building agent execution infrastructure?

B: He should avoid competing with OpenAI on generic agent-loop plumbing. Things like long-running sessions, basic tool orchestration, context compaction, and hosted sandboxes are becoming platform services. That does not mean they are unimportant. It means they are less likely to be the unique product center.

A: Then where is the product center?

B: Accountable continuity of work. Turning messy human intent into a project-bound task. Preserving the reasoning and evidence. Running verifiers. Requesting review. Making a merge or close decision. Producing a handoff that survives context reset. That is still mostly unsolved.

A: Let’s make that concrete. OpenAI can tell you the agent finished a session. What does Ilteris’s layer need to know?

B: It needs to know whether the done criteria were satisfied. What files changed. What tests or checks ran. Which artifacts were produced. Whether a reviewer saw the result. Whether there were scope changes. Whether cost or time exceeded the budget. Whether the parent project state is clean. And whether the next person, or next agent, can continue without reconstructing the whole story.

A: So the important artifact is not only the agent’s answer.

B: Right. The important artifact is the evidence packet. The answer is just one output. The evidence packet includes the task, run steps, tool traces, files, test output, review notes, blockers, decisions, and handoff.

A: That connects to the current project signal: the top issue was a verifier gap.

B: Yes, and that is the most important implication. The announcement makes the verifier layer more valuable. If hosted agents become easy to launch, the world gets flooded with agent-completed work. The scarce thing becomes trust: can you prove this work is correct, safe, useful, and ready to ship?

A: So instead of building a better “agent,” build the layer that decides if an agent’s work can be trusted.

B: That is the sharper direction. Agent as worker, verifier as contract, ledger as continuity.

A: What would an adapter look like?

B: A backend-neutral execution interface. A work item gets routed to a backend: OpenAI Agents API, Codex CLI, Gemini CLI, local Hermes tools, browser automation, or a manual step. The backend returns normalized events, artifacts, logs, and completion state. The local registry remains the source of truth for task state, evidence, review, and closeout.

A: That means the hosted harness should not become the durable memory.

B: Exactly. Use the hosted harness as an executor, not the registry. If session state, artifacts, or traces only live inside the vendor runtime, the project loses portability. The local layer should ingest what matters and keep a normalized record.

A: What would you capture from an OpenAI Agents API run?

B: Start with the basics: session id, backend name, model, environment type, input task, streamed events, final message, tool calls, files touched, artifacts created, errors, runtime, cost if available, and links back to source events. Then attach verifier outputs and review notes in the local project record.

A: What should not be captured?

B: Do not blindly dump every token or every transient event into permanent memory. Keep the durable layer compact and useful. Capture source-linked evidence, decisions, artifacts, and reproducible checks. Long raw logs can be archived, but they should not become the primary interface.

A: What is the biggest risk if Ilteris goes in the wrong direction?

B: Building a second generic harness. That would mean chasing the platform vendors on sandbox provisioning, context compaction, model routing, and tool plumbing. It is technically interesting, but strategically weak unless there is a very specific local-first reason.

A: What is the better path?

B: Build a control plane that can use any harness. The hosted agent is replaceable. The project record is not. The review and evidence model is not. The handoff discipline is not. The local filesystem and project history are not.

A: There is also a product design point here. Users do not only want agents to do things. They want to know what happened.

B: Yes. Progress, accountability, and recoverability are product features. A good system says: here is the task, here is what changed, here is the proof, here is the risk, here is the approval needed, and here is the next step. That is more useful than “done.”

A: How does this change the pitch?

B: I would stop pitching it as “a local agent work OS” if that makes people think it is primarily an execution runtime. I would pitch it as a project-aware work control plane for agents. It routes work, records evidence, enforces review, and preserves handoff across sessions and backends.

A: What about voice and everyday use?

B: The same logic applies. Voice should not just trigger an agent. Voice should create or update a durable work item, attach context, ask for approval when needed, and summarize state. The magic is not the speech interface. The magic is that spoken intent lands in the right project memory with the right proof trail.

A: So if the market moves toward Harness-as-a-Service, Ilteris’s system becomes the layer that makes those harnesses usable in real project life.

B: Exactly. More hosted harnesses create more need for coordination above them. The more agents can act, the more you need task ownership, verification, review, provenance, and handoff.

A: What would be the first practical design slice?

B: I would write an “external harness adapter boundary” design note. No big implementation yet. Define the minimal contract: input shape, backend selection, event normalization, artifact import, verifier attachment, review requirements, cost metadata, failure modes, and closeout semantics.

A: And what is the first thing to avoid?

B: Avoid putting vendor-specific session objects at the center of the architecture. They belong in an adapter record. The local task and run model should stay stable even if the backend changes.

A: Give me the one-sentence takeaway.

B: OpenAI is making the agent harness a platform service; Ilteris should own the project layer that decides what work matters, proves whether it was done, and carries that proof forward.

A: That is a strong wedge.

B: It is. The market is moving from “models answer” to “agents work.” The next layer is “work is governed.” That is where this project should focus.
