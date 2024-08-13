<script>
	import { onMount } from 'svelte';
	import gsap from 'gsap';
	import { PrismicImage, PrismicText, SliceZone } from '@prismicio/svelte';

	import { components } from '$lib/slices';
	import Bounded from '$lib/components/Bounded.svelte';
	import TriangleGrid from '$lib/components/TriangleGrid.svelte';

	export let data;

	onMount(() => {
		const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce').matches;

		if (prefersReducedMotion) {
			gsap.set('.case-study__image', { opacity: 1 });
			return;
		}

		gsap.fromTo(
			'.case-study__image',
			{
				opacity: 0,
				y: 100
			},
			{
				opacity: 1,
				y: 0,
				duration: 1,
				delay: 0.5,
				ease: 'power2.inOut'
			}
		);
	});
	
</script>

<Bounded>
	<div class="relative grid w-full text-center place-items-center">
		<TriangleGrid />
		<h1 class="text-5xl font-medium md:text-7xl">
			<PrismicText field={data.page.data.company} />
			<span class="block text-lg text-yellow-500">Case Study</span>
		</h1>
		<p class="max-w-xl mt-8 mb-4 text-lg text-slate-300">
			<PrismicText field={data.page.data.description} />
		</p>
		<PrismicImage field={data.page.data.image} class="rounded-lg opacity-0 case-study__image" />
	</div>

	<div class="mx-auto mt-12 md:mt-16">
		<SliceZone slices={data.page.data.slices} {components} />
	</div>
</Bounded>
