<script lang="ts">
  import editorWorker from "monaco-editor/esm/vs/editor/editor.worker?worker";
  import jsonWorker from "monaco-editor/esm/vs/language/json/json.worker?worker";
  import cssWorker from "monaco-editor/esm/vs/language/css/css.worker?worker";
  import htmlWorker from "monaco-editor/esm/vs/language/html/html.worker?worker";
  import tsWorker from "monaco-editor/esm/vs/language/typescript/ts.worker?worker";
  import { onMount } from "svelte";

  let divEl: HTMLElement;
  let code = "";

  onMount(async () => {
    self.MonacoEnvironment = {
      getWorker: function (_moduleId, label) {
        if (label === "json") {
          return new jsonWorker();
        }
        if (label === "css" || label === "scss" || label === "less") {
          return new cssWorker();
        }
        if (label === "html" || label === "handlebars" || label === "razor") {
          return new htmlWorker();
        }
        if (label === "typescript" || label === "javascript") {
          return new tsWorker();
        }
        return new editorWorker();
      },
    };

    let Monaco = await import("monaco-editor");

    Monaco.editor.create(divEl, {
      theme: "vs-dark",
      language: "javascript",
      // fontFamily: "JetBrainsMono",
    });

    let editor = Monaco.editor.getEditors()[0];

    editor.onDidChangeModelContent((_) => {
      let val = editor.getValue();
      code = val;
    });
  });

  $: console.log(code);
</script>

<nav class="flex items-center h-16 bg-zinc-950">Devmentor</nav>

<div class="grid grid-cols-2 h-[calc(100vh-4rem)]">
  <div>left</div>
  <div bind:this={divEl} class="w-full h-full" />
</div>

<style>
</style>
