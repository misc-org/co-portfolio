<script lang="ts">
	import type TopAppBarComponentDev from '@smui/top-app-bar';
	import TopAppBar, { Row, Section, Title, AutoAdjust } from '@smui/top-app-bar';
	import IconButton from '@smui/icon-button';
	import { AppContent } from '@smui/drawer';
	import { currentPath, isLandscape, isLoading } from '$lib/model/store';
	import { onMount } from 'svelte';
	import PageTransition from '$lib/components/page-transition.svelte';
	import LinearProgress from '@smui/linear-progress';
	import Splash from '$lib/components/splash.svelte';
	import { isLandscapeDetect, PathId, runTransition } from '$lib/model/constants';
	import type { PageData } from './$types';

	export let data: PageData;

	let topAppBar: TopAppBarComponentDev;
	let hasAppMounted = false;

	onMount(async () => {
		currentPath.set(new URL(location.href).pathname);
		hasAppMounted = true;
		updateSize();

		// callback windows width event
		window.addEventListener('resize', updateSize);
	});

	function updateSize(): void {
		$isLandscape = isLandscapeDetect();
		let path = new URL(location.href).pathname;
		if (path === '/') return;
	}
</script>

<div style={`cursor: ${$isLoading ? 'progress' : 'normal'};`}>
	{#if hasAppMounted}
		<TopAppBar bind:this={topAppBar} variant="fixed">
			<Row>
				<Section>
					<IconButton class="material-icons-outlined" on:click={() => runTransition(PathId.HOME)}>
						person_pin</IconButton
					>
					<Title><strong>みんなで作るポートフォリオ</strong></Title>
				</Section>
			</Row>

			{#if $isLoading}
				<div class="progress-bar">
					<LinearProgress indeterminate />
				</div>
			{/if}
		</TopAppBar>

		<AutoAdjust {topAppBar}>
			<AppContent class="app-content">
				<PageTransition {data}>
					<slot />
				</PageTransition>
			</AppContent>
		</AutoAdjust>
	{:else}
		<Splash />
	{/if}
</div>

<style lang="scss">
	:global(.app-content) {
		overflow-y: visible;
		height: 100%;
		width: 100%;
	}

	:global(
			.mdc-circular-progress__determinate-circle,
			.mdc-circular-progress__indeterminate-circle-graphic
		) {
		stroke: var(--m3-on-primary);
	}
	:global(.app-content) {
		overflow-y: visible;
		overflow-x: hidden;
	}

	.progress-bar {
		position: fixed;
		width: 100%;
		top: 0;
		z-index: 999;
	}
</style>
