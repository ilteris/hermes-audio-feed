---
title: 'Portfolio Meeting Deck: 13-Slide Architecture & Leadership Walkthrough'
date: '2026-09-21'
hosts:
  Alex: 'Aoede'
  Marcus: 'Algenib'
---

Alex: Let's do a deep dive into the updated portfolio meeting deck from Ilteris Kaplan. It covers eleven years at Google, with six years leading Cloud AI UX engineering, and it now spans thirteen slides including brand new operating principles.
Marcus: What immediately strikes me about this deck is the philosophy. In AI product development, teams get trapped in endless theoretical arguments about what models might do. His approach is completely empirical: build a working prototype fast, surface the real engineering constraints, and once the pattern works, turn it into durable infrastructure.
Alex: Exactly. Making ambiguous capabilities concrete so teams can actually decide. And that foundational perspective is laid out right on slide one.

Alex: Slide one sets the stage. Eleven years at Google, starting with frontend and canvas work on Jamboard, and then six years in Cloud AI building flagship Cloud Next demos and developer platforms.
Marcus: And notice the metric: his shared tools reached more than fifty UX, product, and engineering partners. These weren't disposable hackathon demos. They became production systems and team standards.
Alex: And his technical stack is truly full-stack for AI: TypeScript, React, Vue, Next.js, Python, FastAPI, Cloud Run, Firestore, Vertex AI, Gemini, and the Google Agent Development Kit.

Marcus: That technical breadth leads right into slide two: the Figma Model Context Protocol server.
Alex: This is a standout architectural artifact. In June 2025, coding agents like Cursor could reason over codebases, but they were completely blind to Figma design files. If you fed them raw Figma JSON, they'd choke on fifty-megabyte payloads and blow past HTTP 413 limits.
Marcus: Right. So rather than dumping raw vector trees, he built a FastMCP server in Python with dual stdio and HTTP transports. It uses scoped node-first querying and AST reduction to strip raw vectors down to compact YAML.
Alex: That cut agent token consumption by seventy percent. And it resolved external component library references on the fly, allowing an agent to generate production-ready UI directly from design context.
Marcus: And he committed the working implementation on June 1, 2025, three days before Figma announced their own official MCP server beta. That tells you everything about his forward-deployed instinct.

Alex: That connects seamlessly to slide three: the Cloud AI UX Platform, featuring GitHub Enterprise and the vibe CLI.
Marcus: This solved a massive organizational bottleneck. At Google, building early prototypes in google3 or Gerrit carried heavy setup and governance overhead, but rogue local sandboxes left prototypes unshareable and unreviewable.
Alex: So he pushed Cloud AI UX to become an early pilot for internal GitHub Enterprise, and then built the vibe CLI. It automated developer identity, SSH keys, project scaffolding, and preview deployments in a single command.
Marcus: That cut developer onboarding from days to minutes, scaling to over fifty direct adopters and fifteen hundred pull requests across the organization.
Alex: And look at the preview sandbox architecture. Every git branch push automatically provisioned a unique Cloud Run URL behind Identity-Aware Proxy, bridged via an L4 TCP proxy and Private Service Connect.
Marcus: Plus, he built an in-product token inspector where designers could tweak a token like corner radius on a live preview and generate a pull request straight back to the component repo. That eliminated the tedious screenshot redline cycle entirely.

Alex: Moving to slide four, we get into the Agentic UXR Analysis Platform. This is serious systems engineering.
Marcus: It tackles a brutal pain point in qualitative research. Analyzing a two-hour user interview video used to take four hours of manual transcription, tagging, and clipping.
Alex: He built an agentic platform that automates transcription, semantic chaptering, and pain point extraction, anchoring every single insight to verbatim timestamps in the source video.
Marcus: And the speedup was forty-eight times: four hours down to five minutes across two research teams.
Alex: Technically, the architecture is brilliant. He split it into a stateless FastAPI web service paired with a persistent worker mesh using atomic Cloud Storage state queues.
Marcus: And to handle two-hour video files without running into quota exhaustion, he used Vertex AI multimodal context caching with a one-hour TTL. That slashed token costs by eighty percent and prevented 429 rate limit errors.
Alex: Plus, he used EnvPointers to mount addressable transcript slices dynamically, avoiding the classic lost-in-the-middle context degradation on long transcripts.
Marcus: All coordinated by seven specialist tools under an ADK supervisor, with an adversarial judge pass auditing claim fidelity before researchers see the output.

Alex: Slide five focuses on Dubbing Editability: what happens when generative speech models meet real human video editors.
Marcus: Right. Generative audio models are impressive, but in production, an editor needs to tweak a single word without waiting four minutes to re-render the entire video.
Alex: He built a three-panel studio with an HTML5 Canvas timeline, synchronized video player, and dual-pane script editor. And he designed a zero-wait ingestion loop where diarization populates silent text clips immediately while transcoding runs in the background.
Marcus: On the backend, he solved an interesting Identity-Aware Proxy conflict using a twin-service Cloud Run model: one service handled human IAP traffic, while a shadow service handled machine webhooks and transcoding events.
Alex: And by slicing transcoded audio into discrete segment WAVs, he enabled surgical, clip-level speech-to-speech regeneration. That turned multi-minute re-renders into sub-second updates.
Marcus: Over three hundred commits as the sole designer and engineer. You can tell this was built by someone who actually sat with editors and watched where the workflow broke.

