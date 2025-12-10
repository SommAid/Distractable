<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  import { FingerprintPattern } from 'lucide-svelte';

  // State
  let rotation = 0;
  let velocity = 0;
  let isDragging = false;
  let lastAngle = 0;
  let spinnerElement: HTMLElement;
  let animationFrame: number;
  let spinsCounted = 0; // Local session tracking
  export let showHint = true;

  // Helper to calculate angle between center of spinner and touch point
  function getAngle(clientX: number, clientY: number) {
    if (!spinnerElement) return 0;
    const rect = spinnerElement.getBoundingClientRect();
    const cx = rect.left + rect.width / 2;
    const cy = rect.top + rect.height / 2;
    return Math.atan2(clientY - cy, clientX - cx) * (180 / Math.PI);
  }

  // --- Interaction Handlers ---

  function handleStart(clientX: number, clientY: number) {
    isDragging = true;
    lastAngle = getAngle(clientX, clientY);
    velocity = 0; // Stop momentum when grabbing
  }

  function handleMove(clientX: number, clientY: number) {
    if (!isDragging) return;
    
    const currentAngle = getAngle(clientX, clientY);
    let delta = currentAngle - lastAngle;
    
    // Handle wrap-around (e.g. going from 359 to 1 degrees)
    if (delta > 180) delta -= 360;
    if (delta < -180) delta += 360;

    rotation += delta;
    velocity = delta; // Velocity is just the difference in this frame
    lastAngle = currentAngle;
  }

  function handleEnd() {
    isDragging = false;
    // Add haptic feedback if velocity is high
    if (Math.abs(velocity) > 5 && typeof navigator !== 'undefined' && navigator.vibrate) {
        navigator.vibrate(50);
        spinsCounted++; 
    }
  }

  // --- Mouse Wrappers ---
  const onMouseDown = (e: MouseEvent) => handleStart(e.clientX, e.clientY);
  const onMouseMove = (e: MouseEvent) => handleMove(e.clientX, e.clientY);
  
  // --- Touch Wrappers ---
  const onTouchStart = (e: TouchEvent) => {
      // Prevent scrolling while spinning
      e.preventDefault(); 
      handleStart(e.touches[0].clientX, e.touches[0].clientY);
  };
  const onTouchMove = (e: TouchEvent) => {
      e.preventDefault();
      handleMove(e.touches[0].clientX, e.touches[0].clientY);
  };

  // --- Physics Loop ---
  function animate() {
    if (!isDragging) {
      if (Math.abs(velocity) > 0.1) {
        rotation += velocity;
        velocity *= 0.98; // Friction factor (0.98 = slippery, 0.90 = sticky)
      } else {
        velocity = 0;
      }
    }
    animationFrame = requestAnimationFrame(animate);
  }

  onMount(() => {
    animationFrame = requestAnimationFrame(animate);
  });

  onDestroy(() => {
    cancelAnimationFrame(animationFrame);
  });
</script>

<div class="w-full h-screen bg-slate-900 text-slate-100 overflow-hidden flex flex-col relative touch-none select-none">
  <!-- Main Area -->
  <div 
    class="flex-1 flex flex-col items-center justify-center space-y-16"
    on:mousemove={onMouseMove}
    on:mouseup={handleEnd}
    on:mouseleave={handleEnd}
    on:touchmove={onTouchMove}
    on:touchend={handleEnd}
    role="application"
  >
    <div class="swipe-motion flex items-center gap-1 text-lg font-bold mb-2 text-slate-500 text-sm transition-opacity duration-500 {showHint ? 'opacity-100' : 'opacity-0'}">
        Swipe to spin
        <FingerprintPattern class="text-emerald-400"/>
    </div>
    <!-- Spinner Object -->
    <div 
       bind:this={spinnerElement}
       on:mousedown={onMouseDown}
       on:touchstart={onTouchStart}
       style="transform: rotate({rotation}deg)"
       class="relative w-72 h-72 cursor-grab active:cursor-grabbing will-change-transform drop-shadow-2xl"
    >
       <!-- Classic Tri-Spinner SVG -->
       <svg viewBox="-100 -100 200 200" class="w-full h-full">
          <!-- Defs for gradients -->
          <defs>
            <radialGradient id="bearingGrad" cx="0.5" cy="0.5" r="0.5">
              <stop offset="0%" stop-color="#334155" />
              <stop offset="90%" stop-color="#0f172a" />
              <stop offset="100%" stop-color="#94a3b8" />
            </radialGradient>
            <linearGradient id="bodyGrad" x1="0" y1="0" x2="1" y2="1">
              <stop offset="0%" stop-color="#ef4444" />
              <stop offset="100%" stop-color="#b91c1c" />
            </linearGradient>
          </defs>

          <!-- Main Body: Uses a thick stroke on a triangle to create the tri-lobe shape -->
          <path 
            d="M 0 -60 L 52 30 L -52 30 Z" 
            stroke="url(#bodyGrad)" 
            stroke-width="75" 
            stroke-linejoin="round" 
            stroke-linecap="round"
            fill="url(#bodyGrad)" 
          />

          <!-- Outer Bearings (Weights) -->
          <g fill="url(#bearingGrad)" stroke="#cbd5e1" stroke-width="3">
            <circle cx="0" cy="-60" r="18" />
            <circle cx="52" cy="30" r="18" />
            <circle cx="-52" cy="30" r="18" />
          </g>

          <!-- Center Cap -->
          <circle cx="0" cy="0" r="22" fill="#f8fafc" stroke="#e2e8f0" stroke-width="2" />
          <circle cx="0" cy="0" r="12" fill="#e2e8f0" />
       </svg>
    </div>

    <!-- Stats Display -->
    <a href="/" class="text-slate-400 hover:text-white text-sm">Close Tool</a>
  </div>
</div>

<style>
    @keyframes swipe-right-fade {
        0% {
            transform: translateX(0) translateY(0);
            opacity: 1;
        }
        60% {
            opacity: 1; /* Keep opacity full for most of the movement */
        }
        100% {
            transform: translateX(60px) translateY(-20px);
            opacity: 0;
        }
    }

    .swipe-motion {
        animation: swipe-right-fade 2.5s ease-out forwards;
    }
</style>