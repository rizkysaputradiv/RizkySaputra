<!DOCTYPE html>
<html lang="id" class="scroll-smooth">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>iOS 26 Liquid Bubble Experience — Antarmuka Masa Depan yang Mengalir</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Plus+Jakarta+Sans:wght@500;600;700;800&display=swap" rel="stylesheet" />

  <style>
    /* ==========================================================================
       CSS VARIABLES & iOS 26 LIQUID GLASS DESIGN TOKENS
       ========================================================================== */
    :root {
      --glass-blur: 28px;
      --glass-specular: 0.62;
      --spring-bounce: 0.65;
      --spring-easing: cubic-bezier(0.34, 1.56, 0.64, 1);
      --smooth-easing: cubic-bezier(0.22, 1, 0.36, 1);

      /* Aurora Gelap (Dark Mode - Default) */
      --bg-base: #040711;
      --text-primary: #f8fafc;
      --text-secondary: rgba(226, 232, 240, 0.74);
      --text-muted: rgba(148, 163, 184, 0.72);

      --glass-bg: rgba(255, 255, 255, 0.065);
      --glass-bg-elevated: rgba(255, 255, 255, 0.11);
      --glass-bg-dock: rgba(15, 23, 42, 0.48);
      --glass-border: rgba(255, 255, 255, 0.18);
      --glass-shadow: 0 24px 48px -12px rgba(0, 0, 0, 0.55), 0 4px 16px -2px rgba(0, 0, 0, 0.3);
      --specular-color: 255, 255, 255;
      --chromatic-1: rgba(56, 189, 248, 0.45);
      --chromatic-2: rgba(168, 85, 247, 0.45);
      --chromatic-3: rgba(255, 255, 255, 0.65);

      --aurora-1: #1e1b4b;
      --aurora-2: #0369a1;
      --aurora-3: #6d28d9;
      --aurora-4: #0891b2;
      --accent-tint: rgba(56, 189, 248, 0.25);
    }

    /* Kristal Siang (Light Frost Mode) */
    [data-theme="light"] {
      --bg-base: #eef4fb;
      --text-primary: #0f172a;
      --text-secondary: rgba(30, 41, 59, 0.78);
      --text-muted: rgba(71, 85, 105, 0.75);

      --glass-bg: rgba(255, 255, 255, 0.46);
      --glass-bg-elevated: rgba(255, 255, 255, 0.68);
      --glass-bg-dock: rgba(255, 255, 255, 0.62);
      --glass-border: rgba(255, 255, 255, 0.78);
      --glass-shadow: 0 20px 40px -15px rgba(15, 23, 42, 0.12), 0 4px 14px -2px rgba(56, 189, 248, 0.1);
      --specular-color: 255, 255, 255;
      --chromatic-1: rgba(14, 165, 233, 0.45);
      --chromatic-2: rgba(139, 92, 246, 0.4);
      --chromatic-3: rgba(255, 255, 255, 0.95);

      --aurora-1: #bae6fd;
      --aurora-2: #ddd6fe;
      --aurora-3: #a5f3fc;
      --aurora-4: #fbcfe8;
      --accent-tint: rgba(14, 165, 233, 0.18);
    }

    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
      background-color: var(--bg-base);
      color: var(--text-primary);
      overflow-x: hidden;
      transition: background-color 0.7s var(--smooth-easing), color 0.5s var(--smooth-easing);
      min-height: 100vh;
    }

    h1, h2, h3, h4, .font-display {
      font-family: 'Plus Jakarta Sans', 'Inter', sans-serif;
    }

    /* ==========================================================================
       ORGANIC ANIMATED AURORA MESH BACKGROUND
       ========================================================================== */
    .aurora-canvas {
      position: fixed;
      inset: 0;
      z-index: -1;
      overflow: hidden;
      pointer-events: none;
      background: var(--bg-base);
      transition: background 0.8s var(--smooth-easing);
    }

    .aurora-blob {
      position: absolute;
      border-radius: 50%;
      filter: blur(95px);
      opacity: 0.72;
      will-change: transform;
      transition: background 0.9s var(--smooth-easing), opacity 0.7s ease;
    }

    .aurora-blob-1 {
      width: 58vw;
      height: 58vw;
      max-width: 780px;
      max-height: 780px;
      top: -12%;
      left: -8%;
      background: radial-gradient(circle, var(--aurora-2) 0%, transparent 72%);
      animation: auroraFloat1 18s ease-in-out infinite alternate;
    }

    .aurora-blob-2 {
      width: 54vw;
      height: 54vw;
      max-width: 740px;
      max-height: 740px;
      top: 18%;
      right: -10%;
      background: radial-gradient(circle, var(--aurora-3) 0%, transparent 72%);
      animation: auroraFloat2 22s ease-in-out infinite alternate;
    }

    .aurora-blob-3 {
      width: 62vw;
      height: 62vw;
      max-width: 820px;
      max-height: 820px;
      bottom: -18%;
      left: 20%;
      background: radial-gradient(circle, var(--aurora-4) 0%, transparent 72%);
      animation: auroraFloat3 20s ease-in-out infinite alternate;
    }

    .aurora-blob-4 {
      width: 45vw;
      height: 45vw;
      max-width: 620px;
      max-height: 620px;
      top: 42%;
      left: 35%;
      background: radial-gradient(circle, var(--accent-tint) 0%, transparent 70%);
      animation: auroraFloat1 15s ease-in-out infinite alternate-reverse;
    }

    .aurora-pulse-active .aurora-blob {
      opacity: 0.94;
      filter: blur(82px);
    }

    @keyframes auroraFloat1 {
      0% { transform: translate3d(0, 0, 0) scale(1); }
      50% { transform: translate3d(6%, 8%, 0) scale(1.08); }
      100% { transform: translate3d(-4%, 12%, 0) scale(0.96); }
    }

    @keyframes auroraFloat2 {
      0% { transform: translate3d(0, 0, 0) scale(1.04); }
      50% { transform: translate3d(-8%, 6%, 0) scale(0.95); }
      100% { transform: translate3d(-3%, -9%, 0) scale(1.09); }
    }

    @keyframes auroraFloat3 {
      0% { transform: translate3d(0, 0, 0) scale(0.98); }
      50% { transform: translate3d(7%, -7%, 0) scale(1.07); }
      100% { transform: translate3d(-6%, -4%, 0) scale(1.02); }
    }

    /* Subtle noise grain overlay for tactile glass depth */
    .glass-grain {
      position: fixed;
      inset: 0;
      z-index: -1;
      pointer-events: none;
      opacity: 0.032;
      background-image: radial-gradient(rgba(255,255,255,0.8) 1px, transparent 0);
      background-size: 24px 24px;
    }

    /* ==========================================================================
       CORE LIQUID GLASS MATERIAL & SPECULAR REFRACTION SHADERS
       ========================================================================== */
    .liquid-glass {
      position: relative;
      background: var(--glass-bg);
      backdrop-filter: blur(var(--glass-blur)) saturate(185%);
      -webkit-backdrop-filter: blur(var(--glass-blur)) saturate(185%);
      border: 1px solid var(--glass-border);
      box-shadow:
        var(--glass-shadow),
        inset 0 1.5px 1.5px rgba(var(--specular-color), var(--glass-specular)),
        inset 0 -1.5px 2px rgba(var(--specular-color), calc(var(--glass-specular) * 0.25)),
        inset 1px 0 1.5px rgba(56, 189, 248, calc(var(--glass-specular) * 0.35)),
        inset -1px 0 1.5px rgba(168, 85, 247, calc(var(--glass-specular) * 0.35));
      transition:
        background-color 0.45s var(--smooth-easing),
        border-color 0.45s var(--smooth-easing),
        box-shadow 0.35s var(--smooth-easing),
        transform 0.5s var(--spring-easing);
      overflow: hidden;
    }

    /* Chromatic Refraction Rim Border */
    .liquid-glass::before {
      content: '';
      position: absolute;
      inset: 0;
      border-radius: inherit;
      padding: 1px;
      background: linear-gradient(
        135deg,
        rgba(255, 255, 255, calc(var(--glass-specular) * 0.85)) 0%,
        var(--chromatic-1) 32%,
        rgba(255, 255, 255, 0.05) 55%,
        var(--chromatic-2) 82%,
        rgba(255, 255, 255, calc(var(--glass-specular) * 0.7)) 100%
      );
      -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
      -webkit-mask-composite: xor;
      mask-composite: exclude;
      pointer-events: none;
      opacity: 0.85;
    }

    /* Real-time Cursor-Tracking Radial Specular Glint */
    .liquid-glass::after {
      content: '';
      position: absolute;
      inset: 0;
      border-radius: inherit;
      background: radial-gradient(
        340px circle at var(--glint-x, 50%) var(--glint-y, 20%),
        rgba(255, 255, 255, calc(var(--glass-specular) * 0.34)),
        rgba(125, 211, 252, calc(var(--glass-specular) * 0.1)) 38%,
        transparent 70%
      );
      opacity: var(--glint-opacity, 0.45);
      pointer-events: none;
      transition: opacity 0.35s ease;
    }

    .liquid-glass-interactive {
      cursor: pointer;
    }

    .liquid-glass-interactive:hover {
      background: var(--glass-bg-elevated);
      transform: translateY(-4px) scale(1.012);
    }

    .liquid-glass-interactive:active {
      transform: translateY(0px) scale(0.975);
      transition-duration: 0.15s;
    }

    /* ==========================================================================
       3D SPATIAL LIQUID BUBBLE STYLING
       ========================================================================== */
    .spatial-bubble {
      position: absolute;
      top: 0;
      left: 0;
      border-radius: 9999px;
      background: radial-gradient(
        130% 130% at 28% 24%,
        rgba(255, 255, 255, calc(var(--glass-specular) * 0.58)) 0%,
        rgba(255, 255, 255, 0.12) 34%,
        rgba(56, 189, 248, 0.08) 68%,
        rgba(168, 85, 247, 0.18) 100%
      );
      backdrop-filter: blur(var(--glass-blur)) saturate(200%);
      -webkit-backdrop-filter: blur(var(--glass-blur)) saturate(200%);
      border: 1px solid rgba(255, 255, 255, 0.34);
      box-shadow:
        0 26px 50px -12px rgba(2, 6, 23, 0.42),
        inset 0 3px 6px rgba(255, 255, 255, calc(var(--glass-specular) * 0.95)),
        inset 0 -6px 14px rgba(56, 189, 248, 0.22),
        inset -6px -4px 16px rgba(192, 132, 252, 0.25),
        inset 4px 0 10px rgba(255, 255, 255, 0.25);
      user-select: none;
      touch-action: none;
      will-change: transform;
      cursor: grab;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      transition: box-shadow 0.35s ease, border-color 0.35s ease;
    }

    .spatial-bubble:active {
      cursor: grabbing;
    }

    /* Glossy curved upper crescent highlight inside every bubble */
    .spatial-bubble::before {
      content: '';
      position: absolute;
      top: 7%;
      left: 16%;
      width: 58%;
      height: 30%;
      border-radius: 50%;
      background: linear-gradient(
        180deg,
        rgba(255, 255, 255, calc(var(--glass-specular) * 0.85)) 0%,
        rgba(255, 255, 255, 0.02) 100%
      );
      transform: rotate(-16deg);
      pointer-events: none;
      filter: blur(0.5px);
    }

    /* Bottom caustic bounce light */
    .spatial-bubble::after {
      content: '';
      position: absolute;
      bottom: 8%;
      right: 18%;
      width: 34%;
      height: 16%;
      border-radius: 50%;
      background: radial-gradient(
        ellipse at center,
        rgba(125, 211, 252, calc(var(--glass-specular) * 0.65)) 0%,
        transparent 75%
      );
      transform: rotate(-18deg);
      pointer-events: none;
    }

    .spatial-bubble:hover {
      border-color: rgba(255, 255, 255, 0.65);
      box-shadow:
        0 32px 60px -10px rgba(14, 165, 233, 0.32),
        inset 0 4px 8px rgba(255, 255, 255, 0.9),
        inset 0 -8px 18px rgba(56, 189, 248, 0.35),
        inset -6px -4px 18px rgba(192, 132, 252, 0.35);
    }

    /* ==========================================================================
       DYNAMIC ISLAND & EQUALIZER ANIMATION
       ========================================================================== */
    .dynamic-island {
      background: rgba(10, 14, 26, 0.72);
      backdrop-filter: blur(32px) saturate(200%);
      -webkit-backdrop-filter: blur(32px) saturate(200%);
      border: 1px solid rgba(255, 255, 255, 0.18);
      box-shadow:
        0 18px 40px -10px rgba(0, 0, 0, 0.55),
        inset 0 1px 1px rgba(255, 255, 255, 0.38);
      color: #f8fafc;
      transition: all 0.5s var(--spring-easing);
    }

    [data-theme="light"] .dynamic-island {
      background: rgba(255, 255, 255, 0.78);
      border: 1px solid rgba(255, 255, 255, 0.92);
      box-shadow:
        0 16px 36px -10px rgba(15, 23, 42, 0.14),
        inset 0 1.5px 1.5px rgba(255, 255, 255, 1);
      color: #0f172a;
    }

    .eq-bar {
      width: 2.5px;
      border-radius: 999px;
      background: linear-gradient(to top, #38bdf8, #c084fc);
      height: 5px;
      transition: height 0.25s ease;
    }

    .eq-active .eq-bar:nth-child(1) { animation: eqBounce 0.75s ease-in-out infinite alternate; }
    .eq-active .eq-bar:nth-child(2) { animation: eqBounce 0.55s ease-in-out 0.15s infinite alternate; }
    .eq-active .eq-bar:nth-child(3) { animation: eqBounce 0.85s ease-in-out 0.3s infinite alternate; }
    .eq-active .eq-bar:nth-child(4) { animation: eqBounce 0.62s ease-in-out 0.1s infinite alternate; }

    @keyframes eqBounce {
      0% { height: 4px; }
      100% { height: 15px; }
    }

    /* ==========================================================================
       CUSTOM LIQUID GLASS SLIDERS
       ========================================================================== */
    input[type="range"].glass-slider {
      -webkit-appearance: none;
      appearance: none;
      width: 100%;
      height: 10px;
      border-radius: 999px;
      background: rgba(255, 255, 255, 0.14);
      border: 1px solid rgba(255, 255, 255, 0.22);
      box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.25);
      outline: none;
      cursor: pointer;
    }

    [data-theme="light"] input[type="range"].glass-slider {
      background: rgba(15, 23, 42, 0.09);
      border: 1px solid rgba(255, 255, 255, 0.8);
    }

    input[type="range"].glass-slider::-webkit-slider-thumb {
      -webkit-appearance: none;
      appearance: none;
      width: 24px;
      height: 24px;
      border-radius: 50%;
      background: radial-gradient(circle at 32% 30%, #ffffff 0%, #bae6fd 55%, #38bdf8 100%);
      border: 2px solid rgba(255, 255, 255, 0.92);
      box-shadow:
        0 4px 12px rgba(14, 165, 233, 0.5),
        inset 0 1.5px 2px rgba(255, 255, 255, 1);
      cursor: grab;
      transition: transform 0.25s var(--spring-easing), box-shadow 0.25s ease;
    }

    input[type="range"].glass-slider::-webkit-slider-thumb:hover {
      transform: scale(1.18);
      box-shadow: 0 6px 18px rgba(56, 189, 248, 0.72);
    }

    input[type="range"].glass-slider::-webkit-slider-thumb:active {
      cursor: grabbing;
      transform: scale(1.06);
    }

    input[type="range"].glass-slider::-moz-range-thumb {
      width: 24px;
      height: 24px;
      border-radius: 50%;
      background: radial-gradient(circle at 32% 30%, #ffffff 0%, #bae6fd 55%, #38bdf8 100%);
      border: 2px solid rgba(255, 255, 255, 0.92);
      box-shadow: 0 4px 12px rgba(14, 165, 233, 0.5);
      cursor: grab;
    }

    /* ==========================================================================
       FLOATING BOTTOM LIQUID DOCK
       ========================================================================== */
    .liquid-dock {
      background: var(--glass-bg-dock);
      backdrop-filter: blur(32px) saturate(200%);
      -webkit-backdrop-filter: blur(32px) saturate(200%);
      border: 1px solid var(--glass-border);
      box-shadow:
        0 24px 48px -10px rgba(0, 0, 0, 0.45),
        inset 0 1.5px 1.5px rgba(255, 255, 255, calc(var(--glass-specular) * 0.85));
    }

    .dock-indicator {
      position: absolute;
      top: 6px;
      bottom: 6px;
      left: 0;
      border-radius: 9999px;
      background: linear-gradient(
        135deg,
        rgba(255, 255, 255, 0.26) 0%,
        rgba(56, 189, 248, 0.22) 100%
      );
      border: 1px solid rgba(255, 255, 255, 0.42);
      box-shadow:
        0 4px 14px rgba(56, 189, 248, 0.25),
        inset 0 1px 1px rgba(255, 255, 255, 0.8);
      transition: transform 0.52s var(--spring-easing), width 0.52s var(--spring-easing);
      pointer-events: none;
      z-index: 0;
    }

    [data-theme="light"] .dock-indicator {
      background: linear-gradient(
        135deg,
        rgba(255, 255, 255, 0.92) 0%,
        rgba(224, 242, 254, 0.85) 100%
      );
      border: 1px solid rgba(255, 255, 255, 1);
      box-shadow: 0 6px 16px rgba(14, 165, 233, 0.18);
    }

    .dock-item {
      position: relative;
      z-index: 1;
      transform-origin: bottom center;
      will-change: transform;
    }

    /* ==========================================================================
       MODAL MORPH & RIPPLE ANIMATIONS
       ========================================================================== */
    .modal-backdrop {
      opacity: 0;
      pointer-events: none;
      backdrop-filter: blur(0px);
      -webkit-backdrop-filter: blur(0px);
      transition: opacity 0.4s var(--smooth-easing), backdrop-filter 0.45s ease;
    }

    .modal-backdrop.open {
      opacity: 1;
      pointer-events: auto;
      backdrop-filter: blur(18px);
      -webkit-backdrop-filter: blur(18px);
    }

    .modal-card {
      opacity: 0;
      transform: scale(0.82) translateY(24px);
      transition: transform 0.55s var(--spring-easing), opacity 0.35s ease;
    }

    .modal-backdrop.open .modal-card {
      opacity: 1;
      transform: scale(1) translateY(0);
    }

    /* Harmony Wave Ring */
    .harmony-ring {
      position: fixed;
      border-radius: 50%;
      border: 2px solid rgba(125, 211, 252, 0.75);
      box-shadow:
        0 0 35px rgba(56, 189, 248, 0.45),
        inset 0 0 25px rgba(192, 132, 252, 0.35);
      pointer-events: none;
      transform: translate(-50%, -50%) scale(0.05);
      opacity: 0.95;
      z-index: 30;
      animation: harmonyExpand 1.35s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }

    @keyframes harmonyExpand {
      0% {
        width: 40px;
        height: 40px;
        transform: translate(-50%, -50%) scale(0.2);
        opacity: 0.95;
      }
      100% {
        width: 140vmax;
        height: 140vmax;
        transform: translate(-50%, -50%) scale(1);
        opacity: 0;
      }
    }

    /* Music waveform bars inside modal */
    .music-wave-bar {
      width: 4px;
      border-radius: 999px;
      background: linear-gradient(to top, #38bdf8, #a855f7, #f472b6);
      transition: height 0.2s ease;
    }

    .music-playing .music-wave-bar {
      animation: musicPulse 0.9s ease-in-out infinite alternate;
    }

    @keyframes musicPulse {
      0% { transform: scaleY(0.28); }
      100% { transform: scaleY(1); }
    }
  </style>
</head>

<body class="antialiased selection:bg-sky-400/30 selection:text-sky-100">

  <!-- Animated Aurora Mesh Background -->
  <div id="auroraCanvas" class="aurora-canvas" aria-hidden="true">
    <div class="aurora-blob aurora-blob-1"></div>
    <div class="aurora-blob aurora-blob-2"></div>
    <div class="aurora-blob aurora-blob-3"></div>
    <div class="aurora-blob aurora-blob-4"></div>
  </div>
  <div class="glass-grain" aria-hidden="true"></div>

  <!-- ========================================================================
       TOP DYNAMIC ISLAND PILL
       ======================================================================== -->
  <header class="fixed top-4 left-0 right-0 z-50 flex justify-center px-4 pointer-events-none">
    <div id="dynamicIsland" class="dynamic-island pointer-events-auto rounded-full px-3.5 py-2 flex items-center gap-3 sm:gap-4 max-w-full">
      <!-- Live Status & Jakarta Time -->
      <div class="flex items-center gap-2 pl-1">
        <span class="relative flex h-2.5 w-2.5">
          <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-sky-400 opacity-75"></span>
          <span class="relative inline-flex rounded-full h-2.5 w-2.5 bg-sky-400"></span>
        </span>
        <span id="jakartaClock" class="text-xs sm:text-sm font-semibold tracking-tight tabular-nums">18:38 WIB</span>
      </div>

      <span class="h-4 w-[1px] bg-current opacity-15"></span>

      <!-- Interactive Mini Audio Equalizer Pill -->
      <button
        id="equalizerToggle"
        type="button"
        aria-label="Aktifkan atau matikan denyut visual audio ambien"
        class="group flex items-center gap-2 px-2.5 py-1 rounded-full bg-white/10 hover:bg-white/20 active:scale-95 transition-all duration-300 cursor-pointer"
      >
        <div id="eqBars" class="flex items-end gap-[2.5px] h-4">
          <span class="eq-bar"></span>
          <span class="eq-bar"></span>
          <span class="eq-bar"></span>
          <span class="eq-bar"></span>
        </div>
        <span id="eqStatusText" class="text-[11px] sm:text-xs font-medium opacity-85 hidden sm:inline">Atmosfer</span>
      </button>

      <span class="h-4 w-[1px] bg-current opacity-15"></span>

      <!-- Dark / Light Glass Theme Switcher -->
      <button
        id="themeToggle"
        type="button"
        aria-label="Ubah tema antara Aurora Gelap dan Kristal Siang"
        class="flex items-center gap-1.5 px-2.5 py-1 rounded-full bg-white/10 hover:bg-white/20 active:scale-95 transition-all duration-300 cursor-pointer"
      >
        <span id="themeIcon" class="text-sky-300 flex items-center justify-center">
          <!-- Moon SVG (Aurora Gelap) -->
          <svg class="w-3.5 h-3.5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M21 12.79A9 9 0 1111.21 3 7 7 0 0021 12.79z"/>
          </svg>
        </span>
        <span id="themeLabel" class="text-[11px] sm:text-xs font-semibold">Aurora Gelap</span>
      </button>
    </div>
  </header>

  <!-- Toast Notification Pill (Replaces alert) -->
  <div id="islandToast" class="fixed top-20 left-1/2 -translate-x-1/2 z-50 pointer-events-none opacity-0 -translate-y-3 transition-all duration-500" style="transition-timing-function: cubic-bezier(0.34, 1.56, 0.64, 1);">
    <div class="dynamic-island rounded-full px-4 py-1.5 text-xs font-medium flex items-center gap-2 shadow-xl">
      <span class="w-2 h-2 rounded-full bg-emerald-400"></span>
      <span id="islandToastText">Efek diperbarui</span>
    </div>
  </div>

  <!-- ========================================================================
       MAIN CONTENT CONTAINER
       ======================================================================== -->
  <main class="relative pt-24 pb-32">

    <!-- ======================================================================
         SECTION 1: INTERACTIVE HERO WITH PHYSICS SPATIAL BUBBLES
         ====================================================================== -->
    <section id="beranda" data-section="beranda" class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 pt-4 sm:pt-8 pb-12">
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 lg:gap-6 items-center">

        <!-- Left Editorial Copy -->
        <div class="lg:col-span-5 text-center lg:text-left z-10 space-y-5">
          <div class="liquid-glass inline-flex items-center gap-2 px-3.5 py-1.5 rounded-full text-xs font-semibold tracking-wide">
            <span class="w-2 h-2 rounded-full bg-gradient-to-tr from-sky-400 to-fuchsia-400"></span>
            <span style="color: var(--text-secondary);">iOS 26 Spatial Interface</span>
          </div>

          <h1 class="text-3xl sm:text-5xl xl:text-[3.35rem] font-extrabold tracking-tight leading-[1.12]">
            iOS 26 Liquid Bubble Experience
            <span class="block mt-2 bg-gradient-to-r from-sky-400 via-indigo-300 to-fuchsia-400 bg-clip-text text-transparent">
              Antarmuka Masa Depan yang Mengalir
            </span>
          </h1>

          <p class="text-sm sm:text-base leading-relaxed max-w-xl mx-auto lg:mx-0" style="color: var(--text-secondary);">
            Rasakan kejernihan optik <strong>Liquid Glass</strong> generasi terbaru dengan fisika pegas 60fps, refraksi cahaya real-time, serta gelembung spasial interaktif yang merespons setiap sentuhan dan gerakan kursor Anda.
          </p>

          <div class="flex flex-wrap items-center justify-center lg:justify-start gap-3 pt-2">
            <a
              href="#playground"
              class="liquid-glass liquid-glass-interactive px-6 py-3.5 rounded-full font-semibold text-sm flex items-center gap-2.5 shadow-lg"
            >
              <span>Eksplor Studio Kaca</span>
              <svg class="w-4 h-4 text-sky-400" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M13 7l5 5m0 0l-5 5m5-5H6"/>
              </svg>
            </a>

            <button
              id="heroHarmonyBtn"
              type="button"
              class="liquid-glass liquid-glass-interactive px-5 py-3.5 rounded-full font-semibold text-sm flex items-center gap-2"
            >
              <svg class="w-4 h-4 text-fuchsia-400" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2">
                <circle cx="12" cy="12" r="3"/>
                <circle cx="12" cy="12" r="8" stroke-dasharray="3 3"/>
              </svg>
              <span>Gelombang Harmoni</span>
            </button>
          </div>

          <!-- Micro Hint -->
          <p class="text-xs flex items-center justify-center lg:justify-start gap-2 pt-1" style="color: var(--text-muted);">
            <span class="inline-block w-1.5 h-1.5 rounded-full bg-sky-400"></span>
            <span>Tarik, dorong, atau klik gelembung spasial di samping untuk membuka widget</span>
          </p>
        </div>

        <!-- Right Interactive Physics-Driven Spatial Bubble Arena -->
        <div class="lg:col-span-7 relative">
          <div
            id="bubbleArena"
            class="liquid-glass relative w-full h-[430px] sm:h-[500px] lg:h-[540px] rounded-[2.25rem] overflow-hidden select-none"
            aria-label="Arena Gelembung Spasial Interaktif"
          >
            <!-- Subtle inner ambient grid glow -->
            <div class="absolute inset-0 pointer-events-none opacity-35" style="background: radial-gradient(circle at 50% 45%, rgba(56, 189, 248, 0.16), transparent 68%);"></div>

            <!-- Top-left arena label -->
            <div class="absolute top-4 left-4 z-10 px-3 py-1 rounded-full bg-white/10 backdrop-blur-md border border-white/15 text-[11px] font-medium flex items-center gap-1.5 pointer-events-none" style="color: var(--text-secondary);">
              <span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse"></span>
              <span>Fisika Pegas 60fps Aktif</span>
            </div>

            <!-- Top-right bubble count pill -->
            <div class="absolute top-4 right-4 z-10 px-3 py-1 rounded-full bg-white/10 backdrop-blur-md border border-white/15 text-[11px] font-medium pointer-events-none" style="color: var(--text-secondary);">
              <span id="bubbleCounterText">7 Gelembung Aktif</span>
            </div>

            <!-- Spatial Bubbles are dynamically rendered and animated via JS inside this arena -->
          </div>
        </div>

      </div>
    </section>

    <!-- ======================================================================
         SECTION 2: INTERACTIVE WIDGET BUBBLE CARDS
         ====================================================================== -->
    <section id="widget" data-section="widget" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-14">
      <div class="text-center max-w-2xl mx-auto mb-10 space-y-2.5">
        <span class="text-xs font-bold uppercase tracking-widest text-sky-400">Ekosistem Widget Spasial</span>
        <h2 class="text-2xl sm:text-4xl font-extrabold tracking-tight">Widget Bubble Interaktif</h2>
        <p class="text-sm sm:text-base" style="color: var(--text-secondary);">
          Pilih salah satu widget kaca di bawah ini atau ketuk gelembungnya secara langsung untuk membuka antarmuka penuh dengan animasi morphing mulus.
        </p>
      </div>

      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5">
        <!-- Card 1: Musik Spasial -->
        <button
          type="button"
          data-open-widget="music"
          class="liquid-glass liquid-glass-interactive text-left p-6 rounded-[2rem] flex flex-col justify-between min-h-[215px] group"
        >
          <div class="flex items-start justify-between">
            <div class="w-12 h-12 rounded-2xl bg-gradient-to-br from-sky-400/30 to-indigo-500/30 border border-white/30 flex items-center justify-center text-sky-300 shadow-inner group-hover:scale-110 transition-transform duration-500" style="transition-timing-function: var(--spring-easing);">
              <svg class="w-6 h-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zm12-3c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2z"/>
              </svg>
            </div>
            <span class="text-[11px] font-semibold px-2.5 py-1 rounded-full bg-white/10 border border-white/15">Audio 3D</span>
          </div>
          <div class="mt-6">
            <h3 class="text-lg font-bold">Musik Spasial</h3>
            <p class="text-xs mt-1 leading-relaxed" style="color: var(--text-secondary);">Pemutar audio kaca dengan visualisasi gelombang spektrum & perpindahan trek mulus.</p>
          </div>
        </button>

        <!-- Card 2: Cuaca & Atmosfer -->
        <button
          type="button"
          data-open-widget="weather"
          class="liquid-glass liquid-glass-interactive text-left p-6 rounded-[2rem] flex flex-col justify-between min-h-[215px] group"
        >
          <div class="flex items-start justify-between">
            <div class="w-12 h-12 rounded-2xl bg-gradient-to-br from-amber-400/30 to-sky-400/30 border border-white/30 flex items-center justify-center text-amber-300 shadow-inner group-hover:scale-110 transition-transform duration-500" style="transition-timing-function: var(--spring-easing);">
              <svg class="w-6 h-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <circle cx="12" cy="12" r="4"/>
                <path stroke-linecap="round" d="M12 2v2m0 16v2M4.93 4.93l1.41 1.41m11.32 11.32l1.41 1.41M2 12h2m16 0h2M4.93 19.07l1.41-1.41m11.32-11.32l1.41-1.41"/>
              </svg>
            </div>
            <span class="text-[11px] font-semibold px-2.5 py-1 rounded-full bg-white/10 border border-white/15">29°C Jakarta</span>
          </div>
          <div class="mt-6">
            <h3 class="text-lg font-bold">Cuaca & Atmosfer</h3>
            <p class="text-xs mt-1 leading-relaxed" style="color: var(--text-secondary);">Orb atmosfer bercahaya dengan simulasi suhu, kelembapan, dan prakiraan jam.</p>
          </div>
        </button>

        <!-- Card 3: Fokus & Produktivitas -->
        <button
          type="button"
          data-open-widget="focus"
          class="liquid-glass liquid-glass-interactive text-left p-6 rounded-[2rem] flex flex-col justify-between min-h-[215px] group"
        >
          <div class="flex items-start justify-between">
            <div class="w-12 h-12 rounded-2xl bg-gradient-to-br from-emerald-400/30 to-teal-500/30 border border-white/30 flex items-center justify-center text-emerald-300 shadow-inner group-hover:scale-110 transition-transform duration-500" style="transition-timing-function: var(--spring-easing);">
              <svg class="w-6 h-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <circle cx="12" cy="13" r="8"/>
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v4l2.5 2.5M9 2h6"/>
              </svg>
            </div>
            <span class="text-[11px] font-semibold px-2.5 py-1 rounded-full bg-white/10 border border-white/15">25:00</span>
          </div>
          <div class="mt-6">
            <h3 class="text-lg font-bold">Fokus & Produktivitas</h3>
            <p class="text-xs mt-1 leading-relaxed" style="color: var(--text-secondary);">Timer Pomodoro interaktif dengan cincin progres melingkar berpresisi tinggi.</p>
          </div>
        </button>

        <!-- Card 4: Galeri Cahaya -->
        <button
          type="button"
          data-open-widget="gallery"
          class="liquid-glass liquid-glass-interactive text-left p-6 rounded-[2rem] flex flex-col justify-between min-h-[215px] group"
        >
          <div class="flex items-start justify-between">
            <div class="w-12 h-12 rounded-2xl bg-gradient-to-br from-fuchsia-400/30 to-violet-500/30 border border-white/30 flex items-center justify-center text-fuchsia-300 shadow-inner group-hover:scale-110 transition-transform duration-500" style="transition-timing-function: var(--spring-easing);">
              <svg class="w-6 h-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 3l1.9 5.8a2 2 0 001.3 1.3L21 12l-5.8 1.9a2 2 0 00-1.3 1.3L12 21l-1.9-5.8a2 2 0 00-1.3-1.3L3 12l5.8-1.9a2 2 0 001.3-1.3L12 3z"/>
              </svg>
            </div>
            <span class="text-[11px] font-semibold px-2.5 py-1 rounded-full bg-white/10 border border-white/15">Prisma</span>
          </div>
          <div class="mt-6">
            <h3 class="text-lg font-bold">Galeri Cahaya</h3>
            <p class="text-xs mt-1 leading-relaxed" style="color: var(--text-secondary);">Eksplorasi spektrum refraksi kristal dan ubah nuansa cahaya latar secara instan.</p>
          </div>
        </button>
      </div>
    </section>

    <!-- ======================================================================
         SECTION 3: INTERACTIVE 'PLAYGROUND LIQUID GLASS' STUDIO
         ====================================================================== -->
    <section id="playground" data-section="playground" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-14">
      <div class="liquid-glass rounded-[2.5rem] p-6 sm:p-10 lg:p-12">
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 lg:gap-10 items-center">

          <!-- Left Controls Panel -->
          <div class="lg:col-span-6 space-y-6">
            <div>
              <span class="text-xs font-bold uppercase tracking-widest text-fuchsia-400">Studio Optik & Fisika</span>
              <h2 class="text-2xl sm:text-3xl font-extrabold tracking-tight mt-1">Playground Liquid Glass</h2>
              <p class="text-sm mt-2 leading-relaxed" style="color: var(--text-secondary);">
                Sesuaikan parameter material optik dan dinamika pegas secara real-time. Setiap perubahan langsung memengaruhi seluruh gelembung dan panel kaca di layar.
              </p>
            </div>

            <!-- Sliders Container -->
            <div class="space-y-5">
              <!-- Slider 1: Tingkat Blur Kaca -->
              <div class="space-y-2">
                <div class="flex justify-between items-center text-sm">
                  <label for="blurSlider" class="font-semibold">Tingkat Blur Kaca</label>
                  <span id="blurValueLabel" class="px-2.5 py-0.5 rounded-full bg-white/10 text-xs font-bold tabular-nums text-sky-400">28 px</span>
                </div>
                <input id="blurSlider" type="range" min="8" max="48" value="28" class="glass-slider" />
              </div>

              <!-- Slider 2: Kilau Refraksi -->
              <div class="space-y-2">
                <div class="flex justify-between items-center text-sm">
                  <label for="specularSlider" class="font-semibold">Kilau Refraksi</label>
                  <span id="specularValueLabel" class="px-2.5 py-0.5 rounded-full bg-white/10 text-xs font-bold tabular-nums text-fuchsia-400">62%</span>
                </div>
                <input id="specularSlider" type="range" min="15" max="100" value="62" class="glass-slider" />
              </div>

              <!-- Slider 3: Elastisitas Pegas -->
              <div class="space-y-2">
                <div class="flex justify-between items-center text-sm">
                  <label for="springSlider" class="font-semibold">Elastisitas Pegas</label>
                  <span id="springValueLabel" class="px-2.5 py-0.5 rounded-full bg-white/10 text-xs font-bold tabular-nums text-emerald-400">0.65x</span>
                </div>
                <input id="springSlider" type="range" min="25" max="100" value="65" class="glass-slider" />
              </div>
            </div>

            <!-- Interactive Action Buttons -->
            <div class="flex flex-wrap items-center gap-3 pt-2">
              <button
                id="addBubbleBtn"
                type="button"
                class="liquid-glass liquid-glass-interactive px-5 py-3 rounded-full text-sm font-bold flex items-center gap-2 bg-sky-400/15"
              >
                <span class="w-5 h-5 rounded-full bg-sky-400/30 flex items-center justify-center text-sky-300 font-extrabold">+</span>
                <span>Tambah Bubble</span>
              </button>

              <button
                id="harmonyWaveBtn"
                type="button"
                class="liquid-glass liquid-glass-interactive px-5 py-3 rounded-full text-sm font-bold flex items-center gap-2 bg-fuchsia-400/15"
              >
                <svg class="w-4 h-4 text-fuchsia-300" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M2 12c2.5-4 5.5-4 8 0s5.5 4 8 0 4-4 4-4"/>
                </svg>
                <span>Gelombang Harmoni</span>
              </button>

              <button
                id="resetStudioBtn"
                type="button"
                class="px-4 py-3 rounded-full text-xs font-semibold opacity-75 hover:opacity-100 transition-opacity cursor-pointer"
              >
                Reset Default
              </button>
            </div>
          </div>

          <!-- Right Live Optical Refraction Preview Card -->
          <div class="lg:col-span-6">
            <div class="relative rounded-[2rem] p-6 sm:p-8 overflow-hidden border border-white/20 min-h-[300px] flex flex-col items-center justify-center text-center" style="background: linear-gradient(135deg, rgba(14,165,233,0.22), rgba(168,85,247,0.24), rgba(236,72,153,0.18));">
              <!-- Vivid floating background shapes inside preview to showcase blur & refraction -->
              <div class="w-36 h-36 rounded-full bg-gradient-to-tr from-sky-400 to-cyan-300 absolute -top-6 -left-6 opacity-80 animate-pulse"></div>
              <div class="w-44 h-44 rounded-full bg-gradient-to-tr from-fuchsia-500 to-indigo-500 absolute -bottom-10 -right-8 opacity-80"></div>
              <div class="w-24 h-24 rounded-full bg-amber-300/80 absolute top-1/2 left-1/4 -translate-y-1/2"></div>

              <!-- Foreground Layered Liquid Glass Sample Lens -->
              <div id="previewGlassLens" class="liquid-glass relative z-10 w-full max-w-sm p-6 rounded-[1.75rem] space-y-3">
                <div class="w-14 h-14 mx-auto rounded-full bg-white/20 border border-white/50 flex items-center justify-center shadow-lg">
                  <svg class="w-7 h-7 text-white" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"/>
                  </svg>
                </div>
                <h3 class="text-lg font-bold">Lensa Optik Real-Time</h3>
                <p class="text-xs leading-relaxed" style="color: var(--text-secondary);">
                  Amati bagaimana lengkungan tepi spekular dan saturasi latar di balik lensa ini berubah secara halus mengikuti pengaturan slider Anda.
                </p>
                <div class="pt-1 flex items-center justify-center gap-2 text-[11px] font-semibold">
                  <span class="px-2.5 py-1 rounded-full bg-white/15">Saturate 185%</span>
                  <span class="px-2.5 py-1 rounded-full bg-white/15">Refraksi Rim</span>
                </div>
              </div>
            </div>
          </div>

        </div>
      </div>
    </section>

    <!-- ======================================================================
         SECTION 4: FITUR DESAIN ARCHITECTURE
         ====================================================================== -->
    <section id="fitur" data-section="fitur" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-14">
      <div class="text-center max-w-2xl mx-auto mb-10 space-y-2.5">
        <span class="text-xs font-bold uppercase tracking-widest text-emerald-400">Arsitektur Visual</span>
        <h2 class="text-2xl sm:text-4xl font-extrabold tracking-tight">Keunggulan Desain Liquid Glass</h2>
        <p class="text-sm sm:text-base" style="color: var(--text-secondary);">
          Dirancang untuk menghadirkan kedalaman spasial tanpa mengorbankan performa dan keterbacaan di setiap ukuran layar.
        </p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div class="liquid-glass p-7 rounded-[2rem] space-y-3">
          <div class="w-11 h-11 rounded-2xl bg-sky-400/20 border border-sky-300/30 flex items-center justify-center text-sky-300">
            <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"/>
              <path stroke-linecap="round" stroke-linejoin="round" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"/>
            </svg>
          </div>
          <h3 class="text-lg font-bold">Refraksi Spekular Dinamis</h3>
          <p class="text-sm leading-relaxed" style="color: var(--text-secondary);">
            Kilau cahaya radial mengikuti posisi kursor maupun sentuhan jari secara real-time, menciptakan ilusi permukaan kaca cembung yang hidup.
          </p>
        </div>

        <div class="liquid-glass p-7 rounded-[2rem] space-y-3">
          <div class="w-11 h-11 rounded-2xl bg-fuchsia-400/20 border border-fuchsia-300/30 flex items-center justify-center text-fuchsia-300">
            <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M13 10V3L4 14h7v7l9-11h-7z"/>
            </svg>
          </div>
          <h3 class="text-lg font-bold">Akselerasi GPU 60fps</h3>
          <p class="text-sm leading-relaxed" style="color: var(--text-secondary);">
            Seluruh simulasi pegas, pembesaran ikon dock, dan pergerakan gelembung dijalankan menggunakan transformasi GPU tanpa pergeseran tata letak.
          </p>
        </div>

        <div class="liquid-glass p-7 rounded-[2rem] space-y-3">
          <div class="w-11 h-11 rounded-2xl bg-emerald-400/20 border border-emerald-300/30 flex items-center justify-center text-emerald-300">
            <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707"/>
            </svg>
          </div>
          <h3 class="text-lg font-bold">Adaptasi Cahaya Otomatis</h3>
          <p class="text-sm leading-relaxed" style="color: var(--text-secondary);">
            Transisi mulus antara atmosfer malam <em>Aurora Gelap</em> dan kejernihan siang <em>Kristal Siang</em> dengan kontras tipografi yang selalu terjaga.
          </p>
        </div>
      </div>
    </section>

  </main>

  <!-- ========================================================================
       FLOATING BOTTOM iOS 26 LIQUID DOCK
       ======================================================================== -->
  <nav aria-label="Navigasi Utama Liquid Dock" class="fixed bottom-5 left-0 right-0 z-50 flex justify-center px-3 pointer-events-none">
    <div id="liquidDock" class="liquid-dock pointer-events-auto relative rounded-full px-2 py-1.5 flex items-center gap-1 sm:gap-2">
      <!-- Sliding Active Glass Pill Indicator -->
      <div id="dockIndicator" class="dock-indicator"></div>

      <a
        href="#beranda"
        data-dock-target="beranda"
        class="dock-item px-3.5 sm:px-5 py-2.5 rounded-full text-xs sm:text-sm font-semibold flex items-center gap-2 transition-colors"
      >
        <svg class="w-4 h-4 text-sky-400 shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2">
          <path stroke-linecap="round" stroke-linejoin="round" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"/>
        </svg>
        <span>Beranda</span>
      </a>

      <a
        href="#widget"
        data-dock-target="widget"
        class="dock-item px-3.5 sm:px-5 py-2.5 rounded-full text-xs sm:text-sm font-semibold flex items-center gap-2 transition-colors"
      >
        <svg class="w-4 h-4 text-indigo-400 shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2">
          <rect x="3" y="3" width="7" height="7" rx="2"/>
          <rect x="14" y="3" width="7" height="7" rx="2"/>
          <rect x="14" y="14" width="7" height="7" rx="2"/>
          <rect x="3" y="14" width="7" height="7" rx="2"/>
        </svg>
        <span>Widget Bubble</span>
      </a>

      <a
        href="#playground"
        data-dock-target="playground"
        class="dock-item px-3.5 sm:px-5 py-2.5 rounded-full text-xs sm:text-sm font-semibold flex items-center gap-2 transition-colors"
      >
        <svg class="w-4 h-4 text-fuchsia-400 shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2">
          <path stroke-linecap="round" stroke-linejoin="round" d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4"/>
        </svg>
        <span>Playground</span>
      </a>

      <a
        href="#fitur"
        data-dock-target="fitur"
        class="dock-item px-3.5 sm:px-5 py-2.5 rounded-full text-xs sm:text-sm font-semibold flex items-center gap-2 transition-colors"
      >
        <svg class="w-4 h-4 text-emerald-400 shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2">
          <path stroke-linecap="round" stroke-linejoin="round" d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z"/>
        </svg>
        <span>Fitur Desain</span>
      </a>
    </div>
  </nav>

  <!-- ========================================================================
       INTERACTIVE FLOATING GLASS MODAL WIDGET
       ======================================================================== -->
  <div id="widgetModalBackdrop" class="modal-backdrop fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/45" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
    <div id="widgetModalCard" class="modal-card liquid-glass w-full max-w-md rounded-[2.25rem] p-6 sm:p-7 relative shadow-2xl">
      <!-- Close Button -->
      <button
        id="closeModalBtn"
        type="button"
        aria-label="Tutup widget"
        class="absolute top-5 right-5 w-9 h-9 rounded-full bg-white/15 hover:bg-white/25 border border-white/25 flex items-center justify-center transition-transform active:scale-90 cursor-pointer z-20"
      >
        <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4">
          <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12"/>
        </svg>
      </button>

      <!-- Dynamic Widget Content Injected Here -->
      <div id="widgetModalBody"></div>
    </div>
  </div>

  <!-- ========================================================================
       JAVASCRIPT: 60FPS SPRING PHYSICS, SPECULAR TRACKING & WIDGET ENGINE
       ======================================================================== -->
  <script>
    (function () {
      'use strict';

      /* ----------------------------------------------------------------------
         1. STATE & DESIGN CONFIGURATION
         ---------------------------------------------------------------------- */
      const state = {
        theme: 'dark', // 'dark' (Aurora Gelap) | 'light' (Kristal Siang)
        ambientActive: false,
        glassBlur: 28,
        glassSpecular: 0.62,
        springBounce: 0.65,
        activeSection: 'beranda',
        activeWidget: null,
        // Music widget state
        musicPlaying: false,
        trackIndex: 0,
        tracks: [
          { title: 'Simfoni Aurora', artist: 'Spatial Ensemble • Dolby Atmos', duration: '03:42', color: 'from-sky-400 to-indigo-500' },
          { title: 'Kristal Hujan Jakarta', artist: 'Ambient Liquid • 96kHz', duration: '04:15', color: 'from-cyan-400 to-teal-500' },
          { title: 'Orbit Senja Spasial', artist: 'Horizon Synth • Spatial Audio', duration: '02:58', color: 'from-fuchsia-400 to-purple-600' }
        ],
        // Focus timer state
        focusSeconds: 25 * 60,
        focusTotal: 25 * 60,
        focusRunning: false,
        focusModeLabel: 'Fokus Mendalam',
        // Gallery orb state
        selectedOrbIndex: 0,
        lightOrbs: [
          { name: 'Safir Malam', grad: 'radial-gradient(circle at 30% 30%, #bae6fd, #0284c7 45%, #1e1b4b 90%)', a1: '#1e1b4b', a2: '#0369a1', a3: '#6d28d9' },
          { name: 'Prisma Lembayung', grad: 'radial-gradient(circle at 30% 30%, #f5d0fe, #c026d3 45%, #3b0764 90%)', a1: '#3b0764', a2: '#a21caf', a3: '#4f46e5' },
          { name: 'Embun Zamrud', grad: 'radial-gradient(circle at 30% 30%, #a7f3d0, #059669 45%, #064e3b 90%)', a1: '#064e3b', a2: '#0d9488', a3: '#0284c7' },
          { name: 'Fajar Koral', grad: 'radial-gradient(circle at 30% 30%, #fed7aa, #f97316 45%, #881337 90%)', a1: '#4c0519', a2: '#e11d48', a3: '#d97706' }
        ]
      };

      /* ----------------------------------------------------------------------
         2. LIVE JAKARTA CLOCK (WIB)
         ---------------------------------------------------------------------- */
      const clockEl = document.getElementById('jakartaClock');
      function updateJakartaClock() {
        try {
          const now = new Date();
          const formatted = new Intl.DateTimeFormat('id-ID', {
            timeZone: 'Asia/Jakarta',
            hour: '2-digit',
            minute: '2-digit',
            hour12: false
          }).format(now);
          clockEl.textContent = `${formatted.replace('.', ':')} WIB`;
        } catch (e) {
          const now = new Date();
          const h = String(now.getHours()).padStart(2, '0');
          const m = String(now.getMinutes()).padStart(2, '0');
          clockEl.textContent = `${h}:${m} WIB`;
        }
      }
      updateJakartaClock();
      setInterval(updateJakartaClock, 10000);

      /* ----------------------------------------------------------------------
         3. DYNAMIC ISLAND TOAST HELPER
         ---------------------------------------------------------------------- */
      const toastEl = document.getElementById('islandToast');
      const toastTextEl = document.getElementById('islandToastText');
      let toastTimeout = null;

      function showIslandToast(message) {
        toastTextEl.textContent = message;
        toastEl.classList.remove('opacity-0', '-translate-y-3');
        toastEl.classList.add('opacity-100', 'translate-y-0');
        clearTimeout(toastTimeout);
        toastTimeout = setTimeout(() => {
          toastEl.classList.remove('opacity-100', 'translate-y-0');
          toastEl.classList.add('opacity-0', '-translate-y-3');
        }, 2300);
      }

      /* ----------------------------------------------------------------------
         4. SUBTLE WEB AUDIO SYNTHESIZER (TACTILE CHIME & AMBIENT PULSE)
         ---------------------------------------------------------------------- */
      let audioCtx = null;
      function playGlassChime(freq = 587.33, duration = 0.28) {
        if (!state.ambientActive) return;
        try {
          if (!audioCtx) {
            const AudioContext = window.AudioContext || window.webkitAudioContext;
            audioCtx = new AudioContext();
          }
          if (audioCtx.state === 'suspended') audioCtx.resume();
          const osc = audioCtx.createOscillator();
          const gain = audioCtx.createGain();
          osc.type = 'sine';
          osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
          osc.frequency.exponentialRampToValueAtTime(freq * 1.25, audioCtx.currentTime + duration);

          gain.gain.setValueAtTime(0.001, audioCtx.currentTime);
          gain.gain.exponentialRampToValueAtTime(0.055, audioCtx.currentTime + 0.03);
          gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + duration);

          osc.connect(gain);
          gain.connect(audioCtx.destination);
          osc.start();
          osc.stop(audioCtx.currentTime + duration);
        } catch (err) {
          // Ignore audio restrictions silently
        }
      }

      /* ----------------------------------------------------------------------
         5. THEME SWITCHER ('Aurora Gelap' <-> 'Kristal Siang') & EQUALIZER
         ---------------------------------------------------------------------- */
      const themeToggleBtn = document.getElementById('themeToggle');
      const themeLabelEl = document.getElementById('themeLabel');
      const themeIconEl = document.getElementById('themeIcon');

      themeToggleBtn.addEventListener('click', () => {
        state.theme = state.theme === 'dark' ? 'light' : 'dark';
        if (state.theme === 'light') {
          document.documentElement.setAttribute('data-theme', 'light');
          themeLabelEl.textContent = 'Kristal Siang';
          themeIconEl.className = 'text-amber-500 flex items-center justify-center';
          themeIconEl.innerHTML = `<svg class="w-3.5 h-3.5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><circle cx="12" cy="12" r="4"/><path stroke-linecap="round" d="M12 2v2m0 16v2M4.93 4.93l1.41 1.41m11.32 11.32l1.41 1.41M2 12h2m16 0h2M4.93 19.07l1.41-1.41m11.32-11.32l1.41-1.41"/></svg>`;
          showIslandToast('Mode Kristal Siang aktif');
        } else {
          document.documentElement.removeAttribute('data-theme');
          themeLabelEl.textContent = 'Aurora Gelap';
          themeIconEl.className = 'text-sky-300 flex items-center justify-center';
          themeIconEl.innerHTML = `<svg class="w-3.5 h-3.5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><path stroke-linecap="round" stroke-linejoin="round" d="M21 12.79A9 9 0 1111.21 3 7 7 0 0021 12.79z"/></svg>`;
          showIslandToast('Mode Aurora Gelap aktif');
        }
        playGlassChime(state.theme === 'light' ? 784 : 523.25);
        updateDockIndicator();
      });

      const eqToggleBtn = document.getElementById('equalizerToggle');
      const eqBarsEl = document.getElementById('eqBars');
      const auroraCanvasEl = document.getElementById('auroraCanvas');

      eqToggleBtn.addEventListener('click', () => {
        state.ambientActive = !state.ambientActive;
        eqBarsEl.classList.toggle('eq-active', state.ambientActive);
        auroraCanvasEl.classList.toggle('aurora-pulse-active', state.ambientActive);
        if (state.ambientActive) {
          playGlassChime(659.25, 0.35);
          showIslandToast('Denyut Visual & Suara Sentuh Aktif');
        } else {
          showIslandToast('Atmosfer Ambien Dijeda');
        }
      });

      /* ----------------------------------------------------------------------
         6. REAL-TIME CURSOR-TRACKING RADIAL SPECULAR LIGHT GLINT
         ---------------------------------------------------------------------- */
      let pointerX = window.innerWidth * 0.5;
      let pointerY = window.innerHeight * 0.3;

      function updateGlassSpecularGlints() {
        const glassElements = document.querySelectorAll('.liquid-glass');
        glassElements.forEach((el) => {
          const rect = el.getBoundingClientRect();
          // Skip elements far outside viewport for optimal 60fps
          if (rect.bottom < -120 || rect.top > window.innerHeight + 120) return;
          const relX = pointerX - rect.left;
          const relY = pointerY - rect.top;
          el.style.setProperty('--glint-x', `${relX}px`);
          el.style.setProperty('--glint-y', `${relY}px`);

          // Brighten glint when pointer is near or over the card
          const inside =
            pointerX >= rect.left - 80 &&
            pointerX <= rect.right + 80 &&
            pointerY >= rect.top - 80 &&
            pointerY <= rect.bottom + 80;
          el.style.setProperty('--glint-opacity', inside ? '0.95' : '0.35');
        });
      }

      window.addEventListener('pointermove', (e) => {
        pointerX = e.clientX;
        pointerY = e.clientY;
        requestAnimationFrame(updateGlassSpecularGlints);
      }, { passive: true });

      /* ----------------------------------------------------------------------
         7. 60FPS SPRING-PHYSICS SPATIAL BUBBLE ENGINE
         ---------------------------------------------------------------------- */
      const arenaEl = document.getElementById('bubbleArena');
      const counterEl = document.getElementById('bubbleCounterText');
      let bubbles = [];
      let arenaRect = { width: 600, height: 500, left: 0, top: 0 };
      let arenaPointer = { x: -9999, y: -9999, active: false };

      function refreshArenaBounds() {
        const r = arenaEl.getBoundingClientRect();
        arenaRect = { width: r.width, height: r.height, left: r.left, top: r.top };
      }

      const widgetDefinitions = [
        {
          id: 'music',
          title: 'Musik Spasial',
          subtitle: 'Dolby Atmos',
          radius: 66,
          accent: 'text-sky-300',
          icon: `<svg class="w-7 h-7" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zm12-3c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2z"/></svg>`
        },
        {
          id: 'weather',
          title: 'Cuaca',
          subtitle: '29°C Cerah',
          radius: 62,
          accent: 'text-amber-300',
          icon: `<svg class="w-7 h-7" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="4"/><path stroke-linecap="round" d="M12 2v2m0 16v2M4.93 4.93l1.41 1.41m11.32 11.32l1.41 1.41M2 12h2m16 0h2M4.93 19.07l1.41-1.41m11.32-11.32l1.41-1.41"/></svg>`
        },
        {
          id: 'focus',
          title: 'Fokus',
          subtitle: '25:00 Timer',
          radius: 60,
          accent: 'text-emerald-300',
          icon: `<svg class="w-7 h-7" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="13" r="8"/><path stroke-linecap="round" stroke-linejoin="round" d="M12 9v4l2.5 2.5M9 2h6"/></svg>`
        },
        {
          id: 'gallery',
          title: 'Galeri Cahaya',
          subtitle: 'Prisma Orb',
          radius: 64,
          accent: 'text-fuchsia-300',
          icon: `<svg class="w-7 h-7" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M12 3l1.9 5.8a2 2 0 001.3 1.3L21 12l-5.8 1.9a2 2 0 00-1.3 1.3L12 21l-1.9-5.8a2 2 0 00-1.3-1.3L3 12l5.8-1.9a2 2 0 001.3-1.3L12 3z"/></svg>`
        }
      ];

      function createBubbleEntity(opts) {
        const isMobile = window.innerWidth < 640;
        const scaleFactor = isMobile ? 0.78 : 1;
        const radius = Math.round((opts.radius || 36) * scaleFactor);

        const el = document.createElement('div');
        el.className = 'spatial-bubble';
        el.style.width = `${radius * 2}px`;
        el.style.height = `${radius * 2}px`;

        if (opts.widget) {
          el.innerHTML = `
            <div class="${opts.widget.accent} mb-1 pointer-events-none drop-shadow">${opts.widget.icon}</div>
            <span class="text-[11px] sm:text-xs font-bold leading-tight px-2 pointer-events-none">${opts.widget.title}</span>
            <span class="text-[9px] sm:text-[10px] opacity-75 pointer-events-none mt-0.5">${opts.widget.subtitle}</span>
          `;
        } else {
          // Pure refractive droplet bubble
          el.innerHTML = `
            <span class="w-2.5 h-2.5 rounded-full bg-white/50 pointer-events-none"></span>
          `;
        }

        const bubble = {
          el,
          widgetId: opts.widget ? opts.widget.id : null,
          radius,
          x: opts.x,
          y: opts.y,
          vx: (Math.random() - 0.5) * 1.4,
          vy: (Math.random() - 0.5) * 1.4,
          scale: 0.1,
          scaleV: 0,
          targetScale: 1,
          phase: Math.random() * Math.PI * 2,
          freq: 0.012 + Math.random() * 0.01,
          dragging: false,
          dragStartX: 0,
          dragStartY: 0,
          movedDistance: 0
        };

        // Pointer interaction (Drag + Tap/Click)
        el.addEventListener('pointerdown', (e) => {
          e.stopPropagation();
          bubble.dragging = true;
          bubble.dragStartX = e.clientX;
          bubble.dragStartY = e.clientY;
          bubble.movedDistance = 0;
          bubble.targetScale = 1.12;
          el.setPointerCapture(e.pointerId);
          playGlassChime(440 + bubble.radius * 3, 0.18);
        });

        el.addEventListener('pointermove', (e) => {
          if (!bubble.dragging) return;
          const dx = e.clientX - bubble.dragStartX;
          const dy = e.clientY - bubble.dragStartY;
          bubble.movedDistance += Math.hypot(e.movementX || 0, e.movementY || 0);

          refreshArenaBounds();
          const targetX = e.clientX - arenaRect.left;
          const targetY = e.clientY - arenaRect.top;

          bubble.vx = (targetX - bubble.x) * 0.35;
          bubble.vy = (targetY - bubble.y) * 0.35;
          bubble.x = Math.max(bubble.radius + 8, Math.min(arenaRect.width - bubble.radius - 8, targetX));
          bubble.y = Math.max(bubble.radius + 8, Math.min(arenaRect.height - bubble.radius - 8, targetY));
        });

        const endDrag = (e) => {
          if (!bubble.dragging) return;
          bubble.dragging = false;
          bubble.targetScale = 1;
          try { el.releasePointerCapture(e.pointerId); } catch (_) {}

          // Treat as tap/click if movement was minimal
          if (bubble.movedDistance < 10) {
            bubble.scaleV += 0.14 * state.springBounce;
            if (bubble.widgetId) {
              openWidgetModal(bubble.widgetId);
            } else {
              // Decorative droplet pops with ripple!
              triggerHarmonyWave(bubble.x + arenaRect.left, bubble.y + arenaRect.top, false);
              playGlassChime(784, 0.25);
            }
          }
        };

        el.addEventListener('pointerup', endDrag);
        el.addEventListener('pointercancel', endDrag);

        arenaEl.appendChild(el);
        bubbles.push(bubble);
        updateBubbleCounter();
        return bubble;
      }

      function updateBubbleCounter() {
        counterEl.textContent = `${bubbles.length} Gelembung Aktif`;
      }

      function initArenaBubbles() {
        refreshArenaBounds();
        const w = arenaRect.width;
        const h = arenaRect.height;

        const positions = [
          { x: w * 0.30, y: h * 0.33 },
          { x: w * 0.68, y: h * 0.30 },
          { x: w * 0.33, y: h * 0.70 },
          { x: w * 0.69, y: h * 0.68 }
        ];

        widgetDefinitions.forEach((widget, idx) => {
          createBubbleEntity({
            widget,
            radius: widget.radius,
            x: positions[idx].x,
            y: positions[idx].y
          });
        });

        // Add 3 translucent ambient droplets
        [
          { x: w * 0.50, y: h * 0.50, radius: 34 },
          { x: w * 0.16, y: h * 0.52, radius: 26 },
          { x: w * 0.84, y: h * 0.48, radius: 28 }
        ].forEach((d) => createBubbleEntity(d));
      }

      // Arena cursor proximity tracking
      arenaEl.addEventListener('pointermove', (e) => {
        refreshArenaBounds();
        arenaPointer.x = e.clientX - arenaRect.left;
        arenaPointer.y = e.clientY - arenaRect.top;
        arenaPointer.active = true;
      }, { passive: true });

      arenaEl.addEventListener('pointerleave', () => {
        arenaPointer.active = false;
      });

      // 60fps Physics Loop
      function stepBubblePhysics() {
        const stiffness = 0.08 + state.springBounce * 0.12;
        const damping = 0.82 - state.springBounce * 0.06;

        for (let i = 0; i < bubbles.length; i++) {
          const b = bubbles[i];

          // Spring scale interpolation
          const scaleForce = (b.targetScale - b.scale) * stiffness;
          b.scaleV = (b.scaleV + scaleForce) * damping;
          b.scale += b.scaleV;

          if (!b.dragging) {
            // Organic sinusoidal float
            b.phase += b.freq;
            b.vx += Math.cos(b.phase) * 0.028;
            b.vy += Math.sin(b.phase * 1.15) * 0.028;

            // Cursor proximity gentle magnetic field
            if (arenaPointer.active) {
              const dx = b.x - arenaPointer.x;
              const dy = b.y - arenaPointer.y;
              const dist = Math.hypot(dx, dy);
              const influenceRadius = b.radius + 95;
              if (dist > 1 && dist < influenceRadius) {
                const factor = (1 - dist / influenceRadius) * 0.42;
                b.vx += (dx / dist) * factor;
                b.vy += (dy / dist) * factor;
              }
            }

            // Soft bubble-to-bubble collision repulsion
            for (let j = i + 1; j < bubbles.length; j++) {
              const b2 = bubbles[j];
              const dx = b2.x - b.x;
              const dy = b2.y - b.y;
              const dist = Math.hypot(dx, dy);
              const minDist = b.radius + b2.radius + 10;
              if (dist > 0.1 && dist < minDist) {
                const overlap = (minDist - dist) * 0.032 * (0.6 + state.springBounce * 0.6);
                const nx = dx / dist;
                const ny = dy / dist;
                if (!b.dragging) {
                  b.vx -= nx * overlap;
                  b.vy -= ny * overlap;
                }
                if (!b2.dragging) {
                  b2.vx += nx * overlap;
                  b2.vy += ny * overlap;
                }
              }
            }

            // Velocity damping & position integration
            b.vx *= 0.965;
            b.vy *= 0.965;
            b.x += b.vx;
            b.y += b.vy;

            // Soft elastic wall bounce inside arena
            const pad = 10;
            if (b.x - b.radius < pad) {
              b.x = b.radius + pad;
              b.vx = Math.abs(b.vx) * (0.65 + state.springBounce * 0.25);
              b.scaleV += 0.03;
            } else if (b.x + b.radius > arenaRect.width - pad) {
              b.x = arenaRect.width - b.radius - pad;
              b.vx = -Math.abs(b.vx) * (0.65 + state.springBounce * 0.25);
              b.scaleV += 0.03;
            }

            if (b.y - b.radius < pad) {
              b.y = b.radius + pad;
              b.vy = Math.abs(b.vy) * (0.65 + state.springBounce * 0.25);
              b.scaleV += 0.03;
            } else if (b.y + b.radius > arenaRect.height - pad) {
              b.y = arenaRect.height - b.radius - pad;
              b.vy = -Math.abs(b.vy) * (0.65 + state.springBounce * 0.25);
              b.scaleV += 0.03;
            }
          }

          // GPU-accelerated transform
          const tx = b.x - b.radius;
          const ty = b.y - b.radius;
          b.el.style.transform = `translate3d(${tx.toFixed(2)}px, ${ty.toFixed(2)}px, 0) scale(${b.scale.toFixed(4)})`;
        }

        requestAnimationFrame(stepBubblePhysics);
      }

      /* ----------------------------------------------------------------------
         8. HARMONY WAVE & SPAWN BUBBLE ACTIONS
         ---------------------------------------------------------------------- */
      function triggerHarmonyWave(originPageX, originPageY, announce = true) {
        refreshArenaBounds();
        const cx = originPageX ?? arenaRect.left + arenaRect.width * 0.5;
        const cy = originPageY ?? arenaRect.top + arenaRect.height * 0.5;

        // Visual expanding glass shockwave ring
        const ring = document.createElement('div');
        ring.className = 'harmony-ring';
        ring.style.left = `${cx}px`;
        ring.style.top = `${cy}px`;
        document.body.appendChild(ring);
        ring.addEventListener('animationend', () => ring.remove());

        // Apply radial spring impulse to all bubbles based on distance
        const localX = cx - arenaRect.left;
        const localY = cy - arenaRect.top;

        bubbles.forEach((b) => {
          const dx = b.x - localX;
          const dy = b.y - localY;
          const dist = Math.max(20, Math.hypot(dx, dy));
          const delay = Math.min(380, dist * 0.8);

          setTimeout(() => {
            const impulse = (4.8 * state.springBounce) / (1 + dist * 0.004);
            b.vx += (dx / dist) * impulse;
            b.vy += (dy / dist) * impulse;
            b.scaleV += 0.16 * state.springBounce;
          }, delay);
        });

        playGlassChime(659.25, 0.35);
        if (announce) showIslandToast('Gelombang Harmoni dikirim ke seluruh gelembung');
      }

      document.getElementById('heroHarmonyBtn').addEventListener('click', (e) => {
        triggerHarmonyWave(e.clientX, e.clientY);
      });

      document.getElementById('harmonyWaveBtn').addEventListener('click', () => {
        refreshArenaBounds();
        triggerHarmonyWave(arenaRect.left + arenaRect.width / 2, arenaRect.top + arenaRect.height / 2);
      });

      document.getElementById('addBubbleBtn').addEventListener('click', () => {
        refreshArenaBounds();
        if (bubbles.length >= 16) {
          // Recycle oldest non-widget droplet so arena stays balanced
          const dropletIdx = bubbles.findIndex((b) => !b.widgetId);
          if (dropletIdx !== -1) {
            bubbles[dropletIdx].el.remove();
            bubbles.splice(dropletIdx, 1);
          }
        }
        const newRadius = 24 + Math.floor(Math.random() * 18);
        const spawnX = arenaRect.width * (0.25 + Math.random() * 0.5);
        const spawnY = arenaRect.height * (0.25 + Math.random() * 0.5);
        const created = createBubbleEntity({ x: spawnX, y: spawnY, radius: newRadius });
        created.scaleV = 0.22 * state.springBounce;
        playGlassChime(587.33 + Math.random() * 220, 0.24);
        showIslandToast('Gelembung spasial baru ditambahkan');
      });

      /* ----------------------------------------------------------------------
         9. PLAYGROUND LIQUID GLASS STUDIO SLIDERS
         ---------------------------------------------------------------------- */
      const blurSlider = document.getElementById('blurSlider');
      const specularSlider = document.getElementById('specularSlider');
      const springSlider = document.getElementById('springSlider');

      const blurValueLabel = document.getElementById('blurValueLabel');
      const specularValueLabel = document.getElementById('specularValueLabel');
      const springValueLabel = document.getElementById('springValueLabel');

      function applyGlassTokens() {
        document.documentElement.style.setProperty('--glass-blur', `${state.glassBlur}px`);
        document.documentElement.style.setProperty('--glass-specular', state.glassSpecular.toFixed(2));
        blurValueLabel.textContent = `${state.glassBlur} px`;
        specularValueLabel.textContent = `${Math.round(state.glassSpecular * 100)}%`;
        springValueLabel.textContent = `${state.springBounce.toFixed(2)}x`;
      }

      blurSlider.addEventListener('input', (e) => {
        state.glassBlur = Number(e.target.value);
        applyGlassTokens();
      });

      specularSlider.addEventListener('input', (e) => {
        state.glassSpecular = Number(e.target.value) / 100;
        applyGlassTokens();
      });

      springSlider.addEventListener('input', (e) => {
        state.springBounce = Number(e.target.value) / 100;
        applyGlassTokens();
      });

      document.getElementById('resetStudioBtn').addEventListener('click', () => {
        state.glassBlur = 28;
        state.glassSpecular = 0.62;
        state.springBounce = 0.65;
        blurSlider.value = 28;
        specularSlider.value = 62;
        springSlider.value = 65;
        applyGlassTokens();
        triggerHarmonyWave();
        showIslandToast('Parameter optik & pegas dikembalikan ke default');
      });

      /* ----------------------------------------------------------------------
         10. INTERACTIVE FLOATING GLASS WIDGET MODAL SYSTEM
         ---------------------------------------------------------------------- */
      const modalBackdrop = document.getElementById('widgetModalBackdrop');
      const modalBody = document.getElementById('widgetModalBody');
      const closeModalBtn = document.getElementById('closeModalBtn');
      let focusInterval = null;

      function openWidgetModal(widgetId) {
        state.activeWidget = widgetId;
        renderWidgetContent(widgetId);
        modalBackdrop.classList.add('open');
        playGlassChime(659.25, 0.25);
        requestAnimationFrame(updateGlassSpecularGlints);
      }

      function closeWidgetModal() {
        modalBackdrop.classList.remove('open');
        state.activeWidget = null;
      }

      closeModalBtn.addEventListener('click', closeWidgetModal);
      modalBackdrop.addEventListener('click', (e) => {
        if (e.target === modalBackdrop) closeWidgetModal();
      });
      window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape' && state.activeWidget) closeWidgetModal();
      });

      document.querySelectorAll('[data-open-widget]').forEach((btn) => {
        btn.addEventListener('click', () => {
          openWidgetModal(btn.getAttribute('data-open-widget'));
        });
      });

      function formatSeconds(sec) {
        const m = String(Math.floor(sec / 60)).padStart(2, '0');
        const s = String(sec % 60).padStart(2, '0');
        return `${m}:${s}`;
      }

      function renderWidgetContent(widgetId) {
        if (widgetId === 'music') {
          const track = state.tracks[state.trackIndex];
          const barsHtml = Array.from({ length: 16 })
            .map((_, idx) => {
              const h = 14 + ((idx * 17) % 26);
              const delay = (idx * 0.08).toFixed(2);
              return `<span class="music-wave-bar" style="height:${h}px; animation-delay:${delay}s"></span>`;
            })
            .join('');

          modalBody.innerHTML = `
            <div class="space-y-5">
              <div class="flex items-center gap-2 text-xs font-bold uppercase tracking-wider text-sky-400">
                <span>Musik Spasial</span>
              </div>

              <!-- Spatial Album Orb -->
              <div class="flex items-center gap-4">
                <div class="w-20 h-20 rounded-[1.5rem] bg-gradient-to-br ${track.color} shadow-lg border border-white/40 flex items-center justify-center shrink-0 relative overflow-hidden">
                  <div class="w-8 h-8 rounded-full bg-white/25 backdrop-blur-md border border-white/50"></div>
                </div>
                <div class="min-w-0">
                  <h3 id="modalTitle" class="text-xl font-extrabold truncate">${track.title}</h3>
                  <p class="text-xs mt-1 truncate" style="color: var(--text-secondary);">${track.artist}</p>
                  <span class="inline-block mt-2 text-[10px] font-semibold px-2.5 py-0.5 rounded-full bg-white/15">Lossless Spatial</span>
                </div>
              </div>

              <!-- Animated Waveform Visualizer -->
              <div id="modalWaveform" class="${state.musicPlaying ? 'music-playing' : ''} h-14 rounded-2xl bg-white/10 border border-white/15 px-4 flex items-center justify-between">
                ${barsHtml}
              </div>

              <!-- Progress Bar -->
              <div class="space-y-1.5">
                <div class="h-1.5 w-full rounded-full bg-white/15 overflow-hidden">
                  <div class="h-full w-2/5 rounded-full bg-gradient-to-r from-sky-400 to-fuchsia-400"></div>
                </div>
                <div class="flex justify-between text-[11px] opacity-70 tabular-nums">
                  <span>01:28</span>
                  <span>${track.duration}</span>
                </div>
              </div>

              <!-- Transport Controls -->
              <div class="flex items-center justify-center gap-5 pt-1">
                <button id="prevTrackBtn" type="button" aria-label="Trek sebelumnya" class="w-11 h-11 rounded-full bg-white/10 hover:bg-white/20 flex items-center justify-center transition-transform active:scale-90 cursor-pointer">
                  <svg class="w-5 h-5" viewBox="0 0 24 24" fill="currentColor"><path d="M6 6h2v12H6zm3.5 6l8.5 6V6z"/></svg>
                </button>
                <button id="playPauseTrackBtn" type="button" aria-label="Putar atau jeda" class="w-14 h-14 rounded-full bg-gradient-to-tr from-sky-400 to-indigo-500 text-white shadow-lg shadow-sky-500/30 flex items-center justify-center transition-transform hover:scale-105 active:scale-95 cursor-pointer">
                  ${
                    state.musicPlaying
                      ? `<svg class="w-6 h-6" viewBox="0 0 24 24" fill="currentColor"><path d="M6 5h4v14H6zm8 0h4v14h-4z"/></svg>`
                      : `<svg class="w-6 h-6 translate-x-0.5" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>`
                  }
                </button>
                <button id="nextTrackBtn" type="button" aria-label="Trek berikutnya" class="w-11 h-11 rounded-full bg-white/10 hover:bg-white/20 flex items-center justify-center transition-transform active:scale-90 cursor-pointer">
                  <svg class="w-5 h-5" viewBox="0 0 24 24" fill="currentColor"><path d="M6 18l8.5-6L6 6v12zM16 6v12h2V6h-2z"/></svg>
                </button>
              </div>
            </div>
          `;

          document.getElementById('playPauseTrackBtn').addEventListener('click', () => {
            state.musicPlaying = !state.musicPlaying;
            renderWidgetContent('music');
            playGlassChime(state.musicPlaying ? 659.25 : 440);
          });
          document.getElementById('prevTrackBtn').addEventListener('click', () => {
            state.trackIndex = (state.trackIndex - 1 + state.tracks.length) % state.tracks.length;
            renderWidgetContent('music');
            playGlassChime(523.25);
          });
          document.getElementById('nextTrackBtn').addEventListener('click', () => {
            state.trackIndex = (state.trackIndex + 1) % state.tracks.length;
            renderWidgetContent('music');
            playGlassChime(587.33);
          });
        } else if (widgetId === 'weather') {
          modalBody.innerHTML = `
            <div class="space-y-5">
              <div class="flex items-center justify-between">
                <div>
                  <span class="text-xs font-bold uppercase tracking-wider text-amber-400">Cuaca & Atmosfer</span>
                  <h3 id="modalTitle" class="text-xl font-extrabold mt-0.5">Jakarta Timur, ID</h3>
                </div>
              </div>

              <!-- Glowing Weather Orb -->
              <div class="relative rounded-[1.75rem] p-6 bg-gradient-to-br from-sky-400/20 via-amber-300/15 to-indigo-500/20 border border-white/25 flex items-center justify-between overflow-hidden">
                <div>
                  <div class="text-5xl font-extrabold tracking-tight">29°C</div>
                  <p class="text-sm font-medium mt-1" style="color: var(--text-secondary);">Cerah Berawan • Terasa 31°</p>
                </div>
                <div class="relative w-20 h-20 flex items-center justify-center">
                  <div class="w-14 h-14 rounded-full bg-gradient-to-tr from-amber-400 to-yellow-200 shadow-[0_0_35px_rgba(251,191,36,0.75)] animate-pulse"></div>
                  <div class="w-14 h-8 rounded-full bg-white/75 backdrop-blur-md border border-white absolute bottom-1 right-0 shadow-md"></div>
                </div>
              </div>

              <!-- Atmospheric Metrics -->
              <div class="grid grid-cols-3 gap-2.5 text-center">
                <div class="p-3 rounded-2xl bg-white/10 border border-white/15">
                  <div class="text-[11px] opacity-70">Kelembapan</div>
                  <div class="text-sm font-bold mt-0.5">68%</div>
                </div>
                <div class="p-3 rounded-2xl bg-white/10 border border-white/15">
                  <div class="text-[11px] opacity-70">Indeks UV</div>
                  <div class="text-sm font-bold mt-0.5">Sedang (4)</div>
                </div>
                <div class="p-3 rounded-2xl bg-white/10 border border-white/15">
                  <div class="text-[11px] opacity-70">Angin</div>
                  <div class="text-sm font-bold mt-0.5">12 km/j</div>
                </div>
              </div>

              <!-- Hourly Forecast Pills -->
              <div class="grid grid-cols-4 gap-2 text-center text-xs">
                <div class="py-2.5 px-2 rounded-2xl bg-white/15 border border-white/25 font-semibold">
                  <div class="opacity-75 text-[10px]">Sekarang</div>
                  <div class="my-1 text-amber-300">☀</div>
                  <div>29°</div>
                </div>
                <div class="py-2.5 px-2 rounded-2xl bg-white/10">
                  <div class="opacity-75 text-[10px]">20:00</div>
                  <div class="my-1 text-sky-300">🌤</div>
                  <div>27°</div>
                </div>
                <div class="py-2.5 px-2 rounded-2xl bg-white/10">
                  <div class="opacity-75 text-[10px]">22:00</div>
                  <div class="my-1 text-indigo-300">🌙</div>
                  <div>26°</div>
                </div>
                <div class="py-2.5 px-2 rounded-2xl bg-white/10">
                  <div class="opacity-75 text-[10px]">00:00</div>
                  <div class="my-1 text-indigo-300">🌙</div>
                  <div>25°</div>
                </div>
              </div>
            </div>
          `;
        } else if (widgetId === 'focus') {
          const circumference = 2 * Math.PI * 66;
          const progress = state.focusSeconds / state.focusTotal;
          const dashOffset = circumference * (1 - progress);

          modalBody.innerHTML = `
            <div class="space-y-5 text-center">
              <div class="text-left">
                <span class="text-xs font-bold uppercase tracking-wider text-emerald-400">Fokus & Produktivitas</span>
                <h3 id="modalTitle" class="text-xl font-extrabold mt-0.5">${state.focusModeLabel}</h3>
              </div>

              <!-- Circular Progress Ring -->
              <div class="relative w-44 h-44 mx-auto flex items-center justify-center">
                <svg class="w-44 h-44 -rotate-90" viewBox="0 0 160 160">
                  <circle cx="80" cy="80" r="66" stroke="rgba(255,255,255,0.14)" stroke-width="10" fill="none" />
                  <circle
                    id="focusRingCircle"
                    cx="80"
                    cy="80"
                    r="66"
                    stroke="url(#focusGrad)"
                    stroke-width="10"
                    stroke-linecap="round"
                    fill="none"
                    stroke-dasharray="${circumference}"
                    stroke-dashoffset="${dashOffset}"
                    style="transition: stroke-dashoffset 0.5s ease;"
                  />
                  <defs>
                    <linearGradient id="focusGrad" x1="0%" y1="0%" x2="100%" y2="100%">
                      <stop offset="0%" stop-color="#34d399" />
                      <stop offset="100%" stop-color="#38bdf8" />
                    </linearGradient>
                  </defs>
                </svg>
                <div class="absolute inset-0 flex flex-col items-center justify-center">
                  <span id="focusTimeDisplay" class="text-3xl font-extrabold tabular-nums tracking-tight">${formatSeconds(state.focusSeconds)}</span>
                  <span class="text-[11px] opacity-70 mt-0.5">${state.focusRunning ? 'Sedang Berjalan' : 'Siap Mulai'}</span>
                </div>
              </div>

              <!-- Mode Presets -->
              <div class="flex justify-center gap-2">
                <button type="button" data-focus-preset="1500" data-focus-name="Fokus Mendalam" class="px-3 py-1.5 rounded-full text-xs font-semibold bg-white/15 hover:bg-white/25 cursor-pointer">Fokus 25m</button>
                <button type="button" data-focus-preset="300" data-focus-name="Istirahat Singkat" class="px-3 py-1.5 rounded-full text-xs font-semibold bg-white/10 hover:bg-white/20 cursor-pointer">Istirahat 5m</button>
                <button type="button" data-focus-preset="60" data-focus-name="Napas Spasial" class="px-3 py-1.5 rounded-full text-xs font-semibold bg-white/10 hover:bg-white/20 cursor-pointer">Napas 1m</button>
              </div>

              <!-- Start / Reset Controls -->
              <div class="flex items-center justify-center gap-3 pt-1">
                <button id="toggleFocusBtn" type="button" class="px-6 py-2.5 rounded-full font-bold text-sm bg-gradient-to-r from-emerald-400 to-sky-500 text-slate-950 shadow-lg transition-transform active:scale-95 cursor-pointer">
                  ${state.focusRunning ? 'Jeda Timer' : 'Mulai Fokus'}
                </button>
                <button id="resetFocusBtn" type="button" class="px-4 py-2.5 rounded-full font-semibold text-xs bg-white/10 hover:bg-white/20 cursor-pointer">
                  Ulangi
                </button>
              </div>
            </div>
          `;

          document.querySelectorAll('[data-focus-preset]').forEach((btn) => {
            btn.addEventListener('click', () => {
              clearInterval(focusInterval);
              state.focusRunning = false;
              const secs = Number(btn.getAttribute('data-focus-preset'));
              state.focusSeconds = secs;
              state.focusTotal = secs;
              state.focusModeLabel = btn.getAttribute('data-focus-name');
              renderWidgetContent('focus');
              playGlassChime(587.33);
            });
          });

          document.getElementById('toggleFocusBtn').addEventListener('click', () => {
            state.focusRunning = !state.focusRunning;
            clearInterval(focusInterval);
            if (state.focusRunning) {
              focusInterval = setInterval(() => {
                if (state.focusSeconds > 0) {
                  state.focusSeconds -= 1;
                  if (state.activeWidget === 'focus') {
                    const disp = document.getElementById('focusTimeDisplay');
                    const ring = document.getElementById('focusRingCircle');
                    if (disp) disp.textContent = formatSeconds(state.focusSeconds);
                    if (ring) {
                      const prog = state.focusSeconds / state.focusTotal;
                      ring.style.strokeDashoffset = circumference * (1 - prog);
                    }
                  }
                } else {
                  state.focusRunning = false;
                  clearInterval(focusInterval);
                  showIslandToast('Sesi Fokus Selesai!');
                  if (state.activeWidget === 'focus') renderWidgetContent('focus');
                }
              }, 1000);
            }
            renderWidgetContent('focus');
            playGlassChime(state.focusRunning ? 784 : 440);
          });

          document.getElementById('resetFocusBtn').addEventListener('click', () => {
            clearInterval(focusInterval);
            state.focusRunning = false;
            state.focusSeconds = state.focusTotal;
            renderWidgetContent('focus');
          });
        } else if (widgetId === 'gallery') {
          const orb = state.lightOrbs[state.selectedOrbIndex];
          const swatchesHtml = state.lightOrbs
            .map(
              (item, idx) => `
              <button
                type="button"
                data-orb-idx="${idx}"
                class="p-2.5 rounded-2xl border transition-all text-left flex items-center gap-2.5 cursor-pointer ${
                  idx === state.selectedOrbIndex ? 'bg-white/20 border-white/60 scale-[1.02]' : 'bg-white/10 border-white/15 hover:bg-white/15'
                }"
              >
                <span class="w-6 h-6 rounded-full shrink-0 border border-white/50" style="background: ${item.grad};"></span>
                <span class="text-xs font-semibold truncate">${item.name}</span>
              </button>
            `
            )
            .join('');

          modalBody.innerHTML = `
            <div class="space-y-5">
              <div>
                <span class="text-xs font-bold uppercase tracking-wider text-fuchsia-400">Galeri Cahaya</span>
                <h3 id="modalTitle" class="text-xl font-extrabold mt-0.5">Prisma Orb — ${orb.name}</h3>
              </div>

              <!-- 3D Refractive Glass Sphere Showcase -->
              <div class="py-4 flex justify-center">
                <div
                  class="w-36 h-36 rounded-full relative shadow-2xl border border-white/50 transition-all duration-700"
                  style="background: ${orb.grad}; box-shadow: 0 24px 48px -10px rgba(0,0,0,0.55), inset 0 4px 10px rgba(255,255,255,0.85);"
                >
                  <div class="absolute top-3 left-5 w-16 h-8 rounded-full bg-white/45 blur-[1px] -rotate-12"></div>
                </div>
              </div>

              <p class="text-xs text-center" style="color: var(--text-secondary);">
                Ketuk preset spektrum di bawah untuk menyelaraskan pencahayaan latar Aurora secara langsung.
              </p>

              <div class="grid grid-cols-2 gap-2.5">
                ${swatchesHtml}
              </div>
            </div>
          `;

          document.querySelectorAll('[data-orb-idx]').forEach((btn) => {
            btn.addEventListener('click', () => {
              const idx = Number(btn.getAttribute('data-orb-idx'));
              state.selectedOrbIndex = idx;
              const chosen = state.lightOrbs[idx];
              if (state.theme === 'dark') {
                document.documentElement.style.setProperty('--aurora-1', chosen.a1);
                document.documentElement.style.setProperty('--aurora-2', chosen.a2);
                document.documentElement.style.setProperty('--aurora-3', chosen.a3);
              }
              renderWidgetContent('gallery');
              playGlassChime(659.25 + idx * 80);
              showIslandToast(`Spektrum "${chosen.name}" diterapkan`);
            });
          });
        }
      }

      /* ----------------------------------------------------------------------
         11. FLOATING BOTTOM DOCK: NEIGHBOR MAGNIFICATION & SLIDING PILL
         ---------------------------------------------------------------------- */
      const dockEl = document.getElementById('liquidDock');
      const dockIndicatorEl = document.getElementById('dockIndicator');
      const dockItems = Array.from(dockEl.querySelectorAll('.dock-item'));

      function updateDockIndicator() {
        const activeItem = dockItems.find((item) => item.getAttribute('data-dock-target') === state.activeSection) || dockItems[0];
        if (!activeItem) return;
        dockIndicatorEl.style.width = `${activeItem.offsetWidth}px`;
        dockIndicatorEl.style.transform = `translate3d(${activeItem.offsetLeft}px, 0, 0)`;
      }

      // Smooth neighbor magnification on desktop hover
      dockEl.addEventListener('pointermove', (e) => {
        if (window.innerWidth < 640) return;
        dockItems.forEach((item) => {
          const rect = item.getBoundingClientRect();
          const center = rect.left + rect.width / 2;
          const distance = Math.abs(e.clientX - center);
          const maxDist = 130;
          const scale = distance < maxDist ? 1 + (1 - distance / maxDist) * 0.16 : 1;
          const lift = distance < maxDist ? -(1 - distance / maxDist) * 3 : 0;
          item.style.transform = `translate3d(0, ${lift.toFixed(2)}px, 0) scale(${scale.toFixed(3)})`;
        });
      });

      dockEl.addEventListener('pointerleave', () => {
        dockItems.forEach((item) => {
          item.style.transform = 'translate3d(0, 0, 0) scale(1)';
        });
      });

      dockItems.forEach((item) => {
        item.addEventListener('click', () => {
          const target = item.getAttribute('data-dock-target');
          state.activeSection = target;
          updateDockIndicator();
          playGlassChime(587.33, 0.16);
        });
      });

      // Sync active section automatically on scroll via IntersectionObserver
      const sections = document.querySelectorAll('[data-section]');
      const sectionObserver = new IntersectionObserver(
        (entries) => {
          entries.forEach((entry) => {
            if (entry.isIntersecting) {
              state.activeSection = entry.target.getAttribute('data-section');
              updateDockIndicator();
            }
          });
        },
        { threshold: 0.42 }
      );
      sections.forEach((sec) => sectionObserver.observe(sec));

      /* ----------------------------------------------------------------------
         12. INITIALIZATION & RESIZE HANDLERS
         ---------------------------------------------------------------------- */
      window.addEventListener('resize', () => {
        refreshArenaBounds();
        updateDockIndicator();
      }, { passive: true });

      window.addEventListener('load', () => {
        applyGlassTokens();
        initArenaBubbles();
        updateDockIndicator();
        updateGlassSpecularGlints();
        requestAnimationFrame(stepBubblePhysics);
      });
    })();
  </script>
</body>
</html>