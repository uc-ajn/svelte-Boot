<!-- File Name   :Modal.svelte
Description :Svelte with Bootstrap Project
Author      :Aayush Jain
Version     :1
Package     :
Created     :29/04/2022 -->

<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	export let isOpenModal;

	function closeModal() {
		isOpenModal = false;
		dispatch('closeModal', { isOpenModal });
	}
</script>

<div class="position-fixed top-0 start-0 w-100 h-100 " style="display: {isOpenModal ? 'block' : 'none'}; background-color: rgba(0, 0, 0, 0.4);" on:click={closeModal} />
<div class="modal height150 w-25 position-fixed top-50 start-50 translate-middle bg-light border rounded" style="display: {isOpenModal ? 'block' : 'none'};">
	<div class="modal_content p-4 text-center">
        <slot name = "para"></slot>
        <slot name="ok_btn" id="btn"></slot>
		<slot name="cancel_btn"></slot>
	</div>
</div>

