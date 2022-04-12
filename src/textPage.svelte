<script>
	import { onMount } from 'svelte';
	import List from './List.svelte';
	import EndTest from './EndTest.svelte';
	import Modal from './UI/Modal.svelte';

	let answerSheet = [];
	let endTestBtn = true;
	let list = false;
	let data = [];
	let count = 0;
	let found = {};
	let stopper;
	const TOTAL_TIME = 120;
	let time = TOTAL_TIME;
	let isOpenModal = false;
	let TimeOverModal = false;


	onMount(async function () {
		const response = await fetch('/static/data/question.json');
		data = await response.json();
	});

	stopper = setInterval(() => {
		if (time > 0) time--;
		else {
			// isOpenModal = true;
			timeOut();
			clearInterval(stopper);
		}
	}, 1000);

	let clickValue;

	function EndPage(event){
		clickValue = event.detail;
		if(clickValue === 1){
			clearInterval(stopper);
			endTestBtn = false;
			isOpenModal = false;
			
		}
	}

	function endPage() {
			isOpenModal = true;
	}

	function onChange(content_id, answer, is_correct, i) {
		let option = String.fromCharCode(65 + i);
		let ansSheet = answerSheet.filter((o) => o.content_id !== content_id);
		ansSheet = [
			...ansSheet,
			{ content_id: content_id, answer: answer, is_correct: is_correct, option: option }
		];
		answerSheet = [...ansSheet];
	}

	function timeOut() {
		endTestBtn = false;
		TimeOverModal = true
		isOpenModal = true;
	}

	$: minute = Math.floor(time / 60);
	$: minname = minute > 1 ? ':' : ':';
	$: second = Math.floor(time - minute * 60);

	function closeModal() {
		isOpenModal = false;
	}

	
</script>

{#if TimeOverModal}
<Modal {isOpenModal} >
	<p slot="para">Time is over and test will be End</p>
	<button slot="ok_btn" class="btnOk" on:click={closeModal}>OK</button>
</Modal>
{:else}
<Modal {isOpenModal} on:closeModal={closeModal}>
	<p slot="para">Are sure to End the test</p>
	<button slot="ok_btn" class="btnOk" on:click={EndPage}>OK</button>
	<button slot="cancel_btn" class="btnOk" on:click={closeModal}>Cancel</button>
</Modal>
{/if}

{#if endTestBtn}
	<div class="container">
		<List
			{answerSheet}
			{data}
			{list}
			on:displayQues={(event) => {
				count = event.detail;
			}}
		/>
		{#each data as item, j}
			{#if count == j}
				<h1>Q{j + 1}.{JSON.parse(item.content_text).question}</h1>
				{#each Array(JSON.parse(item.content_text).answers.length) as _, i}
					<label>
						<span class="span-hide"
							>{(found = answerSheet.find(
								(o) => o.answer === JSON.parse(item.content_text).answers[i].answer
							))}</span
						>
						<input
							type="radio"
							data-value={i + 1}
							name="choose_correct"
							checked={found && found.answer === JSON.parse(item.content_text).answers[i].answer ? true : false}
							value={JSON.parse(item.content_text).answers[i].answer}
							on:change={() =>
								onChange(
									item.content_id,
									JSON.parse(item.content_text).answers[i].answer,
									JSON.parse(item.content_text).answers[i].is_correct,
									i + 1
								)}
						/>
						{@html JSON.parse(item.content_text).answers[i].answer}
					</label><br />
				{/each}
			{/if}
		{/each}
	</div>
	<div class="navigation">
		<div class="navigation_content">
			<p>0{minute}{minname}{second < 10 ? '0' + second : second}</p>

			<button
				on:click={() => {
					list = !list;
				}}>List</button
			>
			<button on:click={endPage}>End Test</button>
			<button on:click={() => count--} disabled={count == 0}>Previous</button>
			<p>{count + 1} of 11</p>
			<button id="next_btn" on:click={() => count++} disabled={count == data.length - 1}
				>Next</button
			>
		</div>
	</div>
{:else}
	<EndTest {data} {answerSheet} />
{/if}

<style>
	.container {
		margin: 0 auto;
		width: 65%;
	}
	.container h1 {
		font-size: 27px;
		margin-bottom: 20px;
	}

	.container label {
		font-size: 20px;
	}

	.navigation {
		position: absolute;
		bottom: 0;
		left: 0;
		width: 100vw;
		height: 65px;
		background-color: #eae4e4cb;
		border-top: 2px solid rgb(167, 170, 170);
	}

	.navigation_content {
		display: flex;
		justify-content: end;
		width: 97%;
		margin: 15px 0px;
	}

	button {
		width: 90px;
		height: 32px;
		border: 2px solid #9e9797;
		border-radius: 5px;
		font-weight: bold;
		margin-right: 10px;
		cursor: pointer;
		margin: auto 5px;
	}

	.btnOk {
		margin-top: 25px;
	}

	p {
		font-weight: bold;
		font-size: 18px;
		margin: auto 8px;
	}

	.span-hide {
		display: none;
	}
</style>
