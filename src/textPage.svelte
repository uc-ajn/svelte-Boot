<!-- File Name   :textPage.svelte
Description :Svelte with bootstrap Project
Author      :Aayush Jain
Version     :1
Package     :
Created     :29/04/2022
-->
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
		const response = await fetch('/data/question.json');
		data = await response.json();
	});

	stopper = setInterval(() => {
		if (time > 0) time--;
		else {
			timeOut();
			clearInterval(stopper);
		}
	}, 1000);

	let clickValue;

	function EndPage(event) {
		clickValue = event.detail;
		if (clickValue === 1) {
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
		TimeOverModal = true;
		isOpenModal = true;
	}

	$: minute = Math.floor(time / 60);
	$: minname = minute > 1 ? ':' : ':';
	$: second = Math.floor(time - minute * 60);

	function closeModal() {
		isOpenModal = false;
		endTestBtn = false;
	}

	function closeModal2() {
		isOpenModal = false;
	}
</script>

{#if TimeOverModal}
	<Modal {isOpenModal}>
		<p slot="para">Time is over and test will be End</p>
		<button slot="ok_btn" class="btn btn-outline-danger" on:click={closeModal}>OK</button>
	</Modal>
{:else}
	<Modal {isOpenModal} on:closeModal={closeModal}>
		<p slot="para" class="fw-bold fs-5">Are sure to End the test</p>
		<button slot="ok_btn" class="btn btn-outline-danger" data-bs-dismiss="modal" on:click={EndPage}
			>Confirm</button
		>
		<button
			slot="cancel_btn"
			class="btn btn-outline-secondary"
			data-bs-dismiss="modal"
			on:click={closeModal2}>Cancel</button
		>
	</Modal>
{/if}

{#if endTestBtn}
	<div class="container-fluid p-0">
		<List
			{answerSheet}
			{data}
			{list}
			on:displayQues={(event) => {
				count = event.detail;
			}}
		/>
		<div class="container w-75">
			{#each data as item, j}
				{#if count == j}
					<h4 class="mb-3 pt-4">Q{j + 1}.{JSON.parse(item.content_text).question}</h4>
					{#each Array(JSON.parse(item.content_text).answers.length) as _, i}
						<label class="mt-2">
							<span class="d-none"
								>{(found = answerSheet.find(
									(o) => o.answer === JSON.parse(item.content_text).answers[i].answer
								))}</span
							>
							<input
								type="radio"
								data-value={i + 1}
								name="choose_correct"
								checked={found && found.answer === JSON.parse(item.content_text).answers[i].answer
									? true
									: false}
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
	</div>
	<!-- {/if} -->
	<div
		class="p-2 vw-100 bg-secondary text-white position-absolute end-0 bottom-0 border-dark border-top"
	>
		<div class="d-flex justify-content-end">
			<p class="my-auto col-auto mx-2">0{minute}{minname}{second < 10 ? '0' + second : second}</p>

			<button
				class="btn btn-secondary col-auto mx-2 border"
				on:click={() => {
					list = !list;
				}}>List</button
			>
			<button
				class="btn btn-secondary col-auto border mx-2"
				on:click={() => count--}
				disabled={count == 0}>Previous</button
			>
			<p class="my-auto col-auto mx-2">{count + 1} of 11</p>
			<button
				class="btn btn-secondary col-auto border mx-3"
				id="next_btn"
				on:click={() => count++}
				disabled={count == data.length - 1}>Next</button
			>
			<button
				class="btn btn-secondary col-auto border text-nowrap mx-2"
				data-bs-toggle="modal"
				data-bs-target="#exampleModal"
				on:click={endPage}>End Test</button
			>
		</div>
	</div>
{:else}
	<EndTest {data} {answerSheet} {count}/>
{/if}
