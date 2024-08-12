<script>
	const tabs = [
		'Baixar',
		'Explorar',
		'API',
		'---',
		'Documentação',
		'Linhagem',
		'Qualidade',
		'Ficha Técnica'
	];
	export let selectedTab = tabs[0];
</script>

<span class="w-12" />
<div class="flex-1">
	<nav class="flex items-center justify-start px-4 text-sm font-medium lg:px-4">
		{#each tabs as tab}
			{#if tab == '---'}
				<span class="w-12" />
			{:else}
				<a
					href="##"
					class="flex items-center gap-8  mx-2 px-2 py-1 transition-all
                        {selectedTab === tab ? 'text-primary border-b-2 border-primary': 'text-muted-foreground hover:text-primary'}"
					on:click={() => (selectedTab = tab)}
				>
					{tab}
				</a>
			{/if}
		{/each}
	</nav>
</div>
