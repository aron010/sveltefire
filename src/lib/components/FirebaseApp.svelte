<script lang="ts">
  import { setFirebaseContext } from "$lib/stores/sdk.js";
  import type { Auth } from "firebase/auth";
  import type { Firestore } from "firebase/firestore";
  import type { Database } from "firebase/database";
  import type { FirebaseStorage } from "firebase/storage";
  import type { Analytics } from "firebase/analytics";

  interface Props {
    firestore?: Firestore | undefined;
    rtdb?: Database | undefined;
    auth?: Auth | undefined;
    storage?: FirebaseStorage | undefined;
    analytics?: Analytics | undefined;
    children?: import('svelte').Snippet;
  }

  let {
    firestore = undefined,
    rtdb = undefined,
    auth = undefined,
    storage = undefined,
    analytics = undefined,
    children
  }: Props = $props();

  setFirebaseContext({ firestore, rtdb, auth, storage, analytics });
</script>

{@render children?.()}
