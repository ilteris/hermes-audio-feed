# Two-Host Audio Overview: Contextual Policies in Omnigent — Spoken Draft

A: Today we’re looking at a Databricks post about Omnigent, and specifically something they call contextual policies.

B: The title is “Contextual Policies in Omnigent: Using session state to better govern AI agents.”

A: That sounds very governance-heavy. What is the plain version?

B: The plain version is: most agent guardrails look only at the next action. Omnigent wants policies that remember what happened earlier in the session, then use that history to decide whether the next action is okay.

A: So instead of asking, “is this tool allowed?” it asks, “is this tool allowed, given what the agent has already done?”

B: Exactly. That little phrase, “given what the agent has already done,” is the whole point.

A: Give me an example.

B: Imagine a coding agent wants to push code to GitHub. If it has just been editing local source files for an engineer, that might be totally normal.

A: Right.

B: But if the same agent just read an untrusted web page, and that page might contain prompt injection, then pushing code outward is much riskier.

A: Because now the session has crossed from untrusted input into external write access.

B: Yes. The action is the same: push to GitHub. But the safety of that action depends on the prior context.

A: That feels like the missing piece in a lot of agent systems. We tend to treat every tool call as a separate decision.

B: And that gives you very blunt controls. You either allow Git pushes, or deny Git pushes, or ask for approval every time.

A: Which gets annoying fast.

B: Exactly. If every useful action triggers a permission prompt, people stop reading the prompts. Approval fatigue becomes its own security problem.

A: So contextual policies are an attempt to be stricter only when the session actually becomes risky.

B: That’s the idea. The policy can watch the session accumulate state. What documents did the agent read? Did it touch confidential data? How many emails has it sent? How much money has it spent on model calls? What was the user’s original request?

A: Let’s talk about how Omnigent implements this. What is a policy in their model?

B: A policy listens to agent events: tool calls, tool responses, user inputs, and LLM outputs. When a new event happens, the policy receives its previous state for that session, plus the event the agent is trying to perform.

A: And then it returns what?

B: Two things. First, updates to its state. Second, a decision: allow, deny, transform, or ask the user.

A: So the policy has memory, but scoped to that session and that policy.

B: Right. It’s not a giant shared memory blob. It is policy-local session state. The Omnigent server stores it and passes it back the next time the policy runs.

A: That sounds simple, but pretty powerful.

B: It is simple in the way a lot of good infrastructure ideas are simple. You turn a stateless filter into a small state machine.

A: And Omnigent is not just an agent framework, right? They describe it as a meta-harness.

B: Yes. That means it can wrap agents you already use instead of asking you to rewrite everything. The article names Claude Code, Codex, Antigravity, Pi, OpenCode, Hermes, and custom agents using OpenAI or Claude agent SDKs.

A: So you launch the agent through Omnigent, and Omnigent intercepts the tool calls.

B: Exactly. The promise is: one policy layer across multiple agent runtimes.

A: Okay, walk me through the example policies in the post.

B: The first one is a Google Drive policy.

A: What does it do?

B: It governs what the agent can read and modify in Docs, Sheets, and Slides. By default, writes are confined to documents that the agent created during the current session.

A: So if the agent creates a new doc, it can edit that doc freely.

B: Yes. But it cannot quietly modify some existing company doc that it was never supposed to touch.

A: That would be hard to express with a normal allow-list.

B: Exactly. You don’t want to say “document writes are always allowed.” But you also don’t want to say “document writes are always denied.” You want to say, “allow writing to documents created during this session.”

A: The condition depends on history.

B: Right. The policy remembers which docs the agent created.

A: There’s also a confidential-doc version, right?

B: Yes. You can mark some documents as confidential. Once the agent opens one, the policy tightens. From that point, writes are confined to that confidential set.

A: Why?

B: To prevent the agent from copying confidential content into a less protected file. Even a doc the agent created a minute ago can become off-limits after it has read confidential material.

A: So the session’s security level changes based on what the agent has seen.

B: Exactly. The article connects this to Bell-LaPadula’s no-write-down rule: once you read high-sensitivity material, you cannot write it down into a lower-sensitivity location.

A: That’s a nice example because it shows why static permissions aren’t enough.

B: Yes. Static permissions miss the flow of information.

A: What’s the second policy?

