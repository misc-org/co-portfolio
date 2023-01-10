<script lang="ts">
	import { isLoading } from '$lib/model/store';
	import { onMount } from 'svelte';

	onMount(() => {
		isLoading.set(false);
	});
</script>

<div class="wide_title">
	<p>みんなで作るポートフォリオ<br />”co-portfolio”</p>
</div>

<style lang="scss">
	.main {
		padding: 0 100px;
		max-width: 1400px;
		margin: 0 auto;

		@media screen and (max-width: 860px) {
			padding: 0 20px;
		}
	}

	.back-to-top {
		opacity: 1;
		transition: opacity 0.5s, visibility 0.5s;
		position: fixed;
		z-index: 99;
		right: 20px;
		user-select: none;
		bottom: 20px;
	}

	.back-to-top.hidden {
		opacity: 0;
		visibility: hidden;
	}
</style>
