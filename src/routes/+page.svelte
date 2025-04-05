<script lang="ts">
	import * as Avatar from '$lib/components/ui/avatar';
	import Separator from '$lib/components/ui/separator/separator.svelte';
	import * as Tabs from '$lib/components/ui/tabs-alt';
	import { Code, Sun, Moon, Twitter, Github } from '@lucide/svelte';

	import { toggleMode } from 'mode-watcher';
	import * as Tooltip from '$lib/components/ui/tooltip';
	import { Button } from '$lib/components/ui/button';
	import Projects from './projects.svelte';
	import Contact from './contact.svelte';
	import About from './about.svelte';
	import Slide from './slide.svelte';
</script>

<svelte:head>
	<title>Another Hadi</title>
	<meta
		name="description"
		content="French developer, working in infosec, enthusiastic about nix, go & blockchains. I deeply value open-source projects & actively seek ways to contribute to them ☁"
	/>
</svelte:head>

<header class="mb-2 mt-10 flex flex-col gap-4 px-10">
	<Avatar.Root class="lg:h-16 lg:w-16">
		<Avatar.Image src="https://github.com/anotherhadi.png" alt="@anotherhadi" />
		<Avatar.Fallback>Hadi</Avatar.Fallback>
	</Avatar.Root>

	<h2 class="mt-0 mb-2 scroll-m-20 text-3xl font-extrabold tracking-tight lg:text-4xl">Another Hadi</h2>
	<p class="md:test-lg text-md max-w-xl font-medium leading-none text-muted-foreground">
		French developer, working in infosec, enthusiastic about nix, go &amp; blockchains. I deeply value open-source projects &amp; actively seek ways to contribute to them ☁
	</p>

	<div class="flex flex-wrap gap-2">
		<Tooltip.Provider>
			<Tooltip.Root>
				<Tooltip.Trigger>
					<a href="https://github.com/anotherhadi">
						<Button variant="ghost" size="icon">
							<Github />
						</Button>
					</a>
				</Tooltip.Trigger>
				<Tooltip.Content>Github Account</Tooltip.Content>
			</Tooltip.Root>
		</Tooltip.Provider>

		<Tooltip.Provider>
			<Tooltip.Root>
				<Tooltip.Trigger>
					<a href="https://x.com/anotherhadi">
						<Button variant="ghost" size="icon"><Twitter /></Button>
					</a>
				</Tooltip.Trigger>
				<Tooltip.Content>X Account</Tooltip.Content>
			</Tooltip.Root>
		</Tooltip.Provider>

		<Tooltip.Provider>
			<Tooltip.Root>
				<Tooltip.Trigger>
					<a href="https://github.com/anotherhadi/portfolio">
						<Button variant="ghost" size="icon">
							<Code />
						</Button>
					</a>
				</Tooltip.Trigger>
				<Tooltip.Content>Source code</Tooltip.Content>
			</Tooltip.Root>
		</Tooltip.Provider>

		<Tooltip.Provider>
			<Tooltip.Root>
				<Tooltip.Trigger>
					<Button onclick={toggleMode} variant="ghost" size="icon">
						<Sun
							class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0"
						/>
						<Moon
							class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100"
						/>
					</Button>
				</Tooltip.Trigger>
				<Tooltip.Content>Light switch</Tooltip.Content>
			</Tooltip.Root>
		</Tooltip.Provider>
	</div>

	<Separator class="my-2 md:my-4" />
</header>

<div class="mb-2 px-10">
	<Tabs.Root value="about">
		<Tabs.List class="mb-4">
			<Tabs.Trigger value="about">About Me</Tabs.Trigger>
			<Tabs.Trigger value="projects">Projects</Tabs.Trigger>
			<Tabs.Trigger value="contact">Contact</Tabs.Trigger>
		</Tabs.List>
		<Tabs.Content value="about">
			<Slide>
				<About />
			</Slide>
		</Tabs.Content>
		<Tabs.Content value="projects">
			<Projects />
		</Tabs.Content>
		<Tabs.Content value="contact">
			<Contact />
		</Tabs.Content>
	</Tabs.Root>
</div>
