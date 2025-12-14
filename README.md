<script>
	import { fade } from 'svelte/transition';
	import FadeUp from '$lib/components/FadeUp.svelte';
	import ScrollReveal from '$lib/components/ScrollReveal.svelte';
	import * as m from '$lib/paraglide/messages.js';
	import { i18n } from '$lib/i18n.js';

	// Lazy load heavy components for better initial page load
	const ArticleCard = import('$lib/components/ArticleCard.svelte');
	const NewsLetterSignup = import('$lib/components/NewsLetterSignup.svelte');

	// Import TechIcon component for SVG sprite
	import TechIcon from '$lib/components/TechIcon.svelte';

	// Import des icônes
	import {
		FileText,
		Folder,
		Eye,
		MessageCircle,
		Code,
		ArrowRight,
		User,
		CheckCircle2,
		Github,
		Twitter,
		Linkedin,
		Clock,
		Mail,
		TrendingUp,
		FileX
	} from 'lucide-svelte';

	let { data } = $props();

	// Optimized: Use message functions directly instead of wrapping in $derived
	// This avoids creating a large reactive object that recalculates on every change
	const t = m;

	// Photo depuis static pour éviter les problèmes d'hydratation
	const photoSmall = '/images/photo-small.webp';

	// Stats calculées
	const stats = $derived({
		articles: data.posts?.length || 0,
		categories: data.categories?.length || 0,
		views: data.posts?.reduce((sum, post) => sum + (post.viewCount || 0), 0) || 0,
		comments: data.totalComments || 0
	});

	// Technologies - using SVG sprite for better performance (9 requests → 1)
	const technologies = [
		'Python',
		'JavaScript',
		'Vue.js',
		'Django',
		'Svelte',
		'Tailwind',
		'Flask',
		'Flutter',
		'Linux'
	];
</script>

<svelte:head>
	<title>{t['home.metaTitle']()}</title>
	<meta name="description" content={t['home.metaDescription']()} />
</svelte:head>

<!-- Hero Section -->
<section
	class="relative overflow-hidden bg-gradient-to-br from-violet-600 via-purple-700 to-indigo-800 py-20 text-white sm:py-28 dark:from-violet-900 dark:via-purple-900 dark:to-indigo-950"
