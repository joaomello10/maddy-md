<script>
    import "../app.css";
    import * as Sidebar from "$lib/components/ui/sidebar/index.js";
    import AppSidebar from "$lib/components/app-sidebar.svelte";
    import { ModeWatcher } from "mode-watcher";
    let { children } = $props();
</script>

<div class="m-10">
    <Sidebar.Provider>
        <AppSidebar />
        <main>
            <Sidebar.Trigger />
            <ModeWatcher />
            {@render children?.()}
        </main>
    </Sidebar.Provider>
</div>
