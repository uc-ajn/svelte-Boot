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
	import List from './List.svelte';

	export let answerSheet;
	export let data;
	export let review_id;
	export let count = 0;

	let list = false;
	let find1;
	let start_page = false;
	let result_page = false;
</script>

<main>
	{#if !start_page && !result_page}
		<div class="container-fluid p-0 overflow-auto">
			<List
				{answerSheet}
				{data}
				{list}
				on:displayQues={(event) => {
					review_id = event.detail;
				}}
			/>

			<div class="container p-4 px-5">
				{#each data as item, j}
					{#if review_id == j}
						<li class="list-unstyled">
							<strong>Question:{j + 1}</strong>
							{JSON.parse(item.content_text).question}
						</li>
						<br />
						{#each Array(JSON.parse(item.content_text).answers.length) as _, i}
							<label>
								<input
									type="radio"
									checked={JSON.parse(item.content_text).answers[i].is_correct == 1 ? true : false}
									disabled={JSON.parse(item.content_text).answers[i].is_correct == 1 ? false : true}
								/>
								{@html JSON.parse(item.content_text).answers[i].answer}
							</label>
							{#if find1}
								{#if JSON.parse(item.content_text).answers[i].is_correct == 1}
									<i
										class={JSON.parse(item.content_text).answers[i].is_correct == 1 &&
										JSON.parse(item.content_text).answers[i].answer == find1.answer
											? 'fa fa-check-double text-success'
											: 'fa fa-check text-primary'}
										aria-hidden="true"
									/>
								{:else if JSON.parse(item.content_text).answers[i].answer == find1.answer}
									<i class="fa fa-times text-danger" aria-hidden="true" />
								{/if}
							{:else if JSON.parse(item.content_text).answers[i].is_correct == 1}
								<i
									class="fa fa-check text-primary
								"
									aria-hidden="true"
								/>
							{/if}

							<br />
						{/each}
						<br /><br />

						<h4>Explanation:</h4>
						<h6 class="sequ">{@html JSON.parse(item.content_text).explanation}</h6>

						<span class="d-none "
							>{(find1 = answerSheet.find((o) => o.content_id === item.content_id))}</span
						>

						<div class="d-flex justify-content-center mt-5">
							{#if find1 === undefined}
								<span class="text-center px-3 py-1 fs-4 border border-2 border-secondary rounded-3 bg-secondary bg-gradient  text-white "
									>Unattempted</span
								>
							{:else if find1.content_id === item.content_id}
								<span class="text-center px-3 py-1 fs-4 border border-2 border-secondary rounded-3 bg-secondary bg-gradient text-white">Attempted</span>
							{:else}
								<span class="text-center px-3 py-1 fs-4 border border-2 border-secondary rounded-3 text-white bg-secondary">Unattempted</span>
							{/if}
						</div>
					{/if}
				{/each}
			</div>
		</div>
		<div
			class="p-2 vw-100 bg-secondary text-white position-fixed end-0 bottom-0 border-dark border-top"
		>
			<div class="d-flex justify-content-end">
				<button
					class="btn btn-secondary col-auto border text-nowrap mx-2"
					on:click={() => {
						list = !list;
					}}>List</button
				>
				<button
					class="btn btn-secondary col-auto border text-nowrap mx-2"
					on:click={() => (start_page = true)}>Re-Take</button
				>
				<button
					class="btn btn-secondary col-auto border text-nowrap mx-2"
					on:click={() => review_id--}
					disabled={review_id == 0}>Previous</button
				>
				<button
					class="btn btn-secondary col-auto border text-nowrap mx-2"
					on:click={() => review_id++}
					disabled={review_id == 10}>Next</button
				>
				<button
					class="btn btn-secondary col-auto border text-nowrap mx-2"
					on:click={() => (result_page = true)}>Result Page</button
				>
			</div>
		</div>
	{:else if start_page}
		<Start />
	{:else if result_page}
		<EndTest {data} {answerSheet} {count} />
	{/if}
</main>
