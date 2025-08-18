<script lang="ts">
  import { preventDefault } from 'svelte/legacy';

  import { router } from '@inertiajs/svelte'

  const locationVisit = () => {
    router.visit('/location')
  }
</script>

<div>
  <span class="text">This is the page that demonstrates location visits</span>

  <a href={'#'} onclick={preventDefault(locationVisit)} class="example">Location visit</a>
</div>
