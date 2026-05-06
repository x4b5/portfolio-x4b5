<script lang="ts">
	type Project = {
		name: string;
		url: string;
		description: string;
		updatedAt: string;
		tech: string[];
	};

	const projects: Project[] = [
		{ name: 'AI Wegwijzer', url: 'https://ai-wegwijzer.vercel.app', description: 'Informatieplatform over kunstmatige intelligentie', updatedAt: '2026-04-12', tech: ['SvelteKit', 'TypeScript', 'OpenAI API', 'Vercel'] },
		{ name: 'Baannana', url: 'https://baannana.vercel.app', description: 'Educatief platform voor docenten en trainers', updatedAt: '2026-05-03', tech: ['SvelteKit', 'TypeScript', 'Vercel'] },
		{ name: 'Babl', url: 'https://babl-xaviers-projects-f781c2fc.vercel.app', description: 'Conversatie-app', updatedAt: '2026-03-25', tech: ['SvelteKit', 'TypeScript', 'Vercel'] },
		{ name: 'Blink', url: 'https://blink-app-mu.vercel.app', description: 'Talentontdekkingsapp', updatedAt: '2026-02-28', tech: ['SvelteKit', 'TypeScript', 'Vercel'] },
		{ name: 'De Grote MS Quiz', url: 'https://ms-eight-olive.vercel.app', description: 'Experimenteel project', updatedAt: '2026-02-22', tech: ['SvelteKit', 'TypeScript', 'Vercel'] },
		{ name: 'PII Scrubber', url: 'https://pii-scrubber-wheat.vercel.app', description: 'Verwijdert persoonsgegevens uit teksten', updatedAt: '2026-04-16', tech: ['SvelteKit', 'TypeScript', 'NLP', 'Vercel'] },
		{ name: 'Quacker', url: 'https://quacker-omega.vercel.app', description: 'Blogs, naslagwerken, cartoons en AI-tools', updatedAt: '2025-11-04', tech: ['SvelteKit', 'TypeScript', 'OpenAI API', 'Vercel'] },
		{ name: 'Image Upscaler', url: 'https://frontend-henna-eight-15.vercel.app/', description: 'Afbeeldingen opschalen met AI', updatedAt: '2026-05-06', tech: ['SvelteKit', 'TypeScript', 'AI Upscaling', 'Vercel'] },
		{ name: 'SHIFT Game', url: 'https://shift-game-taupe.vercel.app', description: 'Geheugenspel met patronen op een glitchend raster', updatedAt: '2026-05-05', tech: ['SvelteKit', 'TypeScript', 'Canvas API', 'Vercel'] },
		{ name: 'Vragen Agids', url: 'https://vragen-agids.vercel.app', description: 'Vragenlijst-app', updatedAt: '2026-04-22', tech: ['SvelteKit', 'TypeScript', 'Vercel'] },
	];

	let expanded = $state<string | null>(null);

	function toggle(name: string) {
		expanded = expanded === name ? null : name;
	}

	type SortMode = 'a-z' | 'z-a' | 'newest' | 'oldest';

	const sortOptions: { value: SortMode; label: string }[] = [
		{ value: 'a-z', label: 'A \u2192 Z' },
		{ value: 'z-a', label: 'Z \u2192 A' },
		{ value: 'newest', label: 'Nieuwste eerst' },
		{ value: 'oldest', label: 'Oudste eerst' },
	];

	let sortMode = $state<SortMode>('a-z');

	const sorted = $derived.by(() => {
		switch (sortMode) {
			case 'a-z':
				return projects.toSorted((a, b) => a.name.localeCompare(b.name, 'nl'));
			case 'z-a':
				return projects.toSorted((a, b) => b.name.localeCompare(a.name, 'nl'));
			case 'newest':
				return projects.toSorted((a, b) => b.updatedAt.localeCompare(a.updatedAt));
			case 'oldest':
				return projects.toSorted((a, b) => a.updatedAt.localeCompare(b.updatedAt));
		}
	});

	const projectColors: string[] = [
		'#3b82f6', // blauw
		'#8b5cf6', // paars
		'#f59e0b', // amber
		'#10b981', // groen
		'#ec4899', // roze
		'#ef4444', // rood
		'#06b6d4', // cyaan
		'#f97316', // oranje
		'#84cc16', // limoen
		'#14b8a6', // teal
		'#a855f7', // violet
		'#e11d48', // karmijn
	];

	function avatarBg(index: number): string {
		return projectColors[index % projectColors.length];
	}

	function formatDate(dateStr: string): string {
		const date = new Date(dateStr);
		return date.toLocaleDateString('nl-NL', { day: 'numeric', month: 'short', year: 'numeric' });
	}
</script>

<svelte:head>
	<title>x4b5 - Portfolio</title>
	<meta name="description" content="Portfolio van Xavier Buise - projecten en experimenten" />
</svelte:head>

