<script lang="js">
  import { onMount } from 'svelte';
  import { fly } from 'svelte/transition';
  import { MousePointerClick } from 'lucide-svelte';

  // Switch Tool State
  let switchOn = false;
  let showHint = true; // Hint visibility control

  // Hide hint visibility after 2 seconds
  onMount(() => {
    const timer = setTimeout(() => {
      showHint = false;
    }, 2000);

    return () => clearTimeout(timer); // Cleanup component
  });
</script>

<div in:fly={{ y: 50, duration: 300 }} class="h-full flex flex-col items-center justify-center space-y-12">
    <p class="flex items-center gap-1 text-lg font-bold mb-2 text-slate-500 text-sm transition-opacity duration-500 {showHint ? 'opacity-100' : 'opacity-0'}">
        Tap the switch.
        <MousePointerClick class="text-emerald-400"/>
    </p>
    
    <button 
        on:click={() => switchOn = !switchOn}
        class="w-32 h-64 rounded-full border-4 transition-all duration-200 shadow-2xl relative flex flex-col items-center p-4 {switchOn ? 'bg-emerald-900/50 border-emerald-500' : 'bg-slate-800 border-slate-600'}"
    >
        <div class="w-24 h-24 rounded-full bg-white shadow-lg transition-all duration-200 absolute {switchOn ? 'top-4 bg-emerald-400' : 'bottom-4 bg-slate-400'}"></div>
    </button>

    <a href="/" class="text-slate-400 hover:text-white text-sm">Close Tool</a>
</div>