<script lang="ts" generics="Data extends DocumentData">
  import type {
    DocumentData,
    DocumentReference,
    Firestore,
  } from "firebase/firestore";
  import { docStore } from "../stores/firestore.js";
  import { getFirebaseContext } from "../stores/sdk.js";

  interface Props {
    ref: string | DocumentReference<Data>;
    startWith?: Data | undefined;
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

  let store = docStore(firestore!, ref, startWith);

  interface $$Slots {
    default: {
      data: Data;
      ref: DocumentReference<Data> | null;
      firestore?: Firestore;
    };
    loading: {};
  }
</script>

{#if $store !== undefined && $store !== null}
  {@render children?.({ data: $store, ref: store.ref, firestore, })}
{:else}
  {@render loading?.()}
{/if}
