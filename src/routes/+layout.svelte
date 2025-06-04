<script lang="ts">
  import FirebaseApp from "$lib/components/FirebaseApp.svelte";
  import { db as firestore, auth, rtdb, storage, analytics } from "./firebase.js";
  interface Props {
    children?: import('svelte').Snippet;
  }

  let { children }: Props = $props();
</script>

<FirebaseApp {auth} {firestore} {rtdb} {storage} {analytics}>
  {@render children?.()}
</FirebaseApp>
