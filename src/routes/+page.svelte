<script lang="ts">
  import { fade } from 'svelte/transition';
  import { Wind, ToggleLeft, Grid3x3, Flame } from 'lucide-svelte';
  
  // State
  let activeTool: string | null = null;
  
</script>


<main class="flex-1 overflow-y-auto relative bg-slate-900">
  <!-- Dashboard -->
  <div in:fade={{ duration: 300 }} class="p-6 space-y-8">
    <div class="space-y-2">
      <h2 class="text-2xl font-semibold text-white">Recently Played</h2>
    </div>

    <div class="grid grid-cols-2 gap-4">
      <!-- Breathe Card -->
      <a href="/breathe"
        class="bg-sky-500/10 hover:bg-sky-500/20 aspect-square rounded-2xl border border-white/5 flex flex-col items-center justify-center gap-3 transition-all active:scale-95 group"
      >
        <div class="group-hover:scale-110 transition-transform duration-300">
            <Wind size={32} class="text-sky-400" />
        </div>
        <span class="font-medium text-slate-200">Breathe</span>
      </a>

      <!-- Switch Card -->
      <a 
        href="/switch"
        class="bg-emerald-500/10 hover:bg-emerald-500/20 aspect-square rounded-2xl border border-white/5 flex flex-col items-center justify-center gap-3 transition-all active:scale-95 group"
      >
        <div class="group-hover:scale-110 transition-transform duration-300">
            <ToggleLeft size={32} class="text-emerald-400" />
        </div>
        <span class="font-medium text-slate-200">Switch</span>
      </a>

      <!-- Pop It Card -->
      <a href="/popIt"
        class="bg-rose-500/10 hover:bg-rose-500/20 aspect-square rounded-2xl border border-white/5 flex flex-col items-center justify-center gap-3 transition-all active:scale-95 group"
      >
        <div class="group-hover:scale-110 transition-transform duration-300">
            <Grid3x3 size={32} class="text-rose-400" />
        </div>
        <span class="font-medium text-slate-200">Pop It</span>
      </a>
    </div>

    <!-- Streak Widget -->
    <div class="bg-slate-800/50 p-4 rounded-xl border border-slate-700">
        <!-- <h3 class="text-sm font-medium text-slate-300 "> -->
        <div class="flex items-center gap-1 text-lg font-bold mb-2">
          Activity Streak <Flame size={26} class="text-rose-400" /> 
        </div>

        <div class="flex gap-2">
          {#each Array(7) as _, i}
            <div class="h-8 flex-1 rounded-md {i < 4 ? 'bg-teal-500' : 'bg-slate-700'}"></div>
          {/each}
        </div>
    </div>
  </div>
</main>
