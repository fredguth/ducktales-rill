<script lang="ts">
  import { page } from "$app/stores";
  import { IconButton } from "$lib/components/button";
  import HideRightSidebar from "$lib/components/ui/icons/HideRightSidebar.svelte";
  import { workspaces } from "./workspace-stores";
  // import { navigationOpen } from "../navigation/Navigation.svelte";
  import TableSwitcher from "./table-switcher.svelte";
  import TableTabs from "./table-tabs.svelte"
  import { browser } from "$app/environment";

  export let showInspectorToggle = true;
  
  let width: number;
  

  export let selectedTable: string;
  export let selectedTab: string;
  $: context = $page.url.pathname;
  $: workspaceLayout =browser && workspaces && workspaces.get(context);
  
</script>

<header class="p-1">
    <div class="wrapper slide" > 
  <!-- <div class="wrapper slide" class:pl-4={!$navigationOpen}> -->
    <TableSwitcher bind:selected={selectedTable}/>
    <TableTabs bind:selectedTab/>
  </div>

  <div class="flex items-center mr-4 flex-none">
    <slot name="workspace-controls" {width} />

    {#if workspaceLayout && showInspectorToggle}
      <IconButton on:click={workspaceLayout.inspector.toggle}>
        <span class="text-gray-500">
          <HideRightSidebar size="18px" />
        </span>
      </IconButton>
    {/if}

    <div class="pl-4 flex-none">
      <slot name="cta" {width} />
    </div>
  </div>
</header>

<style lang="postcss">
  input:focus,
  input:not(:disabled):hover {
    @apply border-2 border-gray-400 outline-none;
  }

  input,
  label {
    @apply whitespace-pre rounded border-2 border-transparent;
  }

  input {
    @apply absolute text-left;
    text-indent: 6px;
  }

  label {
    @apply w-fit min-w-8 px-2 text-transparent max-w-full;
  }

  header {
    @apply justify-between;
    @apply flex items-center pl-4 border-b border-gray-300 overflow-hidden;
    height: var(--header-height);
  }

  .wrapper {
    @apply overflow-hidden max-w-full gap-x-2;
    @apply size-full relative flex items-center;
    @apply font-bold pr-2 self-start;
    font-size: 16px;
  }
</style>
