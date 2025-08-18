<script module lang="ts">
  export { default as layout } from '@/Layouts/WithScrollRegion.svelte'
</script>

<script lang="ts">
  import { preventDefault } from 'svelte/legacy';

  import { router } from '@inertiajs/svelte'

  interface Props {
    foo?: string;
  }

  let { foo = 'default' }: Props = $props();

  const preserve = () => {
    router.visit('/visits/preserve-scroll-page-two', {
      data: { foo: 'foo' },
      preserveScroll: true,
    })
  }

  const preserveFalse = () => {
    router.visit('/visits/preserve-scroll-page-two', {
      data: { foo: 'bar' },
    })
  }

  const preserveCallback = () => {
    router.visit('/visits/preserve-scroll-page-two', {
      data: { foo: 'baz' },
      preserveScroll: (page) => {
        console.log(JSON.stringify(page))

        return true
      },
    })
  }

  const preserveCallbackFalse = () => {
    router.visit('/visits/preserve-scroll-page-two', {
      data: { foo: 'foo' },
      preserveScroll: (page) => {
        console.log(JSON.stringify(page))

        return false
      },
    })
  }

  const preserveGet = () => {
    router.get(
      '/visits/preserve-scroll-page-two',
      {
        foo: 'bar',
      },
      {
        preserveScroll: true,
      },
    )
  }

  const preserveGetFalse = () => {
    router.get('/visits/preserve-scroll-page-two', {
      foo: 'baz',
    })
  }
</script>

<div style="height: 800px; width: 600px">
  <span class="text">
    This is the page that demonstrates scroll preservation with scroll regions when using manual visits
  </span>
  <span class="foo">Foo is now {foo}</span>

  <a href={'#'} onclick={preventDefault(preserve)} class="preserve">Preserve Scroll</a>
  <a href={'#'} onclick={preventDefault(preserveFalse)} class="reset">Reset Scroll</a>
  <a href={'#'} onclick={preventDefault(preserveCallback)} class="preserve-callback">Preserve Scroll (Callback)</a>
  <br />
  <a href={'#'} onclick={preventDefault(preserveCallbackFalse)} class="reset-callback">Reset Scroll (Callback)</a>
  <a href={'#'} onclick={preventDefault(preserveGet)} class="preserve-get">Preserve Scroll (GET)</a>
  <a href={'#'} onclick={preventDefault(preserveGetFalse)} class="reset-get">Reset Scroll (GET)</a>

  <a href="/non-inertia" class="off-site">Off-site link</a>
</div>
