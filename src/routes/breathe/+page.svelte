<script>
  import { scale } from 'svelte/transition';
  import { } from 'lucide-svelte';
    
  let breathingState = 'Inhale';
  let breatheScale = 1.5;
  let breatheInterval

  function startBreathing() {
    // Oscillation
    breatheInterval = setInterval(() => {
        if (breathingState === 'Inhale') {
            breathingState = 'Exhale';
            breatheScale = 1;
        } else {
            breathingState = 'Inhale';
            breatheScale = 1.5;
        }
    }, 4000);
  }
  startBreathing();

</script>


<div in:scale={{ duration: 300, start: 0.95 }} class="h-full flex flex-col items-center justify-center space-y-12">
    <div 
        class="w-48 h-48 rounded-full bg-sky-500/20 border-2 border-sky-400 blur-sm flex items-center justify-center transition-transform duration-[4000ms] ease-in-out relative"
        style="transform: scale({breatheScale})"
    >
        <div class="absolute w-full h-full bg-sky-400/10 rounded-full animate-pulse"></div>
    </div>
    <div class="text-center space-y-2 z-10">
        <h2 class="text-3xl font-light text-sky-100 tracking-widest uppercase">{breathingState}</h2>
        <p class="text-slate-500 text-sm">Match your breath to the circle.</p>
    </div>
    <a href="/" class="text-slate-400 hover:text-white text-sm mt-8">Close Tool</a>
</div>