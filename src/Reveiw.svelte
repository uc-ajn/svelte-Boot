<!-- File Name   :Reveiw.svelte
Description :Svelte Project
Author      :Aayush Jain
Version     :1
Package     :
Created     :06/04/2022
-->

<script>
	import Start from './UI/start.svelte';
	import EndTest from './EndTest.svelte';

	export let answerSheet;
	export let data;
	export let review_id;

	let start_page = false;
	let result_page = false;
</script>

<main>
	{#if !start_page && !result_page}
		<div class="container mt-5">
			{#each data as item, j}
				<div class="">
					{#if review_id == j + 1}
						<li class="list-unstyled">
							<strong>Question:{j + 1}</strong>
							{JSON.parse(item.content_text).question}
						</li>
						<br /> 
						{#each Array(JSON.parse(item.content_text).answers.length) as _, i}
							<label>
								<input
									type="radio"
									checked={true}
									disabled={JSON.parse(item.content_text).answers[i].is_correct == 1 ? false : true}
								/>
								{@html JSON.parse(item.content_text).answers[i].answer}
							</label> <br />
						{/each}
						<br /><br />

						<h4>Explanation:</h4>
						<h6 class="sequ">{@html JSON.parse(item.content_text).explanation}</h6>
					{/if}
				</div>
			{/each}
		</div>
		<div class="p-2 vw-100 bg-secondary text-white position-absolute end-0 bottom-0 border-dark border-top">
			<div class="d-flex justify-content-end">
				<button class="btn btn-secondary col-auto border text-nowrap mx-2" on:click={() => (start_page = true)}>Re-Take</button>
				<button class="btn btn-secondary col-auto border text-nowrap mx-2" on:click={() => review_id--} disabled={review_id == 1}>Previous</button>
				<button class="btn btn-secondary col-auto border text-nowrap mx-2" on:click={() => review_id++} disabled={review_id == 11}>Next</button>
				<button class="btn btn-secondary col-auto border text-nowrap mx-2" on:click={() => (result_page = true)}>Result Page</button>
			</div>
		</div>
	{:else if start_page}
		<Start />
	{:else if result_page}
		<EndTest {data} {answerSheet} />
	{/if}
</main>

