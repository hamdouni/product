<script>
	export let type;
	export let value;
</script>
{#if value && value != "unknown"}
<img alt="{type}" src="/assets/{type}-{value}.svg"/>
{/if}
<style>
	img {
		max-height: 2.5rem;
		margin-right: 2rem;
	}
</style>
