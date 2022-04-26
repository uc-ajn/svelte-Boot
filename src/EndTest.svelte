<!-- File Name   :index.svelte
Description :Svelte Project
Author      :Aayush Jain
Version     :2
Package     :
Created     :06/04/2022
Updated By  :Author-->

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
	let result ;

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
		result = result.toFixed(2);
	});

	function ReveiwAnswer(e) {
		review_id = e+1;
		console.log(review_id);
		reveiw = true;
	}
</script>

{#if reveiw}
	<ReveiwPage {data} {review_id} {answerSheet} />
{:else if !restart}
	<main>
		<div class="result_container">
			<div class="result_block" id="total">
				<h3>Total</h3>
				<h4>{total_questions}</h4>
			</div>
			<div class="result_block" id="correct">
				<h3>Correct</h3>
				<h4>{correct_answers}</h4>
			</div>
			<div class="result_block" id="incorrect">
				<h3>Incorrect</h3>
				<h4>{incorrect_answers}</h4>
			</div>
			<div class="result_block" id="attempted">
				<h3>Attempted</h3>
				<h4>{attempted_questions}</h4>
			</div>
			<div class="result_block" id="unattempted">
				<h3>Unattempted</h3>
				<h4>{unattempted_questions}</h4>
			</div>
			<div class="result_block" id="result">
				<h3>Result</h3>
				<h4>{result}%</h4>
			</div>
		</div>
		<ul class="result_list">
			{#if data && data.length}
				{#each data as item, j}
					<div class="result">
						<li on:click={() =>ReveiwAnswer(j)} >
							<strong>Question:{j + 1}</strong>
							{item.snippet}
						</li>
						<span class="span-hide"
							>{(found = answerSheet.find((o) => o.content_id === item.content_id))}</span
						>
						<div class="options">
							{#each Array(JSON.parse(item.content_text).answers.length) as _, i}
								{#if found}
									{#if JSON.parse(item.content_text).answers[i].is_correct == 1}
										<div class="option bg-green">{String.fromCharCode(65 + i)}</div>
									{:else if JSON.parse(item.content_text).answers[i].answer == found.answer && JSON.parse(item.content_text).answers[i].is_correct == 1}
										<div class="option bg-green">{String.fromCharCode(65 + i)}</div>
									{:else if JSON.parse(item.content_text).answers[i].answer == found.answer}
										<div class="option bg-red">{String.fromCharCode(65 + i)}</div>
									{:else}
										<div class="option">{String.fromCharCode(65 + i)}</div>
									{/if}
								{:else if JSON.parse(item.content_text).answers[i].is_correct == 1}
									<div class="option bg-green">{String.fromCharCode(65 + i)}</div>
								{:else}
									<div class="option">{String.fromCharCode(65 + i)}</div>
								{/if}
							{/each}
						</div>
					</div>
				{/each}
			{/if}
		</ul>
		<div id="restart">
			<button id="restar_btn" on:click={() => (restart = true)}>Re-take</button>
		</div>
	</main>
{:else}
	<Start />
{/if}

<style>
	.bg-red {
		background-color: red;
	}

	.bg-green {
		background-color: green;
	}

	.options {
		display: flex;
		margin-left: 35px;
	}

	.result_list {
		padding-left: 15px;
	}

	.result {
		display: flex;
		justify-content: center;
		padding: 0px;
	}

	.option {
		border: 1.5px solid gray;
		height: 25px;
		width: 25px;
		text-align: center;
		border-radius: 50%;
		margin-left: 10px;
		/* justify-items: center; */
		/* justify-content: center; */
		/* align-items: center; */
		padding-top: 1px;
	}

	.result_container {
		display: flex;
		justify-content: center;
	}
	.result_block {
		border: 2px solid gray;
		border-radius: 10px;
		width: 150px;
		height: 90px;
		margin-left: 20px;
		/* margin-top: 6px; */
		padding: 10px;
		font-size: 20px;
		text-align: center;
	}

	h4{
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
	}

	ul li {
		list-style-type: none;
		padding-bottom: 20px;
		line-height: 1.2;
		white-space: nowrap;
		width: 775px;
		cursor: pointer;
	}

	ul {
		margin-top: 50px;
	}
	#restart {
		margin-top: 10px;
		text-align: center;
	}

	#restar_btn {
		height: 35px;
		width: 90px;
		border-radius: 8px;
		background: rgb(220, 229, 231);
	}

	.span-hide {
		display: none;
	}
</style>
