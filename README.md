# no-use-effect

A ClaudeKit skill that enforces a strict no-direct-`useEffect` rule in React and React Native codebases.

Based on [Factory's approach](https://x.com/alvinsng/status/1900587498498195) and React's official [You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect) guide.

## Install

```bash
npx skills add yosnap/no-use-effect
```

## What it does

When active, this skill prevents Claude from writing `useEffect` directly in components. Instead, it guides toward the correct primitive for each case:

| Pattern | Instead of useEffect |
|---------|---------------------|
| Derived state | Compute inline or `useMemo` |
| Data fetching | React Query / SWR / `use()` + Suspense |
| User actions | Event handlers |
| External store | `useSyncExternalStore` |
| Prop-driven reset | `key` prop |
| Mount-time sync | `useMountEffect` |
| DOM side effects | Callback refs |

## The 8 Rules

1. **Derive state, don't sync it** — Compute inline or use `useMemo`
2. **Use data-fetching libraries** — React Query, SWR, or `use()` in React 19+
3. **Event handlers, not effects** — User actions belong in handlers
4. **`useMountEffect` for mount-time sync** — Named wrapper for `useEffect(..., [])`
5. **Reset with `key`** — Use React's remount semantics instead of reset effects
6. **Never patch broken effects with refs** — Remove root cause, not symptoms
7. **`useSyncExternalStore` for external stores** — Built-in React primitive
8. **Callback refs for DOM side effects** — Fires exactly when node attaches

## The only allowed useEffect

Only inside reusable custom hooks — never directly in components:

```tsx
export function useMountEffect(effect: () => void | (() => void)) {
  useEffect(effect, []);
}
```
