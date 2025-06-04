<script lang="ts">
  import { nodeListStore } from "../stores/rtdb.js";
  import { getFirebaseContext } from "../stores/sdk.js";
  import type { DatabaseReference, Database } from "firebase/database";

  interface Props {
    path: string;
    startWith?: any[];
    children?: import('svelte').Snippet<[any]>;
    loading?: import('svelte').Snippet;
  }

  let {
    path,
    startWith = [],
    children,
    loading
  }: Props = $props();

  const { rtdb } = getFirebaseContext();
  let store = nodeListStore(rtdb!, path, startWith);

  interface $$Slots {
    default: {
      data: any[];
      ref: DatabaseReference | null;
      count: number;
      rtdb?: Database;
    };
    loading: {};
  }
</script>

{#if $store !== undefined}
  {@render children?.({ data: $store, ref: store.ref, count: $store?.length ?? 0, rtdb, })}
{:else}
  {@render loading?.()}
{/if}
