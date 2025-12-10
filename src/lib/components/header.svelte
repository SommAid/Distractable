<script lang="ts">
  import { CircleUserRound } from 'lucide-svelte';
</script>

<!-- Header -->
<header class="px-6 py-5 flex justify-between items-center bg-slate-900/95 backdrop-blur z-10 border-b border-slate-800">
    <h1 class="text-4xl font-bold bg-gradient-to-r from-gray-400 to-white-400 bg-clip-text text-transparent">
        Distractable
    </h1>
    <a href="/settings" class="p-2 text-slate-400 hover:text-white transition-colors">
        <CircleUserRound size={36} />
    </a>
</header>