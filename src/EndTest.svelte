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
		review_id = e + 1;
		console.log(review_id);
		reveiw = true;
	}
</script>

{#if reveiw}
	<ReveiwPage {data} {review_id} {answerSheet} />
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
		<div class="container mt-4">
			{#if data && data.length}
				{#each data as item, j}
					<div class="list-group col-10 mx-auto">
						<a
							href="#ResultPage"
							class="list-group-item list-group-item-action list-group-item-secondary shadow-sm rounded d-flex justify-content-between mb-2 py-1 px-4"
							on:click={() => ReveiwAnswer(j)}
						>
							<div>
								<strong>Question:{j + 1}</strong>
								{item.snippet}
							</div>
							<span class="d-none "
								>{(found = answerSheet.find((o) => o.content_id === item.content_id))}</span
							>

							<div class="">
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
						</a>
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

<style>
	/* .bg-red {
		background-color: red;
	}

	.bg-green {
		background-color: green;
	}

	.options {
		display: flex;
		margin-left: 35px;
	} */

	/* .result_list {
		padding-left: 15px;
	} */

	/* .result {
		display: flex;
		justify-content: center;
		padding: 0px;
	} */

	/* .option {
		border: 1.5px solid gray;
		height: 25px;
		width: 25px;
		text-align: center;
		border-radius: 50%;
		margin-left: 10px;
		padding-top: 1px;
	} */

	/* .result_container {
		display: flex;
		justify-content: center;
	} */
	/* .result_block {
		border: 2px solid gray;
		border-radius: 10px;
		width: 150px;
		height: 90px;
		margin-left: 20px;
		padding: 10px;
		font-size: 20px;
		text-align: center;
	} */

	/* h4 {
		padding: 10px;
	}
	#total h3 {
		color: rgb(8, 8, 184);
	}
	#correct h3 {
		color: rgb(9, 182, 9);
	}
	#incorrect h3 {
		color: red;
	}
	#attempted h3 {
		color: blueviolet;
	}
	#unattempted h3 {
		color: rgb(216, 216, 6);
	}

	#result {
		color: rgba(222, 6, 222, 0.948);
	}

	.result_block h3 {
		margin-top: 5px;
		font-size: 22px;
	} */

	/* ul li {
		list-style-type: none;
		padding-bottom: 20px;
		line-height: 1.2;
		white-space: nowrap;
		width: 775px;
		cursor: pointer;
	} */

	/* ul {
		margin-top: 50px;
	} */
	/* #restart {
		margin-top: 10px;
		text-align: center;
	}

	#restar_btn {
		height: 35px;
		width: 90px;
		border-radius: 8px;
		background: rgb(220, 229, 231);
	} */

	/* .span-hide {
		display: none;
	} */
</style>
