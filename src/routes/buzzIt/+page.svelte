<script lang="ts">
  import { scale } from 'svelte/transition';
  import { Smartphone, Zap, RotateCcw, Frown } from 'lucide-svelte';

  // Game State
  type GameState = 'start' | 'waiting' | 'action' | 'success' | 'gameover';
  let gameState: GameState = 'start';
  let score = 0;
  let highScore = 23;
  let currentCommand: 'tap' | 'flip' | null = null;
  
  // Timers
  let actionTimer: any;
  let gameLoopTimeout: any;

  // Visuals
  let isShaking = false;

  function startGame() {
    score = 0;
    gameState = 'waiting';
    nextRound();
  }

  function nextRound() {
    // 1. Reset
    currentCommand = null;
    isShaking = false;
    gameState = 'waiting';

    // 2. Random delay before action (1s to 3s)
    const delay = Math.random() * 2000 + 1000;
    
    gameLoopTimeout = setTimeout(() => {
      triggerAction();
    }, delay);
  }

  function triggerAction() {
    gameState = 'action';
    
    // Randomly choose command
    currentCommand = Math.random() > 0.5 ? 'tap' : 'flip';
    isShaking = true;

    // SIMULATE HAPTICS (If supported on mobile)
    if (typeof navigator !== 'undefined' && navigator.vibrate) {
        if (currentCommand === 'tap') navigator.vibrate(100); // Short buzz
        else navigator.vibrate(1000); // Long buzz
    }

    // Time limit gets shorter as score increases
    const timeLimit = Math.max(800, 2000 - (score * 100));

    actionTimer = setTimeout(() => {
      endGame();
    }, timeLimit);
  }

  function handleInput(type: 'tap' | 'flip') {
    if (gameState !== 'action') return;

    if (type === currentCommand) {
      // Success
      clearTimeout(actionTimer);
      gameState = 'success';
      score++;
      if (score > highScore) highScore = score;
      
      // Brief pause before next round
      setTimeout(() => nextRound(), 500);
    } else {
      // Wrong input
      endGame();
    }
  }

  function endGame() {
    clearTimeout(actionTimer);
    clearTimeout(gameLoopTimeout);
    isShaking = false;
    gameState = 'gameover';
    
    // Error Haptic
    if (typeof navigator !== 'undefined' && navigator.vibrate) {
        navigator.vibrate([100, 50, 100]); 
    }
  }
</script>

<div class="w-full h-screen bg-slate-900 text-slate-100 font-sans flex flex-col relative overflow-hidden select-none">
  <!-- SIMULATION ZONES (Only visible/active during game) -->
  {#if gameState === 'waiting' || gameState === 'action' || gameState === 'success'}
    <!-- Top Right: FLIP ZONE -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div 
      on:click={() => handleInput('flip')}
      class="absolute top-0 right-0 w-1/2 h-1/3 z-20 flex justify-end items-start p-6
             bg-gradient-to-bl from-amber-500/10 to-transparent
             active:bg-amber-500/30 transition-colors touch-manipulation"
    >
      <span class="text-xs font-bold text-amber-500/50 uppercase tracking-widest pointer-events-none">
        Click to Flip
      </span>
    </div>

    <!-- Bottom Left: TAP ZONE -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div 
      on:click={() => handleInput('tap')}
      class="absolute bottom-0 left-0 w-1/2 h-1/3 z-20 flex justify-start items-end p-6
             bg-gradient-to-tr from-teal-500/10 to-transparent
             active:bg-teal-500/30 transition-colors touch-manipulation"
    >
      <span class="text-xs font-bold text-teal-500/50 uppercase tracking-widest pointer-events-none">
        Click to Press
      </span>
    </div>
  {/if}

  <!-- Main Game Display -->
  <div class="flex-1 flex flex-col items-center justify-center p-8 z-10 pointer-events-none">
    
    <!-- START SCREEN -->
    {#if gameState === 'start'}
      <div in:scale class="text-center space-y-8 pointer-events-auto">
        <div class="relative inline-block">
          <Smartphone size={80} class="text-slate-600" />
          <Zap size={32} class="absolute -top-2 -right-2 text-amber-400 animate-bounce" />
        </div>
        
        <div class="space-y-4">
          <h1 class="text-4xl font-bold text-white tracking-tight">Buzz It!</h1>
          <div class="bg-slate-800 p-4 rounded-xl text-left space-y-3 shadow-xl border border-slate-700">
            <div class="flex items-center gap-3">
              <span class="text-teal-400 font-bold">Short buzz</span> 
              <span class="text-slate-400 text-sm">→ Tap Bottom Left</span>
            </div>
            <div class="flex items-center gap-3">
              <span class="text-amber-400 font-bold">Long buzz</span> 
              <span class="text-slate-400 text-sm">→ Tap Top Right</span>
            </div>
          </div>
        </div>

        <button 
          on:click={startGame}
          class="w-full bg-gradient-to-r from-teal-500 to-emerald-500 text-white font-bold py-4 rounded-xl shadow-lg shadow-teal-500/20 active:scale-95 transition-transform"
        >
          Start Game
        </button>
      </div>

    <!-- PLAYING SCREEN -->
    {:else if gameState !== 'gameover'}
      <div class="text-center space-y-12">
        <!-- Score -->
        <div class="text-8xl font-black text-slate-800">{score}</div>

        <!-- Indicator -->
        <div class="relative h-32 flex items-center justify-center">
            {#if gameState === 'waiting'}
                <div class="text-slate-500 animate-pulse text-sm font-medium tracking-widest">WAIT FOR IT...</div>
            
            {:else if gameState === 'action'}
                {#if currentCommand === 'tap'}
                    <div in:scale={{duration:100}} class="flex flex-col items-center">
                        <span class="text-4xl font-black text-teal-400 mb-4 animate-bounce">bzz!</span>
                        <Smartphone size={64} class="text-teal-500/50" />
                    </div>
                {:else}
                    <div in:scale={{duration:100}} class="flex flex-col items-center">
                        <span class="text-4xl font-black text-amber-400 mb-4 animate-pulse">BZZZZZZ!</span>
                        <Smartphone size={64} class="text-amber-500/50 rotate-12" />
                    </div>
                {/if}
            
            {:else if gameState === 'success'}
                <div in:scale={{duration:150}} class="text-emerald-400 font-bold text-2xl">Good!</div>
            {/if}
        </div>
      </div>

    <!-- GAME OVER SCREEN -->
    {:else}
      <div in:scale class="text-center space-y-8 pointer-events-auto bg-slate-800 p-8 rounded-3xl border border-slate-700 shadow-2xl">
        <div class="flex flex-col items-center space-y-2">
          <h2 class="text-3xl font-bold text-white">Game Over</h2>
          <Frown class="text-red-400" size={36}/> 
        </div>

        <div class="flex justify-center gap-8 text-center">
            <div>
                <div class="text-xs text-slate-500 uppercase tracking-wider">Score</div>
                <div class="text-4xl font-bold text-white">{score}</div>
            </div>
            <div>
                <div class="text-xs text-slate-500 uppercase tracking-wider">Best</div>
                <div class="text-4xl font-bold text-amber-400">{highScore}</div>
            </div>
        </div>

        <button 
          on:click={startGame}
          class="flex items-center justify-center gap-2 w-full bg-slate-700 hover:bg-slate-600 text-white font-bold py-4 rounded-xl transition-colors"
        >
          <RotateCcw size={20} /> Try Again
        </button>
      </div>
    {/if}
  </div>
</div>