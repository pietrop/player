<script lang="ts">
  import { isUndefined } from '@vidstack/foundation';

  import clsx from 'clsx';

  export let primary = false;
  export let type: 'flat' | 'raised' = 'flat';
  export let arrow: 'left' | 'right' | null = null;

  let __as: 'button' | 'a' = 'button';
  export { __as as as };

  let __class = '';
  export { __class as class };

  $: isButton = __as === 'button' && isUndefined($$restProps['href']);

  $: buttonClass = clsx(
    'group transform-gpu text-base font-medium transition-transform hover:scale-105',
    type === 'raised' && 'shadow-md hover:shadow-lg flex items-center justify-center',
    (isButton || type === 'raised') && 'rounded-full px-4 992:px-5 py-2',
    type === 'raised'
      ? primary
        ? 'bg-inverse text-current'
        : 'bg-current border-2 border-inverse text-inverse'
      : false,
    __class,
  );

  $: contentClass = clsx(
    'inline-block transform transition-transform duration-100 group-hover:translate-x-0',
    arrow === 'left' && '-translate-x-3 ',
    arrow === 'right' && 'translate-x-2',
  );

  $: arrowClass = clsx(
    arrow &&
      'opacity-0 transition-opacity duration-100 group-hover:visible group-hover:opacity-100',
    !arrow ? 'hidden' : 'inline-block',
  );
</script>

{#if isButton}
  <button class={buttonClass} {...$$restProps}>
    {#if arrow === 'left'}
      <span class={arrowClass}>&lt;-</span>
    {/if}
    <span class={contentClass}><slot /></span>
    {#if arrow === 'right'}
      <span class={arrowClass}>-></span>
    {/if}
  </button>
{:else}
  <!-- svelte-ignore a11y-missing-attribute -->
  <a class={buttonClass} {...$$restProps}>
    {#if arrow === 'left'}
      <span class={arrowClass}>&lt;-</span>
    {/if}
    <span class={contentClass}><slot /></span>
    {#if arrow === 'right'}
      <span class={arrowClass}>-></span>
    {/if}
  </a>
{/if}
