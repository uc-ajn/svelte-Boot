<script>
	import TextPage from './textPage.svelte';
	import { createEventDispatcher } from 'svelte';
	import {beforeUpdate} from 'svelte';

	const dispatch = createEventDispatcher();

	export let answerSheet;
	let total_question = 0;
	let attempted_ques = 0;
	let unattempted_ques = 11;
	export let data;
	export let list = false;
	let count = 0;

	beforeUpdate(() => {
		let i = 0;
		total_question = data.length ;
		for (i; i < answerSheet.length; i++) {
			attempted_ques = i + 1;
		}
		unattempted_ques = total_question - i;
	});

	function getDetail(x) {
		dispatch('displayQues', x);
	}
</script>

{#if list == true}
	<div class="side_bar">
		<div class="question">
			<p>Attempted question {attempted_ques}</p>
			<p>Unattempted question {unattempted_ques}</p>
		</div>
		<div class="question_list">
			<ul>
				{#each data as item, j}
					<li on:click={() => getDetail(j)}>
						Qus:{j + 1}
						{JSON.parse(item.content_text).question}
					</li>
				{/each}
			</ul>
		</div>
	</div>
{/if}

<style>
	.question p {
		text-align: center;
		border: 1px solid gainsboro;
	}

	.side_bar {
		position: absolute;
		top: 70px;
		left: 0;
		height: calc(88.2vh - 70px);
		width: 250px;
		border: 2px solid gray;
		float: left;
	}

	.question_list ul li {
		white-space: nowrap;
		width: 240px;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.question_list ul {
		margin: 0px;
		padding: 10px;
	}

	.question_list li {
		line-height: 2.5;
		list-style-type: none;
		cursor: pointer;
	}
</style>
