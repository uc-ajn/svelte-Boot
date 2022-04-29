<!-- File Name   :index.svelte
Description :Svelte with bootstrap Project
Author      :Aayush Jain
Version     :1
Package     :
Created     :29/04/2022 -->

<script>
	import Start from '../UI/start.svelte';
</script>

<div class="container-fluid vh-100 p-0">
	<nav class="navbar navbar-light bg-light border-bottom">
		<a class="navbar-brand position-absolute" href="#Start">
			<img
				src="https://www.ucertify.com/layout/themes/bootstrap4/images/logo/ucertify_logo.png"
				alt=""
				width="130"
				height="34"
				class="d-inline-block align-text-top"
			/>
		</a>
		<h3 class="text-center w-100">uCertify Test Prep</h3>
	</nav>

	<Start />
</div>

