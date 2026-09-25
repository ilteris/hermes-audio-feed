# Mindy General Coding: Four Progressive UX Engineering Drills

Alex: Let's work through four browser-coding exercises Ilteris is using to prepare for next week's UX engineering interview: Calendar, Icon Painter, Rainbow, and Accordion. The point isn't to memorize four finished files. It's to hear the pattern of how a simple requirement evolves, how each diff changes the design, and where an L5 or L6 signal actually comes from.

Marcus: Right. The governing rule is simple: baseline ships the behavior, L5 hardens the behavior, and L6 governs how the behavior evolves. Working code comes first. Senior judgment shows up at decision boundaries, not in a long speech before anything runs.

Alex: So during the interview, ask only clarifying questions that change the implementation. State a short plan. Build the smallest correct version. Then preserve the earlier behavior as each follow-up arrives.

Marcus: And explain blocks of intent, not every keystroke. A useful rhythm is: what state am I representing, what event changes it, and what rendering rule makes it visible? That rhythm appears in all four prompts.

Alex: Let's start with Calendar. Version one is intentionally static. The candidate needs a month with seven weekday columns, thirty dates, and an offset because day one may not begin on Sunday.

Marcus: The tight answer is: I'll create a seven-column CSS Grid, put weekday labels in the first row, then place the dates in chronological DOM order. I'll offset the first date with grid-column-start, and Grid will wrap the remaining dates into week rows.

Alex: There are alternatives. A table naturally expresses two-dimensional data. A list can work with layout rules. But CSS Grid is defensible for a prototype because the visual contract is literally seven columns. The senior move is to name the alternative once, choose, and start coding.

Marcus: In the static file, September first, twenty twenty-six is a Tuesday. A Sunday-first grid numbers its CSS columns from one, so Tuesday is column three. That fixed value is acceptable only because version one is deliberately static.

Alex: The first Calendar diff replaces those hard-coded date spans with JavaScript. This is where the exercise can feel more complicated than it is. Reduce it to two numbers and two loops: the weekday index where the month begins, and the number of dates in the month.

Marcus: First, add blank cells equal to the starting weekday index. If Tuesday has index two in JavaScript's Sunday-first system, add two placeholders for Sunday and Monday. Second, loop from one through the total days and append one date control per iteration. CSS Grid continues to do the wrapping.

Alex: The historical prompt supplies the date calculations. A standalone file includes them so it can run independently, but in the interview, clarify what starter code is already provided. Don't spend time proving you memorized calendar arithmetic if the interviewer already handed it to you.

Marcus: Still, understand the idiom. Day zero of the next month resolves to the final day of the current month, so asking for its date gives the month length. And creating the first day of the current month, then calling getDay, gives the starting weekday index.

Alex: Calendar version three adds range selection. This is not primarily a click-handler question. It's a state-policy question. Before coding, clarify four things: are both endpoints included; what happens if the second click is earlier; is the same date twice a valid one-day range; and does a click after a completed range begin a new selection?

Marcus: The chosen practice policy is: inclusive endpoints; an earlier second click becomes the new start; clicking the same date twice creates a one-day range; and any click after a completed range starts over.

Alex: Notice the precision. “A third click starts over” is not always correct. If the second click was earlier, it replaced the start and the range is still incomplete. The robust rule is state-based: a click starts over when an end already exists.

Marcus: The implementation stores start and end explicitly. A click updates that state. Then one render function derives three visual classes: in range, start, and end. That separation matters. The interaction logic decides what the selection means; the renderer decides what the DOM should look like.

Alex: That's a solid L5 signal. The code doesn't scatter class mutations across multiple branches. It defines one state contract and one rendering path.

Marcus: Calendar version four adds accessibility. Use native buttons for actionable dates. Give each one a full accessible name, such as September sixteenth, twenty twenty-six. Keep visual and DOM order aligned. Use roving tabindex so the calendar contributes one Tab stop rather than thirty.

Alex: Then Left and Right move one date, Up and Down move seven. Enter and Space already activate a native button. Focus needs a visible style distinct from the selected range. Focus says where I am; selection says what I chose.

Marcus: A polite live region can announce that a start date was selected or that a completed range runs from one full date through another. Use ARIA only where native HTML doesn't already express the behavior.

Alex: The L6 discussion comes after that working solution. What happens across month boundaries? Which locale determines the first weekday? Is this a calendar, a date picker, or a booking-range component? What timezone defines “today”? How do disabled dates and unavailable ranges work? Those are architecture-driving questions, but they should not block the baseline.

Marcus: Calendar memory hook: seven columns; offset then dates; explicit start and end; focus is not selection.

Alex: Next, Icon Painter. Version one builds a twenty-four by twenty-four grid using HTML elements instead of canvas. The plan is a twenty-four-column CSS Grid and five hundred seventy-six generated pixel elements in row-major order.

