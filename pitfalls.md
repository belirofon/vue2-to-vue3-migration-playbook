# Common Pitfalls in Vue 2 → Vue 3 Migration

## 1. `this` is undefined in Composition API
**Problem:** Code that relies on `this` (e.g., `this.$emit`, `this.$refs`) breaks.
**Fix:** Use `defineEmits`, `ref()` for template refs, and composables for shared state.

## 2. Reactivity loss with destructuring
**Problem:** Destructuring a reactive object loses reactivity.
```ts
const state = reactive({ count: 0 })
const { count } = state // count is not reactive!
```
**Fix:** Use `toRefs()` or use `ref` instead of `reactive`.

## 3. Watchers need explicit source
**Problem:** `watch(() => state.count, ...)` works, but `watch(state.count, ...)` doesn't.
**Fix:** Always pass a getter function or ref to `watch`.

## 4. Filters are removed in Vue 3
**Problem:** `{{ price | currency }}` no longer works.
**Fix:** Replace with computed properties or helper functions.

## 5. `$listeners` is removed
**Problem:** `v-on="$listeners"` no longer works.
**Fix:** In Vue 3, `$attrs` includes event listeners. Use `v-bind="$attrs"`.

## 6. Global properties changed
**Problem:** `Vue.prototype.$http = axios` no longer works.
**Fix:** Use `app.config.globalProperties.$http = axios`.

## 7. Plugin installation changed
**Problem:** `Vue.use(plugin)` no longer works.
**Fix:** Use `app.use(plugin)`.

## 8. Custom directives changed
**Problem:** Directive hooks renamed (`bind` → `beforeMount`, `inserted` → `mounted`, etc.).
**Fix:** Update hook names per Vue 3 docs.

## 9. `v-model` changes
**Problem:** `v-model` default prop changed from `value` to `modelValue`, event from `input` to `update:modelValue`.
**Fix:** Update component definitions or use `v-model:propName`.

## 10. TypeScript inference in templates
**Problem:** Template type checking requires `vue-tsc` and proper setup.
**Fix:** Install `vue-tsc` and `@vue/tsconfig`, configure `tsconfig.json`.

## 11. Pinia store initialization
**Problem:** Stores must be created after `app.use(pinia)`.
**Fix:** Initialize stores inside components or after app mount.

## 12. SSR / hydration mismatches
**Problem:** Server-rendered HTML doesn't match client-rendered HTML.
**Fix:** Ensure consistent data fetching and rendering logic.
