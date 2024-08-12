<script lang="ts">
	import { onMount } from 'svelte';
    import * as Workspace from "$lib/components/workspace"
    import * as Card from "$lib/components/ui/card";
    import  ModelEditor from "$lib/components/editor/ModelEditor.svelte";
	import { get } from "svelte/store";
    let selectedTable: string;
    let selectedTab: string;


    let blob = "";
    
  // $: blob = $fileQuery.data?.blob ?? blob;

    $: latest = blob;
  // $: hasUnsavedChanges = latest !== blob;

  // $: pathname = $page.url.pathname;
  // $: workspace = workspaces.get(pathname);
  // $: autoSave = workspace.editor.autoSave;
  // $: disableAutoSave = FILES_WITHOUT_AUTOSAVE.includes(filePath);


 </script>
<Workspace.Container>
    <Workspace.Header
      slot="header"
      showInspectorToggle={true}
      bind:selectedTable
      bind:selectedTab
    />

    <div
      slot="body"
      class="editor-pane size-full overflow-hidden flex flex-col"
    >
      <Workspace.EditorContainer>
        <!-- <Editor
          {blob}
          {hasUnsavedChanges}
          extensions={getExtensionsForFile(filePath)}
          {disableAutoSave}
          bind:latest
          bind:autoSave={$autoSave}
          on:save={save}
          on:revert={revert}
        /> -->
        <ModelEditor/>
       

      </Workspace.EditorContainer>
      <Workspace.TableContainer/>
      
    </div>


    <!-- <Card.Root slot="body" class="m-2">
      <Card.Header>
        <Card.Title>{selectedTab}</Card.Title>
        <Card.Description>{selectedTable}</Card.Description>
      </Card.Header>
      
      <Card.Content>
        
        <p>Card Content</p>
      </Card.Content>
      <Card.Footer>
        <p>Card Footer</p>
      </Card.Footer>
    </Card.Root>
    -->

    
</Workspace.Container>
