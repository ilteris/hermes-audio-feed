---
title: 'Portfolio Deep Dive: 11 Years of AI Systems and UX Engineering'
date: '2026-09-20'
hosts:
  A: 'Aoede'
  B: 'Puck'
---

Host A: Welcome back. Today we have something pretty special to break down. We are looking through an extensive portfolio deck and speaker notes from Ilteris Kaplan, spanning eleven years at Google and six years building inside Cloud AI.
Host B: Yeah, and what makes this deck fascinating is the philosophy behind it. His whole thesis is that AI product teams often hit this awkward phase where a model capability is emerging, but nobody actually knows what the product should look like.
Host A: Right, and instead of debating abstractions in meetings, he builds concrete, working prototypes that force the hard engineering and design questions to the surface.
Host B: Exactly. And then he turns those hard-earned lessons into reusable platforms and developer tools for the rest of the organization. Let us walk through the entire twelve-slide narrative, starting with slide one.

Host A: Slide one sets the foundation. Eleven years at Google, starting with frontend work on Jamboard, and then six years right in Cloud AI.
Host B: Working across Gemini Enterprise, Vertex AI Studio, Vertex AI Search, and core media workflows. His tools ended up reaching more than fifty UX, product, and engineering partners.
Host A: Which brings us directly to slide two, and this is a big one: the Figma MCP Server.
Host B: Back in June 2025, coding agents like Cursor were great at reading Git repos, but they were completely blind to design files. Raw Figma JSON is massive, deeply nested, blows through token limits, and leaves out component definitions.
Host A: So he wanted to make a Figma file as addressable for an AI agent as a code repository. How did he tackle that?
Host B: He built a FastMCP server in Python with dual stdio and HTTP transports. But the real engineering wins were solving two huge bottlenecks: large Figma files exceeding fifty megabytes that trigger HTTP 413 payload errors, and external component libraries breaking references.
Host A: And he fixed that with bounded subtree querying and on-demand dependency resolution. Plus, converting vectors into compact YAML saved seventy percent of model context tokens!
Host B: And here is the kicker: he committed version one on June 1st, 2025, exactly three days before Figma announced their own official MCP server beta. It fed right into the GM3 Vue design system.

Host A: Now slide three moves into internal developer platforms: GitHub Enterprise and the vibe CLI.
Host B: In Cloud AI UX, prototyping was stuck in no-man's-land. Designers could play in AI Studio or local sandboxes, but couldn't easily branch or share code. Meanwhile, Google production tools like Gerrit and google3 took days of setup.
Host A: So he led Cloud AI UX as an early pilot for internal GitHub Enterprise, and built the vibe CLI.
Host B: The vibe CLI automated Git identity, SSH keys, scaffolding, diagnostics, and preview deploys in a single command. Setup dropped from days to minutes, over fifty people adopted it directly, and the team shipped more than fifteen hundred pull requests.
Host A: And he didn't stop at local setup. He built automated preview sandboxes on Cloud Run!
Host B: Using an L4 Internal TCP Proxy and Private Service Connect across VPCs, orchestrated by Cloud Build and secured behind Identity-Aware Proxy. Every Git push automatically created an isolated, live Cloud Run URL for reviews.
Host A: He also built Zipline, a Jetski skill that packaged prototypes into one-command public links for research studies, and an in-product visual token inspector that generated one-click GitHub pull requests right from live UI.

Host B: Slide four takes us to research infrastructure: the Agentic UXR Analysis Platform.
Host A: User researchers were sitting on huge backlogs of video interviews. Manually analyzing a single one-hour study took four hours to watch, transcribe, and pull quotes.
Host B: He used Gemini multimodal video to build an end-to-end platform in Vue 3 and FastAPI. It reduced per-study analysis time from four hours down to just five minutes. That is a forty-eight-x speedup across two UXR teams.
Host A: But scaling to real two-hour videos must have been tough on token limits and quotas.
Host B: It was brutal. Re-sending two-hour video tokens hit 429 quota exhaustion, and models suffered from Lost in the Middle context decay. So he implemented Vertex AI Context Caching with a one-hour TTL, slashing token costs by eighty percent.
Host A: And for long transcripts, he built EnvPointers in virtual memory!
Host B: Right! Instead of dumping forty thousand words into a prompt, specialist tools mounted specific transcript slices on demand, coordinated by an ADK Knowledge Supervisor with an adversarial judge audit.
Host A: Plus a Conversation Map that showed researchers the exact agent reasoning path, which turned an opaque black box into an auditable, trusted tool.

