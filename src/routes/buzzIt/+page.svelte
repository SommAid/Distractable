<script>
  import { scale } from 'svelte/transition';
  import { Smartphone, Zap, RotateCcw, Frown } from 'lucide-svelte';

  let gameState = "start"
  let score = 0
  let highScore = 15
  let currentCommand = null

  // Timers
  let actionTimer
  let gameLoopTimeout

  // Visuals
  let isShaking = false

  function startGame() {
    score = 0
    gameState = "waiting"
    nextRound()
  }

  function nextRound() {
    // Reset
    currentCommand = null
    isShaking = false
    gameState = "waiting"

    // Random delay before action (1s to 2s)
    const delay = Math.random() * 1000 + 1000

    gameLoopTimeout = setTimeout(() => {
      triggerAction()
    }, delay)
  }

  function triggerAction() {
    gameState = "action"

    // Choose Command
    currentCommand = Math.random() > 0.5 ? "tap" : "flip"
    isShaking = true

    if (typeof navigator !== "undefined" && navigator.vibrate) {
      if (currentCommand === "tap") navigator.vibrate(100) // Short buzz
      else navigator.vibrate(1000) // Long buzz
    }

    const timeLimit = Math.max(800, 2000 - score * 100)

    actionTimer = setTimeout(() => {
      endGame()
    }, timeLimit)
  }

  function handleInput(type) {
    if (gameState !== "action") return

    if (type === currentCommand) {
      clearTimeout(actionTimer)
      gameState = "success"
      score++
      if (score > highScore) highScore = score

      setTimeout(() => nextRound(), 500)
    } else {
      endGame()
    }
  }

  function endGame() {
    clearTimeout(actionTimer)
    clearTimeout(gameLoopTimeout)
    isShaking = false
    gameState = "gameover"

    if (typeof navigator !== "undefined" && navigator.vibrate) {
      navigator.vibrate([100, 50, 100])
    }
  }
</script>

<div class="w-full h-screen bg-slate-900 text-slate-100 font-sans flex flex-col relative overflow-hidden select-none">
  {#if gameState === 'waiting' || gameState === 'action' || gameState === 'success'}
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

  <div class="flex-1 flex flex-col items-center justify-center p-8 z-10 pointer-events-none">
    
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

    {:else if gameState !== 'gameover'}
      <div class="text-center space-y-12">
        <div class="text-8xl font-black text-slate-800">{score}</div>

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