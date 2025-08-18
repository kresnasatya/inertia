<script lang="ts">
  import { page } from '../index'
  import { onDestroy } from 'svelte'

  interface Props {
    data: string | string[];
    fallback?: import('svelte').Snippet;
    children?: import('svelte').Snippet;
  }

  let { data, fallback, children }: Props = $props();

  const keys = Array.isArray(data) ? data : [data]
  let loaded = $state(false)

  const isServer = typeof window === 'undefined'

  if (!isServer) {
    const unsubscribe = page.subscribe(({ props }) => {
      // Ensures the slot isn't loaded before the deferred props are available
      window.queueMicrotask(() => {
        loaded = keys.every((key) => typeof props[key] !== 'undefined')
      })
    })

    onDestroy(() => {
      unsubscribe()
    })
  }

  if (!fallback) {
    throw new Error('`<Deferred>` requires a `<svelte:fragment slot="fallback">` slot')
  }
</script>

{#if loaded}
  {@render children?.()}
{:else}
  {@render fallback?.()}
{/if}
