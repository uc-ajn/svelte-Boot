<!-- File Name   :index.svelte
Description :Svelte Project
Author      :Aayush Jain
Version     :2
Package     :
Created     :06/04/2022
Updated By  :Author
Update Date :11/04/2022 -->

<header id="header" class="flex-center">
	<img
		src="	https://www.ucertify.com/layout/themes/bootstrap4/images/logo/ucertify_logo.png"
		alt="uCertify logo"
	/>
	<h2>uCertify Test Prep</h2>
</header>

<style>
	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	.flex-center {
		display: flex;
		justify-content: center;
		align-items: center;
	}
	#header {
		height: 70px;
		box-sizing: border-box;
		border-bottom: 3px solid black;
	}

	#header img {
		position: absolute;
		left: 0;
		width: 100px;
		border: none;
		font-size: 14px;
		font-weight: 500;
		margin-left: 20px;
	}

	h2 {
		margin: 10px;
		font-size: 224x;
	}
</style>
