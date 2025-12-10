<script>
  import { fade } from 'svelte/transition';
  import { Wind, ToggleLeft, Grid3x3, Timer, Blend, Fan, Shell } from 'lucide-svelte';
</script>


<main class="flex-1 overflow-y-auto relative bg-slate-900">
  <!-- Dashboard -->
  <div in:fade={{ duration: 300 }} class="p-6 space-y-8">
    <div class="space-y-2">
      <h2 class="text-2xl font-semibold text-white">All Games:</h2>
    </div>

    <div class="grid grid-cols-2 gap-4">
      <!-- Buzz It Card -->
      <a href="/buzzIt"
        class="bg-amber-500/10 hover:bg-amber-500/20 aspect-square rounded-2xl border border-white/5 flex flex-col items-center justify-center gap-3 transition-all active:scale-95 group"
      >
        <div class="group-hover:scale-110 transition-transform duration-300">
            <Blend size={32} class="text-amber-400" />
        </div>
        <span class="font-medium text-slate-200">Buzz It</span>
      </a>

      <!-- Breathe Card -->
      <a href="/breathe"
        class="bg-sky-500/10 hover:bg-sky-500/20 aspect-square rounded-2xl border border-white/5 flex flex-col items-center justify-center gap-3 transition-all active:scale-95 group"
      >
        <div class="group-hover:scale-110 transition-transform duration-300">
            <Wind size={32} class="text-sky-400" />
        </div>
        <span class="font-medium text-slate-200">Breathe</span>
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

      <!-- Spinner Card -->
      <a href="/spinner"
        class="bg-purple-500/10 hover:bg-purple-500/20 aspect-square rounded-2xl border border-white/5 flex flex-col items-center justify-center gap-3 transition-all active:scale-95 group"
      >
        <div class="group-hover:scale-110 transition-transform duration-300">
            <Fan size={32} class="text-purple-400" />
        </div>
        <span class="font-medium text-slate-200">Spinner</span>
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

     <!-- Timer Card -->
     <a href="/timer"
        class="bg-pink-500/10 hover:bg-pink-500/20 aspect-square rounded-2xl border border-white/5 flex flex-col items-center justify-center gap-3 transition-all active:scale-95 group"
      >
        <div class="group-hover:scale-110 transition-transform duration-300">
            <Timer size={32} class="text-pink-400" />
        </div>
        <span class="font-medium text-slate-200">Timer</span>
      </a>

      <!-- Sample Final Card For Demo Only -->
      <a href="/tools"
        class="bg-gray-500/10 hover:bg-gray-500/20 aspect-square rounded-2xl border border-white/5 flex flex-col items-center justify-center gap-3 transition-all active:scale-95 group"
      >
        <div class="group-hover:scale-110 transition-transform duration-300">
            <Shell size={32} class="text-gray-400" />
        </div>
        <span class="font-medium text-slate-200">Placeholder (Demo Only)</span>
      </a>
    </div>
  </div>
</main>
