---
title: 'Google Search Senior Design System UX Engineer: Live Drill Masterclass'
date: '2026-09-23'
hosts:
  Alex: 'Aoede'
  Marcus: 'Algenib'
---

Alex: Welcome back to The Staff Engineer's Playbook. Today we are breaking down a live technical rehearsal from Ilteris Kaplan as he prepares for his interview with Mindy Deli Carpini, the hiring manager for the Google Search Design System team.
Marcus: And Alex, what makes this session so compelling is that we get to see his actual live answers to the toughest prompts: the 90-second opening, the Google exit, the Wiz 3-layer architecture, and a real L6 conflict resolution story from Cloud AI.

Alex: Let's start with his refined opening pitch. Listen to his core positioning statement: We are moving from design systems built for human engineers to write against, to machine-readable systems built for autonomous models to render against.
Marcus: That is a ten out of ten hook. Because Mindy's team is tasked with figuring out machine-readable design systems for AI Overviews and dynamic rendering. In twenty seconds, Ilteris aligned his entire career with her team's roadmap.
Alex: And he backed it up with hard numbers: an 80-plus component Material 3 library for Gemini Enterprise, an early Figma MCP server extracting design tokens into compact context for LLMs, and the vibe CLI distributed via Mule to 50-plus engineers and designers.

Marcus: Then came the question everyone dreads: You spent 11 years at Google and stepped away earlier in 2026. What happened there?
Alex: Most candidates ramble or sound defensive. Ilteris nailed it in two sentences: My role in Cloud AI was eliminated following an organizational restructuring last spring. After 11 continuous years, I took a deliberate break to build independent projects, experiment deeply with model capabilities and local agent architectures, and evaluate what I wanted next.
Marcus: Completely objective, no lingering politics, and he proved he spent the interval sharpening his technical tools with local agent execution.

Alex: Turning to technical architecture, Mindy probes component state boundaries across 40-plus vertical teams like Shopping, Maps, and AI Overviews.
Marcus: His answer was built on Inversion of Control across three distinct layers. Layer one is internal interaction mechanics like keyboard focus, ARIA attributes, and dropdown toggles. The design system owns this completely so accessibility is guaranteed by default.
Alex: Layer two is the state contract: a hybrid controlled-uncontrolled pattern. Simple teams pass defaultValue and let the component manage its state, while high-stakes verticals like Shopping pass value and onChange to sync with URL query parameters and browser history.
Marcus: And layer three is composition over configuration. Instead of adding 40 boolean props, the design system exposes compound primitives and slots, allowing vertical teams to inject domain UI without modifying the shared core bundle.

Alex: And Ilteris made a crucial architectural point: in Google Search, this isn't React. It's Wiz!
Marcus: Exactly. On the SERP, layer one interaction uses jsaction for lazy event delegation. Layer two state is anchored in the URL. And layer three composition uses Soy template transclusion and native Web Component slot elements. That signals immediate platform fluency.

Alex: Next, performance under Search's strict latency budgets.
Marcus: In Search, JavaScript execution is your scarcest resource. Tokens are compiled at build time into native CSS custom properties. Dark mode and theming switch directly in the browser's C++ cascade engine with zero JavaScript recalculation.
Alex: And to protect Cumulative Layout Shift against streaming AI responses, the design system uses CSS containment—contain layout size—with pre-allocated aspect-ratio boxes. This ensures generative tokens streaming into an AI Overview never push organic search results down the screen.

Marcus: Then we had the live coding drill: building an accessible autocomplete typeahead.
Alex: He flagged the four landmines before typing a single line: request cancellation using AbortController to prevent race conditions; WAI-ARIA 1.2 combobox navigation using aria-activedescendant so physical DOM focus stays in the input; listening on mousedown with preventDefault so blur doesn't close the dropdown before a click registers; and checking event.isComposing so Japanese and Chinese IME selection doesn't accidentally submit the query.
Marcus: That IME handling is a hallmark of an engineer who has shipped software to billions of global users.

Alex: Finally, the L6 behavioral story on technical conflict.
Marcus: In Cloud AI, a partner team wanted to bypass the shared design system and hardcode streaming cards to meet an aggressive deadline.
Alex: Ilteris didn't play bureaucratic gatekeeper. He offered a two-track compromise: an immediate Inversion of Control slot to unblock their launch, followed by paired extraction where they built a decoupled StreamingCard with CSS containment and aria-live polite regions that graduated into the shared library and was reused by two other teams.

Marcus: That is the complete L6 package: clear technical vision, deep platform literacy, and pragmatic engineering leadership.
Alex: That's our breakdown for today. Good luck to Ilteris in the interview—until next time, keep building.
