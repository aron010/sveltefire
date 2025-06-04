<script lang="ts">
    import { storageListStore } from '$lib/stores/storage.js';
    import { getFirebaseContext } from '$lib/stores/sdk.js';
    import type { FirebaseStorage, ListResult, StorageReference } from 'firebase/storage';

    interface Props {
        ref: string | StorageReference;
        children?: import('svelte').Snippet<[any]>;
        loading?: import('svelte').Snippet;
    }

    let { ref, children, loading }: Props = $props();

    const { storage } = getFirebaseContext();
    const listStore = storageListStore(storage!, ref);

    interface $$Slots {
        default: { list: ListResult | null; ref: StorageReference | null; storage?: FirebaseStorage },
        loading: {},
    }
</script>

{#if $listStore !== undefined}
    {@render children?.({ list: $listStore, ref: listStore.reference, storage, })}
{:else}
    {@render loading?.()}
{/if}

