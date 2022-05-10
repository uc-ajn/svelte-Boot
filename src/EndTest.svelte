<!-- File Name   :EndTest.svelte
Description :Svelte Project
Author      :Aayush Jain
Version     :2
Package     :
Created     :06/04/2022-->
<script>
	import { onMount } from 'svelte';
	import Start from './UI/start.svelte';
	import ReveiwPage from './Reveiw.svelte';

	export let data;
	export let answerSheet;
	export let count;

	let total_questions = 0;
	let correct_answers = 0;
	let incorrect_answers = 0;
	let attempted_questions = 0;
	let unattempted_questions = 0;
	let restart = false;
	let found = {};
	let reveiw = false;
	let review_id;
	let result = 0;

	onMount(async function () {
		let i = 0;
		total_questions = data.length;
		for (i; i < answerSheet.length; i++) {
			if (answerSheet[i].is_correct == 1) {
				correct_answers++;
			} else {
				incorrect_answers++;
			}
		}
		attempted_questions = i;
		unattempted_questions = total_questions - i;
		result = (100 * attempted_questions) / data.length;
		// @ts-ignore
		result = result.toFixed(2);
	});

	function ReveiwAnswer(e) {
		review_id = e;
		reveiw = true;
	}
</script>

{#if reveiw}
	<ReveiwPage {data} {review_id} {answerSheet} {count}/>
{:else if !restart}
	<main>
		<div class="container d-flex justify-content-center mt-4 ">
			<div
				class="card text-dark bg-light mx-1 text-nowrap h-25 shadow bg-body rounded border width160"
			>
				<div class="card-body">
					<h5 class="card-title text-center text-primary">Total</h5>
					<p class="card-text text-center">
						{total_questions}
					</p>
				</div>
			</div>
			<div
				class="card text-dark bg-light mx-1 text-nowrap h-25 shadow  bg-body rounded border width160"
			>
				<div class="card-body">
					<h5 class="card-title text-center text-success">Correct</h5>
					<p class="card-text text-center">
						{correct_answers}
					</p>
				</div>
			</div>
			<div
				class="card text-dark bg-light mx-1 text-nowrap h-25 shadow  bg-body rounded border width160"
			>
				<div class="card-body">
					<h5 class="card-title text-center text-danger ">Incorrect</h5>
					<p class="card-text text-center">
						{incorrect_answers}
					</p>
				</div>
			</div>
			<div
				class="card text-dark bg-light mx-1 text-nowrap h-25 shadow  bg-body rounded border width160"
			>
				<div class="card-body">
					<h5 class="card-title text-center text-info">Attempted</h5>
					<p class="card-text text-center">
						{attempted_questions}
					</p>
				</div>
			</div>
			<div
				class="card text-dark bg-light mx-1 text-nowrap h-25 shadow  bg-body rounded border width160"
			>
				<div class="card-body">
					<h5 class="card-title text-center text-warning text-sm-wrap">Unattempted</h5>
					<p class="card-text text-center">
						{unattempted_questions}
					</p>
				</div>
			</div>
			<div
				class="card text-dark bg-light mx-1 text-nowrap h-25 shadow bg-body rounded border width160"
			>
				<div class="card-body">
					<h5 class="card-title text-center text-secondary">Result</h5>
					<p class="card-text text-center">
						{result}%
					</p>
				</div>
			</div>
		</div>
		<div class="container mt-4 p-0 ">
			{#if data && data.length}
				{#each data as item, j}
					<div class="list-group m-auto d-flex flex-row col-auto m-auto justify-content-center">
						<a
							href="#"
							class="list-group-item col-8 list-group-item-action list-group-item-secondary shadow-sm rounded mb-2 py-1 px-4"
							on:click={() => ReveiwAnswer(j)}
						>
							<div>
								<strong>Question:{j + 1}</strong>
								{item.snippet}
							</div>
							<span class="d-none "
								>{(found = answerSheet.find((o) => o.content_id === item.content_id))}</span
							>
						</a>
						<div class="ml-2 align-middle">
							{#each Array(JSON.parse(item.content_text).answers.length) as _, i}
								{#if found}
									{#if JSON.parse(item.content_text).answers[i].is_correct == 1}
										<div class="btn btn-sm rounded-circle border-secondary mx-2 bg-success">
											{String.fromCharCode(65 + i)}
										</div>
									{:else if JSON.parse(item.content_text).answers[i].answer == found.answer && JSON.parse(item.content_text).answers[i].is_correct == 1}
										<div class="btn btn-sm rounded-circle border-secondary mx-2 bg-success">
											{String.fromCharCode(65 + i)}
										</div>
									{:else if JSON.parse(item.content_text).answers[i].answer == found.answer}
										<div class="btn btn-sm rounded-circle border-secondary mx-2 bg-danger">
											{String.fromCharCode(65 + i)}
										</div>
									{:else}
										<div class="btn btn-sm rounded-circle border-secondary mx-2">
											{String.fromCharCode(65 + i)}
										</div>
									{/if}
								{:else if JSON.parse(item.content_text).answers[i].is_correct == 1}
									<div class="btn btn-sm rounded-circle border-secondary mx-2 bg-success">
										{String.fromCharCode(65 + i)}
									</div>
								{:else}
									<div class="btn btn-sm rounded-circle border-secondary mx-2 ">
										{String.fromCharCode(65 + i)}
									</div>
								{/if}
							{/each}
						</div>
					</div>
				{/each}
			{/if}
			<div class="mt-2 d-flex justify-content-center">
				<button
					type="button"
					class="btn btn-outline-primary"
					id="restar_btn"
					on:click={() => (restart = true)}>Re-take</button
				>
			</div>
		</div>
	</main>
{:else}
	<Start />
{/if}
