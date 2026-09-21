---
title: "The Agent Hypervisor: Why Google Calls AX Orchestration"
date: "2026-09-20"
hosts:
  Alex: "en-US-AvaNeural"
  Marcus: "en-US-AndrewNeural"
---

Alex: Let's unpack the new Google repository that just dropped over the weekend, google slash ax. It's titled an open agentic orchestrator, and it was architected by Jaana Dogan's team.
Marcus: Yeah, the name immediately caused some confusion in the AI engineering community. When most AI developers hear the word orchestration today, they think of cognitive coordinators: LangGraph state graphs, CrewAI swarms, prompt chains, or consensus loops.
Alex: Right. They think orchestration means answering who speaks next and what tools are in context. But when you open the AX source code, there are no prompt chains. There's no vector store, no agent chat room, and no consensus graph.
Marcus: Exactly. Because Jaana Dogan and the engineers building this come from the Kubernetes and distributed systems world. They're using orchestration in the classic Borg sense: physical resource scheduling, sandboxing, network perimeter fencing, storage attachment, and cluster reconciliation.
Alex: It's the difference between the conductor of the choir and the structural engineers who built the concert hall with fire exits, soundproofing, and power limits so the building doesn't burn down.
Marcus: That's a great distinction. AX is fundamentally an agent hypervisor.

Alex: Let's talk about why Google needed a new systems hypervisor for agents in the first place. Why couldn't they just run agents as standard Kubernetes Pods or Jobs?
Marcus: Because agents are an uncooperative, hostile workload. Standard microservices are long-running and stateless. Standard batch jobs run from start to finish, exit zero or one, and have predictable resource bounds.
Alex: But autonomous agents don't fit either bucket.
Marcus: Not at all. An agent is transient, but it accumulates gigabytes of local workspace state. It executes untrusted code, clones arbitrary Git repos, runs package managers, and spends long periods idling while waiting on API responses or human approvals.
Alex: And if you spin up tens of thousands of transient Kubernetes Custom Resources or Pods every day, you destroy etcd.
Marcus: Precisely. The write churn and compaction overhead pushes etcd past its single-digit gigabyte limits. That's why AX's architectural pivot in commit dc4f36c is so interesting: they completely bypassed etcd for task state and moved it to Redis.

Alex: Let's trace that control plane architecture. How does a task move through AX?
Marcus: A developer or parent agent applies a manifest through the ax CLI, which hits ax-server over gRPC on port 8080.
Alex: And ax-server is completely stateless.
Marcus: Right. It validates the manifest, writes the task state into a Redis Hash, and appends a reconcile event to a Redis Stream called ax colon stream colon tasks.
Alex: Then you have a horizontally scaled pool of reconciler workers, ax-controller, that join a consumer group and pull events using XREADGROUP.
Marcus: Exactly. And those reconcilers translate the desired task specification into reality on top of Agent Substrate.

Alex: That brings us to the four declarative primitives in AX: Task, Workspace, Gateway, and Model.
Marcus: Right. The Task is the isolated execution unit. It specifies the container image, the command, CPU and memory limits, and bound workspaces.
Alex: The Workspace decouples the environment from the container image. It defines Git repos to clone, MCP server configurations, and an optional natural-language goal that an internal setup agent bootstraps before the primary task command even starts.
Marcus: And the Gateway handles the network perimeter. Instead of relying on a model prompt to not leak data, the Gateway enforces a kernel-level egress allowlist. If an agent tries to phone home to an unapproved IP or port, the network layer physically drops the packet.
Alex: And finally, Model is a cluster-wide resource that holds provider settings and Kubernetes secrets for API keys, so credentials aren't scattered across task manifests.

Marcus: The sandboxing layer in Agent Substrate is also worth highlighting. AX doesn't use standard shared container runtimes. It enforces gVisor sandboxes using the gVisor-default sandbox class.
Alex: So the agent gets its own virtualized user-space kernel. Even if untrusted code or a prompt injection escapes the application, it remains trapped in the sandbox.
Marcus: And look at how they handle idle agents with suspend and resume. When an agent is waiting, AX sends SIGTERM to the container, snapshots the durable workspace directory to Google Cloud Storage, and frees the worker node entirely.
Alex: When resumed, it schedules a fresh container on any available node, restores the snapshot from GCS, and resumes without re-cloning the workspace.

Marcus: Now, let's get into the audit findings and the rough edges we discovered in the source code.
Alex: Yes, because this is an alpha release, and there are several clear implementation gaps.
Marcus: The first one is in the Redis Stream consumer group logic. In internal controller worker dot go, the worker calls XACK immediately after processing an event, even if reconciliation failed.
Alex: Which means there's no dead-letter queue and no automatic retry loop. If there's a transient network timeout to Substrate, that task event is permanently acknowledged and dropped, leaving the task stuck in Pending.
Marcus: And the stream consumer only reads new events with the greater-than symbol. It never calls XAUTOCLAIM or inspects the Pending Entries List. So if a worker pod crashes mid-reconcile, that task is stranded in the PEL forever.
Alex: There's also an interesting bug in runner dot go. If workspace preparation fails, the runner flags ready as false, but still proceeds to execute the task command anyway. The agent command launches inside an uninitialized directory.
Marcus: And finally, the task runner keeps the container alive after the child process finishes so developers can inspect output via ax ssh, but it never bubbles the command's exit code back to the control plane. In Redis, the task stays in the Running phase indefinitely.

Alex: So what's the broader strategic takeaway for engineers building agent systems?
Marcus: The takeaway is architectural layering. Don't expect your agent hypervisor to be your cognitive reasoning engine, and don't expect your reasoning engine to be a secure hypervisor.
Alex: AX handles the systems layer: scheduling, gVisor sandboxing, egress gates, persistent storage, and process supervision.
Marcus: Exactly. And higher-level frameworks—whether that's Hermes, LangGraph, or custom agent loops—can sit on top, using AX as the reliable compute substrate to safely run untrusted code at scale.
Alex: That's a clean separation of concerns. The hypervisor runs the sandbox; the agent runs the intelligence.
