A: Today we’re unpacking the rationale behind a frontend code review challenge for MoneyLion.

B: Right. The source is a review of a small Next.js and React art-search prototype. The key is that the review is not just a list of defects. It is a demonstration of judgment: what matters, why it matters, and how to coach the engineer toward safer code.

A: So the framing matters before any individual comment.

B: Exactly. The PR is an early prototype, not a production-ready release candidate. That means the review can be direct, but it should not sound punitive. The useful posture is: this works well enough to show the product direction, and here are the places that would become expensive or risky as the app grows.

A: What is the product doing?

B: It searches the Art Institute of Chicago API, gets artwork records, and renders result cards. The code is split across a search component, a fetch helper, a results component, and a types file.

A: And the review starts with the data-fetching layer.

B: Yes, because that is the trust boundary. User input goes into a URL, a network response comes back from an external API, and the app has to decide what it can safely assume.

A: First issue: manual URL interpolation.

B: The code builds the request with a template string, directly putting the search term into the query string. That is fragile. A search with spaces, ampersands, hashes, equals signs, or something that looks like another query parameter can change the meaning of the request.

A: This is the kind of bug that looks small but signals a bigger habit.

B: Exactly. Query encoding is a solved problem. Use URLSearchParams or a URL object. It is shorter, clearer, and removes a whole category of edge cases.

A: The second fetch issue is response.ok.

B: Fetch only rejects on network-level failures. A 404, 429, or 500 still resolves. If the code immediately calls JSON and treats that body like a successful response, the UI can end up with malformed data, confusing empty states, or runtime errors.

A: So the fetch helper should normalize HTTP failures.

B: Yes. Check response.ok, throw a clear error when the API returns a failing status, then let the component handle that through its error state.

A: The third fetch issue is runtime validation.

B: The code casts the body to the expected TypeScript response shape. But TypeScript does not validate network responses at runtime. If the API returns an error payload or changes shape, the cast only gives false confidence.

A: What is the pragmatic recommendation there?

B: At minimum, check that body.data is an array before returning it. In more serious code, use a schema validator or a small explicit parser. The interview point is simple: types describe what our code expects, not what the network guarantees.

A: After fetching, the review moves into search state.

B: Right. The component initializes loading to true and search to an empty string. Then useEffect fires immediately and calls the API with an empty query.

A: That means the app fetches before the user searches.

B: Yes. Maybe default results are a valid product choice, but the UI does not say that. It creates ambiguity: are these curated defaults, popular works, or accidental results? If the intended behavior is user-initiated search, start in an idle state and fetch only after submit.

A: Then there is the stale response problem.

B: This is one of the most important correctness issues. If a user searches for Monet and then quickly searches for Picasso, the Monet request might resolve last and overwrite the Picasso results.

A: So the visible state can belong to an older query.

B: Exactly. Search UIs need ordering discipline. Use AbortController to cancel the old request, or track a request id and ignore stale responses. The newest request should own the screen.

A: There is also an error cleanup issue.

B: If a request fails, error becomes true. On a later success, the code updates data and loading state, but does not reset error. That allows contradictory UI: valid results and an old error message at the same time.

A: The review suggests replacing multiple booleans with a status enum.

B: Yes. Idle, loading, success, and error are mutually exclusive states. A single status value makes impossible combinations harder to represent.

A: Let’s talk accessibility.

B: The input has no label or accessible name, and the app manually wires Enter key behavior instead of using a form.

A: That is a baseline component correctness issue, not polish.

B: Exactly. A labeled form gives screen reader users a real input name, gives the browser standard submit behavior, and reduces custom keyboard handling. It is also easier to extend later.

A: The API type issues center on images.

B: The UI assumes every artwork has an image_id and builds a IIIF URL from it. But real API records can have image_id null.

A: Which produces URLs containing the literal string null.

B: Right. That creates broken images. The product has to choose: filter image-less works if the app is image-first, render a placeholder, or show metadata without an image. The type should say image_id is string or null, because that is the real contract.

