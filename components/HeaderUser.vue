<script lang="ts" setup></script>

<template>
	<div class="wrap_header_user">
		<label for="search" class="wrap_input">
			<nuxt-icon name="search" filled class="search_icon"></nuxt-icon>
			<input
				type="text"
				id="search"
				class="input_search"
				placeholder="Search transactions,invoices or help"
			/>
		</label>
		<div class="user_cont">
			<button><nuxt-icon name="circle" filled></nuxt-icon></button>
			<button><nuxt-icon name="messages" filled></nuxt-icon></button>
			<button><nuxt-icon name="notify" filled></nuxt-icon></button>
			<div class="line"></div>
			<div class="wrap_set_avatar">
				<h2 class="user_cont__name">John Doe</h2>
				<nuxt-icon name="downarrow" filled class="button_arrow"></nuxt-icon>
				<img src="../assets/img/Avatar.png" alt="avatar" />
			</div>
		</div>
	</div>
</template>

<style scoped lang="scss">
.wrap_input {
	display: flex;
	align-items: center;
	justify-content: flex-start;
	max-width: 24rem;
	width: 100%;
}
.wrap_set_avatar {
	display: flex;
	align-items: center;
	justify-content: center;
	gap: 1rem;
}
.button_arrow {
	cursor: pointer;
}
.user_cont__name {
	color: #555555;
	font-weight: 500;
	font-size: 1.4rem;
	font-family: Roboto;
}
.line {
	height: 4rem;
	background-color: #bcbcbc;
	width: 2px;
}
.user_cont {
	display: flex;
	align-items: center;
	justify-content: center;
	gap: 3rem;
}
.wrap_header_user {
	display: flex;
	width: 100%;
	align-items: center;
	justify-content: space-between;
	padding: 1.4rem;
	background-color: #ffffff;
	-webkit-box-shadow: 1px 9px 7px -4px rgba(66, 68, 90, 1);
	-moz-box-shadow: 1px 9px 7px -4px rgba(66, 68, 90, 1);
	box-shadow: 1px 9px 7px -4px rgba(66, 68, 90, 1);
}
.search_icon {
	max-width: 2rem;
	margin-right: 0.6rem;
}
.input_search {
	border: none;
	outline: none;
	padding: 0;
	max-width: 18rem;
	width: 100%;
	font-size: 1.4rem;
	font-weight: 400;
	font-family: Roboto;
}
.input_search::placeholder {
	color: #bcbccb;
	font-size: 1.4rem;
	font-weight: 500;
	font-family: Roboto;
	width: auto;
}
</style>
