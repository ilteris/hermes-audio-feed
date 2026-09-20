---
title: 'Portfolio Notes: Ilteris Kaplan (11 Years at Google)'
date: '2026-09-20'
hosts:
  Alex: 'Aoede'
  Marcus: 'Algenib'
---

Alex: Let's look through these portfolio deck notes from Ilteris Kaplan. It spans eleven years at Google, with the last six focused on Cloud AI and developer platforms.
Marcus: Yeah, what's interesting here is his focus on prototyping as an empirical tool. In AI product work, teams often spend months debating theoretical capabilities before anyone actually builds something testable.
Alex: Right. Instead of debating abstractions in meetings, he builds working prototypes that force the concrete engineering and design questions to the surface early.
Marcus: Exactly. And once those patterns stabilize, he turns them into reusable infrastructure for other teams. Let's walk through slide one, which sets the baseline across Gemini Enterprise and Vertex AI.

Alex: Slide one sets the foundation. Eleven years at Google, starting with frontend work on Jamboard, and then six years in Cloud AI building Cloud Next demos and internal platforms.
Marcus: And what stands out in the notes is that his tools reached over fifty UX, product, and engineering partners. So these weren't throwaway prototypes. They were shared foundations.
Alex: Which leads right into slide two on the Figma Model Context Protocol server. This was built three days ahead of Figma's official MCP announcement.
Marcus: Right. The core issue was that coding agents could reason over git repositories, but had no structured way to inspect design files. They'd hit 50-megabyte HTTP 413 payload limits trying to ingest raw Figma JSON.
Alex: So instead of dumping the raw design tree, he built a FastMCP server in Python with dual stdio and HTTP transports. It uses scoped node-first fetching and recursive simplification to strip raw vectors down to compact YAML.
Marcus: That cut context token usage by about seventy percent. And it resolved external component library references on demand, so an agent could actually translate design context directly into production code.

Alex: That connects directly to slide three, which covers GitHub Enterprise and the vibe CLI.
Marcus: This solved a real organizational bottleneck. At Google, building early prototypes in google3 or Gerrit often felt too heavy, but isolated local sandboxes made collaboration difficult.
Alex: So he proposed and led Cloud AI UX as an early pilot for internal GitHub Enterprise, and then wrote the vibe CLI to automate developer identity, SSH keys, project scaffolding, and preview deployments in a single command.
Marcus: That cut onboarding from days down to minutes, reaching over fifty direct adopters and fifteen hundred pull requests.
Alex: And look at the preview sandbox architecture on that slide. Every git branch push automatically provisioned a unique Cloud Run URL.
Marcus: Bridging internal GitHub Enterprise to Cloud Run with an L4 internal TCP proxy and Private Service Connect. All behind Identity-Aware Proxy for zero-trust review.
Alex: And he embedded an in-product token inspector. A designer could inspect a component, adjust a design token like corner radius, and generate a pull request directly back into the component repository.
Marcus: That completely removes the traditional screenshot redline loop.

Alex: Moving to slide four, we get into the Agentic UXR Analysis Platform. This is a substantial systems architecture.
Marcus: It addresses a massive pain point in qualitative research. Analyzing two-hour user study videos usually takes hours of manual transcription, tagging, and clipping.
Alex: He built an agentic platform that automates transcription, semantic chaptering, and pain point extraction, anchoring every single insight to verbatim timestamps in the video.
Marcus: And the speedup was dramatic. First-pass synthesis went from about four hours down to five minutes across two research teams.
Alex: What I appreciate technically is the infrastructure. He decoupled it into a split Cloud Run architecture: a stateless FastAPI web service paired with a persistent worker mesh using atomic Cloud Storage state queues.
Marcus: And to handle two-hour video files without quota exhaustion, he used Vertex AI's multimodal caching with a one-hour TTL. That cut token costs by eighty percent and prevented 429 rate limit errors.
Alex: Plus, he used EnvPointers to mount addressable transcript slices dynamically, avoiding the classic lost-in-the-middle context degradation on long transcripts.
Marcus: All coordinated by seven specialist tools under an ADK supervisor, with an adversarial judge pass auditing claim fidelity before researchers see the output.

Alex: Slide five focuses on Dubbing Editability. This explores what happens when generated media meets real human editors.
Marcus: Right. Generative speech models are impressive, but in production, an editor needs to tweak a single word without re-rendering an entire four-minute video.
Alex: He architected a three-panel studio with a Canvas timeline, synchronized video player, and dual-pane script editor. And he designed a zero-wait ingestion loop where diarization populates silent text clips immediately.
Marcus: On the backend, he solved an interesting Identity-Aware Proxy conflict using a twin-service Cloud Run model. One service handled human IAP traffic, while a shadow service handled machine webhooks and transcoding events.
Alex: And by slicing transcoded audio into discrete segment WAVs, he enabled surgical, clip-level speech-to-speech regeneration. That turned four-to-eight minute full-render delays into sub-second updates.
Marcus: Over three hundred commits as the sole designer and engineer. You can tell this was built by someone who actually sat with editors and watched where the workflow broke.

Alex: Slide six is a short, high-impact example: the Vertex AI Studio Model Picker.
Marcus: This was an overnight prototype built after executive feedback flagged that the existing model list was crowded and difficult to scan.
Alex: He organized it into a clean two-column family-and-version layout. It turned an abstract design disagreement into a concrete artifact that product and engineering could immediately evaluate and ship.
Marcus: Sometimes a twenty-four-hour working prototype resolves weeks of roadmap debate.

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

Alex: Slide eleven is Video Search, which was adopted by over four hundred Google Customer Engineers.
Marcus: Built in Vue, Vuex, Video.js, and Firebase. It allowed enterprise teams to run semantic queries across video archives.
Alex: It featured real-time canvas bounding box overlays synced to playback, with custom chips to filter tracked objects and a dual-perspective sidebar separating search matches from label classification.
Marcus: A solid, production-tested internal tool that scaled across the customer engineering org.

Alex: Finally, slide twelve wraps up with Discussion and next steps.
Marcus: And I like the framing here. Instead of continuing to broadcast slides, he stops and asks the audience where their design-to-engineering handoff is currently breaking down.
Alex: Exactly. Identifying whether the team needs rapid prototyping, agent infrastructure, or developer tooling, and diving deep into the most relevant artifact.
Marcus: Overall, this is a strong narrative. It demonstrates how concrete prototypes bridge the gap between emerging model capabilities and production software.
Alex: It shows consistent architectural rigor across eleven years. Let's wrap it here and keep an eye on how these developer patterns evolve.
