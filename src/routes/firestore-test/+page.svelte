<script lang="ts">
  import Doc from "$lib/components/Doc.svelte";
  import SignedOut from '$lib/components/SignedOut.svelte';
  import { signInAnonymously } from "firebase/auth";
  import {
    addDoc,
    collection,
    orderBy,
    query,
  } from "firebase/firestore";
  import Collection from "$lib/components/Collection.svelte";
  import SignedIn from "$lib/components/SignedIn.svelte";
  import { getFirebaseContext } from "$lib/stores/sdk.js";

  const firestore = getFirebaseContext().firestore!;

  async function addPost(uid: string) {
    const posts = collection(firestore, `users/${uid}/posts`);
    await addDoc(posts, {
      content: 'firestore item ' + (Math.random() + 1).toString(36).substring(7),
      created: Date.now(),
    });
  }

  let makeQuery = $derived((uid: string) => {
    const q = query(
      collection(firestore, `users/${uid}/posts`),
      orderBy("created")
    );
    return q;
  });
</script>

<h1>Firestore Test</h1>

<h2>Single Document</h2>

<Doc ref="posts/test" >
  {#snippet children({ data: post })}
    <p data-testid="doc-data">{post?.title}</p>
    {/snippet}
  {#snippet loading()}
    <div >
      <p data-testid="loading">Loading...</p>
    </div>
  {/snippet}
</Doc>

<h2>User Owned Posts</h2>

<SignedOut >
    {#snippet children({ auth })}
    <h2>Signed Out</h2>
     <button onclick={() => signInAnonymously(auth)}>Sign In</button>
  {/snippet}
</SignedOut>



<SignedIn >
  {#snippet children({ user })}
    <h2>Collection</h2>
    <Collection
      ref={makeQuery(user.uid)}
      startWith={[]}
      
      
    >
      {#snippet children({ data: posts, count })}
        <p data-testid="count">You've made {count} posts</p>

        <ul>
          {#each posts as post (post.id)}
            <li>{post?.content} ... {post.id}</li>
          {/each}
        </ul>

        <button onclick={() => addPost(user.uid)}>Add Post</button>
            {/snippet}
    </Collection>
  {/snippet}
</SignedIn>
