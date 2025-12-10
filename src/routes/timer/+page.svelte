<script lang="ts">
  import { onDestroy } from 'svelte';
  import { Play, Pause, RotateCcw } from 'lucide-svelte';
  import { scale } from 'svelte/transition';

  // State
  let timeLeft = 60 * 5; // Default 5 mins
  let initialTime = 0;
  let isActive = false;
  let interval: any;

  // Formatting
  $: minutes = Math.floor(timeLeft / 60);
  $: seconds = timeLeft % 60;
  $: formattedTime = `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
  
  // Progress Ring Calc
  // r=125 = circumference ~ 786

  $: progress = initialTime > 0 ? ((initialTime - timeLeft) / initialTime) * 100 : 0;
  $: dashOffset = 786 - (786 * progress) / 100; // Inverse for countdown style

  function toggleTimer() {
    if (isActive) {
      pauseTimer();
    } else {
      startTimer();
    }
  }

  function startTimer() {
    if (timeLeft === 0) return;
    isActive = true;
    interval = setInterval(() => {
      if (timeLeft > 0) {
        timeLeft -= 1;
      } else {
        completeTimer();
      }
    }, 1000);
  }

  function pauseTimer() {
    isActive = false;
    clearInterval(interval);
  }

  function resetTimer() {
    pauseTimer();
    initialTime = 0;
    timeLeft = initialTime;
  }

  function setTime(mins: number) {
    pauseTimer();
    initialTime += mins * 60;
    initialTime = initialTime > 0 ? initialTime : 0;
    timeLeft = initialTime;
  }

  function completeTimer() {
    pauseTimer();
    // Vibrate to signal end
    if (typeof navigator !== 'undefined' && navigator.vibrate) {
        navigator.vibrate([200, 100, 200]);
    }
  }

  onDestroy(() => {
    if (interval) clearInterval(interval);
  });
</script>

<div class="w-full h-screen bg-slate-900 text-slate-100 flex flex-col relative overflow-hidden">
    <div class="flex-1 flex flex-col items-center justify-center space-y-12">
      
      <!-- Timer Display -->
      <div class="relative w-72 h-72 flex items-center justify-center" in:scale>
        <!-- Background Ring -->
        <svg class="w-full h-full transform -rotate-90 drop-shadow-2xl">
            <!-- Track -->
            <circle cx="50%" cy="50%" r="125" class="stroke-slate-800" stroke-width="8" fill="transparent" />
            
            <!-- Progress -->
            <circle 
                cx="50%" cy="50%" r="125" 
                class="stroke-pink-500 transition-all duration-1000 ease-linear {timeLeft === 0 ? 'stroke-emerald-400' : ''}"
                stroke-width="8" 
                fill="transparent" 
                stroke-dasharray="786"
                stroke-dashoffset={-dashOffset}
                stroke-linecap="round"
            />
        </svg>

        <!-- Digital Time -->
        <div class="absolute flex flex-col items-center">
            <span class="text-6xl font-mono font-bold text-white tabular-nums tracking-tighter">
                {formattedTime}
            </span>
            <span class="text-xs text-slate-500 uppercase tracking-widest mt-2 font-medium">
                {#if timeLeft === 0}
                    <span class="text-emerald-400">Done!</span>
                {:else if isActive}
                    <span class="text-pink-400 opacity-0">Working</span>
                {:else}
                    Paused
                {/if}
            </span>
        </div>
      </div>

      <!-- Controls -->
      <div class="flex flex-col items-center gap-8 w-full px-8">
        
        <!-- Main Buttons -->
        <div class="flex gap-6 items-center">
            <button 
                on:click={toggleTimer}
                class="w-54 h-20 bg-pink-500 rounded-full flex items-center justify-center text-white shadow-lg shadow-pink-500/30 active:scale-95 transition-all hover:bg-pink-400"
            >
                {#if isActive}
                    <Pause size={32} fill="currentColor" />
                {:else}
                    <Play size={32} fill="currentColor" class="ml-2" />
                {/if}
            </button>
            
            <button 
                on:click={resetTimer}
                class="w-20 h-20 bg-slate-800 rounded-full flex items-center justify-center text-slate-400 border border-slate-700 active:scale-95 transition-all hover:text-white hover:border-slate-500"
            >
                <RotateCcw size={20} />
            </button>
        </div>

        <!-- Presets -->
        <div class="grid grid-cols-4 gap-3 w-full max-w-xs">
            {#each [1, 5, 15, 30] as min}
                <button 
                    on:click={() => setTime(min)}
                    class="py-3 rounded-xl text-sm font-bold transition-all border bg-slate-800 border-slate-700 text-slate-400 hover:bg-pink-500/20 hover:text-pink-400 hover:border-slate-600"
                >
                    +{min}m
                </button>
            {/each}
            {#each [-1, -5, -15, -30] as min}
                <button 
                    on:click={() => setTime(min)}
                    class="py-3 rounded-xl text-sm font-bold transition-all border bg-slate-800 border-slate-700 text-slate-400 hover:bg-pink-500/20 hover:text-pink-400 hover:border-slate-600"
                >
                    {min}m
                </button>
            {/each}
        </div>
      </div>
  </div>
</div>