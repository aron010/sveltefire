<script lang="ts" generics="Data extends DocumentData">
  import type {
    CollectionReference,
    DocumentData,
    Firestore,
    Query,
  } from "firebase/firestore";
  import { collectionStore } from "../stores/firestore.js";
  import { getFirebaseContext } from "../stores/sdk.js";

  interface Props {
    ref: string | CollectionReference<Data> | Query<Data>;
    startWith?: Data[] | undefined;
    children?: import('svelte').Snippet<[any]>;
    loading?: import('svelte').Snippet;
  }

  let {
    ref,
    startWith = undefined,
    children,
    loading
  }: Props = $props();

  const { firestore } = getFirebaseContext();

  let store = collectionStore<Data>(firestore!, ref, startWith);

  interface $$Slots {
    default: {
      data: Data[];
      ref: CollectionReference<Data[]> | Query<Data[]> | null;
      count: number;
      firestore?: Firestore;
    };
    loading: {};
  }
</script>

{#if $store !== undefined}
  {@render children?.({ data: $store, ref: store.ref, count: $store?.length ?? 0, firestore, })}
{:else}
  {@render loading?.()}
{/if}