Marcus: A shared pixel class controls width, height, border, and background. Border-box means the declared sixteen-by-sixteen size includes the border, so the geometry doesn't unexpectedly grow.

Alex: Version two adds click toggling. Each pixel stores a binary data-painted value. CSS reads that attribute to render white or black. One delegated click listener lives on the grid, finds the closest pixel, validates that it belongs to the grid, converts the stored string into a Boolean, flips it, and writes the new value back as a string.

Marcus: That read-convert-flip-write sequence is worth hearing clearly. Dataset values are strings because they correspond to HTML attributes. Boolean logic is easier on a Boolean. So read the string, compare it with true, negate the Boolean, then store its string form. CSS reacts to the attribute.

Alex: Delegation avoids five hundred seventy-six separate listeners and centralizes behavior. But don't pretend it's automatically faster in every measurable way. It's simply a clean lifecycle choice for this prototype and future dynamically added pixels.

Marcus: The next diff changes click toggling into drag painting. This introduces gesture state. Track whether drawing is active, and determine the stroke's painted state once, from the first pixel.

Alex: On mouse-down, begin the drag and choose the opposite state of the first pixel. If the first pixel is white, the stroke paints black. If it is black, the stroke erases to white. On mouse-move, apply that same state to each pixel crossed. On mouse-up, stop.

Marcus: The important word is apply, not toggle. If every crossed pixel toggles independently, mixed starting colors create a checkerboard, and revisiting a pixel flips it repeatedly. Setting one stroke state is idempotent. Re-entering the same pixel produces the same result.

Alex: Listen outside the grid for mouse-up so the drawing flag doesn't remain stuck if the pointer leaves the component before release. That's a small detail with strong correctness signal.

Marcus: Version four adds Clear. A button iterates through all pixel elements and sets data-painted to false. It resets the model represented by those attributes without rebuilding the grid.

Alex: The final mobile follow-up is usually discussion-oriented. Replace mouse-specific events with Pointer Events: pointer-down, pointer-move, pointer-up, and pointer-cancel. Add touch-action none where the product truly intends dragging to paint rather than scroll. Handle cancellation and release outside the grid. And recognize that sixteen-pixel cells are visually useful but too small as independent finger targets.

Marcus: An L6 extension would separate the pixel model from the DOM renderer, define a stroke as one undoable transaction, and measure when five hundred seventy-six DOM elements become a bottleneck. But don't introduce a framework or canvas before completing the requested DOM exercise.

Alex: Icon Painter memory hook: grid; toggle; stroke state; clear; adapt input.

Marcus: Now Rainbow. Version one is static HTML and CSS. A neutral parent span represents the word, and each letter gets a colored span. The parent can carry the complete accessible name while the visual letter spans are hidden from assistive technology to reduce noisy letter-by-letter speech.

Alex: Span is semantically neutral. Em would imply emphasis; mark would imply relevance or highlighting. Color here is presentation, so span is the cleaner default.

Marcus: Version two generalizes the behavior into a function accepting any string and returning an HTML node. It preserves whitespace but doesn't consume a palette position for it. A separate color index advances only when a non-whitespace character is processed, and modulo cycles through the palette.

Alex: Use textContent rather than concatenating innerHTML. That keeps input as text instead of parsing it as markup. Use for-of or Array.from rather than naive UTF-sixteen indexing, so surrogate-pair emoji aren't split. Full grapheme-cluster correctness is a separate boundary that can use Intl Segmenter if required.

Marcus: Version three changes the color mapping: use the palette once across the complete string rather than cycling. The first eligible character must use the first color, and the last eligible character must use the final color.

Alex: The central formula maps eligible character index times palette length minus one, divided by eligible character count minus one. The minus one in the denominator is the off-by-one defense. It guarantees both endpoints.

Marcus: Then state the edge policies. Zero eligible characters preserves whitespace. One eligible character gets the first color. With fewer characters than colors, sample across the palette while preserving endpoints. With more characters than colors, neighboring characters may share palette entries.

Alex: The next Rainbow diff moves from string transformation to DOM traversal. Recursively collect eligible matching text nodes, but skip script, style, form controls, code, preformatted content, and any node already inside generated rainbow output.

Marcus: Collect first, mutate second. ChildNodes is live enough that mutating while traversing can skip nodes or reprocess generated nodes. For each matched text node, build a document fragment containing untouched text, a safely colorized node for each match, and the remaining tail. Replace the original text node once.

Alex: Version five integrates collection and replacement across the chosen root. The prototype contract should be narrow and explicit: case-insensitive occurrences contained within one eligible text node. Multiple matches in one text node are supported. A word split across sibling elements is not silently claimed as solved; that requires range mapping across DOM boundaries.

Marcus: Also mention repeat-run safety, dynamic content, preserving neighboring listeners and selection, Unicode case behavior, and the possibility of a carefully scoped Mutation Observer. But again: complete the ordinary text-node case first.

