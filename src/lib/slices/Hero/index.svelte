<script lang="ts">
	import { onMount } from 'svelte';
	import gsap from 'gsap';

	import { PrismicImage, PrismicText } from '@prismicio/svelte';

	import Bounded from '$lib/components/Bounded.svelte';
	import ButtonLink from '$lib/components/ButtonLink.svelte';
	import TriangleGrid from '$lib/components/TriangleGrid.svelte';
	import type { Content } from '@prismicio/client';

	export let slice: Content.HeroSlice;

	onMount(() => {

		const prefersReducedMotion = window.matchMedia('(prefers-prefers-reduced-motion: reduce').matches

		if (prefersReducedMotion){
			gsap.set('.hero__heading, .hero__body, .hero__button, .hero__image, .hero__glow', {
				opacity: 1
			})
			return
		}

		const tl = gsap.timeline({ defaults: { ease: 'power2.inOut' } });
	
		tl.fromTo(".hero__heading", {scale:0.5}, {scale:1, duration: 1.4, opacity: 1});
		tl.fromTo(".hero__body", {y: 20}, {y: 0, duration: 1.2, opacity: 1}, "-=0.6");
		tl.fromTo(".hero__button", {scale:1.5}, {scale:1, opacity: 1, duration: 1.3}, "-=0.8");
		tl.fromTo(".hero__image", {y:100}, { y:0, opacity: 1, duration: 1.3}, "+=0.3");
		tl.fromTo(".hero__glow", {scale:.5}, {scale:1, opacity: 1, duration: 1.8}, "-=1");


		gsap.to(".hero__glow--one", {
			ease: 'power2.inOut',
			repeat: -1,
			repeatDelay: 0,
			keyframes: [
				{top: "0%", left: "33%", duration: 0},
				{top: "33%", left: "33%", duration: 2},
				{top: "33%", left: "0%", duration: 3},
				{top: "0%", left: "0%", duration: 2},
				{top: "0%", left: "33%", duration: 3},
			]
		});
		gsap.to(".hero__glow--two", {
			ease: 'power2.inOut',
			repeat: -1,
			repeatDelay: 0,
			keyframes: [
				{top: "33%", left: "0%", duration: 0},
				{top: "0%", left: "0%", duration: 2},
				{top: "0%", left: "33%", duration: 3},
				{top: "33%", left: "33%", duration: 2},
				{top: "33%", left: "0%", duration: 3},
			]
		});
	})
</script>

<Bounded data-slice-type={slice.slice_type} data-slice-variation={slice.variation}>
	<div class="relative text-center">
		<TriangleGrid />
		{#if slice.primary.heading}
			<h1 class="max-w-3xl mx-auto text-5xl font-medium opacity-0 hero__heading text-balance md:text-7xl">
				<PrismicText field={slice.primary.heading} />
			</h1>
		{/if}
		{#if slice.primary.body}
			<p class="mx-auto mt-6 text-gray-300 opacity-0 hero__body maw-w-md text-balance">
				<PrismicText field={slice.primary.body} />
			</p>
		{/if}
		{#if slice.primary.button_link}
			<ButtonLink class="mt-8 opacity-0 hero__button" field={slice.primary.button_link}
				>{slice.primary.button_label}</ButtonLink
			>
		{/if}
		{#if slice.primary.image}
			<div class="mt-16 opacity-0 hero__image glass-container w-fit">
				<div
					class="absolute left-1/3 top-0 -z-10 h-2/3 w-2/3 bg-violet-700/50 mix-blend-screen blur-3xl md:blur-[120px] filter opacity-0 hero__glow hero__glow--one"
				/>
				<div
					class="absolute left-0 top-1/3 -z-10 h-2/3 w-2/3 bg-orange-600/50 mix-blend-screen blur-3xl md:blur-[120px] filter opacity-0 hero__glow hero__glow--two"
				/>

				<PrismicImage class="rounded-lg" field={slice.primary.image} />
			</div>
		{/if}
	</div>
</Bounded>
