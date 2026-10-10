---
name: react-performance
description: Diagnose and improve React rendering and perceived performance. Use for slow interactions, unnecessary renders or remounts, ineffective memoization, Context updates, stale closures, debounce/throttle bugs, layout flicker, request waterfalls, and asynchronous races. Also use when reviewing these behaviors or designing their state and component boundaries.
---

# React-Performance

Make React interfaces respond faster while preserving correct state, data, and interaction behavior. Trace the cause of work before choosing an optimization. Prefer clear state ownership and composition when they remove unrelated work; use memoization where an identifiable consumer benefits from stable identity.

## Investigation workflow

1. **Inspect the relevant environment.** Read the component, its callers, custom hooks, providers, and data-loading boundary. Establish the installed React/framework versions, rendering mode, and whether React Compiler actually processes this code. Use the project's existing state, fetching, and tooling conventions.
2. **Define the observable problem.** Identify the triggering action and desired outcome: typing latency, dialog opening, scroll smoothness, stable layout, or time until useful content appears. Reproduce with representative data. Use React profiling for component work, browser performance tools for scripting/layout/paint, and network timing for request initiation and dependencies. Console render counts are clues, not a performance metric.
3. **Build a causal explanation.** Locate the update or request source, the component owning it, the work it triggers, and any identity or dependency that prevents isolation. Distinguish a render from a remount, DOM mutation, paint, and asynchronous completion. A useful hypothesis predicts which work should disappear after a change.
4. **Make the smallest justified change.** Choose the matching topic below. Localize interaction state, preserve intended component identity, remove accidental request dependencies, or stabilize a measured boundary. Retain necessary reactive dependencies and visible behavior. Broader refactoring or a new library needs a concrete benefit to the requested problem.
5. **Verify the hypothesis and correctness.** Repeat the same interaction and compare the relevant timing or profile. Exercise the affected state transitions and cleanup paths. Run existing relevant checks. Account for development Strict Mode and profiling overhead; use production behavior or the appropriate profiling build when comparing performance.
6. **Report evidence.** Explain the cause, change, observable result, and remaining limitation. When runtime measurement is unavailable, distinguish a mechanism-based expectation from a measured improvement. Keep an optimization only when its benefit justifies its complexity.

## Route by symptom

| Observed behavior | Inspect first | Likely intervention |
| --- | --- | --- |
| Opening a local dialog re-renders the page | State owner and hidden hook state | Move state and the feature into a small branch |
| Scroll or pointer state repeatedly renders expensive content | Where the content's JSX is created | Stateful wrapper receiving independent elements as props or children |
| Memoized component still renders on parent updates | Every prop, including children and forwarded values | Simplify props or stabilize the identity that defeats the boundary |
| Input loses focus, state resets, or effects restart | Component type, parent position, and keys | Preserve identity and remove unintended remounts |
| Many consumers render for an unrelated setting | Provider value and consumer subscriptions | Split independent Context values or isolate a selected value behind memo |
| Delayed work reads old data or sends too many requests | Closure dependencies and timer lifetime | Correct captured data and retain one timer mechanism per intended lifetime |
| UI flashes before its position or size is corrected | Measurement and paint sequence | Narrow pre-paint layout correction with an SSR-compatible fallback |
| Fast-rendering page still takes too long to become useful | Request start times and loading gates | Start independent critical requests concurrently; choose reveal boundaries |
| Old results overwrite new navigation/search results | Operation identity and cleanup | Guard obsolete work and cancel supported operations |

## Rendering, state ownership, and composition

