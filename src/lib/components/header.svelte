<script lang="ts">
  import { Settings } from 'lucide-svelte';
</script>

<!-- Header -->
<header class="px-6 py-5 flex justify-between items-center bg-slate-900/95 backdrop-blur z-10 border-b border-slate-800">
    <h1 class="text-xl font-bold bg-gradient-to-r from-teal-400 to-emerald-400 bg-clip-text text-transparent">
        Distractable
    </h1>
    <button class="p-2 text-slate-400 hover:text-white transition-colors">
        <Settings size={20} />
    </button>
</header>