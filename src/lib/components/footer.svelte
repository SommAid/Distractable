<script lang="ts">
  import { Vibrate, VibrateOff, Grid3x3, House, Volume2, VolumeOff } from 'lucide-svelte';

  let isVibrateOn = true;
  let isSoundOn = true;

  // toggle functions
  function toggleVibrate() {
    isVibrateOn = !isVibrateOn;
  }
  
  function toggleSound() {
    isSoundOn = !isSoundOn;
  }
</script>

<footer class="h-20 bg-slate-900 border-t border-slate-800 grid grid-cols-4 items-center pb-2 z-10">
  <a 
    href="/" 
    class="flex flex-col items-center justify-center gap-1 transition-all duration-300 text-slate-500 hover:text-teal-400 hover:scale-105"
  >
    <House size={24} />
    <span class="text-xs font-medium">Home</span>
  </a>

  <a href="/tools"
    class="flex flex-col items-center justify-center gap-1 transition-all duration-300 text-slate-500 hover:text-teal-400 hover:scale-105"
  >
    <Grid3x3 size={24} />
    <span class="text-xs font-medium">Tools</span>
  </a>

  <button 
    on:click={toggleVibrate}
    class="flex flex-col items-center justify-center gap-1 transition-all duration-300 text-slate-500 hover:text-teal-400 hover:scale-105">
    
    <svelte:component this={isVibrateOn ? Vibrate : VibrateOff} size={24} />
    
    <span class="text-xs font-medium">
        {isVibrateOn ? 'Vibrate' : 'Silent'}
    </span>
  </button>

  <button 
    on:click={toggleSound}
    class="flex flex-col items-center justify-center gap-1 transition-all duration-300 text-slate-500 hover:text-teal-400 hover:scale-105">
    
    <svelte:component this={isSoundOn ? Volume2 : VolumeOff} size={24} />

    <span class="text-xs font-medium">
        {isSoundOn ? 'Sound' : 'Mute'}
    </span>
  </button>
</footer>