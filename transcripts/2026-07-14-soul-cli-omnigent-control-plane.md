# Two-Host Audio Overview: Soul CLI vs Omnigent Control Planes

A: Today we’re unpacking a code audit of Omnigent and Soul CLI as agent control planes. The question is simple: how do you coordinate multiple agents without losing track of who did what, why, and where the result landed?

B: Exactly. The audit says these systems solve adjacent layers. Omnigent is strongest as a live session control plane. Soul CLI is strongest as a durable work ledger and operator workflow.

A: Let’s make that concrete. What is Omnigent’s core idea?

B: Omnigent makes the session, or conversation, the main product primitive. A child agent is not just a hidden subprocess. It becomes a durable conversation node with a parent, a root session, a runner, a name, status, files, permissions, and UI visibility.

A: So if an agent spawns a researcher, or a reviewer, the system can show that worker as a real thing.

B: Right. The report calls this Omnigent’s biggest design win: if a worker exists, it is visible as a session node. That means the UI can render a tree of parent and child sessions, and the parent can receive results through an inbox-style path instead of just waiting for a synchronous tool call.

A: The continuation piece stood out to me. Omnigent can address a worker by agent and title, not just by an opaque ID.

B: That is important. If you have a recurring “architecture reviewer” or “implementation researcher,” you want to continue that thread by semantic role. Omnigent’s spawn-or-continue model supports that. It can reuse a child session by the pair of agent and title, and it rejects create-time changes that should not apply to an existing session.

A: That sounds like the kind of small design choice that matters a lot once the system gets used daily.

B: It does. Raw IDs are fine for machines, but people and orchestrators need stable names. A named worker can build context over time. A one-off delegation ID usually cannot.

A: What about authority? The audit says Omnigent splits control powers.

B: It separates read-only discovery, declared subagent sends, arbitrary spawning, and session sharing. That is the right mental model. Being able to call one named reviewer should not automatically mean you can spawn any arbitrary agent, share a session, or mutate permissions.

A: And it also mirrors native runtime behavior, right? Claude and Codex children can show up in the same session tree.

B: Yes. That’s another strong product move. Omnigent does not pretend every backend is identical. Instead, it observes native child work and projects it into the common session model.

A: Okay, so Omnigent has a clean live collaboration surface. What is its weakness?

B: The session tree is not the same thing as a typed work graph. Omnigent can say, “A spawned B.” It is weaker at saying, “B produced artifact X, C reviewed it, D blocked merge, and this result supersedes that earlier artifact.” Much of that higher-level coordination still lives in prompts, labels, and conventions.

A: So it knows who talked to whom, but not always what work contract was satisfied.

B: Exactly. The audit points at the Polly example: reviewer and implementer discipline exists, but a lot of it is prompt-level. Useful, but not as enforceable as a real task, run, step, artifact, and verifier model.

A: Now contrast Soul CLI. What is the main primitive there?

B: Soul CLI’s primitive is the audited work transaction. It has task records, run records, step records, worktree records, verifier and review evidence, commit metadata, finalization events, and causal session ledgers.

A: So instead of starting from “what session is this agent in,” it starts from “what work is being done and what evidence proves it.”

B: Yes. A run step in Soul CLI is not just a command. It records the command contract, input hash, workspace fingerprint, idempotency key, retry policy, lock file, attempt artifacts, stdout and stderr references, result payload, and failure diagnostic.

A: That is a very different level of accountability.

B: It is. Soul CLI can reconstruct the path from task intent to execution evidence. Omnigent is better at live agent visibility, but Soul CLI is better at work admissibility: when is something actually reviewed, verified, committed, merged, and closed?

A: The audit also emphasizes commit gates.

B: Yes. Soul CLI’s commit path refuses to commit without completed verifier and review evidence unless there is an explicit override. It requires fixed and verification fields. It builds structured commit metadata and validates it before writing history. That is much stronger than saying, in a prompt, “please have reviewers review.”

A: What about the causal ledger?

B: That is another Soul CLI strength. Hook events are not just flat transcript lines. The ledger stamps node IDs, parent IDs, turn IDs, causal roles, registry snapshots, byte offsets, and pointer updates under lock. That gives you a replayable chain of why one action followed another.

A: And the workstream ledger links projects, tasks, threads, sessions, runs, and workspaces.

B: Right. That is close to the typed layer Omnigent is missing. The report’s key recommendation is not to replace Soul CLI’s work model with Omnigent sessions. It is to add Omnigent-style session visibility around Soul CLI’s existing task-run-step evidence.

A: So what is Soul CLI missing today?

B: First, delegated workers are not first-class session nodes. A subagent can show up as a delegation event, a run step, a live log, and a finding JSON file. That is durable, but fragmented. You can reconstruct it, but the UI cannot simply say, “here is the child session.”

A: That sounds like the difference between auditability after the fact and collaboration while the work is happening.

B: Exactly. Soul CLI is very good after the fact. Omnigent is stronger during the work. The goal should be both: active child sessions that are visible live, and every child session linked back to typed work evidence.

A: The second missing piece is stable worker continuation.

