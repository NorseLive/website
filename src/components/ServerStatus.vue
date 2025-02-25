<template>
	<aside>
		<a v-if="Online && Players" title="View List" aria-label="view list of online players" target="_blank" rel="noopener" href="https://server.norse.live">Online: {{Players}} players</a>
		<a v-if="Online && !Players" title="View List" aria-label="view list of online players" target="_blank" rel="noopener" href="https://server.norse.live">Online</a>
		<a v-if="!Online" class="offline" title="???" aria-label="open a YouTube video" target="_blank" rel="noopener" href="https://www.youtube-nocookie.com/embed/gnyW6uaUgk4?autoplay=1&amp;modestbranding=1">Offline</a>
	</aside>
</template>

<style scoped>
aside :any-link {
	text-decoration: none;
	color: #0E490B;
	display: block;
	padding: 8px;

	&.offline {
		color: #810909;

		&:is(:hover, :focus)::before {
			content: "⚠️";
			padding-right: 1em;
			opacity: .6;
		}

		&:is(:hover, :focus):after {
			content: "⚠️";
			padding-left: 1em;
			opacity: .6;
		}
	}
}
</style>

<script lang="ts">
import { Options, Vue } from "vue-class-component";

interface StatusResponse {
	serverStatus: string;
	playersOnline?: number;
}

@Options({})
export default class ServerStatus extends Vue {
	Players = 0;
	Online = true;

	private async getStatus () {
		const req = new Request(`${process.env.VUE_APP_API}/tmwa/server`, {
			mode: "cors",
			referrer: "no-referrer",
		});

		try {
			const rawResponse = await fetch(req);
			const data: StatusResponse = await rawResponse.json();

			this.Online = data.serverStatus === "Online";
			this.Players = data.playersOnline || 0;

			if (Reflect.has(self, "localStorage")) {
				localStorage.setItem("onlinePlayers", `${this.Players}`);
				localStorage.setItem("serverOnline", this.Online ? "true": "false");
			}
		} catch (err) {
			// API unreachable (assume it's offline as you cannot register)
			this.Online = false;
		}

		setTimeout(this.getStatus, 8000);
	}

	mounted () {
		// use the last cached value to populate prior to first fetch:
		if (Reflect.has(self, "localStorage")) {
			this.Players = +(localStorage.getItem("onlinePlayers") || 99);
			this.Online = !!(localStorage.getItem("serverOnline") || true);
		} else {
			this.Players = 99;
		}

		this.getStatus();
	}
}
</script>
