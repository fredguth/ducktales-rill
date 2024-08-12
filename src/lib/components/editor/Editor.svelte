<script lang="ts">
  import { createEventDispatcher, onMount } from "svelte";
  import { EditorView } from "@codemirror/view";
  import { EditorState } from "@codemirror/state";
  import { base } from "./presets/base";
  import {bindEditorEventsToDispatcher}  from "./dispatch-events";

  export let blob: string;
  export let latest: string;
  export let extensions: Extension[] = [];

  let editor: EditorView;
  let container: HTMLElement;
  const dispatch = createEventDispatcher();

  function updateEditorContents(newContent: string) {
      if (editor && !editor.hasFocus) {
        // NOTE: when changing files, we still want to update the editor
        let curContent = editor.state.doc.toString();
        if (newContent != curContent) {
          editor.dispatch({
            changes: {
              from: 0,
              to: curContent.length,
              insert: newContent,
            },
          });
        }
      }
    }

  function updateEditorExtensions(newExtensions: Extension[]) {
      editor.setState(
        EditorState.create({
          doc: blob,
          extensions: [
            // establish a basic editor
            base(),
            // any extensions passed as props
            ...newExtensions,
            EditorView.updateListener.of((v) => {
              if (v.focusChanged && v.view.hasFocus) {
                dispatch("receive-focus");
              }
              if (v.docChanged) {
                latest = v.state.doc.toString();
              }
            }),
          ],
        }),
      );
    }  
  onMount(() => {
      editor = new EditorView({
        state: EditorState.create({
          doc: blob,
          extensions: [
            base(),
            bindEditorEventsToDispatcher(dispatch),
          ]
        }),
        parent: container,
      });
    });

    $: latest = blob;
    $: updateEditorContents(latest);
    $: if (editor) updateEditorExtensions(extensions);


    </script>
<section>
<div class="editor-container">
  <div
  bind:this={container}
  class="size-full"
  on:click={() => {
    /** give the editor focus no matter where we click */
    if (!editor.hasFocus) editor.focus();
  }}
  on:keydown={() => {
    /** no op for now */
  }}
  role="textbox"
  tabindex="0"
/>
</div>
</section>
<!-- <script lang="ts">
    import type { Extension } from "@codemirror/state";
    import { EditorState } from "@codemirror/state";
    import { EditorView } from "@codemirror/view";
    import Button from "$lib/components/ui/button/button.svelte"
    import Label from "$lib/components/ui/label/label.svelte";
    import Switch from "$lib/components/ui/switch/switch.svelte";
    // import Check from "@rilldata/web-common/components/icons/Check.svelte";
    // import UndoIcon from "@rilldata/web-common/components/icons/UndoIcon.svelte";
    import { createEventDispatcher, onMount } from "svelte";
    import {bindEditorEventsToDispatcher}  from "./dispatch-events";
    import { base } from "./presets/base";
    // import { debounce } from "$lib/utils/debounce";
    // import { FILE_SAVE_DEBOUNCE_TIME } from "./config";
  
    const dispatch = createEventDispatcher();
  
    export let blob: string;
    export let latest: string;
    export let extensions: Extension[] = [];
    // export let autoSave: boolean;
    // export let disableAutoSave: boolean;
    // export let hasUnsavedChanges: boolean;
  
    let editor: EditorView;
    let container: HTMLElement;
  
    $: latest = blob;
    $: updateEditorContents(latest);
    $: if (editor) updateEditorExtensions(extensions);
  
    onMount(() => {
      editor = new EditorView({
        state: EditorState.create({
          doc: blob,
          extensions: [
            // any extensions passed as props
            ...extensions,
            // establish a basic editor
            base(),
            // this will catch certain events and dispatch them to the parent
            bindEditorEventsToDispatcher(dispatch),
          ],
        }),
        parent: container,
      });
    });
  
    function updateEditorExtensions(newExtensions: Extension[]) {
      editor.setState(
        EditorState.create({
          doc: blob,
          extensions: [
            // establish a basic editor
            base(),
            // any extensions passed as props
            ...newExtensions,
            EditorView.updateListener.of((v) => {
              if (v.focusChanged && v.view.hasFocus) {
                dispatch("receive-focus");
              }
              if (v.docChanged) {
                latest = v.state.doc.toString();
  
                // if (!disableAutoSave && autoSave) debounceSave();
              }
            }),
          ],
        }),
      );
    }
  
    function updateEditorContents(newContent: string) {
      if (editor && !editor.hasFocus) {
        // NOTE: when changing files, we still want to update the editor
        let curContent = editor.state.doc.toString();
        if (newContent != curContent) {
          editor.dispatch({
            changes: {
              from: 0,
              to: curContent.length,
              insert: newContent,
            },
          });
        }
      }
    }
  
    function handleKeydown(e: KeyboardEvent) {
      if (e.key === "s" && (e.ctrlKey || e.metaKey)) {
        e.preventDefault();
        save();
      }
    }
  
    function save() {
      dispatch("save");
    }
  
    const debounceSave = debounce(save, FILE_SAVE_DEBOUNCE_TIME);
  
    function revertContent() {
      dispatch("revert");
    }
  </script>
  
  <svelte:window on:keydown={handleKeydown} />
  
  <section>
    <div class="editor-container">
      <div
        bind:this={container}
        class="size-full"
        on:click={() => {
          /** give the editor focus no matter where we click */
          if (!editor.hasFocus) editor.focus();
        }}
        on:keydown={() => {
          /** no op for now */
        }}
        role="textbox"
        tabindex="0"
      />
    </div> -->
  
    <!-- <footer>
      <div class="flex gap-x-3"> -->
        <!-- {#if !autoSave || disableAutoSave} -->
          <!-- <Button disabled={!hasUnsavedChanges} on:click={save}>
            <Check size="14px" />
            Save
          </Button>
  
          <Button
            type="text"
            disabled={!hasUnsavedChanges}
            on:click={revertContent}
          >
            <UndoIcon size="14px" />
            Revert changes
          </Button> -->
        <!-- {/if} -->
      <!-- </div> -->
      <!-- <div
        class="flex gap-x-1 items-center h-full bg-white rounded-full"
        class:hidden={disableAutoSave}
      >
        <Switch bind:checked={autoSave} id="auto-save" small />
        <Label class="font-normal text-xs" for="auto-save">Auto-save</Label>
      </div> -->
    <!-- </footer> -->
  <!-- </section> -->
  
  <style lang="postcss">
    .editor-container {
      @apply size-full overflow-auto p-2 pb-0 flex flex-col;
    }
  
  
    section {
      @apply size-full flex-col rounded-sm bg-white flex overflow-hidden relative;
    }
  </style>
  