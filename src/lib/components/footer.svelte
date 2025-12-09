<script lang="ts">
  import { Grid3x3, House, Volume2 } from 'lucide-svelte';
  
  // State (kept these in case you need them for actual page routing logic, 
  // even though they no longer control the colors)
  let activeTab = 'home';
</script>

<nav class="h-20 bg-slate-900 border-t border-slate-800 grid grid-cols-3 items-center pb-2 z-10">
  <a 
    href="/" 
    class="flex flex-col items-center justify-center gap-1 transition-all duration-300 text-slate-500 hover:text-teal-400 hover:scale-105"
  >
    <House size={24} />
    <span class="text-xs font-medium">Home</span>
  </a>

  <button 
    on:click={() => activeTab = 'tools'}
    class="flex flex-col items-center justify-center gap-1 transition-all duration-300 text-slate-500 hover:text-teal-400 hover:scale-105"
  >
    <Grid3x3 size={24} />
    <span class="text-xs font-medium">Tools</span>
  </button>

  <button 
    on:click={() => activeTab = 'sound'}
    class="flex flex-col items-center justify-center gap-1 transition-all duration-300 text-slate-500 hover:text-teal-400 hover:scale-105"
  >
    <Volume2 size={24} />
    <span class="text-xs font-medium">Sound</span>
  </button>
</nav>