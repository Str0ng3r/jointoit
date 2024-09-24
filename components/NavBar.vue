<script lang="ts" setup>
const route = useRoute();

interface Tab {
	btnText: string;
	btnLink: string;
	icon: string;
}
interface TabBarProps {
	tabArr?: Tab[];
}
const props = withDefaults(defineProps<TabBarProps>(), {
	tabArr: () => [
		{
			btnText: "Home",
			btnLink: "/",
			icon: "home",
		},
		{
			btnText: "Dashboard",
			btnLink: "/Dashboard",
			icon: "dash",
		},
		{
			btnText: "Inbox",
			btnLink: "/Inbox",
			icon: "inbox",
		},
		{
			btnText: "Products",
			btnLink: "/Products",
			icon: "products",
		},
		{
			btnText: "Invoices",
			btnLink: "/Invoices",
			icon: "inv",
		},
		{
			btnText: "Customers",
			btnLink: "/Customers",
			icon: "cust",
		},
		{
			btnText: "Chat Room",
			btnLink: "/ChatRoom",
			icon: "chat",
		},
		{
			btnText: "Calendar",
			btnLink: "/Calendar",
			icon: "calendarevent",
		},
		{
			btnText: "Help Center",
			btnLink: "/HelpCenter",
			icon: "help",
		},
		{
			btnText: "Settings",
			btnLink: "/Settings",
			icon: "settings",
		},
	],
});
</script>

<template>
	<div class="tab-bar">
		<div class="buttons_wrapper">
			<h1 class="buttons_wrapper__title">IMPEKABLE</h1>
			<NuxtLink
				v-for="(tab, key) in tabArr"
				:key="key"
				class="link_tab"
				:class="{ active: route.fullPath === tab.btnLink }"
				:to="tab.btnLink"
			>
				<nuxt-icon :name="tab.icon" filled></nuxt-icon>
				{{ tab.btnText }}
			</NuxtLink>
		</div>
	</div>
</template>

<style scoped lang="scss">
.tab-bar {
	width: 100%;
	max-width: 35rem;
	height: 100vh;
	background-color: #43425d;
}
.buttons_wrapper__title {
	font-weight: 500;
	color: #ffffff;
	font-size: 1.8rem;
	padding: 1.8rem 1rem;
	width: 100%;
	font-family: Roboto;
	line-height: 200%;
	text-align: left;
	background-color: #3c3b54;
	letter-spacing: 0.4rem;
}
.buttons_wrapper {
	display: flex;
	flex-direction: column;
	align-items: flex-start;
	justify-content: flex-start;
	width: 100%;
	.link_tab {
		text-decoration: none;
		padding: 1.6rem 2.4rem;
		width: 100%;
		display: flex;
		color: #ffffff;
		font-family: "Roboto";
		font-size: 1.6rem;
		font-style: normal;
		font-weight: 400;
		line-height: 2.4rem; /* 150% */
	}
	.link_tab .nuxt-icon {
		max-width: 1.2rem;
		margin-right: 0.3rem;
	}
	.link_tab.active {
		border-left: 5px solid #a3a0fb;
		background-color: #3c3b54;
	}
}
</style>
