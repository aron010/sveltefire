<script lang="ts">
  import Doc from "$lib/components/Doc.svelte";

  let { data } = $props();
</script>

<h1>Not Hydrated: {data?.title}</h1>

<Doc ref="posts/test" startWith={data} >
    {#snippet children({ data: realtimeData })}
    <h1>Hydrated: {realtimeData?.title}</h1>
  {/snippet}
</Doc>

<p>
  <a href="/">Home</a>
</p>