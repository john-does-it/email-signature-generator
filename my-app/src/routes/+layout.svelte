<script>
	import { page } from '$app/state'
	import { locales, localizeHref } from '$lib/paraglide/runtime'
  
  import { dev } from '$app/environment'
	import { injectAnalytics } from '@vercel/analytics/sveltekit'
	injectAnalytics({ mode: dev ? 'development' : 'production' })
</script>

<slot></slot>

<div style="display:none">
	{#each locales as locale}
		<a href={localizeHref(page.url.pathname, { locale })}>{locale}</a>
	{/each}
</div>