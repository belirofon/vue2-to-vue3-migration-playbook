# Vue 2 → Vue 3 Migration Playbook

> Practical guide for migrating Vue 2 (Options API) to Vue 3 (Composition API) with strict TypeScript.
> Based on real production experience migrating a high-load enterprise platform.

## Why migrate?

- **Type safety**: strict TypeScript catches bugs at compile time
- **Performance**: Composition API is more tree-shakeable, smaller bundle
- **Maintainability**: reusable composables instead of mixins
- **Ecosystem**: Vue 3 is the future, Vue 2 is EOL

## When NOT to migrate

- Small app with few components — rewrite cost > benefit
- Heavy reliance on Vue 2-only plugins without Vue 3 equivalents
- Team not ready for TypeScript
- App is being sunset soon



## Reflection: Do you really need to migrate?

Before migrating, ask yourself: do you actually need it?

Most applications that are not actively updated, and where you're not constantly integrating new modules, don't need migration. If the app works and nobody touches it — leave it alone. Vue 2 will keep running.

But if you're actively working on the application, shipping new features regularly, and the codebase is growing — migration becomes necessary. The Composition API and strict TypeScript pay for themselves when you have a large, evolving codebase with multiple developers.

The migration cost is real: 2-8 weeks of focused work, depending on codebase size. Make sure the ROI is there before you start.

— Nick Sannikov

## Migration plan (step by step)

### Phase 1: Preparation (1-2 weeks)

1. **Audit the codebase**
   - Count components, stores, mixins, directives, filters
   - Identify Vue 2-only dependencies (check Vue 3 compatibility)
   - Map out shared utilities and mixins

2. **Set up TypeScript**
   - Add `tsconfig.json` with strict mode
   - Install `@vue/tsconfig`, `vue-tsc`
   - Configure Vite (or Webpack) for TS

3. **Set up tooling**
   - ESLint with `@typescript-eslint`
   - Prettier
   - Husky + lint-staged (pre-commit hooks)

### Phase 2: Component migration (ongoing)

#### Pattern: Options API → Composition API

**Before (Vue 2):** see [examples/component-before.vue](./examples/component-before.vue)

**After (Vue 3 + TS):** see [examples/component-after.vue](./examples/component-after.vue)

#### Key rules

1. **Always use `<script setup>`** — recommended syntax, more concise, better TS inference.
2. **Type everything**: props, emits, refs, computed, store state.
3. **Use `ref` for primitives, `reactive` for objects** (or just `ref` for everything — simpler).
4. **Extract composables** for reusable logic (replaces mixins).

#### Props typing

```ts
// Vue 2 (runtime)
props: {
  title: { type: String, required: true },
  count: { type: Number, default: 0 }
}

// Vue 3 (script setup + TS)
defineProps<{
  title: string
  count?: number
}>()

// With defaults
withDefaults(defineProps<{
  title: string
  count?: number
}>(), {
  count: 0
})
```

#### Emits typing

```ts
// Vue 2
this.$emit('update', value)

// Vue 3
const emit = defineEmits<{
  update: [value: string]
  close: []
}>()

emit('update', 'new value')
```

### Phase 3: State management (Vuex → Pinia)

Pinia is the official Vue 3 store. Migration is straightforward:

**Before (Vuex):**
```js
export default new Vuex.Store({
  state: { user: null },
  mutations: { setUser(state, user) { state.user = user } },
  actions: { async login({ commit }, creds) { ... } },
  getters: { isLoggedIn: state => !!state.user }
})
```

**After (Pinia):**
```ts
export const useUserStore = defineStore('user', () => {
  const user = ref<User | null>(null)
  const isLoggedIn = computed(() => !!user.value)
  function setUser(newUser: User | null) { user.value = newUser }
  async function login(creds: Credentials) { ... }
  return { user, isLoggedIn, setUser, login }
})
```

#### Key differences
- No mutations — just actions
- No modules — just multiple stores
- Full TypeScript support out of the box
- DevTools support

### Phase 4: Build tooling (Webpack → Vite)

1. Install Vite + `@vitejs/plugin-vue`
2. Move `index.html` to project root
3. Update env variables (`VITE_` prefix instead of `VUE_APP_`)
4. Update path aliases in `vite.config.ts`
5. Test HMR — it should be significantly faster

### Phase 5: Performance optimization

1. **Code splitting**: lazy-load routes with `defineAsyncComponent`
2. **Table virtualization**: for large datasets, render only visible rows
3. **Bundle analysis**: use `rollup-plugin-visualizer` to find bloat
4. **Tree shaking**: ensure imports are named, not default

### Phase 6: Testing

1. Migrate from Vue Test Utils (Vue 2) to Vue Test Utils (Vue 3)
2. Update Jest config for Vite (or switch to Vitest)
3. Re-run all tests, fix breaking changes

## Common pitfalls

See [pitfalls.md](./pitfalls.md) for detailed list.

## Checklist

See [checklist.md](./checklist.md) for a PR review checklist.

## Real-world results

From my production migration experience:
- Initial page load improved by ~12% (Vite code splitting + table virtualization)
- Task cycle for standard frontend tasks reduced from ~8h to ~3h (AI-assisted workflow)
- Onboarding to unfamiliar modules: ~3 days instead of several weeks

## About me

I'm Nick, a Senior Fullstack Engineer with 6 years of experience in Vue 3 / React / TypeScript and Node.js (NestJS). I led a Vue 2→3 migration with strict TypeScript on a high-load enterprise platform.

- GitHub: [github.com/belirofon](https://github.com/belirofon)
- Open to remote B2B contracts

## License

MIT
