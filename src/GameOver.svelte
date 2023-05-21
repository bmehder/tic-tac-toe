<script>
	import { scale } from 'svelte/transition'
	
	export let isGameOver
	export let winner
</script>

{#if isGameOver}
	<div transition:scale={{duration: 150}}>
		<p>
			{#if winner?.X}
				X Wins!
			{/if}

			{#if winner?.O} 
				O Wins!
			{/if}
		</p>

		<button on:click>Reset</button>
	</div>
{/if}

<style>
	p {
		margin-block-start: 1.5rem;
		color: tomato;
		font-size: 6vw;
	}
</style>