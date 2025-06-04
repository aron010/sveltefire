<script lang="ts">
    import type { PageData } from './$types';
    import PageView from '$lib/components/PageView.svelte';

    interface Props {
        data: PageData;
    }

    let { data }: Props = $props();
</script>

<svelte:head>
    <title>{data.title}</title>
</svelte:head>

<h1>{data.title}</h1>

<p>
    This is a test page for analytics. The title is always server rendered.
</p>

<a href="/analytics-test/client-side">Analytics CSR Test</a>


<!-- <PageView /> -->