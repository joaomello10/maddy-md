<script>
    import Button from "$lib/components/ui/button/button.svelte";
    import { invoke } from "@tauri-apps/api/core";
    import { marked } from "marked";
    import { onMount, onDestroy } from "svelte";
    import { Editor } from "@tiptap/core";
    import StarterKit from "@tiptap/starter-kit";
    import Typography from "@tiptap/extension-typography";
    import * as Sidebar from "$lib/components/ui/sidebar/index.js";
    import Image from "@tiptap/extension-image";

    let element;
    let editor;

    onMount(() => {
        editor = new Editor({
            element: element,
            extensions: [StarterKit, Image],
            content: `
            <h1>New Notes</h1>
            `,
            onTransaction: () => {
                // force re-render so `editor.isActive` works as expected
                editor = editor;
            },
            editorProps: {
                attributes: {
                    class: "focus:outline-none p-5",
                },
            },
        });
    });

    onDestroy(() => {
        if (editor) {
            editor.destroy();
        }
    });
</script>

{#if editor}
    <button
        on:click={() =>
            editor.chain().focus().toggleHeading({ level: 1 }).run()}
        class:active={editor.isActive("heading", { level: 1 })}
    >
        H1
    </button>
    <button
        on:click={() =>
            editor.chain().focus().toggleHeading({ level: 2 }).run()}
        class:active={editor.isActive("heading", { level: 2 })}
    >
        H2
    </button>
    <button
        on:click={() => editor.chain().focus().setParagraph().run()}
        class:active={editor.isActive("paragraph")}
    >
        P
    </button>
    <button
        on:click={() =>
            editor.commands.insertTable({
                rows: 3,
                cols: 3,
                withHeaderRow: true,
            })}
        class:active={editor.isActive("paragraph")}
    >
        d
    </button>
    <button
        on:click={() =>
            editor
                .commands()
                .focus()
                .insertTable({ rows: 3, cols: 3, withHeaderRow: true })}
    >
        Insert table
    </button>
{/if}

<div
    class="prose dark:prose-invert text-xl h-full w-full outline-none"
    bind:this={element}
></div>

<style>
    button.active {
        background: black;
        color: white;
    }

    .prose {
        max-width: 100%;
    }

    .ProseMirror:focus {
        outline: none;
    }
</style>
