<script setup></script>

<template>
	<div class="container_wrap">
		<NavBar></NavBar>
		<div class="wrap_header_page">
			<HeaderUser></HeaderUser>
			<slot></slot>
		</div>
	</div>
</template>

<style scoped lang="scss">
.wrap_header_page {
	display: flex;
	width: 100%;
	flex-direction: column;
	align-items: flex-start;
	justify-content: flex-start;
	height: 100%;
}
</style>
