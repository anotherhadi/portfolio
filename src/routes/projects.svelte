<script lang="ts">
	import * as Card from '$lib/components/ui/card';
	import * as Tooltip from '$lib/components/ui/tooltip';
	import { Button } from '$lib/components/ui/button';
	import { ExternalLink, Github, Star } from 'lucide-svelte';
	import * as Carousel from '$lib/components/ui/carousel/index.js';
	import Badge from '$lib/components/ui/badge/badge.svelte';
	import { onMount } from 'svelte';

	interface Project {
		title: string;
		description: string;
		githubRepo: string;
		link?: string;
		logo?: string;
		images?: string[];
		tags?: string[];
		stars?: number;
	}

	async function getStars(githubRepo: string): Promise<number> {
		const response = await fetch(`https://api.github.com/repos/${githubRepo}`);
		const data = await response.json();
		return data.stargazers_count;
	}

	let projects: Project[] = [
		{
			title: 'Nixy',
			description:
				'NixOs dotfiles for a desktop environment. It includes a lot of packages and configurations w/hyprland, home-manager and more.',
			logo: 'https://image.nostr.build/70ebe2b5183879d8c0ffa682cd0089e030ad01909678b242ed86449517eac3a5.png',
			images: [
				'https://image.nostr.build/c208c164ae04e4c58e01adb9fab584b163a565155865833851275f89c9270b7a.jpg',
				'https://image.nostr.build/c208c164ae04e4c58e01adb9fab584b163a565155865833851275f89c9270b7a.jpg'
			],
			githubRepo: 'anotherhadi/nixy',
			tags: ['nixos', 'desktop', 'hyprland', 'home-manager']
		},
		{
			title: 'Portfolio',
			description:
				'My portfolio (the one you are currently on) is a static site built with SvelteKit and TailwindCSS. It showcases my projects, blog posts and more.',
			githubRepo: 'anotherhadi/portfolio',
			tags: ['portfolio', 'sveltekit', 'tailwindcss', 'shadcn/ui']
		},
		{
			title: 'Homepage',
			description:
				'My homepage is a static site built with SvelteKit and TailwindCSS. It allows you to add your own links and customize the theme of your start page.',
			githubRepo: 'anotherhadi/homepage',
			tags: ['homepage', 'sveltekit', 'tailwindcss', 'shadcn/ui']
		},
		{
			title: 'Hyprsettings',
			description:
				'The hyprlandsettings utility is a powerful tool designed for modifying settings within the Hyprland Window Manager (WM) through a Text User Interface (TUI).',
			githubRepo: 'anotherhadi/hyprsettings',
			tags: ['tui', 'hyprland']
		}
	];

	onMount(() => {
		for (let i = 0; i < projects.length; i++) {
			getStars(projects[i].githubRepo).then((stars) => {
				projects[i].stars = stars;
			});
		}
	});
</script>

<h1 class="scroll-m-20 pb-2 text-3xl font-semibold tracking-tight transition-colors first:mt-0">
	Projects
</h1>

<div class="my-10 grid w-full grid-cols-1 justify-around gap-5 md:grid-cols-2">
	{#each projects as project}
		<Card.Root class="w-full transition-shadow duration-300 hover:shadow-lg">
			<Card.Header class="flex-row items-center gap-5">
				{#if project.logo}
					<img
						src={project.logo}
						alt={project.title + ' logo'}
						class="w-10 rounded-md object-cover"
					/>
				{/if}
				<div class="flex flex-col gap-1.5">
					<Card.Title
						><span class="text-muted-foreground">anotherhadi/</span>{project.title}</Card.Title
					>
					<Card.Description>
						{project.description}
					</Card.Description>
				</div>
			</Card.Header>
			{#if project.images}
				{#if project.images.length === 1}
					<Card.Content>
						<img
							src={project.images[0]}
							alt={project.title + ' image: '}
							class="w-full rounded-md object-cover"
						/>
					</Card.Content>
				{:else}
					<Card.Content class="mx-10">
						<Carousel.Root
							opts={{
								loop: true
							}}
						>
							<Carousel.Content>
								{#each project.images as image}
									<Carousel.Item>
										<img
											src={image}
											alt={project.title + ' image: ' + image}
											class="w-full rounded-md object-cover"
										/>
									</Carousel.Item>
								{/each}
							</Carousel.Content>
							<Carousel.Previous />
							<Carousel.Next />
						</Carousel.Root>
					</Card.Content>
				{/if}
			{/if}

			{#if project.tags}
				<Card.Content class="flex flex-wrap gap-2">
					{#each project.tags as tag}
						<Badge variant="outline">{tag}</Badge>
					{/each}
				</Card.Content>
			{/if}
			<Card.Footer class="flex flex-wrap gap-2">
				<Tooltip.Root>
					<Tooltip.Trigger>
						<a href={'https://github.com/' + project.githubRepo}>
							<Button size="icon" variant="outline">
								<Github />
							</Button>
						</a>
					</Tooltip.Trigger>
					<Tooltip.Content>{project.title} github repo</Tooltip.Content>
				</Tooltip.Root>

				<Tooltip.Root>
					<Tooltip.Trigger>
						<a href={'https://github.com/' + project.githubRepo + '/stargazers'}>
							<Button class="flex gap-2" variant="outline">
								<Star />
								{#if project.stars === undefined}
									Loading...
								{:else}
									{project.stars}
								{/if}
							</Button>
						</a>
					</Tooltip.Trigger>
					<Tooltip.Content>{project.title} stars</Tooltip.Content>
				</Tooltip.Root>

				{#if project.link}
					<Tooltip.Root>
						<Tooltip.Trigger>
							<a href={project.link}>
								<Button size="icon" variant="outline">
									<ExternalLink />
								</Button>
							</a>
						</Tooltip.Trigger>
						<Tooltip.Content>{project.title} link</Tooltip.Content>
					</Tooltip.Root>
				{/if}
			</Card.Footer>
		</Card.Root>
	{/each}
</div>
