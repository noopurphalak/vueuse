---
category: State
related: createSharedComposable
---

# createGlobalState

Keep states in the global scope to be reusable across Vue instances.

## Usage

### Without Persistence (Store in Memory)

<div class="ts-api">

```typescript
// store.ts
import { ref } from 'vue'
import { createGlobalState } from '@vueuse/core'

const storageFn = () => {
  const count = ref(0)
  return { count }
}

export const useGlobalState = createGlobalState<typeof storageFn>(storageFn)

```

</div>

A bigger example:

```js
// store.js
import { computed, ref } from 'vue'
import { createGlobalState } from '@vueuse/core'

export const useGlobalState = createGlobalState(
  () => {
    // state
    const count = ref(0)

    // getters
    const doubleCount = computed(() => count.value * 2)

    // actions
    function increment() {
      count.value++
    }

    return { count, doubleCount, increment }
  }
)
```

### With Persistence

Store in `localStorage` with `useStorage()`:

```js
// store.js
import { createGlobalState, useStorage } from '@vueuse/core'

export const useGlobalState = createGlobalState(
  () => useStorage('vueuse-local-storage', 'initialValue'),
)
```

```js
// component.js
import { useGlobalState } from './store'

export default defineComponent({
  setup() {
    const state = useGlobalState()
    return { state }
  },
})
```
