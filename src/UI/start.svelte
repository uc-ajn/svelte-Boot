<!-- File Name   :start.svelte
Description :Svelte with Bootstrap Project
Author      :Aayush Jain
Version     :1
Package     :
Created     :29/04/2022-->
<script>
	import TextPage from '../textPage.svelte';

	let toggle = false;
</script>

{#if !toggle}
	<div class="container">
		<div class="position-absolute top-50 start-50 translate-middle">
			<button class="btn btn-primary" on:click={() => (toggle = true)}>Start Test</button>
		</div>
	</div>
{/if}
{#if toggle}
	<TextPage />
{/if}