>
	<!-- Motif de fond décoratif -->
	<div class="absolute inset-0 opacity-10">
		<div
			class="absolute top-8 left-6 h-48 w-48 rounded-full bg-white blur-3xl sm:h-72 sm:w-72"
		></div>
		<div
			class="absolute right-6 bottom-8 h-64 w-64 rounded-full bg-pink-300 blur-3xl sm:h-96 sm:w-96"
		></div>
	</div>

	<div class="relative z-10 mx-auto max-w-6xl px-6">
		<div class="flex flex-col gap-6 text-center">
			<!-- Badge avec animation -->
			<FadeUp delay={150}>
				{#snippet children()}
					<div
						class="mx-auto inline-flex items-center gap-2 rounded-full border border-white/20 bg-white/10 px-4 py-2 backdrop-blur-sm transition-colors hover:bg-white/15"
					>
						<span class="relative flex h-2 w-2">
							<span
								class="absolute inline-flex h-full w-full animate-ping rounded-full bg-emerald-400 opacity-75"
							></span>
							<span class="relative inline-flex h-2 w-2 rounded-full bg-emerald-500"></span>
						</span>
						<span class="text-sm font-medium">{t['home.availableForProjects']()}</span>
					</div>
				{/snippet}
			</FadeUp>

			<!-- Titre principal -->
			<FadeUp delay={250}>
				{#snippet children()}
					<h1 class="text-4xl leading-tight font-extrabold tracking-tight sm:text-5xl md:text-7xl">
						{t['home.welcomeTo']()}
						<span
							class="bg-gradient-to-r from-white via-pink-200 to-violet-200 bg-clip-text text-transparent"
							>codewithmpia</span
						>
					</h1>
				{/snippet}
			</FadeUp>

			<!-- Description -->
			<FadeUp delay={350}>
				{#snippet children()}
					<p class="mx-auto max-w-2xl text-lg leading-relaxed text-violet-100 md:text-xl">
						{t['home.heroDescription']()}
					</p>
				{/snippet}
			</FadeUp>

			<!-- Stats rapides - 4 stats comme l'original -->
			<FadeUp delay={450}>
				{#snippet children()}
					<div
						class="mx-auto grid max-w-sm grid-cols-2 items-center justify-center gap-2 text-sm sm:flex sm:max-w-none sm:flex-wrap sm:gap-3 md:gap-4"
					>
						<div
							class="flex shrink-0 items-center gap-1.5 rounded-lg bg-white/10 px-2.5 py-1.5 backdrop-blur-sm sm:px-3 sm:py-2"
						>
							<FileText class="h-4 w-4 shrink-0 sm:h-5 sm:w-5" />
							<span class="text-sm font-bold sm:text-base">{stats.articles}+</span>
							<span class="text-[10px] whitespace-nowrap text-violet-100 sm:text-xs"
								>{t['home.articles']()}</span
							>
						</div>
						<div
							class="flex shrink-0 items-center gap-1.5 rounded-lg bg-white/10 px-2.5 py-1.5 backdrop-blur-sm sm:px-3 sm:py-2"
						>
							<Folder class="h-4 w-4 shrink-0 sm:h-5 sm:w-5" />
							<span class="text-sm font-bold sm:text-base">{stats.categories}+</span>
							<span class="text-[10px] whitespace-nowrap text-violet-100 sm:text-xs">
								<span class="sm:hidden">cat.</span>
								<span class="hidden sm:inline">{t['home.categories']()}</span>
							</span>
						</div>
						<div
							class="flex shrink-0 items-center gap-1.5 rounded-lg bg-white/10 px-2.5 py-1.5 backdrop-blur-sm sm:px-3 sm:py-2"
						>
							<Eye class="h-4 w-4 shrink-0 sm:h-5 sm:w-5" />
							<span class="text-sm font-bold sm:text-base">{stats.views}+</span>
							<span class="text-[10px] whitespace-nowrap text-violet-100 sm:text-xs"
								>{t['home.views']()}</span
							>
						</div>
						<div
							class="flex shrink-0 items-center gap-1.5 rounded-lg bg-white/10 px-2.5 py-1.5 backdrop-blur-sm sm:px-3 sm:py-2"
						>
							<MessageCircle class="h-4 w-4 shrink-0 sm:h-5 sm:w-5" />
							<span class="text-sm font-bold sm:text-base">{stats.comments}+</span>
							<span class="text-[10px] whitespace-nowrap text-violet-100 sm:text-xs">
								<span class="sm:hidden">comm.</span>
								<span class="hidden sm:inline">{t['home.comments']()}</span>
							</span>
						</div>
					</div>
				{/snippet}
			</FadeUp>

			<!-- Boutons CTA -->
			<FadeUp delay={550}>
				{#snippet children()}
					<div class="mt-6 flex w-full flex-col items-center justify-center gap-4 sm:flex-row">
						<a
							href="/articles"
							class="inline-flex w-full items-center justify-center gap-2 rounded-xl bg-white px-6 py-3 text-sm font-bold text-violet-700 shadow-xl transition-all hover:scale-105 hover:shadow-2xl sm:w-auto sm:px-8 sm:py-4 sm:text-base"
						>
							<FileText class="h-5 w-5" />
							{t['home.exploreArticles']()}
						</a>
						<a
							href="/about"
							class="w-full rounded-xl border-2 border-white/30 bg-white/10 px-6 py-3 text-center text-sm font-bold text-white backdrop-blur-sm transition-all hover:bg-white/20 sm:w-auto sm:px-8 sm:py-4 sm:text-base"
						>
							{t['home.learnMore']()}
						</a>
					</div>
				{/snippet}
			</FadeUp>
		</div>
	</div>
</section>

<!-- Technologies / Stack Section -->
<section class="bg-white py-20 dark:bg-gray-950">
	<div class="mx-auto max-w-6xl px-6">
		<ScrollReveal delay={0}>
			{#snippet children()}
				<div class="mb-12 text-center">
					<div
						class="mb-4 inline-flex items-center gap-2 rounded-full border border-violet-200 bg-violet-50 px-4 py-2 dark:border-violet-800 dark:bg-violet-900/20"
					>
						<Code class="h-4 w-4 text-violet-600 dark:text-violet-400" />
						<span class="text-sm font-medium text-violet-700 dark:text-violet-300"
							>{t['home.technologiesTitle']()}</span
						>
					</div>
					<h2 class="mb-3 text-3xl font-bold text-gray-900 md:text-4xl dark:text-gray-100">
						{t['home.techStackTitle']()}
					</h2>
					<p class="mx-auto max-w-2xl text-gray-600 dark:text-gray-400">
						{t['home.techStackDescription']()}
					</p>
				</div>
			{/snippet}
		</ScrollReveal>

		<div class="grid grid-cols-2 gap-4 sm:gap-6 md:grid-cols-3">
			{#each technologies as tech, i}
				<ScrollReveal delay={i * 50}>
					{#snippet children()}
						<div
							class="group relative transform rounded-2xl border-2 border-gray-200 bg-gradient-to-br from-gray-50 to-white p-6 transition-all hover:-translate-y-1 hover:border-violet-400 hover:shadow-xl dark:border-gray-700 dark:from-gray-800 dark:to-gray-900 dark:hover:border-violet-500"
						>
							<div class="flex flex-col items-center gap-3">
								<div class="flex h-16 w-16 items-center justify-center">
									<TechIcon name={tech} class="h-12 w-12" />
								</div>
								<span
									class="text-sm font-bold text-gray-900 transition-colors group-hover:text-violet-600 dark:text-gray-100 dark:group-hover:text-violet-400"
								>
									{tech}
								</span>
							</div>
						</div>
					{/snippet}
				</ScrollReveal>
			{/each}
		</div>

		<!-- CTA pour voir les catégories -->
		<ScrollReveal delay={0}>
			{#snippet children()}
				<div class="mt-12 text-center">
					<p class="mb-4 text-gray-600 dark:text-gray-400">
						{t['home.discoverAllArticlesDescription']()}
					</p>
					<a
						href="/articles"
						class="inline-flex items-center gap-2 font-semibold text-violet-600 transition-colors hover:text-violet-700 dark:text-violet-400 dark:hover:text-violet-300"
					>
						{t['home.discoverAllArticles']()}
						<ArrowRight class="h-4 w-4" />
					</a>
				</div>
			{/snippet}
		</ScrollReveal>
	</div>
</section>

<!-- Catégories Section -->
{#if data.categories && data.categories.length > 0}
	<section class="bg-gradient-to-b from-gray-50 to-white py-20 dark:from-gray-900 dark:to-gray-950">
		<div class="mx-auto max-w-6xl px-6">
			<ScrollReveal delay={0}>
				{#snippet children()}
					<div class="mb-12 text-center">
						<h2 class="mb-3 text-3xl font-bold text-gray-900 md:text-4xl dark:text-gray-100">
							{t['home.exploreByCategoryTitle']()}
						</h2>
						<p class="mx-auto max-w-2xl text-gray-600 dark:text-gray-400">
							{t['home.exploreByCategoryDescription']()}
						</p>
					</div>
				{/snippet}
			</ScrollReveal>

			<div class="grid grid-cols-2 gap-4 md:grid-cols-3 md:gap-6 lg:grid-cols-4">
				{#each data.categories as category, i}
					<ScrollReveal delay={i * 80}>
						{#snippet children()}
							<a
								href="/articles/category/{category.slug}"
								class="group relative block transform overflow-hidden rounded-2xl border-2 border-gray-200 bg-white p-6 transition-all hover:-translate-y-1 hover:border-violet-400 hover:shadow-xl dark:border-gray-700 dark:bg-gray-800 dark:hover:border-violet-500 dark:hover:shadow-gray-950"
							>
								<!-- Gradient overlay on hover -->
								<div
									class="absolute inset-0 bg-gradient-to-br from-violet-50 via-purple-50 to-pink-50 opacity-0 transition-opacity group-hover:opacity-100 dark:from-violet-950 dark:via-purple-950 dark:to-pink-950"
								></div>

								<div class="relative z-10">
									<!-- Icon with gradient background -->
									<div
										class="mb-4 flex h-12 w-12 items-center justify-center rounded-xl bg-gradient-to-br from-violet-500 to-purple-600 shadow-lg transition-all group-hover:from-violet-600 group-hover:to-purple-700 group-hover:shadow-xl"
									>
										<Folder class="h-6 w-6 text-white" />
									</div>

									<!-- Category name -->
									<h3
										class="mb-2 text-lg font-bold text-gray-900 transition-colors group-hover:text-violet-700 dark:text-gray-100 dark:group-hover:text-violet-400"
									>
										{category.name}
									</h3>

									<!-- Article count -->
									<div class="flex items-center gap-2 text-sm">
										<div
											class="flex items-center gap-1.5 font-medium text-violet-600 dark:text-violet-400"
										>
											<FileText class="h-4 w-4" />
											<span>{category.postsCount || 0}</span>
										</div>
										<span class="text-gray-500 dark:text-gray-400">
											{category.postsCount > 1 ? t['home.articlesPlural']() : t['home.article']()}
										</span>
									</div>

									<!-- Arrow icon -->
									<div
										class="absolute top-6 right-6 opacity-0 transition-opacity group-hover:opacity-100"
									>
										<ArrowRight class="h-5 w-5 text-violet-600 dark:text-violet-400" />
									</div>
								</div>
							</a>
						{/snippet}
					</ScrollReveal>
				{/each}
			</div>
		</div>
	</section>
{/if}

<!-- About Section -->
<section class="bg-white py-20 dark:bg-gray-950">
	<div class="mx-auto max-w-6xl px-6">
		<div class="grid items-center gap-12 md:grid-cols-2">
			<!-- Image / Avatar -->
			<ScrollReveal delay={0}>
				{#snippet children()}
					<div class="relative mx-auto w-full md:mx-0 md:max-w-[420px]">
						<div
							class="relative transform overflow-hidden rounded-3xl border-4 border-white shadow-2xl transition-transform duration-300 hover:scale-105 dark:border-gray-800 dark:shadow-gray-950"
						>
							<img
								src={photoSmall}
								alt="Mpia M. - Développeur Full-Stack"
								class="aspect-square w-full object-cover"
							/>
						</div>

						<!-- Badge flottant -->
						<div
							class="absolute right-0 -bottom-10 left-0 rounded-2xl border-2 border-violet-100 bg-white p-4 shadow-xl sm:left-6 md:-right-6 md:left-0 dark:border-violet-900 dark:bg-gray-800"
						>
							<div class="flex items-center gap-3">
								<div
									class="hidden h-12 w-12 items-center justify-center rounded-xl bg-gradient-to-br from-violet-500 to-purple-600 sm:flex"
								>
									<Code class="h-6 w-6 text-white" />
								</div>
								<div>
									<div class="text-2xl font-bold text-gray-900 dark:text-gray-100">
										{t['home.fullStackDeveloper']()}
									</div>
									<div class="text-xs text-gray-600 dark:text-gray-400">
										{t['home.passionateAbout']()}
									</div>
								</div>
							</div>
						</div>
					</div>
				{/snippet}
			</ScrollReveal>

			<!-- Contenu texte -->
			<ScrollReveal delay={150}>
				{#snippet children()}
					<div class="mt-8 md:mt-0">
						<div
							class="mb-6 inline-flex items-center gap-2 rounded-full border border-violet-200 bg-violet-50 px-4 py-2 dark:border-violet-800 dark:bg-violet-900/20"
						>
							<User class="h-4 w-4 text-violet-600 dark:text-violet-400" />
							<span class="text-sm font-medium text-violet-700 dark:text-violet-300"
								>{t['home.aboutBadge']()}</span
							>
						</div>

						<h2 class="mb-4 text-3xl font-bold text-gray-900 md:text-4xl dark:text-gray-100">
							{t['home.hiIam']()}
							<span
								class="bg-gradient-to-r from-violet-600 to-purple-600 bg-clip-text text-transparent"
								>Mpia M.</span
							>
						</h2>

						<p class="mb-6 text-lg leading-relaxed text-gray-600 dark:text-gray-400">
							{t['home.aboutDescription']()}
						</p>

						<div class="mb-6 space-y-4">
							{#each [{ title: t['home.skillFullStack'](), desc: t['home.skillFullStackDesc']() }, { title: t['home.skillTeaching'](), desc: t['home.skillTeachingDesc']() }, { title: t['home.skillOpenSource'](), desc: t['home.skillOpenSourceDesc']() }] as skill}
								<div class="flex items-start gap-3">
									<div
										class="flex h-8 w-8 shrink-0 items-center justify-center rounded-lg bg-violet-100 dark:bg-violet-900/30"
									>
										<CheckCircle2 class="h-4 w-4 text-violet-600 dark:text-violet-400" />
									</div>
									<div>
										<h3 class="font-semibold text-gray-900 dark:text-gray-100">{skill.title}</h3>
										<p class="text-sm text-gray-600 dark:text-gray-400">{skill.desc}</p>
									</div>
								</div>
							{/each}
						</div>

						<!-- Social Links & CTA -->
						<div class="flex flex-col items-stretch gap-3 sm:flex-row sm:items-center sm:gap-4">
							<a
								href="/about"
								class="inline-flex transform items-center justify-center gap-2 rounded-xl bg-gradient-to-r from-violet-600 to-purple-600 px-4 py-3 text-sm font-semibold text-white shadow-lg transition-all hover:-translate-y-1 hover:from-violet-700 hover:to-purple-700 hover:shadow-xl md:px-6 md:py-3 md:text-base"
							>
								{t['home.learnMore']()}
								<ArrowRight class="h-4 w-4" />
							</a>

							<!-- Social Icons -->
							<div class="flex items-center justify-center gap-2">
								<a
									href="https://github.com/codewithmpia"
									target="_blank"
									rel="noopener noreferrer"
									aria-label="GitHub"
									class="group flex h-10 w-10 items-center justify-center rounded-lg bg-gray-100 transition-colors hover:bg-violet-100 dark:bg-gray-800 dark:hover:bg-violet-900/30"
								>
									<Github
										class="h-5 w-5 text-gray-600 group-hover:text-violet-600 dark:text-gray-400 dark:group-hover:text-violet-400"
									/>
								</a>
								<a
									href="https://twitter.com/codewithmpia"
									target="_blank"
									rel="noopener noreferrer"
									aria-label="Twitter"
									class="group flex h-10 w-10 items-center justify-center rounded-lg bg-gray-100 transition-colors hover:bg-violet-100 dark:bg-gray-800 dark:hover:bg-violet-900/30"
								>
									<Twitter
										class="h-5 w-5 text-gray-600 group-hover:text-violet-600 dark:text-gray-400 dark:group-hover:text-violet-400"
									/>
								</a>
								<a
									href="https://linkedin.com/in/mpia-puludisu-b42007396/"
									target="_blank"
									rel="noopener noreferrer"
									aria-label="LinkedIn"
									class="group flex h-10 w-10 items-center justify-center rounded-lg bg-gray-100 transition-colors hover:bg-violet-100 dark:bg-gray-800 dark:hover:bg-violet-900/30"
								>
									<Linkedin
										class="h-5 w-5 text-gray-600 group-hover:text-violet-600 dark:text-gray-400 dark:group-hover:text-violet-400"
									/>
								</a>
							</div>
						</div>
					</div>
				{/snippet}
			</ScrollReveal>
		</div>
	</div>
</section>

<!-- Articles récents -->
<section class="bg-gradient-to-b from-gray-50 to-white py-20 dark:from-gray-900 dark:to-gray-950">
	<div class="mx-auto max-w-6xl px-6">
		<ScrollReveal delay={0}>
			{#snippet children()}
				<div class="mb-12 flex flex-col items-center justify-between gap-4 md:flex-row">
					<div class="text-center md:text-left">
						<h2 class="mb-2 text-3xl font-bold text-gray-900 md:text-4xl dark:text-gray-100">
							{t['home.recentArticlesTitle']()}
						</h2>
						<p class="text-gray-600 dark:text-gray-400">
							{t['home.recentArticlesDescription']()}
						</p>
					</div>
					<a
						href="/articles"
						class="group inline-flex w-auto items-center gap-2 rounded-xl bg-violet-600 px-4 py-2 text-sm font-semibold text-white shadow-lg transition-all hover:bg-violet-700 hover:shadow-xl md:px-6 md:py-3 md:text-base"
					>
						{t['home.viewAllArticles']()}
						<ArrowRight class="h-5 w-5 transition-transform group-hover:translate-x-1" />
					</a>
				</div>
			{/snippet}
		</ScrollReveal>

		{#if !data.posts || data.posts.length === 0}
			<div in:fade class="rounded-lg bg-gray-50 py-16 text-center dark:bg-gray-800">
				<FileX class="mx-auto mb-4 h-16 w-16 text-gray-400 dark:text-gray-600" />
				<p class="text-lg text-gray-600 dark:text-gray-400">
					{t['home.noArticles']()}
				</p>
			</div>
		{:else}
			<div class="grid grid-cols-1 gap-8 md:grid-cols-2 lg:grid-cols-3">
				{#each data.posts.slice(0, 6) as post, i}
					<ScrollReveal delay={i * 75}>
						{#snippet children()}
							{#await ArticleCard then { default: ArticleCardComponent }}
								<ArticleCardComponent {post} index={i} variant="default" />
							{/await}
						{/snippet}
					</ScrollReveal>
				{/each}
			</div>
		{/if}
	</div>
</section>

<!-- Articles Populaires Section -->
{#if data.popularPosts && data.popularPosts.length > 0}
	<section class="bg-white py-20 dark:bg-gray-950">
		<div class="mx-auto max-w-6xl px-6">
			<ScrollReveal delay={0}>
				{#snippet children()}
					<div class="mb-12 text-center">
						<div
							class="mb-4 inline-flex items-center gap-2 rounded-full border border-orange-200 bg-orange-50 px-4 py-2 dark:border-orange-800 dark:bg-orange-900/20"
						>
							<TrendingUp class="h-4 w-4 text-orange-600 dark:text-orange-400" />
							<span class="text-sm font-medium text-orange-700 dark:text-orange-300"
								>{t['home.popularArticlesBadge']()}</span
							>
						</div>
						<h2 class="mb-3 text-3xl font-bold text-gray-900 md:text-4xl dark:text-gray-100">
							{t['home.popularArticlesTitle']()}
						</h2>
						<p class="mx-auto max-w-2xl text-gray-600 dark:text-gray-400">
							{t['home.popularArticlesDescription']()}
						</p>
					</div>
				{/snippet}
			</ScrollReveal>

			<div class="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
				{#each data.popularPosts as post, i}
					<ScrollReveal delay={i * 75}>
						{#snippet children()}
							{#await ArticleCard then { default: ArticleCardComponent }}
								<ArticleCardComponent
									{post}
									index={i}
									variant="popular"
									showRank={true}
									rank={i + 1}
								/>
							{/await}
						{/snippet}
					</ScrollReveal>
				{/each}
			</div>

			<!-- CTA -->
			<ScrollReveal delay={0}>
				{#snippet children()}
					<div class="mt-12 text-center">
						<a
							href="/articles"
							class="inline-flex transform items-center gap-2 rounded-xl bg-orange-600 px-6 py-3 font-semibold text-white shadow-lg transition-all hover:-translate-y-1 hover:bg-orange-700 hover:shadow-xl"
						>
							{t['home.viewAllArticles']()}
							<ArrowRight class="h-4 w-4" />
						</a>
					</div>
				{/snippet}
			</ScrollReveal>
		</div>
	</section>
{/if}

<!-- Newsletter Section -->
<section class="bg-gradient-to-b from-gray-50 to-white py-20 dark:from-gray-900 dark:to-gray-950">
	<div class="mx-auto max-w-6xl min-[500px]:px-6">
		<div
			class="relative overflow-hidden bg-gradient-to-br from-violet-600 via-purple-700 to-indigo-800 p-6 shadow-2xl min-[500px]:rounded-3xl min-[500px]:p-8 md:p-12 dark:from-violet-700 dark:via-purple-800 dark:to-indigo-900"
		>
			<!-- Effets de fond décoratifs -->
			<div class="absolute inset-0 opacity-10">
				<div class="absolute top-0 right-0 h-64 w-64 rounded-full bg-white blur-3xl"></div>
				<div class="absolute bottom-0 left-0 h-64 w-64 rounded-full bg-pink-300 blur-3xl"></div>
			</div>

			<div class="relative z-10 grid items-center gap-8 min-[940px]:grid-cols-2">
				<!-- Contenu texte -->
				<div class="text-white">
					<ScrollReveal delay={0}>
						{#snippet children()}
							<div
								class="mb-6 inline-flex items-center gap-2 rounded-full border border-white/20 bg-white/10 px-4 py-2 backdrop-blur-sm"
							>
								<Mail class="h-4 w-4 text-emerald-300" />
								<span class="text-sm font-medium">{t['home.newsletterBadge']()}</span>
							</div>
						{/snippet}
					</ScrollReveal>

					<ScrollReveal delay={100}>
						{#snippet children()}
							<h2 class="mb-4 text-3xl leading-tight font-extrabold md:text-4xl">
								{t['home.newsletterTitle']()}
							</h2>
						{/snippet}
					</ScrollReveal>

					<ScrollReveal delay={200}>
						{#snippet children()}
							<p class="mb-6 leading-relaxed text-violet-100 dark:text-violet-200">
								{t['home.newsletterDescription']()}
							</p>
						{/snippet}
					</ScrollReveal>

					<!-- Points clés -->
					<ScrollReveal delay={300}>
						{#snippet children()}
							<ul class="space-y-2 text-sm text-violet-100">
								<li class="flex items-center gap-2">
									<CheckCircle2 class="h-5 w-5 shrink-0 text-emerald-300" />
									<span>{t['home.newsletterBenefit1']()}</span>
								</li>
								<li class="flex items-center gap-2">
									<CheckCircle2 class="h-5 w-5 shrink-0 text-emerald-300" />
									<span>{t['home.newsletterBenefit2']()}</span>
								</li>
								<li class="flex items-center gap-2">
									<CheckCircle2 class="h-5 w-5 shrink-0 text-emerald-300" />
									<span>{t['home.newsletterBenefit3']()}</span>
								</li>
							</ul>
						{/snippet}
					</ScrollReveal>
				</div>

				<!-- Formulaire -->
				<ScrollReveal delay={400}>
					{#snippet children()}
						<div class="rounded-2xl bg-white p-6 shadow-xl md:p-8 dark:bg-gray-800">
							<h3 class="mb-4 text-xl font-bold text-gray-900 dark:text-gray-100">
								{t['home.joinCommunity']()}
							</h3>
							<p class="mb-6 text-sm text-gray-600 dark:text-gray-400">
								{t['home.trustedDevelopers']()}
							</p>
							{#await NewsLetterSignup then { default: NewsLetterSignupComponent }}
								<NewsLetterSignupComponent variant="light" size="normal" />
							{/await}
							<p class="mt-4 text-xs text-gray-500 dark:text-gray-400">
								{t['home.privacyNotice']()}
								<a href="/privacy" class="text-violet-600 hover:underline dark:text-violet-400"
									>{t['home.privacyPolicy']()}</a
								>.
							</p>
						</div>
					{/snippet}
				</ScrollReveal>
			</div>
		</div>
	</div>
</section>

<!-- Call to Action -->
<section
	class="relative overflow-hidden bg-gradient-to-br from-violet-600 via-purple-700 to-indigo-800 py-20 dark:from-violet-900 dark:via-purple-900 dark:to-indigo-950"
>
	<!-- Effets de fond -->
	<div class="absolute inset-0 opacity-10">
		<div class="absolute top-0 left-1/4 h-96 w-96 rounded-full bg-pink-300 blur-3xl"></div>
		<div class="absolute right-1/4 bottom-0 h-96 w-96 rounded-full bg-blue-300 blur-3xl"></div>
	</div>

	<div class="relative z-10 mx-auto max-w-4xl px-6 text-center">
		<ScrollReveal delay={0}>
			{#snippet children()}
				<div
					class="mx-auto mb-6 inline-flex h-16 w-16 items-center justify-center rounded-2xl bg-white/10 backdrop-blur-sm"
				>
					<Mail class="h-8 w-8 text-white" />
				</div>
			{/snippet}
		</ScrollReveal>

		<ScrollReveal delay={100}>
			{#snippet children()}
				<h2 class="mb-4 text-3xl font-extrabold text-white md:text-4xl">
					{t['home.projectQuestionTitle']()}
				</h2>
			{/snippet}
		</ScrollReveal>

		<ScrollReveal delay={200}>
			{#snippet children()}
				<p class="mx-auto mb-8 max-w-2xl text-lg leading-relaxed text-violet-100 md:text-xl">
					{t['home.projectQuestionDescription']()}
				</p>
			{/snippet}
		</ScrollReveal>

		<ScrollReveal delay={300}>
			{#snippet children()}
				<div class="flex flex-wrap items-center justify-center gap-4">
					<a
						href="/contact"
						class="group w-full rounded-xl bg-white px-6 py-3 text-sm font-bold text-violet-700 shadow-xl transition-all hover:scale-105 hover:shadow-2xl md:w-auto md:px-8 md:py-4 md:text-base"
					>
						<span class="flex items-center justify-center gap-2 md:justify-start">
							{t['home.contactMe']()}
							<ArrowRight class="h-5 w-5 transition-transform group-hover:translate-x-1" />
						</span>
					</a>

					<a
						href="/articles"
						class="w-full rounded-xl border-2 border-white/30 bg-white/10 px-6 py-3 text-sm font-bold text-white backdrop-blur-sm transition-all hover:bg-white/20 md:w-auto md:px-8 md:py-4 md:text-base"
					>
						{t['home.browseArticles']()}
					</a>
				</div>
			{/snippet}
		</ScrollReveal>
	</div>
</section>
