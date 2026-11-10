# Vue 2 → Vue 3 Migration: PR Review Checklist

## Before merging a migrated component

### TypeScript
- [ ] All props are typed (no `any`)
- [ ] All emits are typed with `defineEmits`
- [ ] All refs have explicit types (`ref<Type>()`)
- [ ] Computed properties have return types
- [ ] No implicit `any` in function parameters

### Composition API
- [ ] Uses `<script setup>` syntax
- [ ] No `this` references
- [ ] Lifecycle hooks imported from `vue` (onMounted, etc.)
- [ ] No mixins (replaced with composables)
- [ ] Composables extracted for reusable logic

### State management
- [ ] Vuex mutations migrated to Pinia actions
- [ ] No direct state mutation (use actions)
- [ ] Store is typed (no `any` in state)

### Performance
- [ ] No unnecessary re-renders (check with Vue DevTools)
- [ ] Large lists use virtualization
- [ ] Routes are lazy-loaded
- [ ] No unused imports

### Testing
- [ ] Existing tests still pass
- [ ] New composables have unit tests
- [ ] Edge cases covered

### Code style
- [ ] No `console.log` left
- [ ] Consistent naming (composables use `use` prefix)
- [ ] No commented-out code
