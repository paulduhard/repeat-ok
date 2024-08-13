<script lang="ts">
	import type { Content } from '@prismicio/client';
	import LogoBackground from './LogoBackground.svelte';
	import background from './background.jpg';
	import Bounded from '$lib/components/Bounded.svelte';
	import StylizedLogoMark from './StylizedLogoMark.svelte';
	import { PrismicRichText, PrismicText } from '@prismicio/svelte';
	import clsx from 'clsx';
	import IconNpm from '~icons/fa6-brands/npm';
	import IconCloudflare from '~icons/fa6-brands/cloudflare';
	import IconGithub from '~icons/fa6-brands/github';
	import IconFigma from '~icons/fa6-brands/figma';
	import IconDigitalOcean from '~icons/fa6-brands/digital-ocean';
	import IconFly from '~icons/fa6-brands/fly';

	export let slice: Content.IntegrationsSlice;

	const icons = {
		npm: IconNpm,
		cloudflare: IconCloudflare,
		github: IconGithub,
		figma: IconFigma,
		digitalocean: IconDigitalOcean,
		fly: IconFly
	};
</script>

<Bounded
	data-slice-type={slice.slice_type}
	data-slice-variation={slice.variation}
	class="relative overflow-hidden"
>
	<img src={background} alt="" class="absolute inset-0 h-full w-full object-cover" />
	<LogoBackground />

	<div class="relative">
		<h2
			class="mx-auto max-w-2xl text-balance bg-gradient-to-b from-violet-50 to-violet-300 bg-clip-text py-2 text-center text-5xl font-medium text-transparent md:text-7xl"
		>
			<PrismicText field={slice.primary.heading} />
		</h2>

		<div class="mx-auto mt-6 max-w-md text-balance text-center text-gray-300">
			<PrismicRichText field={slice.primary.body} />
		</div>

		<div class="mt-20 flex flex-col items-center md:flex-row">
			{#each slice.primary.items as item, index}
				{#if item.icon}
					{#if index === Math.floor(slice.primary.items.length / 2)}
						<StylizedLogoMark />
						<div class="signal-line rotate-180"></div>
					{/if}
					<div
						class="pulsing-icon flex aspect-square shrink-0 items-center justify-center rounded-full border border-violet-500/30 bg-violet-50/25 p-3 text-3xl text-violet-100 opacity-40 md:text-3xl lg:text-5xl"
					>
						<svelte:component this={icons[item.icon]} />
					</div>
					{#if index !== slice.primary.items.length - 1}
						<div
							class={clsx(
								'signal-line',
								index >= Math.floor(slice.primary.items.length / 2) ? 'rotate-180' : 'rotate-0 '
							)}
						></div>
					{/if}
				{/if}
			{/each}
		</div>
	</div>
</Bounded>
