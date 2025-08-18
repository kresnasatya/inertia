<script lang="ts">
  import { router } from '@inertiajs/svelte'
  import type { Page } from '@inertiajs/core'

  interface PageProps {
    foo: string
    bar: string
  }

  interface Props {
    foo: string;
    bar: string;
  }

  let { foo, bar }: Props = $props();

  let errors = $state(0)
  let finished = $state(0)
  let success = $state(0)

  const bagErrors = () => {
    router.replace({
      preserveState: true,
      props: (props: Page['props']) => ({ ...props, errors: { bag: { foo: 'bar' } } }),
      errorBag: 'bag',
      onError: (err) => {
        errors = Object.keys(err).length
      },
      onFinish: () => finished++,
      onSuccess: () => success++,
    })
  }

  const defaultErrors = () => {
    router.replace({
      preserveState: true,
      props: (props: PageProps) => ({ ...props, errors: { foo: 'bar', baz: 'qux' } }),
      onError: (err) => {
        errors = Object.keys(err).length
      },
      onFinish: () => finished++,
      onSuccess: () => success++,
    })
  }

  const replace = () => {
    router.replace({
      preserveState: true,
      props: (props) => ({ ...props, foo: 'foo from client' }),
      onFinish: () => finished++,
      onSuccess: () => success++,
    })
  }

  const push = () => {
    router.push({
      url: '/client-side-visit-2',
      component: 'ClientSideVisit/Page2',
      props: { baz: 'baz from client' },
    })
  }
</script>

<div>
  <div>{foo}</div>
  <div>{bar}</div>
  <button onclick={replace}>Replace</button>
  <button onclick={push}>Push</button>
  <button onclick={defaultErrors}>Errors (default)</button>
  <button onclick={bagErrors}>Errors (bag)</button>
  <div>Errors: {errors}</div>
  <div>Finished: {finished}</div>
  <div>Success: {success}</div>
</div>
