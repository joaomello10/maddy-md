<script lang="ts">
    import * as Sidebar from "$lib/components/ui/sidebar/index.js";
    import Sun from "@lucide/svelte/icons/sun";
    import Moon from "@lucide/svelte/icons/moon";
    import { toggleMode } from "mode-watcher";
    import { Button } from "$lib/components/ui/button/index.js";
    import { buttonVariants } from "$lib/components/ui/button";
    import { flip } from "svelte/animate";

    import { dndzone } from "svelte-dnd-action";
    const flipDurationMs = 300;
    function handleDndConsider(e) {
        items = e.detail.items;
    }
    function handleDndFinalize(e) {
        items = e.detail.items;
    }

    let items = [
        { id: 1, title: "I" },
        { id: 2, title: "Am" },
        { id: 3, title: "Yoda" },
    ];

    $: console.log(items);
</script>

<Sidebar.Root>
    <Sidebar.Header />
    <Sidebar.Content>
        <main class="m-5">
            <div class="mb-5">
                <Button onclick={toggleMode} variant="outline" size="icon">
                    <Sun
                        class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0"
                    />
                    <Moon
                        class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100"
                    />
                    <span class="sr-only">Toggle theme</span>
                </Button>
            </div>
            <div>
                <h1 class="text-2xl font-bold mb-3">My notes</h1>
            </div>
            <section
                use:dndzone={{ items, flipDurationMs, dropTargetStyle: {} }}
                on:consider={handleDndConsider}
                on:finalize={handleDndFinalize}
                class="border-none flex flex-col gap-2"
            >
                {#each items as item (item.id)}
                    <div
                        id="note-{item.id}"
                        animate:flip={{ duration: flipDurationMs }}
                    >
                        <a
                            href="/dashboard"
                            class="w-full {buttonVariants({
                                variant: 'outline',
                            })}"
                        >
                            {item.title}
                        </a>
                    </div>
                {/each}
            </section>
        </main>
    </Sidebar.Content>
    <Sidebar.Footer />
</Sidebar.Root>

<style>
    .dnd-action[draggable][aria-pressed],
    .dnd-action[draggable][aria-grabbed] {
        outline: none !important;
        box-shadow: none !important;
    }
</style>