A: There is a second type issue around identity.

B: The API returns an id, but the local type omits it. That omission leads directly to using array indexes as React keys.

A: And index keys are unstable when a list is sorted or filtered.

B: Correct. Keys represent identity, not position. If the data has a stable id, use it. It improves reconciliation today and supports future features like details, saved works, analytics, and deduplication.

A: The results component has another React invariant problem: sorting props in place.

B: Yes. Array sort mutates. If the component receives data as a prop and sorts it directly, it mutates parent-owned state. That can cause subtle bugs across renders, memoization, and reuse.

A: The fix is to copy before sorting.

B: Exactly. Use [...data].sort, and if the derivation grows expensive, wrap it in useMemo. The principle is that render-derived data should not mutate source state.

A: There is also a content filtering comment that is more product-oriented.

B: The code filters titles with a regex for nude or nudity. That is not a reliable moderation system. It is under-inclusive because many sensitive works won’t use those words in the title, and over-inclusive because it may hide historically important or benign cases.

A: So the concern is not just technical.

B: Right. It is product policy embedded as invisible render logic. If content filtering is a real requirement, define the policy explicitly, use better metadata, and test included and excluded examples.

A: The review also points out that the test suite is not meaningful yet.

B: There is a Vitest placeholder that proves the harness can run, but it does not test the application. Useful tests would cover URL construction, HTTP errors, malformed responses, form submission, error cleanup, loading and empty states, missing images, stable sorting, and the filtering policy if it remains.

A: That seems like a good interview signal too.

B: It is. You are not just saying, “add tests.” You are naming the behaviors that deserve tests because they map to the risks found in the review.

A: Dependency health comes up at the end.

B: npm audit reported multiple vulnerabilities, including critical findings in framework and test dependencies. For a prototype, this may not stop the exercise. For anything production-bound, dependency upgrades and retesting have to be part of the next pass.

A: What about styling and component organization?

B: The app uses inline styles and still carries starter CSS. That is acceptable for a spike, but the next structure should separate concerns: search form, artwork results, artwork card, API access, and API types.

A: So the point is not abstraction for its own sake.

B: Exactly. The point is to separate API access, form state, result derivation, and display. That makes future changes cheaper.

A: If Ilteris has to explain this review live, what is the priority order?

B: Start with data-fetching correctness: URL encoding, response.ok, and response validation. Then async state correctness: stale requests and error cleanup. Then API type accuracy: nullable image_id and stable id. Then React rendering correctness: no prop mutation and no index keys.

A: And after that?

B: Accessibility and UX states, then product-policy risk around title-based filtering, then meaningful tests and dependency health.

A: That order helps avoid sounding like every issue has the same weight.

B: Exactly. Strong reviews have prioritization. The most important issues are the ones that affect correctness, user trust, and future maintainability.

A: What is the concise version he can say in conversation?

B: “I treated the PR as an early prototype rather than a late-stage merge. The biggest concerns are the external API trust boundary, async search correctness, and React rendering invariants. I also called out accessibility, product-policy ambiguity, and the lack of meaningful tests. My goal was to give comments a junior engineer could act on, not just point out defects.”

A: That also explains the tone of the review.

B: Yes. The comments are detailed because they are meant to teach. They connect the code smell to the user impact, then to a concrete improvement.

A: One more important note: the review used AI assistance.

B: Right, and the defensible position is ownership. If AI helped with the workflow, the final comments still have to represent Ilteris’s judgment. He should be able to explain the tradeoffs, the priority order, and the implementation direction behind each comment.

A: So the review is not just about finding bugs. It is about showing how he thinks.

B: Exactly. It shows that he can read a prototype, identify the load-bearing risks, coach an engineer without grandstanding, and translate technical details into product consequences.

A: Final takeaway?

B: If asked live, anchor on three themes: trust the network less, make async UI state explicit, and keep React data flow immutable and identifiable. Everything else supports those themes.
