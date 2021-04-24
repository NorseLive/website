<template>
	<main class="home">
		<h1>Norse</h1>
		<p>A community driven free to play mmorpg where the community plays a big part in shaping the game's future. We threw aside the traditional mmo and made it for gamers by gamers..</p>
		<div class="read-more">
			<router-link :to="{ name: 'about' }">Read More >></router-link>
		</div>

		<h1>Recent News</h1>
		<News :count="1"/>
		<div class="read-more">
			<router-link :to="{ name: 'news' }">More News >></router-link>
		</div>
	</main>
</template>

<style scoped>
.read-more {
	text-align: right;
	padding-right: 8px;

	& a, & a:visited {
		color: #2f2e32;
		text-decoration: none;
		font-weight: bold;
		font-size: 0.8em;

		&:hover {
			text-decoration: underline;
		}
	}
}
</style>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import News from "@/components/News.vue";

@Options({
	components: {
		News,
	},
})
export default class Home extends Vue {}
</script>
