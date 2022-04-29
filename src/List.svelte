<!-- File Name   :list.svelte
Description :Svelte with svelte Project
Author      :Aayush  Jain
Version     :1
Package     :
Created     :29/04/2022
Updated By  :Author
-->
<script>
	import { createEventDispatcher } from 'svelte';
	import { beforeUpdate } from 'svelte';

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
		total_question = data.length;
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
	<div class="position-absolute mt-0 left-0 bg-light border text-dark">
		<ul class="list-group list-group-flush mt-1">
			<li class="list-group-item text-center">Attempted question {attempted_ques}</li>
			<li class="list-group-item text-center">Unattempted question {unattempted_ques}</li>
		</ul>

		<div class="question_list mt-2">
			<ul class="list-group ">
				{#each data as item, j}
					<a
						href="#list"
						class="list-group-item m-1 list-group-item-action list-group-item-light text-truncate text-nowrap overflow-hidden"
						style="width:250px;"
						on:click={() => getDetail(j)}
					>
						{j + 1}.
						{JSON.parse(item.content_text).question}
					</a>
				{/each}
			</ul>
		</div>
	</div>
{/if}