- A component is executable code; an element such as `<Chart />` describes output. Creating the element does not call `Chart`, mount it, or run its effects. Expressions used to construct its props still execute immediately.
- Rendering computes output. Reconciliation determines reuse or replacement; commit applies changes; the browser subsequently lays out and paints. A component can render without any DOM change. Remounting destroys and recreates state and effect lifetimes.
- A state update schedules work for its owner. In ordinary parent-driven rendering, freshly produced descendant elements can cause their components to render even with unchanged or absent props. A child's local state update does not by itself re-render its ancestors or siblings.
- Trace Context, external subscriptions, root updates, and independently scheduled descendant work alongside local state updates.
- Custom hooks provide logic reuse, not an isolated render boundary. State inside any nested hook belongs to the component invoking the outer hook. Even state that is neither returned nor read can update that caller.

**Move state down** when a feature's state and consumers fit inside a smaller component. Move the relevant hook invocation too. For example, a dialog trigger and dialog can own their open state beside expensive page content, rather than having the page own that state.

**Wrap state around independent content** when a container must own frequent updates. Let its caller construct unrelated content and pass it as an element prop or `children`. The wrapper's local updates then reuse those supplied element objects.

```jsx
import { useState } from 'react';

function Expandable({ children }) {
  const [expanded, setExpanded] = useState(true);
  return (
    <section>
      <button
        type="button"
        aria-expanded={expanded}
        onClick={() => setExpanded(value => !value)}
      >
        Toggle details
      </button>
      <div hidden={!expanded}>{children}</div>
    </section>
  );
}

// ExpensiveDetails is created by this caller, outside Expandable's updates.
function Screen() {
  return <Expandable><ExpensiveDetails /></Expandable>;
}
```

This isolates parent-driven work from the wrapper's updates. The caller can still recreate the supplied element on its own updates; the supplied component and descendants can still render for their own state or Context changes. Passing children does not move them outside the React tree.

Verify that the original interaction still updates the necessary UI and that expensive unrelated branches no longer contribute comparable work to that interaction.

## Element slots, render props, and higher-order components

Choose the API by who owns configuration and changing data:

| Need | Suitable pattern | Performance consequence |
| --- | --- | --- |
| Consumer configures content; container places it | Element slot, such as `icon={<Icon />}` | Can preserve element identity across container-local updates |
| Content needs container-owned state or mapped parameters | Render callback, such as `renderIcon({ size, hovered })` | Calling it can create new output on each container render |
| Caller should own reusable stateful logic | Custom hook | Updates still belong to the caller |
| Existing wrapper adds cross-cutting behavior or owns a DOM interaction | Component wrapper or established HOC | Adds a boundary whose state, identity, and subscriptions need inspection |

Creating a footer or route element before a conditional does not mount it until the returned tree includes it. Do not confuse this with deferring expensive prop calculations or explicit calls to functions that start requests.

Use render props when explicit data flow is clearer than rewriting supplied elements. If existing code uses `cloneElement` for defaults, define override precedence and preserve intended consumer props and callbacks. Account for null or non-element input when the API permits it. Hidden overrides are fragile; avoid expanding them into a proxy for every child option.

Render-prop children are functions, not the stable element children used in the composition pattern above. Stable callback identity does not imply stable returned JSX. Render components through JSX rather than invoking component functions as ordinary callbacks.

A HOC accepts a component and returns a wrapping component. Keep component definitions and HOC applications at a stable scope; constructing a new wrapper type during render can remount the subtree. When enhancing callbacks or intercepting keyboard/DOM events, preserve callback arguments and intended event semantics. Prefer existing wrappers and hooks that fit the project over adding layers solely for nominal reuse.

## Reconciliation, keys, and state lifetime

Investigate identity before treating repeated mounting as slow rendering. React associates state with a component's position and type in the tree; a key supplies identity among siblings. A key is local to its parent, not a global identifier.

