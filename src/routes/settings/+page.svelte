<script lang="js">
  import { fade, slide } from 'svelte/transition';
  import { Settings, Volume2, Smartphone, Grid3x3, ToggleLeft, Wind, Zap, Activity } from 'lucide-svelte';
  
  // Mock Data for View
  let settings = {
    vibration: 80,
    sound: 60
  };

  let stats = {
    pops: 1243,
    switches: 567,
    breaths: 42,
    buzzHighScore: 15
  };

  function resetStats() {
    stats = {
        pops: 0,
        switches: 0,
        breaths: 0,
        buzzHighScore: 0
    };
  }
</script>

<main class="flex-1 relative bg-slate-900 text-slate-100 p-6 pb-24 overflow-y-auto">
  <div in:fade={{ duration: 300 }} class="space-y-8 max-w-md mx-auto">
    <!-- Preferences Section -->
    <section class="space-y-6">
      <h3 class="text-sm font-bold text-slate-500 uppercase tracking-wider flex items-center gap-2">
        <Activity size={16} /> Preferences
      </h3>
      
      <div class="bg-slate-800/50 p-6 rounded-3xl border border-slate-700 space-y-8 backdrop-blur-sm">
          
          <!-- Vibration Slider -->
          <div class="space-y-4">
              <div class="flex justify-between items-center text-slate-300">
                  <div class="flex items-center gap-3">
                      <Smartphone size={20} class="text-slate-400" />
                      <span class="font-medium">Haptic Strength</span>
                  </div>
                  <span class="text-teal-400 font-mono bg-teal-500/10 px-2 py-1 rounded text-sm">{settings.vibration}%</span>
              </div>
              <input 
                  type="range" 
                  min="0" max="100" 
                  bind:value={settings.vibration}
                  class="w-full h-2 bg-slate-700 rounded-lg appearance-none cursor-pointer accent-teal-500 hover:accent-teal-400 transition-all"
              />
          </div>

          <div class="h-px bg-slate-700/50"></div>

          <!-- Sound Slider -->
          <div class="space-y-4">
              <div class="flex justify-between items-center text-slate-300">
                  <div class="flex items-center gap-3">
                      <Volume2 size={20} class="text-slate-400" />
                      <span class="font-medium">Sound Volume</span>
                  </div>
                  <span class="text-teal-400 font-mono bg-teal-500/10 px-2 py-1 rounded text-sm">{settings.sound}%</span>
              </div>
              <input 
                  type="range" 
                  min="0" max="100" 
                  bind:value={settings.sound}
                  class="w-full h-2 bg-slate-700 rounded-lg appearance-none cursor-pointer accent-teal-500 hover:accent-teal-400 transition-all"
              />
          </div>
      </div>
    </section>

    <!-- Stats Section -->
    <section class="space-y-6">
      <h3 class="text-sm font-bold text-slate-500 uppercase tracking-wider">Lifetime Stats</h3>
      
      <div class="grid grid-cols-2 gap-4">
          <!-- Pop Stats -->
          <div class="bg-slate-800 p-5 rounded-2xl border border-slate-700 flex flex-col gap-2 relative overflow-hidden group">
              <div class="absolute top-0 right-0 p-4 opacity-10 group-hover:opacity-20 transition-opacity">
                  <Grid3x3 size={48} />
              </div>
              <div class="flex items-center gap-2 text-rose-400 mb-1">
                  <Grid3x3 size={18} />
                  <span class="text-xs font-bold uppercase tracking-wider opacity-90">Pops</span>
              </div>
              <span class="text-3xl font-mono text-white font-light tracking-tight">{stats.pops}</span>
          </div>

          <!-- Switch Stats -->
          <div class="bg-slate-800 p-5 rounded-2xl border border-slate-700 flex flex-col gap-2 relative overflow-hidden group">
              <div class="absolute top-0 right-0 p-4 opacity-10 group-hover:opacity-20 transition-opacity">
                  <ToggleLeft size={48} />
              </div>
              <div class="flex items-center gap-2 text-emerald-400 mb-1">
                  <ToggleLeft size={18} />
                  <span class="text-xs font-bold uppercase tracking-wider opacity-90">Switches</span>
              </div>
              <span class="text-3xl font-mono text-white font-light tracking-tight">{stats.switches}</span>
          </div>

          <!-- Breathe Stats -->
          <div class="bg-slate-800 p-5 rounded-2xl border border-slate-700 flex flex-col gap-2 relative overflow-hidden group">
              <div class="absolute top-0 right-0 p-4 opacity-10 group-hover:opacity-20 transition-opacity">
                  <Wind size={48} />
              </div>
              <div class="flex items-center gap-2 text-sky-400 mb-1">
                  <Wind size={18} />
                  <span class="text-xs font-bold uppercase tracking-wider opacity-90">Breaths</span>
              </div>
              <span class="text-3xl font-mono text-white font-light tracking-tight">{stats.breaths}</span>
          </div>

          <!-- Buzz High Score -->
          <div class="bg-slate-800 p-5 rounded-2xl border border-slate-700 flex flex-col gap-2 relative overflow-hidden group">
              <div class="absolute top-0 right-0 p-4 opacity-10 group-hover:opacity-20 transition-opacity">
                  <Zap size={48} />
              </div>
              <div class="flex items-center gap-2 text-amber-400 mb-1">
                  <Zap size={18} />
                  <span class="text-xs font-bold uppercase tracking-wider opacity-90">Best Buzz</span>
              </div>
              <span class="text-3xl font-mono text-white font-light tracking-tight">{stats.buzzHighScore}</span>
          </div>
      </div>
    </section>
    
    <!-- Additional Actions -->
    <div class="pt-8 text-center pb-8">
       <button 
          on:click={resetStats}
          class="text-xs text-slate-500 hover:text-red-400 transition-colors uppercase tracking-widest font-semibold px-4 py-2 hover:bg-red-500/10 rounded-lg"
       >
          Reset All Statistics
       </button>
       <p class="mt-4 text-[10px] text-slate-600">v1.0.0 • Fidgetly Mobile</p>
    </div>

  </div>
</main>