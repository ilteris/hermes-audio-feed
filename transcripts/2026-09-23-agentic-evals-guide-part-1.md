A: So we're looking at Yesha Shah's new piece on agentic evaluations. She's on the Agent Quality team at Google Cloud AI, and the thesis right up front is that deploying an agent is easy, but proving it still works after a prompt tweak or a model upgrade is where teams get burned.

B: That matches what everyone hits the second they leave the playground. With a standard LLM call, you test a single input and output. With an agent, you're evaluating a full trajectory across multiple inferences and tool calls.

A: And she lays out the math on compounding errors really cleanly. 

B: Right. If every individual step is ninety-five percent reliable, a ten-step workflow succeeds less than sixty percent of the time. If you only look at the final answer, you miss the silent degradation happening along the path.

A: Which leads to her metric stack. Traditional LLM evals track fulfillment, groundedness, and safety. But for agents, she adds trajectory coherence, tool usage, side effects, and efficiency.

B: Notice the emphasis on side effects. An agent doesn't just synthesize text; it issues refunds, creates records, or modifies files. Verifying that the external system actually changed as intended—and that nothing unintended happened—is fundamentally an integration test.

A: She breaks the harness into six components: the eval set, the environment, the pinned agent, execution traces, autoraters, and the aggregation layer. Where do the test cases actually come from?

B: Her hierarchy is spot on here. Real production traces and user bug reports are worth more than everything else combined. After that, you have synthetic generation for cold start, followed by public benchmarks and vendor datasets.

A: But she stresses versioning the eval set as code.

B: Absolutely. If you modify your eval dataset and your prompt in the same commit, your benchmark score is meaningless. You have to track them independently, keep hold-out sets, and stratify by traffic slices so aggregate averages don't hide critical regressions.

A: Let's talk about the autoraters. She categorizes them into deterministic rules, LLM judges, and human review.

B: And the biggest takeaway for backend engineers is to push as much as possible into deterministic code. Check argument schemas, inspect database diffs, assert that read actions preceded write actions. That costs zero model tokens, runs in milliseconds, and never drifts.

A: And when you do have to use an LLM judge, she strongly advises against asking for a one to five score.

B: Yes. Scalar scores from LLM judges are notoriously noisy and uncalibrated. Instead, use binary rubrics. Ask atomic yes or no questions, like "Did the agent check inventory before placing the order?" And crucially, require the judge to cite the exact trajectory step.

A: That way you get an actionable pointer to the defect instead of just a vague thumbs down. What about running multiple iterations?

B: Agents are non-deterministic, so a single pass is just a sample, not a measurement. She highlights the difference between pass-at-k and pass-to-the-k.

A: Wait, clarify that distinction. What does pass-to-the-k mean in practice?

B: Pass-at-k means the task succeeded at least once across k runs. That's fine for code generation where a human picks the best patch. But pass-to-the-k means the task succeeded on every single run. If an agent is executing irreversible financial transactions, pass-to-the-k is the only number that reflects user trust.

A: Now, reading this from an infrastructure perspective, what are the gaps? What did she leave out?

B: Well, she explicitly scoped out long-running and coding agents to focus on transactional workflow bots. But in practice, state accumulation is where systems break down.

A: You mean things like memory and context drift?

B: Exactly. In real systems, evaluating memory retrieval precision, context bloat, and stale cache invalidation across turns is just as critical as grading tool calls. The article treats the agent as mostly stateless between requests.

A: There's also the risk of trajectory overfitting. If the harness expects one specific sequence of tools, does it penalize an agent that discovers a more efficient path?

B: It does if you over-specify intermediate steps. What matters is invariant assertions and terminal state diffs, not enforcing an exact sequence of intermediate tool calls.

A: And what about the cost of running all these evals?

B: That's where you need cascaded evaluation. Run zero-cost deterministic assertions first. If those fail, abort early. Only send surviving trajectories to small-model binary rubrics, and save your expensive frontier judges for borderline edge cases.

A: So the bottom line: don't treat agent evaluation as an ML scoring problem. Treat it as distributed systems integration testing with structured telemetry.

B: That's the real shift. If you build your eval harness like a robust integration test suite, prompt updates stop feeling like a roll of the dice.
