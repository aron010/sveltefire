<script lang="ts">
  import { page } from "$app/state";
  import PageView from "$lib/components/PageView.svelte";
  interface Props {
    children?: import('svelte').Snippet;
  }

  let { children }: Props = $props();
</script>

{@render children?.()}

<h2>Analytics Layout</h2>
{#key page.route.id}
  <p>Logged analytics for {page.route.id}</p>
  <PageView />
{/key}
