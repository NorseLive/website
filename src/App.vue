<template>
	<div v-if="globalStatus" class="dialog" v-html="globalStatus"></div>
	<Logo v-else class="header"/>
	<Navigation class="nav"/>
	<router-view v-if="loaded" class="content"/>
	<main v-else class="content" role="alert" aria-busy="true">
		<h1>&nbsp;</h1>
		<p>Loading...</p>
	</main>
	<Copyright class="footer"/>
</template>

<style>
:root {
	background: gray(95);
}
#app {
	z-index: 100;
	& > .nav {
		grid-area: side;
	}
	& > .header {
		grid-area: logo;
	}
	& > .content {
		grid-area: page;
		background: #E1D6CF;
		padding: 15px 15px 30px 15px;
		border-radius: 15px 15px 0 0;
		text-align: justify;
		& h1 {
			margin: 20px 0 0 0 0;
			font-weight: bold;
			font-size: 1.3em;
			border-bottom: 1px solid #9f9894;
			color: gray(24);
			&:nth-of-type(1n + 2) {
				margin-top: 2em;
			}
		}
	}
	& > .footer {
		grid-area: footer;
	}

	& .dialog {
		background: #ff0000;
		color: #fff;
		padding: 2em 1em 2em 1em;
		display: block;
		font-size: 1.5rem;
		margin: 1rem;
		z-index: 10000;
		grid-area: logo;
		border: 5px gray(95) dotted;
	}

	font-family: sans-serif;
	color: #2c3e50;
	width: 100%;
	max-width: 1100px;
	margin: 0 auto;
	display: grid;
	grid-template-areas:
		"logo"
		"page"
		"side"
		"footer";
}
@media (min-width: 1100px) {
	#app {
		grid-column-gap: 0em;
		grid-row-gap: 0px;
		grid-template-columns: 2fr 4fr;
		grid-template-areas:
			"logo logo"
			"page side"
			"footer footer";
		& > .content {
			min-width: 890px;
			padding-bottom: 200px;
			border-radius: 15px 0 0 15px;
			& p {
				margin: 0px 40px 5px 30px;
			}
		}
	}
}
</style>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Navigation from "@/components/Navigation.vue";
import Logo from "@/components/Logo.vue";
import Copyright from "@/components/Footer.vue";

@Options({
	components: {
		Navigation,
		Logo,
		Copyright,
	},
})
export default class AppV extends Vue {
	loaded = false;

	globalStatus = process.env.VUE_APP_STATUS?.trim() ?? "";

	mounted () {
		self.addEventListener("initial-load", () => {
			this.loaded = true;
		});
	}
}
</script>
