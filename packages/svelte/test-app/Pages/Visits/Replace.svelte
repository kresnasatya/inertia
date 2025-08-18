<script lang="ts">
  import { preventDefault } from 'svelte/legacy';

  import { router } from '@inertiajs/svelte'

  const replace = () => {
    router.visit('/dump/get', {
      replace: true,
    })
  }

  const replaceFalse = () => {
    router.visit('/dump/get', {
      replace: false,
    })
  }

  const replaceGet = () => {
    router.get(
      '/dump/get',
      {},
      {
        replace: true,
      },
    )
  }

  const replaceGetFalse = () => {
    router.get(
      '/dump/get',
      {},
      {
        replace: false,
      },
    )
  }
</script>

<div>
  <span class="text">This is the links page that demonstrates manual replace</span>

  <a href={'#'} onclick={preventDefault(replace)} class="replace">[State] Replace visit: true</a>
  <a href={'#'} onclick={preventDefault(replaceFalse)} class="replace-false">[State] Replace visit: false</a>
  <a href={'#'} onclick={preventDefault(replaceGet)} class="replace-get">[State] Replace GET: true</a>
  <a href={'#'} onclick={preventDefault(replaceGetFalse)} class="replace-get-false">[State] Replace GET: false</a>
</div>
