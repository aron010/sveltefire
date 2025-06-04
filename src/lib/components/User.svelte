<script lang="ts">
  import type { User } from "firebase/auth";
  import { userStore } from "$lib/stores/auth.js";
  import { getFirebaseContext } from "$lib/index.js";
  interface Props {
    children?: import('svelte').Snippet<[any]>;
    signedOut?: import('svelte').Snippet;
  }

  let { children, signedOut }: Props = $props();

  const auth = getFirebaseContext().auth!;
  const user = userStore(auth)

  interface $$Slots {
    default: { user: User }
    signedOut: {}
  }
</script>

{#if $user}
  {@render children?.({ user: $user, })}
{:else}
  {@render signedOut?.()}
{/if}