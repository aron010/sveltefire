<script lang="ts">
    import { downloadUrlStore } from '$lib/stores/storage.js';
    import { getFirebaseContext } from '$lib/stores/sdk.js';
    import type { FirebaseStorage, StorageReference } from 'firebase/storage';

    interface Props {
        ref: string | StorageReference;
        children?: import('svelte').Snippet<[any]>;
        loading?: import('svelte').Snippet;
    }

    let { ref, children, loading }: Props = $props();

    const { storage } = getFirebaseContext();
    const store = downloadUrlStore(storage!, ref);

    interface $$Slots {
        default: { link: string | null; ref: StorageReference | null; storage?: FirebaseStorage },
        loading: {},
    }
</script>

{#if $store !== undefined}
    {@render children?.({ link: $store, ref: store.reference, storage, })}
{:else}
    {@render loading?.()}
{/if}

