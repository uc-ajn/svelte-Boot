<script>
	import EndTest from '../EndTest.svelte';
	import TextPage from '../textPage.svelte';

	let toggle = false;
</script>

<main>
	<div class="Container">
		{#if !toggle}
			<div class="Start_btn flex_center">
				<button on:click={() => (toggle = true)}>Start Test</button>
			</div>
		{/if}
		{#if toggle}
			<TextPage />
		{/if}
	</div>
</main>

<style>
	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}
	.flex_center {
		display: flex;
		justify-content: center;
		align-items: center;
		vertical-align: middle;
	}
	.Container {
		width: 100%;
		padding-top: 25px;
		margin: auto;
	}

	.Start_btn{
		margin-top: 250px;
	}

	.Start_btn button {
		width: 100px;
		height: 40px;
		border: 2px solid black;
		cursor: pointer;
		border-radius: 8px;
		margin: auto 0px;
	}

	.Start_btn button:hover {
		background-color: rgba(20, 20, 231, 0.142);
		border: 2px solid rgb(4, 4, 174);
	}
</style>