Host B: Slide five shifts to AI media with Dubbing Editability.
Host A: Google had amazing Speech-to-Speech models, but the APIs were batch black boxes. You uploaded a video, waited minutes, and if one word was mistranslated, you had to re-render the whole video from scratch.
Host B: Ilteris was the sole designer and engineer across three hundred and eleven commits. He built a three-panel studio with an HTML5 Canvas multi-track timeline, synchronized video, and a zero-wait ingestion loop.
Host A: So editors could start refining text immediately while audio processed in the background.
Host B: Exactly. Editing a word marked only that clip as stale, enabling surgical speech-to-speech regeneration of just that segment without touching the rest of the timeline.
Host A: And under the hood, he solved a gnarly infrastructure conflict between Identity-Aware Proxy and machine webhooks.
Host B: The Twin-Service pattern! The Main Service handled human browser traffic behind IAP, while a Shadow Service handled machine-to-machine OIDC webhooks from the Transcoder API. Zero security waivers, and sub-second clip regeneration instead of eight-minute delays.

Host A: Slide six is a great example of production speed: the Vertex AI Studio Model Picker.
Host B: Executive feedback showed the existing picker was too crowded. Customers had to scroll through a long list, and internal teams were stuck debating whether to show every capability or simplify.
Host A: What did he do?
Host B: He built an interactive prototype overnight with a clean two-column layout: model families on the left, versions and details on the right. It reduced choice anxiety, resolved the disagreement, and shipped to Google Cloud Console customers in about a week.

Host A: Slide seven covers Gemini Enterprise Express Mode.
Host B: In Vertex AI Agent Builder, setting up an enterprise search widget required deep Cloud Console configuration with data stores and schemas. But SMBs just wanted a conversational chat widget grounded in their website and local storefront.
Host A: So in thirty-eight commits, he built Express Mode in Vue 3 and FastAPI. An SMB entered a single URL, and the system instantly scaffolded a grounded Site Q and A Agent.
Host B: It combined domain-restricted Google Search and Google Maps location retrieval, normalized into interactive place cards and citations, with an embeddable widget snippet that unblocked multiple internal Google CLs.

Host A: Slide eight takes us to the Cloud Next '25 keynote with Conversational Commerce.
Host B: Shopping had to feel like one continuous buying journey instead of disconnected search queries. A shopper starts broad, refines their need, looks at product carousels, and updates their cart.
Host A: He led the flagship Vue 3 and TypeScript experience and the Python Cloud Functions backend.
Host B: He centralized state in a ConversationOrchestrator singleton with a Pinia history that handled polymorphic messages: suggestion chips, carousels, and cart updates. He also queried product endpoints concurrently with Promise.allSettled to eliminate latency hiccups.
Host A: And the business impact was huge. Nordstrom later reported a twenty-three percent revenue lift per search on conversational queries, and Albertsons shipped Ask AI across its apps.

Host B: Slide nine is a project we know very well: Soul CLI.
Host A: The local, provider-agnostic agent control plane.
Host B: Exactly. Agent work was getting fragmented across Codex, Claude, Gemini, Pi, and terminal tools. Context was trapped in hosted provider silos.
Host A: So he architected Soul CLI to keep durable tasks, workspaces, and append-only hooks.jsonl session ledgers on local disk, completely independent of any single AI vendor.
Host B: Six hundred and seventy-nine commits since June 2026. Tool calls become before and after records with full auditability, verified review artifacts, and a desktop interface that lets you watch bounded agent execution safely.

Host A: Rounding out the technical systems, slide ten covers Store Vision AI for Schwarz Group.
Host B: Google's retail computer vision could detect products and empty shelves, but raw bounding-box JSON was too abstract for store operators.
Host A: So he built a Framer.js dashboard with a custom CanvasLayer drawing bounding boxes from normalized coordinates, connecting GTIN data directly to product images.
Host B: It gave operators a clear path from executive KPIs down to specific aisle conditions and confidence scores.

Host A: Slide eleven is Video Search, built for Google Customer Engineers back in 2020.
Host B: Customer engineers needed a repeatable way to demonstrate the Video Intelligence API on prospective clients' footage. He built an app in Vue and Video.js with dynamic canvas bounding-box overlays, class filter chips, and precision timeline seek points.
Host A: Adopted by more than four hundred Google Customer Engineers worldwide.
Host B: And finally, slide twelve: the Discussion.
Host A: Where the presentation stops broadcasting and turns into a conversation about the specific problems the audience is solving today.
Host B: From design-engineering handoffs to agent infrastructure and multimodal video pipelines, the through-line across all twelve slides is turning emerging AI capabilities into concrete, reliable tools that ship.
Host A: An incredible eleven-year track record. Thanks for listening, and we will catch you in the next breakdown.
