<script lang="ts">
	import Nav from '$lib/components/Nav.svelte';
	import type { PageData } from './$types';
	import type { Visit } from '$lib/api/types';
	import { formatOpeningHours } from '$lib/utils';

	interface Props {
		data: PageData;
	}
	let { data }: Props = $props();
	const visit: Visit = data.visit;

	const formattedHours = formatOpeningHours(visit.hours || []);
</script>

<main>
	<Nav />
	<section class="content">
		<div class="left">
			<div class="hours">
				<p class="title">Opening Hours</p>
				{#if formattedHours}
					{@html formattedHours
						.split('\n')
						.map((line) => `<p>${line}</p>`)
						.join('')}
				{:else}
					<p>Hours not available</p>
				{/if}
			</div>
			<div class="location">
				<p class="title">Location</p>
				<address>{visit.address || 'Address not available'}</address>
			</div>
		</div>
		<div class="middle">
			<div class="images">
				{#if visit.photography && visit.photography.length > 0}
					{#each visit.photography as photo}
						<img src={photo.url} alt={photo.alt || 'Gallery image'} />
					{/each}
				{/if}
			</div>
		</div>
		<div class="right">
			<div class="email">
				<p class="title">Email</p>
				<p>Contact: <a href="mailto:{visit.email}">{visit.email || 'Email not available'}</a></p>
			</div>
			<div class="social">
				<p class="title">Social Media</p>
				{#if visit.socials && visit.socials.length > 0}
					<ul>
						{#each visit.socials as social}
							<li>
								<a href={social.url} target="_blank">{social.platform}</a>
							</li>
						{/each}
					</ul>
				{:else}
					<p>Social media links not available</p>
				{/if}
			</div>
		</div>
	</section>
</main>

<style>
	main {
		padding-block-start: var(--nav-height);
	}

	section.content {
		max-width: var(--content-width);
		margin: 0 auto;
		padding-block-end: var(--space-2);
		display: grid;
		grid-template-columns: 0.5fr 1fr 0.5fr;
		column-gap: var(--space-8);
		font-size: var(--font-size-md);
		font-family: var(--font-primary);
		/* font-family: var(--font-secondary); */
	}

	.left,
	.middle .images,
	.right {
		display: flex;
		flex-direction: column;
		gap: var(--space-8);
	}

	.middle .images img {
		border-radius: var(--radius-xl);
	}

	address {
		font-style: normal;
	}

	p.title {
		font-family: var(--font-secondary);
		text-transform: uppercase;
		margin-bottom: var(--space-2);
		font-size: var(--font-size-lg);
	}

	@media (max-width: 768px) {
		section.content {
			max-width: 90vw;
		}
	}
</style>