- Defining a component inside another component's render creates a new function type on each invocation. Extract the component to stable scope and pass changing values as props. Check HOC and memo wrapper creation for the same problem.
- Two conditional branches rendering the same component type at the same unkeyed position can preserve state even when their props represent different entities. Decide whether that preservation is intended.
- Use stable data identity for lists that insert, remove, filter, or reorder. Index keys can associate state with the wrong entity and force changed props through memoized rows. Index identity is suitable only when positions themselves reliably identify the items throughout their lifetime.
- Random keys or keys that change on every render force replacement. They do not optimize rendering. Stable keys enable correct reuse; they do not independently prevent renders.
- Change a key deliberately when the entire subtree should reset, such as changing the record edited by an isolated form. Account for lost input, focus, selection, effects, and repeated requests.
- A stable key can preserve an item moving among siblings of the same parent. It cannot preserve state across arbitrary changes of parent or component type.
- Inspect the actual returned tree. Fixed conditional slots, null positions, and a dynamic array nested beside a fixed sibling differ from flattening every item into one changing sibling list. Do not infer remounting from source-code proximity alone.

Verify entity-specific state by editing one row, then reordering, inserting, and removing rows. For intentional resets, confirm exactly which state and effects restart. Use [React's state preservation guide](https://react.dev/learn/preserving-and-resetting-state) when identity behavior is uncertain.

## Memoization and identity-sensitive consumers

Before stabilizing a value, identify who observes its identity: a memoized component, a Hook dependency, or another subscription/cache API. Stabilizing an ordinary event-handler prop on a non-memoized component does not by itself prevent that component from rendering.

- `useMemo` caches a calculation result; `useCallback` caches a function reference. Dependencies use `Object.is`. New objects, arrays, functions, and JSX element objects normally have different identities.
- The inline function expression passed to a Hook is still created during rendering. `useCallback(makeHandler(), deps)` also executes `makeHandler()` eagerly. Caching a callback does not avoid this work.
- `memo` can skip parent-driven rendering when its props compare equal. Its default comparison checks each prop with `Object.is`; its own state and consumed Context can still update it.
- Audit the whole boundary: `children`, inline objects, arrays, render callbacks, spreads, forwarded props, and values returned by hooks. One always-new prop can defeat the desired bailout. Primitive values often need no memoization.
- Wrapping `Child` in `memo` does not stabilize the `<Child />` object passed as `Parent`'s children. Memoizing child behavior, child JSX identity, and the receiving parent are distinct interventions.
- Prefer smaller props or derived primitives when that makes the real dependency clearer. Use correct dependencies for values that need caching; preserve identity at the producer when necessary instead of assuming a forwarded value is stable.
- Use `useMemo` for a pure calculation whose measured repeated cost and dependency reuse justify it. It still performs the initial calculation and adds bookkeeping. Measure the calculation separately from rendering its result.
- A custom comparator must preserve output and behavior, including callbacks that close over state. Ignoring a changed function can retain a stale handler. Measure comparison cost against the render it replaces; broad deep comparisons can cost more than rendering.

Memoization is an optimization, not a correctness or permanent-storage guarantee. Keep dependencies complete; program behavior must remain correct if a cached value is recomputed. Favor composition when it cleanly removes unrelated work, without requiring every alternative to fail before using a useful memo boundary.

[React Compiler can provide automatic memoization](https://react.dev/reference/react/memo#do-i-still-need-reactmemo-if-i-use-react-compiler). Check whether it is enabled and covers the affected code before adding manual caches. Its presence does not replace measurement, correct dependencies, or deliberate state ownership; avoid sweeping memo additions or removals without evidence.

## Context update boundaries

Separate two causes: the provider's component re-rendering normally and its Context value changing. Context can deliver data to distant consumers without changing intermediary props, but ordinary parent-driven work still depends on composition and memo boundaries.

When a provider supplies a different value by `Object.is`, components consuming that Context can update even if they read only one field. `memo` cannot block their own Context update. Destructuring a field from `useContext` does not create a field-level subscription.

1. Localize provider state and let independent content arrive through stable children where useful.
2. Stabilize allocated provider values and actions when this removes broadcasts caused by otherwise unrelated renders. Include all real dependencies; memoizing a value cannot hide meaningful state changes.
3. Split values with independent consumers or update frequencies. Separate state/read Context from action/dispatch Context so action-only consumers need not receive state changes.
4. Use `useReducer` when cohesive transitions make the model clearer; its dispatch is stable. A stable toggle can also use `setOpen(previous => !previous)` without introducing a reducer.
5. If a shared Context must remain broad, make a small reader select the needed value and pass it to a memoized expensive child. The reader still renders on Context changes; the child can skip renders when its selected value and all other props remain equal. Keep the selected prop stable when its meaning is unchanged.

Native `useContext` does not provide arbitrary selector subscriptions. Use a project's existing selector-capable store when subscription granularity is the actual issue, rather than treating Context as universally slow or replacing it solely because the app is large. Check [current Context semantics](https://react.dev/reference/react/useContext) before using library-specific selector behavior.

## Refs, imperative APIs, and closures

Use state for data whose change must update rendered output. Use a ref for a mutable value that must persist without scheduling a render: a DOM handle, timer ID, operation token, or intentionally current callback. Ref mutation is synchronous; it does not notify React or automatically update visible UI.

Read DOM refs after commit and handle null values after removal. Prefer a narrow `useImperativeHandle` API when exposing focus, scrolling, or another necessary imperative action; include reactive dependencies used by that API. Directly exposing broad component internals makes later changes harder.

React 18 function components require `forwardRef` to receive React's special `ref` attribute; React 19 supports [receiving ref as a prop](https://react.dev/reference/react/forwardRef). An explicitly named ordinary prop such as `inputRef` is another compatible API. Follow the installed version and existing public component API.

Closures retain their lexical bindings; they do not deep-freeze JavaScript objects. Each React render has its own props and state bindings. A callback retained from an earlier render may therefore observe those earlier values.

Choose freshness deliberately:

- Recreate a callback with complete dependencies when changed values should change its behavior or subscription.
- Use functional state updates when the next state depends only on the previous state.
- Pass event-time values as callback arguments when delayed work should use the initiating input.
- When a long-lived callback intentionally needs current committed values, dereference an appropriately synchronized ref at execution time. Retaining a function in a ref without updating it can retain the old closure too.

Synchronize refs in effects or event handlers according to the required timing, rather than repeatedly mutating them during render. A passive-effect synchronization has a window before that effect runs; it is not a promise of immediate freshness in every scheduling context. Keep render pure, apart from permitted predictable ref initialization. See [useRef's constraints](https://react.dev/reference/react/useRef).

Where the installed version supports [useEffectEvent](https://react.dev/reference/react/useEffectEvent), consider it for non-reactive callbacks belonging to an Effect. It is not a general replacement for UI handlers, stable callback props, or missing reactive dependencies.

## Debounce and throttle without stale data

Debounce postpones work until a quiet period; throttle limits execution rate while calls continue. Select quiet-period search versus periodic progress/autosave behavior intentionally, including leading/trailing behavior. Keep controlled-input state updates immediate and delay only the work that can wait.

Preserve the timer mechanism across ordinary renders while keeping its eventual callback's data correct:

- Constructing a debouncer or throttler on every render creates independent timers. Wrapping that construction around a callback whose dependencies change each keystroke can recreate the same bug.
- A wrapper created once around a closure can retain old state. Updating a callback ref that the wrapper dereferences preserves the wrapper while refreshing behavior.
- Use event-time arguments where sufficient; a ref to the current callback is useful when deliberately reading later committed state. Extract needed event values before asynchronous work, and forward all intended arguments.
- Scope instances to the intended consumer. A module-global debouncer can incorrectly share pending work between component instances.
- Cancel pending timers on unmount and when delay/options change. Decide explicitly whether a final pending action should instead flush. Recreating or canceling a throttle on every state change can defeat periodic progress.
- Cleanup must tolerate development setup/cleanup repetition. Timer cancellation and stale network-response prevention are separate responsibilities.

Use an existing utility when available and inspect its cancellation and leading/trailing API. A reusable hook needs argument handling, configuration changes, and cleanup. Verify rapid typing, sustained calls, two simultaneous consumers, unmount, and the exact value delivered to delayed work.

## Layout measurements, effects, and server rendering

For visible flicker, establish whether an intermediate DOM state paints before measurement-driven correction. `useLayoutEffect` runs after DOM commit and can block paint while layout work and its synchronous state updates finish. Use it narrowly when the user must never see the intermediate layout. Ordinary `useEffect` does not guarantee that it always runs after paint; [effect timing depends on the update](https://react.dev/reference/react/useEffect).

Prefer CSS when it can express the layout adequately. For JavaScript measurement, batch required geometry reads, reserve space for overflow controls, and recalculate for relevant container/content changes. Clean up resize observers or listeners. Keep synchronous work small; blocking paint can itself make the interaction slower.

Neither effect runs during server rendering, where layout geometry is unavailable. If a measured feature requires a client-only reveal, use a framework-supported boundary or matching server/initial-client fallback, then reveal it after hydration. A client-component designation alone may still permit server prerendering. Check [layout-effect SSR guidance](https://react.dev/reference/react/useLayoutEffect); scope the solution to the affected feature.

Verify under slower CPU conditions, narrow widths, resize, and hydration when applicable. At 60 Hz a frame interval is approximately 16.7 ms, and application JavaScript shares that interval with other browser work; use the actual device and trace rather than a universal timing threshold.

## Portals and two different hierarchies

Diagnose containing blocks, overflow clipping, and stacking contexts separately. Absolute positioning uses its containing block, often the nearest positioned ancestor. Fixed positioning commonly uses the viewport but can be affected by transformed ancestors. Increasing a descendant's z-index cannot escape its ancestor's stacking context. Positioning bugs can masquerade as delayed or missing UI.

Use `createPortal(content, domNode)` when an overlay needs a different DOM placement. Keep local dialog state local when possible; portalling does not require lifting that state to the app root and is not itself a render bailout.

- **React hierarchy:** Context, component ownership, lifecycle, and synthetic event propagation follow the React tree.
- **DOM hierarchy:** Native bubbling, descendant CSS, inherited styles, containment queries, geometry, and native form association follow actual DOM placement.

A submit button portalled outside its form needs appropriate native form association or a form inside the overlay. React's synthetic `onSubmit` does not invent that association. Confirm the host exists, handle server/client DOM availability, and keep its identity stable. Preserve keyboard access, focus handling, dismissal, and appropriate overlay semantics. Use [the portal reference](https://react.dev/reference/react-dom/createPortal) for API-specific behavior.

## Request orchestration and perceived performance

Choose what must become useful first. Time to important content, full-page readiness, responsiveness during loading, and disruptive layout shifts are different outcomes. Faster component rendering cannot compensate for a long critical network chain.

Draw request dependencies from actual initiation points, including mount/effect gates. An element created before a loading conditional does not mount its component or start its effects. A child mounted only after its parent's data arrives creates a waterfall even if the child's request needs none of that data.

- Start independent critical requests at a common route, loader, provider, or other appropriate boundary. Preserve genuine dependencies on previous results.
- Starting requests concurrently and awaiting them is different from sequentially starting and awaiting each request. `Promise.all` suits a coordinated result but waits for the slowest member and rejects when a member rejects.
- For independent regions, start concurrently and resolve separately when progressive reveal improves the experience. Design their loading/error boundaries and data ownership so unrelated regions need not render for every result.
- Providers can separate request initiation from distant consumption, but their Context values still need the update-boundary analysis above.
- Scope prefetching to likely, useful resources. Inspect contention, protocol, caching, and deduplication; historical per-origin HTTP/1 connection limits are not universal browser limits. Unconditional module-time requests can compete with critical work and run for features never visited.
- Use the framework or existing fetching layer for cache lifetime, deduplication, retries, and server/client integration when available. A library does not automatically remove a poorly designed dependency graph.

Suspense coordinates reveal for [supported suspending sources](https://react.dev/reference/react/Suspense); adding a boundary does not turn ordinary effect-based fetching into Suspense fetching or make dependent requests independent. Verify installed framework/data-layer support before choosing an integration.

For manual fetches, check HTTP status, model loading/success/error explicitly, and cache parsed data deliberately if sharing it. Falsy valid data is not a loading signal. Effects register synchronous setup and return cleanup; put asynchronous work inside them, not in an `async` effect callback.

Validate request start times and useful-content timing with representative latency. Confirm the reveal strategy and failure behavior, not merely the number of requests launched in parallel.

## Asynchronous races and obsolete results

A reused component can launch several operations whose completion order differs from initiation order. An old closure still has a setter targeting that same component; changing to `async`/`await` does not solve this.

Choose protection according to the intended state lifetime:

| Strategy | Appropriate use | Limitation |
| --- | --- | --- |
| Intentional keyed remount | The entity change should reset the entire subtree | Loses state/focus, restarts effects, and does not cancel old work |
| Compare entity or result identity | Result is valid whenever that identity matches | An old A result can pass after A-B-A navigation; this is not latest-request identity |
| Effect-local active flag or operation generation | Preserve the component; only current work may commit | Guard every success, error, and loading update, including post-fetch work |
| AbortController | Stop a supported request when obsolete | Does not undo server mutations or automatically cancel all subsequent work |

For effect-based fetching, an active guard and abort can work together:

```javascript
useEffect(() => {
  const controller = new AbortController();
  let active = true;
  setResult({ url, status: 'loading', data: null, error: null });

  async function load() {
    try {
      const response = await fetch(url, { signal: controller.signal });
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      const data = await response.json();
      if (active) setResult({ url, status: 'success', data, error: null });
    } catch (error) {
      if (!active || controller.signal.aborted) return;
      setResult({ url, status: 'error', data: null, error });
    }
  }

  void load();
  return () => {
    active = false;
    controller.abort();
  };
}, [url]);
```

This fragment assumes the enclosing component owns `url`, result state, and its setter. Associate displayed results with the requested identity too: before the new effect runs, a result whose stored `url` differs from the current `url` is not current content. Deliberately choose whether to retain old data as a labeled stale view or show a loading state.

Effect cleanup invalidates the previous run before a new setup with changed dependencies and on unmount. Check validity immediately before committing after all asynchronous transformations; an obsolete `finally` must not clear the current operation's loading state. For event-triggered work, use equivalent operation identity and unmount cleanup without relying on effect dependencies.

Verify out-of-order completion, rapid navigation/search, repeated identity such as A-B-A, unmount, rejection, and cancellation. Ignored setters from an unmounted component do not imply that its network, timer, or other resources were canceled.

## Error isolation and recoverable UI

Treat failures as part of useful-content and interaction behavior. Position boundaries around regions that can recover independently, preserving usable surrounding UI and offering a meaningful reset or retry.

`try/catch` catches work executing inside it, including a promise awaited inside that scope. Wrapping `return <Child />` or effect registration does not catch later child rendering or effect execution. Catch inside event/async work or use the appropriate boundary.

Error boundaries handle descendant rendering and React-managed lifecycle failures. They do not generally catch their own failures, server-rendering errors, arbitrary event callbacks, timers, or rejected promises. Modern transition APIs have version-specific exceptions; check [current boundary behavior](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary) when those APIs are involved.

For expected request errors, local error UI may be the appropriate recovery. When an async failure should use a shared boundary, catch it and forward it with the project's established boundary API, or store the error and throw it on a subsequent render below the boundary. Catch returned promise rejections as well as synchronous callback throws. Keep render and state updaters pure; avoid repeatedly setting state during render or hiding errors behind unconditional retries.

Distinguish intentional cancellation from failure. A boundary reset must address the condition that failed or offer a deliberate retry; resetting the same broken inputs indefinitely is not recovery. Verify fallback placement, retained surrounding state, rejection handling, and successful recovery.
