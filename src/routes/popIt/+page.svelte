<script lang="ts">
  import { fly } from 'svelte/transition';

  // Pop Tool State
  let popped = new Array(16).fill(false);
  function togglePop(index: number) {
      popped[index] = !popped[index];
  }
  function resetPop() {
    popped = new Array(16).fill(false);
  }
</script>

<div in:fly={{ x: 50, duration: 300 }} class="h-full flex flex-col items-center justify-center space-y-8">
    <div class="grid grid-cols-4 gap-4 p-6 bg-slate-800 rounded-3xl shadow-inner border border-slate-700">
        {#each popped as isPopped, i}
        <button
            on:click={() => togglePop(i)}
            class="w-12 h-12 rounded-full shadow-lg transition-all duration-150 {isPopped ? 'bg-rose-900/50 shadow-inner scale-95 border border-rose-900' : 'bg-rose-500 border-b-4 border-rose-700 hover:brightness-110 active:border-b-0 active:translate-y-1'}"
        />
        {/each}
    </div>
    <div class="flex gap-4">
        <button on:click={resetPop} class="px-4 py-2 bg-slate-800 rounded-lg text-sm text-slate-300 hover:text-rose-400 hover:scale-105">Reset</button>
        <a href="/" class="px-4 py-2 text-slate-400 hover:text-white text-sm hover:text-teal-400 hover:scale-105">Close Tool</a>
    </div>
</div>