B: Yes. Soul CLI currently generates delegation IDs from call IDs or UUIDs. That is good enough for one-off review steps. It is weaker for a long-running named specialist. The report recommends adding optional title-based continuation: specialist plus title plus parent session becomes the stable key.

A: And the third missing piece is a parent inbox.

B: Right. Today child completion can appear in hook events, step status, finding files, and logs. The parent needs a compact queue: child started, child progressed, child blocked, child completed, child failed, result available, approval requested, approval resolved.

A: This is where Omnigent’s blocked-child notifier is useful as a design lesson.

B: Yes. Completion-only notifications are not enough. A parent needs to wake up when a child is blocked, not only when it finishes. If an approval prompt or permission boundary blocks a child, the parent should see that as a first-class state.

A: The audit also gets specific about authority boundaries.

B: It recommends explicit capability policies: declared delegation allowed, arbitrary delegation denied, session sharing denied, workspace writes allowed only within task scope, maximum child sessions, maximum depth, allowed specialists. The key is that authority should be inspectable and enforced by code, not implied by prompt text.

A: Let’s talk about the sharp critiques from the addendum. One was that Soul CLI still has split authority for writes.

B: Yes. The authority client is intentionally narrow and read-only for generic remote methods. That is safe. But most mutations still happen locally through CLI commands writing filesystem records. If Soul View becomes a multi-client live control plane, write-capable operations need typed authority methods instead of scattered direct writes.

A: Another was that some event paths bypass the strongest ledger writer.

B: Correct. The report calls out raw event append paths like workspace creation and prompt coordinator writes. The recommendation is to preserve any special requirements, like offset acknowledgements, but route actual event construction and stamping through one shared append API.

A: Why does that matter architecturally?

B: Because schema drift happens when every writer constructs event dictionaries independently. A single event writer protects canonical names, fields, timestamps, causal roles, and ledger stamping.

A: The addendum also mentions task lifecycle atomicity.

B: Task mutation is already careful about exact matching and locking, but lifecycle transition can update JSON and then move files. If the write succeeds and the move fails, status and bucket can diverge. The recommendation is either event-source task status, or add a recovery scanner that reconciles mismatches.

A: And run-level concurrency?

B: Runs record concurrency metadata, but the inspected path did not obviously enforce run-level concurrency. If Soul adds Omnigent-like async child spawning, that becomes more important. You need a lease keyed by the concurrency key before creating competing runs.

A: There was also a note about shell execution.

B: Soul’s shell step runner is fine for a local operator CLI. But if remote clients can trigger run steps through a daemon, shell execution needs explicit capability envelopes. Otherwise the remote control plane becomes too broad.

A: Now what is the recommended implementation sequence?

B: First, normalize event writes. Add missing event constructors and remove raw hook writes for workspace and subagent lifecycle events. Done when every new hook event has a canonical constructor and a stamped ledger row.

A: Second?

B: First-class child session records. When a subagent is dispatched, mint a child session ID, write a child thread record with parent session ID, link it to run step and workstream metadata, and preserve native provider transcript IDs as metadata, not identity.

A: Third is parent inbox.

B: Yes. Add an append-only inbox per parent session. Emit lifecycle, result, blockage, and approval rows from subagent and run-step transitions. The parent should not have to scan five surfaces to know what happened.

A: Fourth is stable worker titles.

B: Add title or continue-title to delegation and review. Maintain a child index under the parent session. Repeated review or research calls can continue the same child context by semantic name.

A: Fifth is the unified work graph projection.

B: Exactly. Extend the projection to include task, run, step, workspace, parent session, child session, finding, commit, finalize, and workstream edges. Soul View should render one graph similar to Omnigent’s subagent rail, but richer because it includes typed work evidence.

A: Sixth is authority.

B: Add declarative delegation, session, and workspace capability config. Enforce it in both CLI and registry-server methods. Keep generic remote authority read-only until write schemas and checks are explicit.

A: If you had to pick one first patch, what is it?

B: First-class child session nodes for subagent delegations. It unlocks most of the rest. Mint child_session_id before provider spawn, create thread.json, add a ChildSessionLinked event, route the parent event through the canonical event writer, and expose parent-child edges through thread listing or a session graph endpoint.

A: Why not start with inbox or UI projection?

B: Because the child node is the substrate. An inbox message needs a target. A projection needs nodes and edges. Capability boundaries need a session to attach to. Without child session records, everything else remains a join across logs, steps, and findings.

A: So the north star is not “copy Omnigent.”

B: Right. The north star is a filesystem-backed agent work control plane where every live agent is a session node, every session node links to typed work evidence, and every work transition is causally stamped and replayable.

A: That combines Omnigent’s live inspectability with Soul CLI’s work discipline.

B: Exactly. Omnigent teaches how to make active multi-agent work visible and steerable. Soul CLI teaches how to make work accountable, verified, and replayable. The useful path is to combine those layers, not choose one over the other.

A: Final takeaway?

B: Omnigent’s session tree answers, “who is working, and how can I collaborate with them right now?” Soul CLI’s work ledger answers, “what work happened, what evidence supports it, and is it safe to close?” The next version of Soul CLI should answer both in one graph.
