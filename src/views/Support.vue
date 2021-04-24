<template>
	<main class="support">
		<h1>Support</h1>
		<p>Please select your issue below. If you cannot find your issue, contact us.</p>

		<h1 id="contact">Contact us</h1>
		<p><a href="https://discord.gg/hhaWXq4fmt" target="_blank" rel="noopener">Join our Official Discord</a></p>
	</main>
</template>

<style scoped>
address {
	font-family: monospace;

	&:not(:last-of-type) {
		margin-bottom: 1.5em;
	}
}
</style>

<script lang="ts">
import { Options, Vue } from "vue-class-component";

@Options({})
export default class Copyright extends Vue {
	PGP = process.env.VUE_APP_PGP;
}
</script>