Alex: The dangerous shortcut is rewriting document-body innerHTML. It can destroy node identity, listeners, focus, selection, and semantic structure, while introducing injection risk. The safe solution separates three concerns: transform a string into nodes, discover eligible text nodes, and mutate after collection.

Marcus: Rainbow memory hook: static spans; safe generator; endpoint mapping; collect then mutate; define the boundary.

Alex: Finally, Accordion. Question one is design considerations before code. Clarify initial expanded state, independent versus exclusive behavior, persistence, mixed-state global behavior, pointer and keyboard activation, visible state, animation, reduced motion, nesting, and dynamic content.

Marcus: Then choose scope. For production, prefer a button inside each heading with aria-expanded and aria-controls, or evaluate native details and summary. For the historical prototype, preserve the supplied heading-and-list markup long enough to demonstrate the requested DOM behavior, while naming the accessibility correction.

Alex: Version two adds independent toggling. One delegated listener on the table of contents resolves the nearest heading, guards ownership, and toggles an is-collapsed class on the parent section. CSS hides that section's ordered list. Multiple sections can remain open.

Marcus: The helper structure matters: get the owned sections, set one section's collapsed state, and toggle one section by calculating its resulting state. One mutation helper becomes the seam used by later follow-ups.

Alex: Version three programmatically creates Hide All, uses safe textContent, prepends the button, and reuses the same setter to collapse every section.

Marcus: Version four synchronizes Hide All and Show All. The invariant is that the button describes the action it will perform next. It reads Show All only when every section is collapsed. In all-open or mixed states, it reads Hide All.

Alex: Derive that fact from the sections after every relevant mutation instead of maintaining a second Boolean that can drift. The global click decides whether to expand or collapse based on the derived state, then synchronizes the label. Individual heading clicks also synchronize it.

Marcus: Version five adds Shift-click. Compute the clicked section's resulting state once. If Shift is held, apply that state to all sections. Otherwise, update only the clicked section. Keep both branches in the same delegated handler so Shift-click doesn't also trigger a second ordinary toggle.

Alex: Event.shiftKey is enough. Adding global key-down and key-up listeners introduces state and lifecycle risk for no benefit.

Marcus: The strong L6 discussion is about ownership and evolution: nested accordions, dynamic sections, batch updates that avoid intermediate announcements, animation lifecycles, reduced motion, focus management, testing seams, and when a component class is justified.

Alex: Accordion memory hook: clarify state; one setter; derive the global label; Shift-click chooses one branch.

Marcus: Let's close with the cross-prompt pattern. Calendar teaches explicit selection state and keyboard focus. Icon Painter teaches gesture state and consistent operations. Rainbow teaches safe transformation boundaries. Accordion teaches synchronization invariants across local and global controls.

Alex: In every prompt, baseline means the requested happy path works. Solid L5 means normal boundaries, accessibility, idiomatic browser APIs, named helpers, and preserved behavior as follow-ups accumulate.

Marcus: Strong L6 means the candidate makes the system's evolution legible: what owns state, what invariant must remain true, what failure modes matter next, what should be measured, and what would trigger a different architecture.

Alex: But L6 is not complexity theater. Don't spend five minutes listing production concerns. Don't replace plain DOM code with React because it feels more senior. Don't turn accessibility into an ARIA vocabulary recital. Don't make performance claims without a workload.

Marcus: A useful interview sequence is: one or two high-impact clarifiers; a one-sentence plan; working baseline; one test; then the next follow-up. At each diff, say what changed and what stayed invariant.

Alex: Here's the rapid retrieval drill. Calendar?

Marcus: Seven columns. Offset then dates. Explicit start and end. Focus is not selection.

Alex: Icon Painter?

Marcus: Grid. Toggle. One stroke state. Clear. Pointer adaptation.

Alex: Rainbow?

Marcus: Safe nodes. Whitespace policy. Endpoint mapping. Collect before mutation. State the text-node boundary.

Alex: Accordion?

Marcus: Clarify the state model. One mutation helper. Derive the global label. Shift-click gets one branch.

Alex: And the leveling rule?

Marcus: Baseline ships the behavior. L5 hardens the behavior. L6 governs how the behavior evolves.

Alex: On interview day, if syntax disappears for a moment, return to the invariant. Say what state you need, what event changes it, and what the DOM must show afterward. That gives you a path back into the code.

Marcus: And when you receive a hint, don't apologize or defend the first approach. Say, “That makes sense. I'll update the model so this state is computed once,” then continue. Collaboration under correction is part of the signal.

Alex: The target isn't to sound like someone who memorized these four answers. It's to sound like someone who can take a small browser interaction, ship it, harden it, and explain how it should evolve without losing control of the state.

Marcus: Working code first. Senior judgment at the decision boundaries. That's the practice standard for next week.