Alex: Slide six is a punchy, high-impact case study: the Vertex AI Studio Model Picker.
Marcus: This was an overnight prototype built after executive feedback flagged that the existing model picker was crowded and confusing as model families grew, sparking directional debate between Product and Design.
Alex: He built an interactive prototype overnight with a clean two-column family-and-version layout. It turned an abstract design disagreement into a concrete artifact that product and engineering could immediately evaluate.
Marcus: It shifted the debate from subjective taste to empirical evidence. Product approved it immediately, and it shipped to Google Cloud Console in General Availability within one week.

Alex: Slide seven covers Gemini Enterprise Express Mode.
Marcus: The challenge here was onboarding small and medium businesses. Enterprise data-source ingestion can take days of complex configuration.
Alex: Express Mode collapsed that setup into entering a single company URL. The system crawls the site, establishes grounding with Google Search and Maps, and instantly generates an embeddable Q and A agent.
Marcus: Built with Vue 3, TypeScript, and FastAPI. It proved that conversational search could feel effortless for non-technical merchants.

Alex: That leads naturally into slide eight: Conversational Commerce for Cloud Next twenty-five.
Marcus: This was the keynote prototype that shifted shopping from static catalog filters into a continuous conversation.
Alex: A Vue and TypeScript orchestrator handled multi-turn conversation state, streaming text while concurrently firing product search queries through Python Cloud Functions.
Marcus: Rendering dynamic carousels, filter chips, and cart updates in real time. It showed how conversational search and transactional UI can live in one surface.

Alex: Slide nine covers Soul CLI, which is his personal agent infrastructure.
Marcus: This is essentially Unix for AI. It's a provider-agnostic control plane that stores agent state, task lifecycles, and session ledgers in plain filesystem files outside any single model vendor.
Alex: Right. Instead of trapping memory inside a cloud dashboard, every session appends to a local hooks ledger.
Marcus: And it bridges Codex, Claude, Gemini, and local CLI tools to the exact same registry contracts. It's a very disciplined approach to developer leverage.

Alex: Slide ten is Store Vision AI, focusing on retail telemetry.
Marcus: He built a Framer dashboard that translated edge computer vision detections into actionable store, section, and aisle workflows.
Alex: Mapping raw bounding box coordinates to GTIN product catalogs, with confidence-aware overlays for out-of-stock items and misplaced inventory.
Marcus: It gave store managers executive KPI summaries while letting floor teams drill down into specific shelf cameras.

Alex: Slide eleven is Video Search, which was adopted by over four hundred Google Customer Engineers worldwide.
Marcus: Built in Vue, Vuex, Video.js, and Firebase. It allowed enterprise teams to run semantic queries across video archives.
Alex: It featured real-time canvas bounding box overlays synced to playback, with custom chips to filter tracked objects and a dual-perspective sidebar separating search matches from label classification.
Marcus: A solid, production-tested internal tool that scaled across the customer engineering org.

Alex: Now, slide twelve is the brand-new addition: Operating Principles and Leadership.
Marcus: Yes! This is where the deck pivots from specific technical case studies to how he actually operates as a senior leader and engineer. It breaks down into four core competencies.
Alex: First is his Daily AI Workflow. He doesn't treat AI as an autonomous autopilot; he treats it as a disciplined accelerator. He uses Codex for implementation and Claude or Gemini for critique and edge cases, but retains human ownership over architecture, safety, and performance.
Marcus: And he keeps that work bounded and verifiable using the Soul CLI control plane. For example, he adapted an open-source CarPlay reference using Codex to build an Android client for voice-based rehearsal over cellular and Tailscale.
Alex: Second is the Cross-Functional Triad. He emphasizes clear role boundaries: UX evaluates user clarity, PM evaluates scope and market direction, and Engineering evaluates feasibility and maintenance. He aligns all three disciplines around running software, as shown in the Cloud AI GitHub migration.
Marcus: Third is Conflict to Artifact. When teams disagree on design taste or trade-offs, he doesn't engage in ideological debates. He builds a prototype overnight, like the Vertex AI Model Picker, shifting the conversation from opinion to testable evidence.
Alex: And fourth is Work Prioritization. His rule is to invest energy where there is an acute customer pain point and clear ownership. When the Dubbing sandbox showed its technical limits without dedicated PM ownership, he paused UI work and redirected effort to the UXR analysis platform, delivering that forty-eight times speedup for active researchers.
Marcus: That slide ties the whole career narrative together. It demonstrates that the technical depth is driven by mature leadership principles.

Alex: Finally, slide thirteen wraps up with Discussion.
Marcus: Instead of continuing to broadcast slides, he stops and invites the team to examine where their design-to-engineering handoff is currently breaking down: rapid prototyping, agent infrastructure, or developer tooling.
Alex: It's an exceptional deck: eleven years of deep technical execution, full-stack AI engineering, and proven operational leadership.
Marcus: Absolutely. Let's wrap it here and keep an eye on how these developer patterns continue to evolve.
