<script lang="ts">
    import 'dayjs/locale/fr';

    import favicon from '$lib/assets/favicon-32x32.png';
    import Logo from '$lib/assets/logo.svg';
    import PublicodesEditor from '$lib/component/publicodes/editor.svelte';
    import Callout from '$lib/ui/callout.svelte';
    import { dayjs } from 'svelte-time';

    import { afterNavigate } from '$app/navigation';
    import { insertDocsearch } from '$lib/search/insert-docsearch';
    import NavTab from '$lib/ui/nav-tab.svelte';
    import {
        Github,
        MenuIcon,
        MessageCircleHeart,
        MessagesSquare,
        ChevronRight,
        X
    } from 'lucide-svelte';
    import { onMount } from 'svelte';
    import { fly } from 'svelte/transition';
    import '../app.css';

    /* eslint-disable @typescript-eslint/no-explicit-any */
    (globalThis as any).PublicodesEditor = PublicodesEditor;
    (globalThis as any).Callout = Callout;
    /* eslint-enable @typescript-eslint/no-explicit-any */

    const { children } = $props();

    // Set the locale time to French
    dayjs.locale('fr');

    let showMobileMenu = $state(false);
    let showSurvey = $state(true);

    afterNavigate(() => {
        showMobileMenu = false;
    });
    onMount(() => {
        insertDocsearch('div#search');
    });
</script>

<svelte:head>
    <title>Publicodes ⋅ Langage pour expert·es et développeur·euses</title>

    <meta
        name="description"
        content="Publicodes permet de modéliser des domaines métiers complexes,
		en les décomposant en règles élémentaires simples qui soient
		lisibles par tout le monde."
    />
    <link rel="icon" type="image/png" href={favicon} />
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@docsearch/css@3" />
</svelte:head>

<div class="fixed top-0 z-20 w-screen border-b border-primary-200 bg-white text-cyan-950">
    <div
        class="flex h-16 w-full items-center gap-8 px-6 py-2 lg:container md:px-8 lg:mx-auto
		lg:justify-center"
    >
        <a
            class="inline-flex flex-1 shrink-0 items-center gap-2 text-xl font-light hover:text-primary-400"
            href="/"
        >
            <img src={Logo} class="h-7" alt="Logo de publicodes" />
            Publicodes
        </a>
        <div id="search" class="flex justify-center"></div>
        <button
            class="text-primary-400 hover:text-primary-600 sm:hidden"
            onclick={() => (showMobileMenu = true)}
        >
            <MenuIcon size={24} />
        </button>
        <div class="hidden sm:block">
            {@render Menu()}
        </div>
    </div>
</div>

