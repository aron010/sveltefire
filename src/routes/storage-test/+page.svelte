<script lang="ts">
  import DownloadURL from "$lib/components/DownloadURL.svelte";
  import StorageList from "$lib/components/StorageList.svelte";
  import UploadTask from "$lib/components/UploadTask.svelte";

  let file: any = $state();

  function makeFile() {
    file = new Blob(["test"], { type: "text/plain" });
  }

  function chooseFile(e: any) {
    file = e.target.files[0];
  }
</script>

<h1>Storage Test</h1>

<StorageList ref="/" >
  {#snippet children({ list })}
    <ul>
      {#if list === null}
        <li>Loading...</li>
      {:else if list.prefixes.length === 0 && list.items.length === 0}
        <li>Empty</li>
      {:else}
        {#each list.items as item}
          <li>
            <DownloadURL ref={item}  >
              {#snippet children({ link, ref })}
                            <a data-testid="download-link" href={link} download>{ref?.name}</a>
                                        {/snippet}
                        </DownloadURL>
          </li>
        {/each}
      {/if}
    </ul>
  {/snippet}
</StorageList>

<h2>Upload Task</h2>

<input type="file" onchange={chooseFile} />
<button onclick={makeFile}>Make File</button>

{#if file}
  <UploadTask ref="test-upload.txt" data={file}  >
    {#snippet children({ progress, snapshot })}
        {#if snapshot?.state === "running" || snapshot?.state === "success"}
        <p data-testid="progress">{progress}% uploaded</p>
      {/if}

      {#if snapshot?.state === "error"}
        Upload failed
      {/if}

      {#if snapshot?.state === "success"}
        <DownloadURL ref={snapshot?.ref}  >
          {#snippet children({ link, ref })}
                <a data-testid="download-link2" href={link} download> {ref?.name} </a>
                        {/snippet}
            </DownloadURL>
      {/if}
          {/snippet}
    </UploadTask>
{/if}