<main>
	<header>
		<h1>x4b5</h1>
		<p class="subtitle">projecten</p>
	</header>

	<div class="toolbar">
		<div class="sort-bar">
			{#each sortOptions as opt}
				<button class:active={sortMode === opt.value} onclick={() => (sortMode = opt.value)}>
					{opt.label}
				</button>
			{/each}
		</div>
	</div>

	<div class="grid">
		{#each sorted as project, i}
			{@const isOpen = expanded === project.name}
			<div class="card-wrapper">
				<button class="card" class:expanded={isOpen} onclick={() => toggle(project.name)}>
					<span class="avatar" style="background: {avatarBg(i)}">{project.name[0]}</span>
					<div class="info">
						<span class="name">{project.name}</span>
						<span class="description">{project.description}</span>
					</div>
					<div class="meta">
						<span class="date">{formatDate(project.updatedAt)}</span>
						<span class="chevron" class:open={isOpen}>&#9660;</span>
					</div>
				</button>
				{#if isOpen}
					<div class="details">
						<div class="tech-list">
							{#each project.tech as tag}
								<span class="tech-tag">{tag}</span>
							{/each}
						</div>
						<a href={project.url} target="_blank" rel="noopener noreferrer" class="visit-link">
							Bekijk project &rarr;
						</a>
					</div>
				{/if}
			</div>
		{/each}
	</div>

	{#if sorted.length === 0}
		<p class="empty">Geen projecten gevonden.</p>
	{/if}
</main>

<style>
	main {
		max-width: 720px;
		margin: 0 auto;
		padding: 4rem 1.5rem;
	}

	header {
		margin-bottom: 2rem;
	}

	h1 {
		font-size: 1.5rem;
		font-weight: 600;
		letter-spacing: -0.02em;
	}

	.subtitle {
		color: var(--text-muted);
		font-size: 0.875rem;
		margin-top: 0.25rem;
	}

	.toolbar {
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
		margin-bottom: 1.5rem;
	}

	.sort-bar {
		display: flex;
		flex-wrap: wrap;
		gap: 0.25rem;
	}

	.sort-bar button {
		background: transparent;
		border: 1px solid var(--border);
		color: var(--text-muted);
		font-family: inherit;
		font-size: 0.8125rem;
		font-weight: 500;
		padding: 0.375rem 0.625rem;
		border-radius: 6px;
		cursor: pointer;
		transition: all 0.15s ease;
	}

	.sort-bar button:hover {
		color: var(--text);
		border-color: var(--text-muted);
	}

	.sort-bar button.active {
		background: var(--text);
		color: var(--bg);
		border-color: var(--text);
	}

	.grid {
		display: flex;
		flex-direction: column;
		gap: 1px;
		background: var(--border);
		border: 1px solid var(--border);
		border-radius: 12px;
		overflow: hidden;
	}

	.card-wrapper {
		background: var(--surface);
	}

	.card {
		display: grid;
		grid-template-columns: 36px 1fr auto;
		align-items: center;
		gap: 1rem;
		padding: 1rem 1.25rem;
		background: var(--surface);
		color: var(--text);
		text-decoration: none;
		transition: background 0.15s ease;
		border: none;
		font-family: inherit;
		width: 100%;
		cursor: pointer;
		text-align: left;
	}

	.card:hover {
		background: var(--surface-hover);
	}

	.avatar {
		width: 36px;
		height: 36px;
		border-radius: 8px;
		color: #fff;
		font-size: 0.875rem;
		font-weight: 600;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-shrink: 0;
	}

	.info {
		display: flex;
		flex-direction: column;
		gap: 0.2rem;
		min-width: 0;
	}

	.name {
		font-size: 0.9375rem;
		font-weight: 500;
	}

	.description {
		font-size: 0.8125rem;
		color: var(--text-muted);
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.meta {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		flex-shrink: 0;
	}

	.date {
		font-size: 0.75rem;
		color: var(--text-muted);
		white-space: nowrap;
	}

	.chevron {
		color: var(--text-muted);
		font-size: 0.625rem;
		transition: transform 0.2s ease, color 0.15s ease;
	}

	.chevron.open {
		transform: rotate(180deg);
	}

	.card:hover .chevron {
		color: var(--text);
	}

	.details {
		padding: 0 1.25rem 1rem;
		padding-left: calc(36px + 1.25rem + 1rem);
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
		animation: slideDown 0.15s ease;
	}

	@keyframes slideDown {
		from {
			opacity: 0;
			transform: translateY(-4px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	.tech-list {
		display: flex;
		flex-wrap: wrap;
		gap: 0.375rem;
	}

	.tech-tag {
		font-size: 0.75rem;
		padding: 0.25rem 0.5rem;
		border-radius: 4px;
		background: var(--border);
		color: var(--text-muted);
		font-weight: 500;
	}

	.visit-link {
		font-size: 0.8125rem;
		color: var(--text);
		text-decoration: none;
		font-weight: 500;
		display: inline-flex;
		align-items: center;
		gap: 0.25rem;
		transition: opacity 0.15s ease;
	}

	.visit-link:hover {
		opacity: 0.7;
	}

	.empty {
		text-align: center;
		color: var(--text-muted);
		font-size: 0.875rem;
		padding: 3rem 0;
	}

	@media (max-width: 480px) {
		main {
			padding: 2.5rem 1rem;
		}

		.description {
			white-space: normal;
		}

		.details {
			padding-left: 1.25rem;
		}
	}
</style>
