<script lang="ts">
  import SignedIn from '$lib/components/SignedIn.svelte';
  import SignedOut from '$lib/components/SignedOut.svelte';
  import { signInAnonymously } from "firebase/auth";
</script>

<h1>Auth Test</h1>

<SignedIn  >
    {#snippet children({ user, signOut })}
    <h2>Signed In</h2>
      <p>Hello {user.uid}</p>
      <button onclick={signOut}>Sign Out</button>
  {/snippet}
</SignedIn>

<SignedOut >
     {#snippet children({ auth })}
    <h2>Signed Out</h2>
      <button onclick={() => signInAnonymously(auth)}>Sign In</button>
  {/snippet}
</SignedOut>
