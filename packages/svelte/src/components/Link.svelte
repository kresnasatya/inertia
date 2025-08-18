<script lang="ts">
  import { createBubbler } from 'svelte/legacy';

  const bubble = createBubbler();
  import type {
    CacheForOption,
    FormDataConvertible,
    LinkPrefetchOption,
    Method,
    PreserveStateOption,
    UrlMethodPair,
  } from '@inertiajs/core'
  import { inertia } from '../index'

  interface Props {
    href?: string | UrlMethodPair;
    as?: keyof HTMLElementTagNameMap;
    data?: Record<string, FormDataConvertible>;
    method?: Method;
    replace?: boolean;
    preserveScroll?: PreserveStateOption;
    preserveState?: PreserveStateOption | null;
    only?: string[];
    except?: string[];
    headers?: Record<string, string>;
    queryStringArrayFormat?: 'brackets' | 'indices';
    async?: boolean;
    prefetch?: boolean | LinkPrefetchOption | LinkPrefetchOption[];
    cacheFor?: CacheForOption | CacheForOption[];
    cacheTags?: string | string[];
    children?: import('svelte').Snippet;
    [key: string]: any
  }

  let {
    href = '',
    as = 'a',
    data = {},
    method = 'get',
    replace = false,
    preserveScroll = false,
    preserveState = null,
    only = [],
    except = [],
    headers = {},
    queryStringArrayFormat = 'brackets',
    async = false,
    prefetch = false,
    cacheFor = 0,
    cacheTags = [],
    children,
    ...rest
  }: Props = $props();

  let _method = $derived(typeof href === 'object' ? href.method : method)
  let _href = $derived(typeof href === 'object' ? href.url : href)

  let asProp = $derived(_method !== 'get' ? 'button' : as.toLowerCase())
  let elProps =
    $derived({
      a: { href: _href },
      button: { type: 'button' },
    }[asProp] || {})
</script>

<!-- svelte-ignore a11y_no_static_element_interactions -->
<svelte:element
  this={asProp}
  use:inertia={{
    ...(asProp !== 'a' ? { href: _href } : {}),
    data,
    method: _method,
    replace,
    preserveScroll,
    preserveState: preserveState ?? _method !== 'get',
    only,
    except,
    headers,
    queryStringArrayFormat,
    async,
    prefetch,
    cacheFor,
    cacheTags,
  }}
  {...rest}
  {...elProps}
  onfocus={bubble('focus')}
  onblur={bubble('blur')}
  onclick={bubble('click')}
  ondblclick={bubble('dblclick')}
  onmousedown={bubble('mousedown')}
  onmousemove={bubble('mousemove')}
  onmouseout={bubble('mouseout')}
  onmouseover={bubble('mouseover')}
  onmouseup={bubble('mouseup')}
  oncancel-token={bubble('cancel-token')}
  onbefore={bubble('before')}
  onstart={bubble('start')}
  onprogress={bubble('progress')}
  onfinish={bubble('finish')}
  oncancel={bubble('cancel')}
  onsuccess={bubble('success')}
  onerror={bubble('error')}
  onprefetching={bubble('prefetching')}
  onprefetched={bubble('prefetched')}
>
  {@render children?.()}
</svelte:element>
