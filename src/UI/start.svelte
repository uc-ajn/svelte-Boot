<script>
	import EndTest from '../EndTest.svelte';
	import TextPage from '../textPage.svelte';
	
	let toggle = false;

</script>

<main>
	<div class="Container">
		{#if !toggle}
			<div class="Start_btn flex_center">
				<button on:click={()=> toggle = true}>Start Test</button>
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
	}

	.Start_btn {
		margin-top: 70px;
		height: 600px;
	}


	.Container {
		margin-top: 100px;
	}

	.Start_btn button {
		width: 100px;
		height: 40px;
		border: 2px solid black;
		cursor: pointer;
		border-radius: 8px;
	}

	.Start_btn button:hover{
		background-color: rgba(20, 20, 231, 0.142);
		border: 2px solid rgb(4, 4, 174);
	}

</style>