B: Risk scoring.

A: Like fraud systems and enterprise security tools use for human users?

B: Pretty much. The policy keeps a running risk score for the session. Routine actions might add a point or two. Reading a highly confidential document might add a lot more.

A: And then what happens when the score gets high?

B: Once it crosses a threshold, actions like sending an email or sharing a file can switch from allow to ask.

A: That is better than asking constantly.

B: Right. Early in the session, the same email might be allowed automatically. Later, after the agent has handled sensitive material or done risky actions, the email requires approval.

A: So the user is interrupted only when the situation has changed.

B: Exactly. It’s a more targeted interruption.

A: What about cost? That one seems important for agents that run long.

B: The cost policy tracks how much the session has spent on model calls. After a soft threshold, it pauses and asks whether to continue. At a hard cap, it blocks further calls to an expensive model until the agent switches to a cheaper one.

A: That’s useful because cost is also cumulative. No single model call may look outrageous, but the total can get out of hand.

B: Exactly. The policy is not judging one call in isolation. It is watching the running total.

A: And the article says you can stack per-session limits with per-user daily caps.

B: Yes. Session-level and user-level budgets can work together.

A: Then there’s intent-based authorization.

B: This one is especially interesting for general-purpose agents.

A: How does it work?

B: The policy remembers the user’s initial task. Then it checks later tool calls against that purpose.

A: So if I ask the agent to update a Google Slides deck, it should be able to read and write that deck.

B: Right.

A: But if it suddenly tries to use GitHub, that probably does not match the original task.

B: Exactly. The policy can block that. The agent may technically have access to many tools, but in this session its effective privileges are narrowed to the user’s stated goal.

A: That feels like least privilege, but per session.

B: That is the point. You don’t need to maintain a tiny custom tool list for every possible task. You can give the agent a broad toolbox, then constrain each session based on intent.

A: There’s a subtle design point here. This is not just about safety. It’s also about usability.

B: Yes. The article keeps emphasizing that simple allow-lists can be secure but painful, or convenient but unsafe. Context gives you a way to be selective.

A: The policy can be permissive while things are normal, and strict when the session crosses some boundary.

B: Exactly. Read confidential data, and the write rules change. Accumulate risk, and the approval threshold changes. Spend too much, and the model policy changes. Drift away from the original intent, and tool access changes.

A: This also explains why Omnigent being a meta-harness matters.

B: Right. If every agent framework has its own policy language and its own incomplete controls, teams end up duplicating governance logic.

A: But if Omnigent can sit around multiple agents, the policy layer becomes shared infrastructure.

B: Exactly. A company can define agent governance once and apply it across Claude Code, Codex, Hermes, and other agents.

A: What should we be careful about, though? This sounds good, but not automatic magic.

B: Definitely not magic. First, the policy only sees what the harness can observe. If an agent action happens outside the intercepted path, the policy can’t govern it.

A: So the wrapper boundary matters.

B: A lot. Second, the state design matters. If the policy tracks the wrong signals, it will either block useful work or miss risky work.

A: And third, the original intent has to be represented carefully.

B: Yes. Intent-based authorization is powerful, but interpreting intent can be messy. “Help me prepare this presentation” might legitimately involve reading notes, images, prior docs, maybe even a calendar invite. The policy needs enough nuance not to become a brittle keyword filter.

A: So contextual policies are not a replacement for judgment. They are a better substrate for judgment.

B: That’s a good way to put it. They give governance a session memory.

A: The big takeaway for me is that agent safety is becoming less about one-time permissions and more about trajectories.

B: Exactly. What did the agent read? What did it write? What boundary did it cross? How much risk or cost has accumulated? Is it still doing the thing the user asked for?

A: That’s the agent control problem in a sentence.

B: And the Databricks post argues that contextual policies are a practical way to express those controls today.

A: Especially because Omnigent is open source and can wrap existing agents.

B: Yes. The post points readers to the quickstart, the GitHub repo, the policy tutorial, and the policy docs.

A: Final summary?

B: Static allow-lists are too blunt for real agent work. Contextual policies make the policy decision depend on session state: what the agent has seen, done, spent, and been asked to accomplish.

A: And the result is a system that can stay convenient during normal work, but tighten up when risk accumulates.

B: Exactly. That is the core idea.
