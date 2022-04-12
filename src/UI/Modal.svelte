<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	export let isOpenModal;

	function closeModal() {
		isOpenModal = false;
		dispatch('closeModal', { isOpenModal });
	}
</script>

<div id="background" style="--display: {isOpenModal ? 'block' : 'none'};" on:click={closeModal} />
<div id="modal" style="--display: {isOpenModal ? 'block' : 'none'};">
	<div class="modal_content">
        <slot name = "para"></slot>
        <slot name="ok_btn" id="btn"></slot>
		<slot name="cancel_btn"></slot>
	</div>
</div>

<style>

	#background {
		display: var(--display);
		position: fixed;
		z-index: 1;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		background-color: rgba(0, 0, 0, 0.4);
	}

	#modal {
		height: 150px;
		width: 350px;
		display: var(--display);
		position: fixed;
		z-index:2;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		background: #fff;
		filter: drop-shadow(0 0 8px #333);	
	}
    .modal_content{
        text-align: center;
        padding: 30px;
    }

</style>
