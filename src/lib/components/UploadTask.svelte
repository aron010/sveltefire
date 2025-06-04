<script lang="ts">
  import { uploadTaskStore } from "$lib/stores/storage.js";
  import { getFirebaseContext } from "$lib/stores/sdk.js";
  import type {
    FirebaseStorage,
    UploadTask,
    StorageReference,
    UploadMetadata,
    UploadTaskSnapshot,
  } from "firebase/storage";

  interface Props {
    ref: string | StorageReference;
    data: Blob | Uint8Array | ArrayBuffer;
    metadata?: UploadMetadata | undefined;
    children?: import('svelte').Snippet<[any]>;
  }

  let {
    ref,
    data,
    metadata = undefined,
    children
  }: Props = $props();

  const { storage } = getFirebaseContext();
  const upload = uploadTaskStore(storage!, ref, data, metadata);

  interface $$Slots {
    default: {
      task: UploadTask | undefined;
      ref: StorageReference | null;
      snapshot: UploadTaskSnapshot | null;
      progress: number;
      storage?: FirebaseStorage;
    };
  }

  let progress = $derived(($upload?.bytesTransferred! / $upload?.totalBytes!) * 100 ?? 0);
</script>

{#if $upload !== undefined}
  {@render children?.({ task: $upload?.task, snapshot: $upload, progress, ref: upload.reference, storage, })}
{/if}
