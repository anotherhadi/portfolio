<script lang="ts">
	import * as Card from '$lib/components/ui/card';
	import * as Tooltip from '$lib/components/ui/tooltip';
	import { Button } from '$lib/components/ui/button';
	import { ExternalLink, Github, Star } from '@lucide/svelte';
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

	async function getDescription(githubRepo: string): Promise<string> {
		const response = await fetch(`https://api.github.com/repos/${githubRepo}`);
		const data = await response.json();
		return data.description;
	}

	let projects: Project[] = [
		{
			title: 'Nixy',
			description:
				'NixOs dotfiles for a desktop environment. It includes a lot of packages and configurations w/hyprland, home-manager and more.',
			logo: 'https://github.com/anotherhadi/nixy/blob/main/.github/assets/logo.png?raw=true',
			images: [
				'https://github.com/anotherhadi/nixy/blob/main/.github/assets/nixy/1.png?raw=true',
				'https://github.com/anotherhadi/nixy/blob/main/.github/assets/nixy/2.png?raw=true',
				'https://github.com/anotherhadi/nixy/blob/main/.github/assets/nixy/3.png?raw=true'
			],
			githubRepo: 'anotherhadi/nixy',
			tags: ['nixos', 'desktop', 'hyprland', 'home-manager']
		},
		{
			title: 'Awesome Wallpapers',
			description: 'A curated collection of high-quality wallpapers, organized by tags.',
			logo: 'https://github.com/anotherhadi/awesome-wallpapers/blob/main/.github/assets/icon.png?raw=true',
			images: [
				'https://github.com/anotherhadi/awesome-wallpapers/blob/main/app/static/wallpapers/sakura_pixelart_light_animated.gif?raw=true',
				'https://github.com/anotherhadi/awesome-wallpapers/blob/main/app/static/wallpapers/cloud-in-the-sky_purple.png?raw=true',
				'https://github.com/anotherhadi/awesome-wallpapers/blob/main/app/static/wallpapers/another-one.png?raw=true'
			],
			githubRepo: 'anotherhadi/awesome-wallpapers',
			tags: ['wallpapers', 'awesome-list'],
			link: 'https://wallpapers.hadi.diy'
		},
		{
			title: 'Search NixOS',
			description:
				'A lightweight search engine for NixOS, Nixpkgs, Home Manager, Nix-darwin, and NUR.',
			logo: 'https://github.com/anotherhadi/search-nixos/blob/main/static/logo.png?raw=true',
			images: ['https://github.com/anotherhadi/search-nixos/blob/main/static/demo.png?raw=true'],
			githubRepo: 'anotherhadi/search-nixos',
			tags: ['nixos', 'nixpkgs', 'home-manager', 'nix-darwin', 'nur'],
			link: 'https://search-nixos.hadi.diy'
		},
		{
			title: 'Portfolio',
			logo: 'https://github.com/anotherhadi/portfolio/blob/main/static/logo.png?raw=true',
			description:
				'My portfolio (the one you are currently on) is a static site built with SvelteKit and TailwindCSS. It showcases my projects, blog posts and more.',
			images: ['https://github.com/anotherhadi/portfolio/blob/main/static/image.png?raw=true'],
			githubRepo: 'anotherhadi/portfolio',
			tags: ['portfolio', 'sveltekit', 'tailwindcss', 'shadcn/ui'],
			link: 'https://hadi.diy'
		},
		{
			title: 'Search NixOS API',
			description:
				'The Search NixOS API is a service designed to facilitate the search and retrieval of options across various Nix-related projects.',
			logo: 'https://github.com/anotherhadi/search-nixos/blob/main/static/logo.png?raw=true',
			githubRepo: 'anotherhadi/search-nixos-api',
			tags: ['nixos', 'nixpkgs', 'home-manager', 'nix-darwin', 'nur', 'api'],
			link: 'https://search-nixos-api.hadi.diy'
		},
		{
			title: 'Nix JSON',
			description:
				'The Nix JSON project is a collection of JSON files that provide a structured representation of various Nix-related projects, including Nixpkgs, Home Manager, and more.',
			githubRepo: 'anotherhadi/nix-json',
			tags: ['nixos', 'nixpkgs', 'home-manager', 'nix-darwin', 'nur', 'json']
		},
		{
			title: 'Homepage',
			logo: 'https://github.com/anotherhadi/homepage/blob/main/docs/src/logo.png?raw=true',
			images: [
				'https://github.com/anotherhadi/homepage/blob/main/docs/src/screenshot.png?raw=true'
			],
			description:
				'My homepage is a static site built with SvelteKit and TailwindCSS. It allows you to add your own links and customize the theme of your start page.',
			githubRepo: 'anotherhadi/homepage',
			tags: ['homepage', 'sveltekit', 'tailwindcss', 'shadcn/ui']
		},
		{
			title: 'Hyprsettings',
			images: ['https://github.com/anotherhadi/hyprsettings/raw/main/src/hyprsettings.gif'],
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
			getDescription(projects[i].githubRepo).then((description) => {
				projects[i].description = description;
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
				<Tooltip.Provider>
					<Tooltip.Root>
						<Tooltip.Trigger>
							<Button
								size="icon"
								variant="outline"
								href={`https://github.com/${project.githubRepo}`}
								target="_blank"
							>
								<Github />
							</Button>
						</Tooltip.Trigger>
						<Tooltip.Content>{project.title} github repo</Tooltip.Content>
					</Tooltip.Root>
				</Tooltip.Provider>
				<Tooltip.Provider>
					<Tooltip.Root>
						<Tooltip.Trigger>
							<Button
								class="flex gap-2"
								variant="outline"
								href={`https://github.com/${project.githubRepo}/stargazers`}
								target="_blank"
							>
								<Star />
								{#if project.stars === undefined}
									Loading...
								{:else}
									{project.stars}
								{/if}
							</Button>
						</Tooltip.Trigger>
						<Tooltip.Content>{project.title} stars</Tooltip.Content>
					</Tooltip.Root>
				</Tooltip.Provider>

				{#if project.link}
					<Tooltip.Provider>
						<Tooltip.Root>
							<Tooltip.Trigger>
								<Button size="icon" variant="outline" href={project.link} target="_blank">
									<ExternalLink />
								</Button>
							</Tooltip.Trigger>
							<Tooltip.Content>{project.title} link</Tooltip.Content>
						</Tooltip.Root>
					</Tooltip.Provider>
				{/if}
			</Card.Footer>
		</Card.Root>
	{/each}
</div>
