<!-- File Name   :index.svelte
Description :Svelte Project
Author      :Aayush Jain
Version     :2
Package     :
Created     :06/04/2022
Updated By  :Author
Updated Date:26/04/2022
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
		<ul class="review_container">
			{#each data as item, j}
				<div class="result">
					{#if review_id == j + 1}
						<li>
							<strong>Question:{j + 1}</strong>
							{JSON.parse(item.content_text).question}
						</li>
						<br /> <br />
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
						<br /><br /><br />

						<h2>Explanation:</h2>
						<h3 class="sequ">{@html JSON.parse(item.content_text).explanation}</h3>
					{/if}
				</div>
			{/each}
		</ul>
		<div class="review_navigation">
			<div class="review_Nav_container">
				<button on:click={() => (start_page = true)}>Re-Take</button>
				<button on:click={() => review_id--} disabled={review_id == 1}>Previous</button>
				<button on:click={() => review_id++} disabled={review_id == 11}>Next</button>
				<button on:click={() => (result_page = true)}>Result Page</button>
			</div>
		</div>
	{:else if start_page}
		<Start />
	{:else if result_page}
		<EndTest {data} {answerSheet} />
	{/if}
</main>

<style>
	.review_container {
		width: 80%;
		margin: 0px auto;
	}

	.result li {
		list-style: none;
	}

	.review_navigation {
		position: absolute;
		bottom: 0;
		width: 100vw;
		height: 65px;
		background-color: #eae4e4cb;
		border-top: 2px solid rgb(167, 170, 170);
	}

	.review_Nav_container{
		display: flex;
		justify-content: end;
		width: 97%;
		margin: 15px 0px;
	}
	.review_navigation button {
		width: 90px;
		height: 32px;
		margin-right: 16px;
		border-radius: 5px;
	}
</style>