{#if showMobileMenu}
    <div
        role="dialog"
        class="fixed right-0 top-0 z-40 h-full border-l
        border-primary-300 bg-white will-change-transform sm:hidden"
        transition:fly={{ x: 100 }}
    >
        {@render Menu()}
    </div>
    <div
        class="fixed inset-0 z-20 sm:hidden"
        aria-hidden="true"
        onclick={() => (showMobileMenu = false)}
    ></div>
{/if}

<div
    class="flex h-full flex-col pt-16"
    class:blur-sm={showMobileMenu}
    class:opacity-50={showMobileMenu}
>
    {#if showSurvey}
        <aside
            class="flex items-center justify-between bg-secondary-600 px-2
			py-1 text-white"
        >
            <div
                class="container mx-auto flex flex-col items-start gap-3 p-2 text-left
			sm:flex-row sm:items-center sm:justify-center sm:text-left"
            >
                <MessageCircleHeart class="hidden sm:inline" strokeWidth={1.5} size={28} />
                <span
                    >Aidez-nous à améliorer Publicodes en <span class="font-semibold"
                        >5 minutes chrono !</span
                    ></span
                >
                <a
                    href="https://tally.so/r/3yEBlB"
                    target="_blank"
                    class="border-1 background flex w-fit cursor-pointer items-center
			whitespace-nowrap rounded-full border border-secondary-600
			bg-white bg-gradient-to-r px-4 py-1.5 font-sans text-sm
				font-semibold leading-none text-secondary-600
				duration-100 ease-in-out hover:bg-secondary-900 hover:text-white [&:hover_svg]:translate-x-1"
                    >Participer à l'enquête
                    <ChevronRight class="inline transition-transform" strokeWidth={2} size={22} />
                </a>
            </div>
            <button
                onclick={() => (showSurvey = false)}
                class="absolute right-4 flex h-fit w-fit items-center
				rounded-full bg-secondary-700 p-2 text-white
			hover:bg-secondary-900 hover:text-white"
            >
                <X class="m-0 inline" size={16} strokeWidth={2} />
            </button>
        </aside>
    {/if}
    <div class="flex-1">
        {@render children()}
    </div>
    {@render Footer()}
</div>

{#snippet Menu()}
    <nav class="flex flex-col justify-between gap-6 p-6 sm:flex-row sm:p-0">
        <ul class="flex flex-col items-start justify-center gap-3 sm:flex-row sm:items-center">
            <NavTab href="/docs">Docs</NavTab>
            <NavTab href="/bibliotheque">Bibliothèque</NavTab>
            <NavTab href="/realisations">Réalisations</NavTab>
            <NavTab href="/studio">Studio</NavTab>
            <NavTab href="/blog">Blog</NavTab>
        </ul>
        <div class="self-stretch border-b border-primary-100 sm:border-r sm:max-lg:hidden"></div>
        <ul class="flex flex-row items-start justify-center gap-2 sm:items-center sm:max-lg:hidden">
            <NavTab
                href="https://matrix.to/#/#publicodes:matrix.org"
                title="Discuter avec la communauté"><MessagesSquare class="inline py-1" /></NavTab
            >
            <NavTab href="https://github.com/publicodes/publicodes" title="Voir le code source"
                ><Github class="inline py-1" /></NavTab
            >

            <!-- <li>
                <a
                    class="hover:underline"
                    href="https://app.element.io/#/room/#publicodes:matrix.org">Communauté</a
                >
            </li>
            <li>
                <a class="hover:underline" href="https://github.com/publicodes/publicodes">Github</a
                >
            </li> -->
        </ul>
    </nav>
{/snippet}

{#snippet Footer()}
    <footer class="flex w-full flex-col items-center border-t border-primary-200 py-10">
        <div
            class="flex w-full flex-col items-start gap-8 px-6 md:container md:flex-row md:justify-between"
        >
            <div class="flex flex-col gap-2">
                <span class="inline-flex items-center gap-2 text-2xl font-light">
                    <img src={Logo} class="h-7" alt="Logo de publicodes" />
                    Publicodes
                </span>
                <p class="text-lg font-normal text-dark">Collaboratif, accessible et ouvert.</p>
            </div>
            <div class="flex flex-row gap-16">
                <div class="flex flex-col gap-2">
                    <p class="font-light text-slate-500">Ressources</p>
                    <ul class="flex flex-col gap-1 font-light">
                        <!-- <li>
                        <a href="/docs">À propos</a>
                    </li> -->
                        <li>
                            <a href="/docs">Documentation</a>
                        </li>
                        <li>
                            <a href="/bibliotheque">Bibliothèque</a>
                        </li>
                        <li>
                            <a href="/realisations">Réalisations</a>
                        </li>
                        <li>
                            <a
                                target="_blank"
                                href="https://dashboard.simpleanalytics.com/publi.codes"
                                aria-label="Statistiques de consultation, nouvelle fenêtre"
                                >Statistiques</a
                            >
                        </li>
                        <!-- <li>
                        <a href="/docs">FAQ</a>
                    </li> -->
                    </ul>
                </div>
                <div class="flex flex-col gap-2">
                    <p class="font-light text-slate-500">Communauté</p>
                    <ul class="flex flex-col gap-1 font-light">
                        <li>
                            <a href="https://matrix.to/#/#publicodes:matrix.org">Matrix</a>
                        </li>
                        <li>
                            <a href="https://github.com/publicodes" target="_blank">GitHub</a>
                        </li>
                        <li>
                            <a href="/blog">Blog</a>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </footer>
    <!-- Simple analytics -->
    <script async defer src="https://scripts.simpleanalyticscdn.com/latest.js"></script>
    <script
        async
        data-collect="outbound"
        src="https://scripts.simpleanalyticscdn.com/auto-events.js"
    ></script>
    <noscript
        ><img
            src="https://queue.simpleanalyticscdn.com/noscript.gif"
            alt=""
            referrerpolicy="no-referrer-when-downgrade"
        /></noscript
    >
{/snippet}

<style>
    footer a {
        @apply text-sm hover:text-primary-400;
    }
</style>
