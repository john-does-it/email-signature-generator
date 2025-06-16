<script>
	import { page } from '$app/state'
	import { locales, localizeHref } from '$lib/paraglide/runtime'

	function recalculatePaddingBottomAppliedToSEOContainer() {
		console.log('Window resized')
	}
</script>

<svelte:window on:resize={recalculatePaddingBottomAppliedToSEOContainer}/>

<slot></slot>

<div style="display:none">
	{#each locales as locale}
		<a href={localizeHref(page.url.pathname, { locale })}>{locale}</a>
	{/each}
</div>