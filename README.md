<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>🎨 El Gato Artista 🎨</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800&display=swap');

    :root {
      --sky: #87CEEB;
      --grass: #4CAF50;
      --path: #D2B48C;
      --wood: #8B5A2B;
      --ui-bg: #fff8f0;
      --accent: #ff6b6b;
      --gold: #ffd700;
      --cat: #f4a460;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Nunito', sans-serif;
      background: linear-gradient(180deg, #1a1a2e 0%, #16213e 100%);
      min-height: 100vh;
      color: #333;
      overflow-x: hidden;
    }

    #app {
      max-width: 960px;
      margin: 0 auto;
      padding: 12px;
      display: none;
    }

    header {
      text-align: center;
      padding: 16px 0 8px;
      color: #fff;
      animation: fadeSlideDown 0.6s ease-out;
    }

    header h1 {
      font-size: 2rem;
      font-weight: 800;
      text-shadow: 2px 2px 0 #ff6b6b;
      letter-spacing: -1px;
      animation: titlePulse 3s ease-in-out infinite;
    }

    header p { opacity: 0.85; font-size: 0.95rem; margin-top: 4px; }

    @keyframes fadeSlideDown {
      from { opacity: 0; transform: translateY(-20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @keyframes titlePulse {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.03); }
    }

    .stats {
      display: flex;
      justify-content: center;
      gap: 18px;
      flex-wrap: wrap;
      margin: 12px 0;
      background: rgba(255,255,255,0.12);
      border-radius: 16px;
      padding: 10px 16px;
      color: #fff;
      font-weight: 600;
      animation: fadeSlideDown 0.7s ease-out;
    }

    .stats span {
      display: flex;
      align-items: center;
      gap: 6px;
      transition: transform 0.2s;
    }

    .stats span.pop {
      animation: statPop 0.45s ease;
    }

    @keyframes statPop {
      0% { transform: scale(1); }
      40% { transform: scale(1.25); color: #ffd700; }
      100% { transform: scale(1); }
    }

    .nav {
      display: flex;
      gap: 8px;
      justify-content: center;
      flex-wrap: wrap;
      margin-bottom: 14px;
      animation: fadeSlideDown 0.8s ease-out;
    }

    .nav button, .btn {
      border: none;
      border-radius: 12px;
      padding: 10px 18px;
      font-family: inherit;
      font-weight: 700;
      font-size: 0.95rem;
      cursor: pointer;
      transition: transform 0.15s, box-shadow 0.15s, background 0.2s;
      background: #fff;
      color: #333;
      box-shadow: 0 3px 0 #ccc;
      position: relative;
      overflow: hidden;
    }

    .nav button::after, .btn::after {
      content: '';
      position: absolute;
      top: 50%; left: 50%;
      width: 0; height: 0;
      background: rgba(255,255,255,0.35);
      border-radius: 50%;
      transform: translate(-50%, -50%);
      transition: width 0.4s, height 0.4s;
    }

    .nav button:active::after, .btn:active::after {
      width: 200px; height: 200px;
    }

    .nav button:hover, .btn:hover { transform: translateY(-2px); }
    .nav button:active, .btn:active { transform: translateY(1px); box-shadow: 0 1px 0 #ccc; }
    .nav button.active { background: var(--accent); color: #fff; box-shadow: 0 3px 0 #c0392b; }
    .btn-primary { background: #4CAF50; color: #fff; box-shadow: 0 3px 0 #2e7d32; }
    .btn-gold { background: var(--gold); color: #333; box-shadow: 0 3px 0 #c9a000; }
    .btn:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }

    .panel {
      display: none;
      background: var(--ui-bg);
      border-radius: 20px;
      padding: 16px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.25);
      min-height: 420px;
      animation: panelIn 0.35s ease;
    }

    .panel.active { display: block; }

    @keyframes panelIn {
      from { opacity: 0; transform: translateY(12px) scale(0.98); }
      to { opacity: 1; transform: translateY(0) scale(1); }
    }

    /* ===== PINTAR ===== */
    #paint-area {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 12px;
    }

    .tools {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      justify-content: center;
      align-items: center;
      width: 100%;
    }

    .color-swatch {
      width: 32px; height: 32px;
      border-radius: 50%;
      border: 3px solid #fff;
      box-shadow: 0 0 0 2px #333;
      cursor: pointer;
      transition: transform 0.15s, box-shadow 0.15s;
    }
    .color-swatch:hover { transform: scale(1.15); }
    .color-swatch.selected {
      transform: scale(1.25);
      box-shadow: 0 0 0 3px var(--accent), 0 0 12px var(--accent);
      animation: swatchPulse 1.5s ease infinite;
    }

    @keyframes swatchPulse {
      0%, 100% { box-shadow: 0 0 0 3px var(--accent), 0 0 8px var(--accent); }
      50% { box-shadow: 0 0 0 3px var(--accent), 0 0 16px var(--accent); }
    }

    .brush-size {
      display: flex; gap: 6px; align-items: center;
    }

    .brush-btn {
      width: 36px; height: 36px;
      border-radius: 50%;
      border: 2px solid #333;
      background: #eee;
      cursor: pointer;
      display: flex; align-items: center; justify-content: center;
      font-size: 0.7rem; font-weight: 700;
      transition: transform 0.15s, background 0.15s;
    }
    .brush-btn:hover { transform: scale(1.1); }
    .brush-btn.selected {
      background: var(--accent);
      color: #fff;
      border-color: #c0392b;
      animation: brushSelect 0.3s ease;
    }

    @keyframes brushSelect {
      0% { transform: scale(0.8); }
      60% { transform: scale(1.15); }
      100% { transform: scale(1); }
    }

    #canvas-wrap {
      position: relative;
      border: 8px solid var(--wood);
      border-radius: 4px;
      box-shadow: 0 6px 16px rgba(0,0,0,0.3);
      background: #fff;
      touch-action: none;
      transition: box-shadow 0.3s;
    }

    #canvas-wrap.painting-active {
      box-shadow: 0 6px 24px rgba(255,107,107,0.4);
    }

    #paint-canvas {
      display: block;
      cursor: crosshair;
      background: #fff;
    }

    /* ===== CALLE ===== */
    #street-scene {
      position: relative;
      width: 100%;
      height: 380px;
      border-radius: 16px;
      overflow: hidden;
      background: linear-gradient(180deg, #87CEEB 0%, #b0e0e6 55%, #90EE90 55%, #4CAF50 100%);
    }

    .park-bg {
      position: absolute; inset: 0;
      pointer-events: none;
    }

    .tree {
      position: absolute;
      bottom: 27%; /* apoyados sobre el pasto, justo encima del camino */
      width: 70px;
      height: 100px;
      transform-origin: bottom center;
      animation: treeSway 5s ease-in-out infinite;
    }
    .tree.tree-sm {
      width: 50px;
      height: 75px;
    }
    .tree.tree-lg {
      width: 85px;
      height: 115px;
    }
    .tree:nth-child(1) { animation-delay: 0s; animation-duration: 5.5s; }
    .tree:nth-child(2) { animation-delay: 1.2s; animation-duration: 4.8s; }
    .tree:nth-child(3) { animation-delay: 0.6s; animation-duration: 6s; }
    .tree:nth-child(4) { animation-delay: 1.8s; animation-duration: 5.2s; }

    @keyframes treeSway {
      0%, 100% { transform: rotate(-1.5deg); }
      50% { transform: rotate(1.5deg); }
    }

    .tree .trunk {
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%);
      width: 12%;
      height: 38%;
      background: linear-gradient(90deg, #5d4037, #8d6e63 40%, #5d4037);
      border-radius: 3px 3px 1px 1px;
    }

    .tree .canopy {
      position: absolute;
      border-radius: 50%;
    }
    /* Capas de follaje para aspecto más natural */
    .tree .canopy.c1 {
      bottom: 28%;
      left: 50%;
      transform: translateX(-50%);
      width: 85%;
      height: 48%;
      background: #2e7d32;
    }
    .tree .canopy.c2 {
      bottom: 42%;
      left: 50%;
      transform: translateX(-58%);
      width: 55%;
      height: 38%;
      background: #388e3c;
    }
    .tree .canopy.c3 {
      bottom: 42%;
      left: 50%;
      transform: translateX(-42%);
      width: 55%;
      height: 38%;
      background: #43a047;
    }
    .tree .canopy.c4 {
      bottom: 55%;
      left: 50%;
      transform: translateX(-50%);
      width: 48%;
      height: 32%;
      background: #4caf50;
    }
    /* Variante de color más oscura */
    .tree.dark .canopy.c1 { background: #1b5e20; }
    .tree.dark .canopy.c2 { background: #2e7d32; }
    .tree.dark .canopy.c3 { background: #33691e; }
    .tree.dark .canopy.c4 { background: #388e3c; }

    .path {
      position: absolute;
      bottom: 0; left: 0; right: 0;
      height: 28%;
      background: linear-gradient(180deg, #d2b48c 0%, #c4a484 100%);
    }

    /* Nubes animadas */
    .cloud {
      position: absolute;
      background: #fff;
      border-radius: 40px;
      opacity: 0.9;
      animation: cloudDrift linear infinite;
    }
    .cloud::before, .cloud::after {
      content: '';
      position: absolute;
      background: #fff;
      border-radius: 50%;
    }

    @keyframes cloudDrift {
      from { transform: translateX(0); }
      to { transform: translateX(calc(100vw + 100px)); }
    }

    /* Sol con brillo */
    .sun {
      position: absolute;
      top: 18px; right: 30px;
      width: 50px; height: 50px;
      background: #FFD700;
      border-radius: 50%;
      box-shadow: 0 0 30px #FFD700;
      animation: sunGlow 3s ease-in-out infinite;
    }

    @keyframes sunGlow {
      0%, 100% { box-shadow: 0 0 25px #FFD700, 0 0 50px rgba(255,215,0,0.3); }
      50% { box-shadow: 0 0 40px #FFD700, 0 0 80px rgba(255,215,0,0.5); }
    }

    .easel {
      position: absolute;
      bottom: 22%;
      left: 50%;
      transform: translateX(-50%);
      width: 140px;
      z-index: 5;
      animation: easelAppear 0.6s ease;
    }

    @keyframes easelAppear {
      from { opacity: 0; transform: translateX(-50%) translateY(20px); }
      to { opacity: 1; transform: translateX(-50%) translateY(0); }
    }

    .easel-legs {
      position: absolute;
      bottom: 0; left: 50%;
      transform: translateX(-50%);
      width: 100px; height: 70px;
      border-left: 6px solid #5d4037;
      border-right: 6px solid #5d4037;
    }

    .easel-board {
      position: absolute;
      bottom: 60px; left: 50%;
      transform: translateX(-50%);
      width: 120px; height: 90px;
      background: #f5f5dc;
      border: 4px solid #5d4037;
      box-shadow: 0 4px 8px rgba(0,0,0,0.3);
      overflow: hidden;
      transition: transform 0.3s;
    }

    .easel-board.highlight {
      animation: easelHighlight 0.6s ease;
    }

    @keyframes easelHighlight {
      0%, 100% { transform: translateX(-50%) scale(1); }
      50% { transform: translateX(-50%) scale(1.08); box-shadow: 0 0 20px rgba(255,215,0,0.6); }
    }

    .easel-board canvas {
      width: 100%; height: 100%;
      display: block;
    }

    .cat-artist {
      position: absolute;
      bottom: 14%;
      left: calc(50% + 70px);
      width: 72px;
      height: 72px;
      z-index: 6;
      animation: catBounce 1.2s ease-in-out infinite;
      transform-origin: bottom center;
      filter: drop-shadow(2px 3px 3px rgba(0,0,0,0.25));
    }

    .cat-artist svg {
      width: 100%;
      height: 100%;
      display: block;
    }

    .cat-artist.happy {
      animation: catHappy 0.6s ease;
    }

    @keyframes catBounce {
      0%, 100% { transform: translateY(0) rotate(-3deg); }
      50% { transform: translateY(-8px) rotate(3deg); }
    }

    @keyframes catHappy {
      0% { transform: scale(1) rotate(0); }
      25% { transform: scale(1.25) rotate(-12deg); }
      50% { transform: scale(1.15) rotate(12deg); }
      75% { transform: scale(1.25) rotate(-8deg); }
      100% { transform: scale(1) rotate(0); }
    }

    .citizen {
      position: absolute;
      bottom: 8%;
      width: 56px;
      height: 56px;
      z-index: 10;
      filter: drop-shadow(1px 2px 2px rgba(0,0,0,0.25));
      animation: citizenWalk 0.4s ease-in-out infinite;
      will-change: left;
    }

    .citizen svg {
      width: 100%;
      height: 100%;
      display: block;
    }

    .citizen.looking {
      animation: citizenLook 0.5s ease;
    }

    .citizen-comment {
      position: absolute;
      bottom: 100%;
      left: 50%;
      transform: translateX(-50%);
      background: #fff;
      color: #333;
      font-size: 0.72rem;
      font-weight: 700;
      padding: 5px 10px;
      border-radius: 12px;
      white-space: nowrap;
      box-shadow: 0 3px 10px rgba(0,0,0,0.18);
      pointer-events: none;
      z-index: 15;
      animation: commentPop 0.35s cubic-bezier(0.34, 1.56, 0.64, 1);
      margin-bottom: 4px;
    }

    .citizen-comment::after {
      content: '';
      position: absolute;
      top: 100%;
      left: 50%;
      transform: translateX(-50%);
      border: 6px solid transparent;
      border-top-color: #fff;
    }

    @keyframes commentPop {
      from { opacity: 0; transform: translateX(-50%) translateY(6px) scale(0.8); }
      to { opacity: 1; transform: translateX(-50%) translateY(0) scale(1); }
    }

    @keyframes citizenWalk {
      0%, 100% { transform: translateY(0) rotate(-2deg); }
      50% { transform: translateY(-5px) rotate(2deg); }
    }

    @keyframes citizenLook {
      0% { transform: scale(1); }
      30% { transform: scale(1.2) rotate(-10deg); }
      60% { transform: scale(1.15) rotate(10deg); }
      100% { transform: scale(1); }
    }

    .rating-bubble {
      position: absolute;
      bottom: 55%;
      left: 50%;
      transform: translateX(-50%);
      background: #fff;
      border-radius: 16px;
      padding: 8px 14px;
      font-weight: 800;
      font-size: 1.1rem;
      box-shadow: 0 4px 12px rgba(0,0,0,0.2);
      z-index: 20;
      animation: bubblePop 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
      white-space: nowrap;
    }

    @keyframes bubblePop {
      0% { transform: translateX(-50%) scale(0) rotate(-10deg); opacity: 0; }
      60% { transform: translateX(-50%) scale(1.15) rotate(3deg); opacity: 1; }
      100% { transform: translateX(-50%) scale(1) rotate(0); opacity: 1; }
    }

    /* Partículas de dinero */
    .coin-particle {
      position: absolute;
      font-size: 18px;
      pointer-events: none;
      z-index: 30;
      animation: coinFly 1s ease-out forwards;
    }

    @keyframes coinFly {
      0% { opacity: 1; transform: translateY(0) scale(1); }
      100% { opacity: 0; transform: translateY(-80px) scale(1.4); }
    }

    .street-controls {
      margin-top: 12px;
      text-align: center;
      display: flex;
      gap: 10px;
      justify-content: center;
      flex-wrap: wrap;
    }

    /* ===== TIENDA ===== */
    .shop-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 12px;
    }

    .shop-item {
      background: #fff;
      border-radius: 14px;
      padding: 14px;
      border: 2px solid #eee;
      text-align: center;
      transition: border-color 0.2s, transform 0.2s, box-shadow 0.2s;
      animation: shopItemIn 0.4s ease backwards;
    }
    .shop-item:nth-child(1) { animation-delay: 0.05s; }
    .shop-item:nth-child(2) { animation-delay: 0.1s; }
    .shop-item:nth-child(3) { animation-delay: 0.15s; }
    .shop-item:nth-child(4) { animation-delay: 0.2s; }
    .shop-item:nth-child(5) { animation-delay: 0.25s; }
    .shop-item:nth-child(6) { animation-delay: 0.3s; }
    .shop-item:nth-child(7) { animation-delay: 0.35s; }
    .shop-item:nth-child(8) { animation-delay: 0.4s; }
    .shop-item:nth-child(9) { animation-delay: 0.45s; }
    .shop-item:nth-child(10) { animation-delay: 0.5s; }

    @keyframes shopItemIn {
      from { opacity: 0; transform: translateY(15px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .shop-item:hover {
      border-color: var(--accent);
      transform: translateY(-4px);
      box-shadow: 0 8px 20px rgba(0,0,0,0.1);
    }
    .shop-item h3 { font-size: 1rem; margin-bottom: 4px; }
    .shop-item p { font-size: 0.85rem; color: #666; margin-bottom: 8px; }
    .shop-item .price { font-weight: 800; color: #2e7d32; margin-bottom: 8px; }

    .shop-item.just-bought {
      animation: boughtFlash 0.6s ease;
    }

    @keyframes boughtFlash {
      0%, 100% { background: #fff; }
      50% { background: #c8e6c9; transform: scale(1.05); }
    }

    /* ===== MUSEO ===== */
    .progress-bar-wrap {
      background: #ddd;
      border-radius: 20px;
      height: 18px;
      margin: 12px 0;
      overflow: hidden;
    }
    .progress-bar {
      height: 100%;
      background: linear-gradient(90deg, #ff6b6b, #ffd700);
      border-radius: 20px;
      transition: width 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
      position: relative;
    }
    .progress-bar::after {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0; bottom: 0;
      background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
      animation: shimmer 2s infinite;
    }

    @keyframes shimmer {
      0% { transform: translateX(-100%); }
      100% { transform: translateX(100%); }
    }

    .museum-preview {
      text-align: center;
      padding: 20px;
    }

    .museum-preview .locked {
      font-size: 4rem;
      opacity: 0.4;
      filter: grayscale(1);
    }

    #museum-unlocked {
      animation: museumUnlock 1s ease;
    }

    @keyframes museumUnlock {
      0% { opacity: 0; transform: scale(0.5) rotate(-10deg); }
      60% { transform: scale(1.15) rotate(5deg); }
      100% { opacity: 1; transform: scale(1) rotate(0); }
    }

    .gallery {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
      gap: 12px;
      margin-top: 12px;
    }
    .gallery-item {
      background: #fff;
      border: 4px solid #5d4037;
      border-radius: 4px;
      overflow: hidden;
      position: relative;
      cursor: pointer;
      transition: transform 0.2s, box-shadow 0.2s;
      animation: galleryIn 0.4s ease backwards;
    }
    .gallery-item:hover {
      transform: scale(1.05) rotate(-1deg);
      box-shadow: 0 6px 16px rgba(0,0,0,0.2);
    }
    .gallery-item canvas {
      width: 100%; height: 100px;
      display: block;
    }
    .gallery-item .label {
      font-size: 0.75rem;
      padding: 4px;
      text-align: center;
      background: #f5f5dc;
      font-weight: 600;
    }

    @keyframes galleryIn {
      from { opacity: 0; transform: scale(0.8); }
      to { opacity: 1; transform: scale(1); }
    }

    .inspiration {
      margin-top: 16px;
      padding: 12px;
      background: #fff3e0;
      border-radius: 12px;
      border-left: 4px solid #ff9800;
      font-size: 0.9rem;
      animation: fadeSlideDown 0.5s ease;
    }

    .toast {
      position: fixed;
      bottom: 24px;
      left: 50%;
      transform: translateX(-50%) translateY(80px);
      background: #333;
      color: #fff;
      padding: 12px 24px;
      border-radius: 30px;
      font-weight: 700;
      z-index: 100;
      transition: transform 0.35s cubic-bezier(0.34, 1.56, 0.64, 1);
      pointer-events: none;
      box-shadow: 0 6px 20px rgba(0,0,0,0.3);
    }
    .toast.show { transform: translateX(-50%) translateY(0); }

    /* Confetti para museo */
    .confetti {
      position: fixed;
      width: 10px; height: 10px;
      top: -10px;
      z-index: 200;
      animation: confettiFall 3s ease-in forwards;
      pointer-events: none;
    }

    @keyframes confettiFall {
      0% { transform: translateY(0) rotate(0deg); opacity: 1; }
      100% { transform: translateY(100vh) rotate(720deg); opacity: 0; }
    }

    /* Toggle sonido */
    .sound-toggle {
      position: fixed;
      top: 12px;
      right: 12px;
      background: rgba(255,255,255,0.15);
      border: none;
      border-radius: 50%;
      width: 44px; height: 44px;
      font-size: 1.3rem;
      cursor: pointer;
      z-index: 50;
      transition: transform 0.2s, background 0.2s;
      color: #fff;
    }
    .sound-toggle:hover { transform: scale(1.1); background: rgba(255,255,255,0.25); }
    .sound-toggle.muted { opacity: 0.5; }

    footer {
      text-align: center;
      color: rgba(255,255,255,0.5);
      font-size: 0.8rem;
      padding: 16px 0;
    }

    /* ===== MENÚ PRINCIPAL ===== */
    #main-menu {
      position: fixed;
      inset: 0;
      z-index: 90;
      display: flex;
      align-items: center;
      justify-content: center;
      background: linear-gradient(160deg, #1a1a2e 0%, #16213e 40%, #0f3460 100%);
      overflow: hidden;
    }

    #main-menu.hidden {
      animation: menuOut 0.5s ease forwards;
      pointer-events: none;
    }

    @keyframes menuOut {
      to { opacity: 0; transform: scale(1.05); }
    }

    .menu-bg-deco {
      position: absolute;
      inset: 0;
      pointer-events: none;
      overflow: hidden;
    }

    .menu-bg-deco .blob {
      position: absolute;
      border-radius: 50%;
      filter: blur(60px);
      opacity: 0.25;
      animation: blobFloat 12s ease-in-out infinite;
    }

    .menu-bg-deco .blob:nth-child(1) {
      width: 280px; height: 280px;
      background: #ff6b6b;
      top: -40px; left: -60px;
    }
    .menu-bg-deco .blob:nth-child(2) {
      width: 220px; height: 220px;
      background: #ffd700;
      bottom: 10%; right: -40px;
      animation-delay: 3s;
    }
    .menu-bg-deco .blob:nth-child(3) {
      width: 180px; height: 180px;
      background: #4CAF50;
      bottom: -30px; left: 30%;
      animation-delay: 6s;
    }

    @keyframes blobFloat {
      0%, 100% { transform: translate(0, 0) scale(1); }
      50% { transform: translate(20px, -25px) scale(1.1); }
    }

    .menu-card {
      position: relative;
      background: rgba(255, 248, 240, 0.97);
      border-radius: 28px;
      padding: 36px 40px 32px;
      text-align: center;
      box-shadow: 0 20px 60px rgba(0,0,0,0.4), 0 0 0 1px rgba(255,255,255,0.1);
      max-width: 380px;
      width: 90%;
      animation: menuCardIn 0.7s cubic-bezier(0.34, 1.56, 0.64, 1);
    }

    @keyframes menuCardIn {
      from { opacity: 0; transform: translateY(40px) scale(0.9); }
      to { opacity: 1; transform: translateY(0) scale(1); }
    }

    .menu-cat {
      width: 110px;
      height: 110px;
      margin: 0 auto 12px;
      animation: catBounce 1.4s ease-in-out infinite;
      filter: drop-shadow(0 6px 12px rgba(0,0,0,0.2));
    }

    .menu-card h1 {
      font-size: 1.85rem;
      font-weight: 800;
      color: #1a1a2e;
      margin-bottom: 4px;
      letter-spacing: -0.5px;
    }

    .menu-card .subtitle {
      color: #666;
      font-size: 0.95rem;
      margin-bottom: 28px;
      line-height: 1.4;
    }

    .menu-buttons {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .menu-btn {
      border: none;
      border-radius: 16px;
      padding: 16px 24px;
      font-family: inherit;
      font-weight: 800;
      font-size: 1.1rem;
      cursor: pointer;
      transition: transform 0.15s, box-shadow 0.15s, background 0.2s;
      position: relative;
      overflow: hidden;
    }

    .menu-btn-start {
      background: linear-gradient(135deg, #ff6b6b, #ee5a24);
      color: #fff;
      box-shadow: 0 6px 0 #c0392b, 0 8px 20px rgba(238, 90, 36, 0.35);
    }
    .menu-btn-start:hover {
      transform: translateY(-3px);
      box-shadow: 0 9px 0 #c0392b, 0 12px 28px rgba(238, 90, 36, 0.4);
    }
    .menu-btn-start:active {
      transform: translateY(2px);
      box-shadow: 0 2px 0 #c0392b;
    }

    .menu-btn-gallery {
      background: #fff;
      color: #333;
      border: 2.5px solid #ddd;
      box-shadow: 0 4px 0 #ccc;
    }
    .menu-btn-gallery:hover {
      transform: translateY(-2px);
      border-color: #ff6b6b;
      color: #ff6b6b;
      box-shadow: 0 6px 0 #e0a0a0;
    }
    .menu-btn-gallery:active {
      transform: translateY(1px);
      box-shadow: 0 1px 0 #ccc;
    }

    .menu-footer {
      margin-top: 22px;
      font-size: 0.8rem;
      color: #999;
    }

    /* Galería desde el menú */
    #menu-gallery-overlay {
      position: fixed;
      inset: 0;
      z-index: 95;
      background: rgba(10, 10, 30, 0.85);
      display: none;
      align-items: center;
      justify-content: center;
      padding: 20px;
      backdrop-filter: blur(6px);
    }

    #menu-gallery-overlay.show {
      display: flex;
      animation: fadeIn 0.3s ease;
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    .menu-gallery-card {
      background: #fff8f0;
      border-radius: 24px;
      padding: 24px;
      max-width: 640px;
      width: 100%;
      max-height: 85vh;
      overflow-y: auto;
      box-shadow: 0 20px 50px rgba(0,0,0,0.4);
      animation: menuCardIn 0.4s ease;
    }

    .menu-gallery-card h2 {
      text-align: center;
      margin-bottom: 6px;
      color: #1a1a2e;
    }

    .menu-gallery-card .gallery-hint {
      text-align: center;
      color: #888;
      font-size: 0.9rem;
      margin-bottom: 16px;
    }

    .menu-gallery-empty {
      text-align: center;
      padding: 40px 20px;
      color: #999;
    }

    .menu-gallery-empty .empty-cat {
      width: 80px;
      height: 80px;
      margin: 0 auto 12px;
      opacity: 0.5;
    }

    #app.visible {
      display: block;
      animation: fadeSlideDown 0.5s ease;
    }

    /* ===== OFERTA DE COMPRA ===== */
    #buy-offer-overlay {
      position: fixed;
      inset: 0;
      z-index: 100;
      background: rgba(10, 10, 30, 0.75);
      display: none;
      align-items: center;
      justify-content: center;
      padding: 20px;
      backdrop-filter: blur(5px);
    }

    #buy-offer-overlay.show {
      display: flex;
      animation: fadeIn 0.3s ease;
    }

    .buy-offer-card {
      background: #fff8f0;
      border-radius: 24px;
      padding: 28px 32px;
      max-width: 380px;
      width: 100%;
      text-align: center;
      box-shadow: 0 20px 50px rgba(0,0,0,0.4);
      animation: menuCardIn 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
      border: 3px solid #ffd700;
    }

    .buy-offer-card .offer-icon {
      font-size: 2.8rem;
      margin-bottom: 8px;
      animation: catBounce 1s ease-in-out infinite;
    }

    .buy-offer-card h2 {
      color: #1a1a2e;
      font-size: 1.35rem;
      margin-bottom: 10px;
    }

    .buy-offer-card .offer-amount {
      font-size: 1.8rem;
      font-weight: 800;
      color: #2e7d32;
      margin: 12px 0 20px;
    }

    .buy-offer-card .offer-buttons {
      display: flex;
      gap: 12px;
      justify-content: center;
      flex-wrap: wrap;
    }

    .buy-offer-card .offer-buttons .btn {
      min-width: 140px;
      padding: 12px 18px;
      font-size: 1rem;
    }

    .btn-accept {
      background: #4CAF50;
      color: #fff;
      box-shadow: 0 3px 0 #2e7d32;
    }
    .btn-reject {
      background: #eee;
      color: #555;
      box-shadow: 0 3px 0 #bbb;
    }
  </style>
</head>
<body>
  <button class="sound-toggle" id="sound-toggle" title="Sonido ON/OFF">🔊</button>

  <!-- ===== MENÚ PRINCIPAL ===== -->
  <div id="main-menu">
    <div class="menu-bg-deco">
      <div class="blob"></div>
      <div class="blob"></div>
      <div class="blob"></div>
    </div>
    <div class="menu-card">
      <div class="menu-cat" id="menu-cat"></div>
      <h1>El Gato Pintor</h1>
      <p class="subtitle">Sigue tu sueño de ser un gran artista<br>y abre tu propio museo</p>
      <div class="menu-buttons">
        <button class="menu-btn menu-btn-start" id="btn-start-game">🎨 Empezar</button>
        <button class="menu-btn menu-btn-gallery" id="btn-menu-gallery">🖼️ Ver galería de obras</button>
      </div>
      <p class="menu-footer">Pinta · Exhibe · Colecciona · Sueña</p>
    </div>
  </div>

  <!-- Galería desde el menú -->
  <div id="menu-gallery-overlay">
    <div class="menu-gallery-card">
      <h2>🖼️ Galería de obras</h2>
      <p class="gallery-hint">Todas las obras que has creado</p>
      <div id="menu-gallery-content"></div>
      <div style="text-align:center;margin-top:18px;">
        <button class="btn" id="btn-close-gallery">← Volver al menú</button>
      </div>
    </div>
  </div>

  <div id="app">
    <header>
      <h1>🐱 El Gato Pintor 🎨</h1>
      <p>Sigue tu sueño de ser un gran artista y abre tu propio museo</p>
    </header>

    <div class="stats">
      <span id="stat-money">💰 <span id="money">0</span></span>
      <span>⭐ Mejor nota: <span id="best-rating">-</span></span>
      <span>🖼️ Obras: <span id="works-count">0</span></span>
      <span>🏆 Museo: <span id="museum-progress">0%</span></span>
    </div>

    <div class="nav">
      <button class="active" data-panel="paint">🎨 Pintar</button>
      <button data-panel="street">🌳 Exhibir</button>
      <button data-panel="shop">🛒 Tienda</button>
      <button data-panel="museum">🏛️ Museo</button>
    </div>

    <!-- PINTAR -->
    <div id="panel-paint" class="panel active">
      <div id="paint-area">
        <div class="tools">
          <div id="colors"></div>
          <div class="brush-size" id="brushes"></div>
          <button class="btn" id="btn-clear">🗑️ Borrar</button>
          <button class="btn btn-primary" id="btn-save">💾 Guardar obra</button>
        </div>
        <div id="canvas-wrap">
          <canvas id="paint-canvas" width="400" height="300"></canvas>
        </div>
        <p style="font-size:0.85rem;color:#666;text-align:center;">
          Lienzo actual: <strong id="canvas-size-label">400 × 300</strong> · 
          Pincel: <strong id="brush-label">Mediano</strong>
        </p>
        <div class="inspiration" id="inspiration-box">
          💡 Inspiración del día: <strong id="inspiration-text">Pinta lo que sientas...</strong>
        </div>
      </div>
    </div>

    <!-- CALLE -->
    <div id="panel-street" class="panel">
      <div id="street-scene">
        <div class="park-bg">
          <div class="tree tree-lg" style="left:4%">
            <div class="trunk"></div>
            <div class="canopy c1"></div>
            <div class="canopy c2"></div>
            <div class="canopy c3"></div>
            <div class="canopy c4"></div>
          </div>
          <div class="tree tree-sm dark" style="left:18%">
            <div class="trunk"></div>
            <div class="canopy c1"></div>
            <div class="canopy c2"></div>
            <div class="canopy c3"></div>
            <div class="canopy c4"></div>
          </div>
          <div class="tree" style="left:72%">
            <div class="trunk"></div>
            <div class="canopy c1"></div>
            <div class="canopy c2"></div>
            <div class="canopy c3"></div>
            <div class="canopy c4"></div>
          </div>
          <div class="tree tree-sm dark" style="left:86%">
            <div class="trunk"></div>
            <div class="canopy c1"></div>
            <div class="canopy c2"></div>
            <div class="canopy c3"></div>
            <div class="canopy c4"></div>
          </div>
          <div class="sun"></div>
          <!-- Nubes animadas -->
          <div class="cloud" style="top:25px;left:-80px;width:70px;height:28px;animation-duration:28s;"></div>
          <div class="cloud" style="top:50px;left:-120px;width:50px;height:22px;animation-duration:35s;animation-delay:8s;"></div>
          <div class="cloud" style="top:15px;left:-60px;width:60px;height:24px;animation-duration:42s;animation-delay:15s;"></div>
        </div>
        <div class="path"></div>
        <div class="easel">
          <div class="easel-legs"></div>
          <div class="easel-board" id="easel-board">
            <canvas id="easel-canvas" width="120" height="90"></canvas>
          </div>
        </div>
        <div class="cat-artist" id="cat-artist"></div>
        <div id="citizens-layer"></div>
        <div id="rating-area"></div>
        <div id="particles-layer"></div>
      </div>
      <div class="street-controls">
        <button class="btn btn-primary" id="btn-start-exhibit" disabled>🚶 Empezar exhibición</button>
        <button class="btn" id="btn-stop-exhibit" disabled>⏹️ Detener</button>
        <span id="exhibit-status" style="align-self:center;font-weight:600;color:#555;"></span>
      </div>
      <p style="text-align:center;margin-top:10px;font-size:0.85rem;color:#666;">
        Los gatitos son exigentes: se detienen una sola vez, califican (1⭐ = +5💰) y se van. Cada 5 visitantes hay un 25% de chance de que alguien quiera comprar tu obra.
      </p>
    </div>

    <!-- TIENDA -->
    <div id="panel-shop" class="panel">
      <h2 style="text-align:center;margin-bottom:14px;">🛒 Tienda del Artista</h2>
      <div class="shop-grid" id="shop-grid"></div>
    </div>

    <!-- MUSEO -->
    <div id="panel-museum" class="panel">
      <div class="museum-preview">
        <h2>🏛️ Tu Museo</h2>
        <p style="margin:8px 0;">Meta: reunir <strong>50.000 monedas</strong> y <strong>5 obras</strong> para inaugurar tu museo.</p>
        <div class="progress-bar-wrap">
          <div class="progress-bar" id="museum-bar" style="width:0%"></div>
        </div>
        <p id="museum-status">Aún no has abierto el museo...</p>
        <div id="museum-unlocked" style="display:none;">
          <div style="font-size:3.5rem;margin:10px 0;">🏛️✨</div>
          <p style="font-weight:700;color:#2e7d32;">¡Felicidades! Tu museo está abierto.</p>
          <p style="font-size:0.9rem;margin-top:6px;">Ahora puedes coleccionar estilos de grandes maestros.</p>
        </div>
      </div>
      <h3 style="margin-top:16px;">🖼️ Tu Galería</h3>
      <div class="gallery" id="gallery"></div>
      <div class="inspiration" style="margin-top:16px;">
        <strong>🎨 Estilos desbloqueables:</strong>
        <ul style="margin:8px 0 0 18px;font-size:0.9rem;" id="styles-list">
          <li>Vincent van Gogh — 5.000 💰 (cuando tengas museo)</li>
          <li>Frida Kahlo — 7.500 💰</li>
          <li>Pablo Picasso — 10.000 💰</li>
        </ul>
      </div>
    </div>
  </div>

  <!-- Oferta de compra -->
  <div id="buy-offer-overlay">
    <div class="buy-offer-card">
      <div class="offer-icon">💰🐱</div>
      <h2>¡Enhorabuena!!</h2>
      <p>Alguien desea comprar tu obra por</p>
      <div class="offer-amount" id="offer-amount">0 💰</div>
      <div class="offer-buttons">
        <button class="btn btn-accept" id="btn-accept-offer">✅ Aceptar oferta</button>
        <button class="btn btn-reject" id="btn-reject-offer">❌ Rechazar oferta</button>
      </div>
    </div>
  </div>

  <div class="toast" id="toast"></div>
  <footer>Hecho con ❤️ para soñadores felinos · El Gato Pintor</footer>

  <script>
    // ==================== AUDIO (Web Audio API) ====================
    const AudioCtx = window.AudioContext || window.webkitAudioContext;
    let audioCtx = null;
    let soundEnabled = true;

    function ensureAudio() {
      if (!audioCtx) {
        audioCtx = new AudioCtx();
      }
      if (audioCtx.state === 'suspended') {
        audioCtx.resume();
      }
    }

    function playTone(freq, duration, type = 'sine', volume = 0.15, slideTo = null) {
      if (!soundEnabled) return;
      ensureAudio();
      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.type = type;
      osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
      if (slideTo) {
        osc.frequency.linearRampToValueAtTime(slideTo, audioCtx.currentTime + duration);
      }
      gain.gain.setValueAtTime(volume, audioCtx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + duration);
      osc.connect(gain);
      gain.connect(audioCtx.destination);
      osc.start();
      osc.stop(audioCtx.currentTime + duration);
    }

    function playChord(freqs, duration, volume = 0.1) {
      if (!soundEnabled) return;
      freqs.forEach((f, i) => {
        setTimeout(() => playTone(f, duration, 'sine', volume), i * 40);
      });
    }

    const SFX = {
      click() {
        playTone(600, 0.06, 'square', 0.08);
      },
      paint() {
        // sonido suave de pincel (solo ocasional para no saturar)
        if (Math.random() > 0.7) playTone(200 + Math.random() * 150, 0.04, 'triangle', 0.04);
      },
      save() {
        playChord([523, 659, 784], 0.2, 0.12);
      },
      clear() {
        playTone(300, 0.1, 'sawtooth', 0.06, 150);
      },
      rating(stars) {
        // más estrellas = tono más alegre
        const base = 400 + stars * 80;
        playChord([base, base * 1.25, base * 1.5], 0.25, 0.12);
      },
      coin() {
        playTone(880, 0.08, 'sine', 0.1);
        setTimeout(() => playTone(1100, 0.1, 'sine', 0.1), 70);
      },
      buy() {
        playChord([523, 659, 784, 1046], 0.3, 0.1);
      },
      error() {
        playTone(200, 0.15, 'sawtooth', 0.1, 100);
      },
      museum() {
        // fanfarria
        const notes = [523, 659, 784, 1046, 784, 1046, 1318];
        notes.forEach((n, i) => {
          setTimeout(() => playTone(n, 0.25, 'sine', 0.12), i * 120);
        });
      },
      citizen() {
        playTone(350 + Math.random() * 100, 0.08, 'triangle', 0.06);
      },
      startExhibit() {
        playChord([392, 523, 659], 0.3, 0.1);
        startAmbientMusic();
      },
      stopExhibit() {
        playTone(400, 0.12, 'triangle', 0.08, 250);
        stopAmbientMusic();
      }
    };

    // ===== MÚSICA AMBIENTE SUAVE (durante la exhibición) =====
    let ambientTimer = null;
    let ambientGainNode = null;
    // Escala pentatónica suave (C mayor): do, re, mi, sol, la
    const AMBIENT_NOTES = [261.63, 293.66, 329.63, 392.00, 440.00, 523.25];

    function startAmbientMusic() {
      if (!soundEnabled) return;
      ensureAudio();
      stopAmbientMusic();

      ambientGainNode = audioCtx.createGain();
      ambientGainNode.gain.setValueAtTime(0.04, audioCtx.currentTime); // volumen muy bajo
      ambientGainNode.connect(audioCtx.destination);

      function playAmbientNote() {
        if (!soundEnabled || !ambientGainNode) return;
        const freq = AMBIENT_NOTES[Math.floor(Math.random() * AMBIENT_NOTES.length)];
        const osc = audioCtx.createOscillator();
        const noteGain = audioCtx.createGain();
        osc.type = 'sine';
        osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
        // Ataque y fade suaves
        noteGain.gain.setValueAtTime(0, audioCtx.currentTime);
        noteGain.gain.linearRampToValueAtTime(0.7, audioCtx.currentTime + 0.4);
        noteGain.gain.linearRampToValueAtTime(0, audioCtx.currentTime + 2.2);
        osc.connect(noteGain);
        noteGain.connect(ambientGainNode);
        osc.start();
        osc.stop(audioCtx.currentTime + 2.4);
      }

      // Primera nota y luego cada 1.4–2.2 s
      playAmbientNote();
      ambientTimer = setInterval(() => {
        if (Math.random() < 0.85) playAmbientNote();
      }, 1600);
    }

    function stopAmbientMusic() {
      if (ambientTimer) {
        clearInterval(ambientTimer);
        ambientTimer = null;
      }
      if (ambientGainNode) {
        try {
          ambientGainNode.gain.linearRampToValueAtTime(0, audioCtx.currentTime + 0.5);
        } catch (e) {}
        ambientGainNode = null;
      }
    }

    document.getElementById('sound-toggle').onclick = () => {
      soundEnabled = !soundEnabled;
      const btn = document.getElementById('sound-toggle');
      btn.textContent = soundEnabled ? '🔊' : '🔇';
      btn.classList.toggle('muted', !soundEnabled);
      if (soundEnabled) {
        ensureAudio();
        SFX.click();
        // Si está exhibiendo, reanudar la música
        if (state.exhibiting) startAmbientMusic();
      } else {
        stopAmbientMusic();
      }
    };

    // Activar audio en la primera interacción
    document.body.addEventListener('click', () => ensureAudio(), { once: true });
    document.body.addEventListener('touchstart', () => ensureAudio(), { once: true });

    // ==================== GATO ESTILO DIBUJO (SVG) ====================
    // Estilo de la imagen: contorno grueso negro, cuerpo blanco/color, rayas grises,
    // mejillas rosadas, sonrisa rosa abierta, bigotes, orejas puntiagudas
    function makeCatSVG(bodyColor = '#ffffff', stripeColor = '#b0b0b0', size = 100) {
      return `
<svg viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}">
  <!-- Cuerpo principal (forma de estrella suave / blob) -->
  <path d="M 60 18
           C 52 8, 40 10, 36 22
           C 28 18, 20 28, 24 40
           C 12 48, 14 70, 28 78
           C 24 92, 36 102, 50 98
           C 55 108, 65 108, 70 98
           C 84 102, 96 92, 92 78
           C 106 70, 108 48, 96 40
           C 100 28, 92 18, 84 22
           C 80 10, 68 8, 60 18 Z"
        fill="${bodyColor}" stroke="#1a1a1a" stroke-width="5" stroke-linejoin="round" stroke-linecap="round"/>

  <!-- Oreja izquierda (triángulo redondeado) -->
  <path d="M 38 28 L 32 8 Q 42 6, 48 22" fill="${bodyColor}" stroke="#1a1a1a" stroke-width="5" stroke-linejoin="round"/>
  <!-- Oreja derecha -->
  <path d="M 82 28 L 88 8 Q 78 6, 72 22" fill="${bodyColor}" stroke="#1a1a1a" stroke-width="5" stroke-linejoin="round"/>

  <!-- Rayas de la frente (3 verticales grises) -->
  <rect x="50" y="26" width="5" height="16" rx="2.5" fill="${stripeColor}"/>
  <rect x="57.5" y="24" width="5" height="18" rx="2.5" fill="${stripeColor}"/>
  <rect x="65" y="26" width="5" height="16" rx="2.5" fill="${stripeColor}"/>

  <!-- Ojos -->
  <circle cx="42" cy="48" r="4" fill="#1a1a1a"/>
  <circle cx="78" cy="48" r="4" fill="#1a1a1a"/>

  <!-- Mejillas rosadas -->
  <ellipse cx="30" cy="58" rx="9" ry="6.5" fill="#FF8FAB"/>
  <ellipse cx="90" cy="58" rx="9" ry="6.5" fill="#FF8FAB"/>

  <!-- Bigotes izquierdos -->
  <path d="M 28 52 Q 12 48, 4 46" fill="none" stroke="#1a1a1a" stroke-width="3.2" stroke-linecap="round"/>
  <path d="M 28 58 Q 12 60, 4 62" fill="none" stroke="#1a1a1a" stroke-width="3.2" stroke-linecap="round"/>
  <path d="M 30 64 Q 16 70, 8 74" fill="none" stroke="#1a1a1a" stroke-width="3.2" stroke-linecap="round"/>

  <!-- Bigotes derechos -->
  <path d="M 92 52 Q 108 48, 116 46" fill="none" stroke="#1a1a1a" stroke-width="3.2" stroke-linecap="round"/>
  <path d="M 92 58 Q 108 60, 116 62" fill="none" stroke="#1a1a1a" stroke-width="3.2" stroke-linecap="round"/>
  <path d="M 90 64 Q 104 70, 112 74" fill="none" stroke="#1a1a1a" stroke-width="3.2" stroke-linecap="round"/>

  <!-- Boca / sonrisa grande rosa -->
  <path d="M 44 62 Q 60 92, 76 62" fill="#FF8FAB" stroke="#1a1a1a" stroke-width="4" stroke-linejoin="round"/>
  <!-- Interior más claro -->
  <path d="M 48 66 Q 60 84, 72 66" fill="#FFB3C6"/>
</svg>`;
    }

    // Colores para ciudadanos (diferentes del blanco del artista)
    const CITIZEN_COLORS = [
      { body: '#FFD6A5', stripe: '#E8A87C' },  // durazno
      { body: '#B8E0D2', stripe: '#7BB8A8' },  // menta
      { body: '#FFB4C4', stripe: '#E88A9A' },  // rosa
      { body: '#C9B1FF', stripe: '#9B7ED9' },  // lila
      { body: '#A0E7E5', stripe: '#5BBFBC' },  // celeste
      { body: '#FFE066', stripe: '#E6C200' },  // amarillo
      { body: '#FFADAD', stripe: '#E88A8A' },  // coral
      { body: '#D4A5A5', stripe: '#B07A7A' },  // beige rosado
    ];

    const CITIZEN_COMMENTS = [
      '¡lindo!',
      'le falta color',
      'super cute :3',
      'muy saturado',
      'lindos colores'
    ];

    // ==================== ESTADO DEL JUEGO ====================
    const state = {
      money: 30,
      bestRating: 0,
      works: [],
      currentPainting: null,
      canvasW: 400,
      canvasH: 300,
      brushSize: 8,
      brushType: 'round',
      color: '#1a1a2e',
      owned: {
        colors: ['#1a1a2e','#e74c3c','#3498db','#2ecc71','#f1c40f','#9b59b6','#e67e22','#ffffff'],
        brushes: ['round'],
        sizes: [4, 8, 16],
        canvases: [{w:400,h:300,name:'Pequeño'}],
        eraser: false,
        bucket: false
      },
      museumOpen: false,
      styles: {
        vangogh: false,
        frida: false,
        picasso: false
      },
      exhibiting: false,
      exhibitInterval: null,
      citizensRated: 0,       // contador de gatitos que han calificado
      offerPending: false     // hay una oferta en pantalla
    };

    const SHOP_ITEMS = [
      { id:'tool_eraser', name:'Goma de borrar', desc:'Borra partes del dibujo con precisión', price:60, type:'tool', value:'eraser' },
      { id:'tool_bucket', name:'Tarro de pintura', desc:'Rellena huecos y áreas con un clic', price:150, type:'tool', value:'bucket' },
      { id:'brush_soft', name:'Pincel Suave', desc:'Trazo difuminado y elegante', price:250, type:'brush', value:'soft' },
      { id:'brush_square', name:'Pincel Cuadrado', desc:'Ideal para formas geométricas', price:200, type:'brush', value:'square' },
      { id:'size_big', name:'Pincel Grande', desc:'Trazo grueso (24px)', price:180, type:'size', value:24 },
      { id:'size_xl', name:'Pincel XL', desc:'Trazo enorme (36px)', price:350, type:'size', value:36 },
      { id:'canvas_med', name:'Lienzo Mediano', desc:'500 × 375 px', price:500, type:'canvas', value:{w:500,h:375,name:'Mediano'} },
      { id:'canvas_big', name:'Lienzo Grande', desc:'600 × 450 px', price:1200, type:'canvas', value:{w:600,h:450,name:'Grande'} },
      { id:'color_pack', name:'Pack de colores extra', desc:'Turquesa, Rosa, Marrón, Gris', price:400, type:'colors', value:['#1abc9c','#e91e63','#795548','#607d8b'] },
      { id:'style_vangogh', name:'Estilo Van Gogh', desc:'Inspiración en noche estrellada', price:5000, type:'style', value:'vangogh', reqMuseum:true },
      { id:'style_frida', name:'Estilo Frida Kahlo', desc:'Colores vivos y simbolismo', price:7500, type:'style', value:'frida', reqMuseum:true },
      { id:'style_picasso', name:'Estilo Picasso', desc:'Cubismo y formas audaces', price:10000, type:'style', value:'picasso', reqMuseum:true }
    ];

    const INSPIRATIONS = [
      'Pinta un atardecer en el parque',
      'Un autorretrato felino',
      'Flores como Frida',
      'Una noche estrellada a lo Van Gogh',
      'Formas geométricas estilo Picasso',
      'Tu casa soñada',
      'Un jardín de girasoles',
      'Gatitos jugando',
      'El mar y las nubes',
      'Un sueño surrealista'
    ];

    // ==================== ELEMENTOS DOM ====================
    const canvas = document.getElementById('paint-canvas');
    const ctx = canvas.getContext('2d');
    const easelCanvas = document.getElementById('easel-canvas');
    const easelCtx = easelCanvas.getContext('2d');

    let painting = false;
    let lastX, lastY;
    let lastPaintSound = 0;

    // ==================== UTILIDADES ====================
    function toast(msg) {
      const t = document.getElementById('toast');
      t.textContent = msg;
      t.classList.add('show');
      setTimeout(() => t.classList.remove('show'), 2500);
    }

    function popStat(id) {
      const el = document.getElementById(id);
      if (!el) return;
      el.classList.remove('pop');
      void el.offsetWidth;
      el.classList.add('pop');
    }

    function updateStats() {
      document.getElementById('money').textContent = state.money.toLocaleString('es-ES');
      document.getElementById('best-rating').textContent = state.bestRating || '-';
      document.getElementById('works-count').textContent = state.works.length;
      const progress = Math.min(100, Math.floor((state.money / 50000) * 50 + (state.works.length / 5) * 50));
      document.getElementById('museum-progress').textContent = progress + '%';
      document.getElementById('museum-bar').style.width = progress + '%';

      if (!state.museumOpen && state.money >= 50000 && state.works.length >= 5) {
        state.museumOpen = true;
        document.getElementById('museum-status').textContent = '';
        document.getElementById('museum-unlocked').style.display = 'block';
        SFX.museum();
        launchConfetti();
        toast('🎉 ¡Has inaugurado tu propio museo!');
      }
    }

    function setInspiration() {
      const txt = INSPIRATIONS[Math.floor(Math.random() * INSPIRATIONS.length)];
      document.getElementById('inspiration-text').textContent = txt;
    }

    function launchConfetti() {
      const colors = ['#ff6b6b','#ffd700','#4CAF50','#3498db','#9b59b6','#e67e22'];
      for (let i = 0; i < 60; i++) {
        const el = document.createElement('div');
        el.className = 'confetti';
        el.style.left = Math.random() * 100 + 'vw';
        el.style.background = colors[Math.floor(Math.random() * colors.length)];
        el.style.borderRadius = Math.random() > 0.5 ? '50%' : '2px';
        el.style.animationDuration = (2 + Math.random() * 2) + 's';
        el.style.animationDelay = (Math.random() * 0.8) + 's';
        document.body.appendChild(el);
        setTimeout(() => el.remove(), 4000);
      }
    }

    function spawnCoinParticles(x, y, count = 4) {
      const layer = document.getElementById('particles-layer');
      for (let i = 0; i < count; i++) {
        const el = document.createElement('div');
        el.className = 'coin-particle';
        el.textContent = '💰';
        el.style.left = (x + (Math.random() - 0.5) * 40) + 'px';
        el.style.bottom = (y + Math.random() * 20) + 'px';
        el.style.animationDelay = (i * 0.08) + 's';
        layer.appendChild(el);
        setTimeout(() => el.remove(), 1100);
      }
    }

    // ==================== PINTURA ====================
    function initCanvas() {
      canvas.width = state.canvasW;
      canvas.height = state.canvasH;
      ctx.fillStyle = '#ffffff';
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      document.getElementById('canvas-size-label').textContent = `${state.canvasW} × ${state.canvasH}`;
    }

    function renderColors() {
      const box = document.getElementById('colors');
      box.innerHTML = '';
      state.owned.colors.forEach(c => {
        const d = document.createElement('div');
        d.className = 'color-swatch' + (c === state.color ? ' selected' : '');
        d.style.background = c;
        d.title = c;
        d.onclick = () => {
          state.color = c;
          // Si estaba con la goma, vuelve al pincel
          if (state.brushType === 'eraser') {
            state.brushType = state.owned.brushes[0] || 'round';
            const sizeName = state.brushSize <= 6 ? 'Fino' : state.brushSize <= 12 ? 'Mediano' : state.brushSize <= 20 ? 'Grueso' : 'XL';
            document.getElementById('brush-label').textContent = sizeName;
            renderBrushes();
          }
          SFX.click();
          renderColors();
        };
        box.appendChild(d);
      });
    }

    function renderBrushes() {
      const box = document.getElementById('brushes');
      box.innerHTML = '';
      const sizes = state.owned.sizes;
      sizes.forEach(s => {
        const b = document.createElement('button');
        b.className = 'brush-btn' + (s === state.brushSize && state.brushType !== 'eraser' ? ' selected' : '');
        if (state.brushType === 'eraser' && s === state.brushSize) b.classList.add('selected');
        b.textContent = s;
        b.title = 'Tamaño ' + s;
        b.onclick = () => {
          state.brushSize = s;
          const sizeName = s <= 6 ? 'Fino' : s <= 12 ? 'Mediano' : s <= 20 ? 'Grueso' : 'XL';
          document.getElementById('brush-label').textContent =
            state.brushType === 'eraser' ? 'Goma ' + sizeName : sizeName;
          SFX.click();
          renderBrushes();
        };
        box.appendChild(b);
      });
      // Tipos de pincel
      state.owned.brushes.forEach(t => {
        const b = document.createElement('button');
        b.className = 'brush-btn' + (t === state.brushType ? ' selected' : '');
        b.textContent = t === 'round' ? '●' : t === 'square' ? '■' : '◉';
        b.title = t === 'round' ? 'Redondo' : t === 'square' ? 'Cuadrado' : 'Suave';
        b.onclick = () => {
          state.brushType = t;
          const sizeName = state.brushSize <= 6 ? 'Fino' : state.brushSize <= 12 ? 'Mediano' : state.brushSize <= 20 ? 'Grueso' : 'XL';
          document.getElementById('brush-label').textContent = sizeName;
          canvas.style.cursor = 'crosshair';
          SFX.click();
          renderBrushes();
        };
        box.appendChild(b);
      });
      // Goma (solo si la compraste)
      if (state.owned.eraser) {
        const eraserBtn = document.createElement('button');
        eraserBtn.className = 'brush-btn' + (state.brushType === 'eraser' ? ' selected' : '');
        eraserBtn.textContent = '🧹';
        eraserBtn.title = 'Goma de borrar';
        eraserBtn.style.fontSize = '0.95rem';
        eraserBtn.onclick = () => {
          state.brushType = 'eraser';
          const sizeName = state.brushSize <= 6 ? 'Fino' : state.brushSize <= 12 ? 'Mediano' : state.brushSize <= 20 ? 'Grueso' : 'XL';
          document.getElementById('brush-label').textContent = 'Goma ' + sizeName;
          canvas.style.cursor = 'cell';
          SFX.click();
          renderBrushes();
        };
        box.appendChild(eraserBtn);
      }
      // Tarro de pintura (solo si lo compraste)
      if (state.owned.bucket) {
        const bucketBtn = document.createElement('button');
        bucketBtn.className = 'brush-btn' + (state.brushType === 'bucket' ? ' selected' : '');
        bucketBtn.textContent = '🪣';
        bucketBtn.title = 'Tarro de pintura (rellenar)';
        bucketBtn.style.fontSize = '0.95rem';
        bucketBtn.onclick = () => {
          state.brushType = 'bucket';
          document.getElementById('brush-label').textContent = 'Tarro de pintura';
          canvas.style.cursor = 'cell';
          SFX.click();
          renderBrushes();
        };
        box.appendChild(bucketBtn);
      }
    }

    // Relleno (flood fill) para el tarro de pintura
    function hexToRgba(hex) {
      const h = hex.replace('#', '');
      const full = h.length === 3 ? h.split('').map(c => c + c).join('') : h;
      const n = parseInt(full, 16);
      return [(n >> 16) & 255, (n >> 8) & 255, n & 255, 255];
    }

    function floodFill(startX, startY, fillColor) {
      const w = canvas.width;
      const h = canvas.height;
      const imageData = ctx.getImageData(0, 0, w, h);
      const data = imageData.data;
      const sx = Math.floor(startX);
      const sy = Math.floor(startY);
      if (sx < 0 || sy < 0 || sx >= w || sy >= h) return;

      const i0 = (sy * w + sx) * 4;
      const target = [data[i0], data[i0 + 1], data[i0 + 2], data[i0 + 3]];
      const fill = hexToRgba(fillColor);

      // Si el color es casi igual, no hacer nada
      if (Math.abs(target[0] - fill[0]) < 2 &&
          Math.abs(target[1] - fill[1]) < 2 &&
          Math.abs(target[2] - fill[2]) < 2 &&
          Math.abs(target[3] - fill[3]) < 2) return;

      const match = (i) =>
        Math.abs(data[i] - target[0]) < 8 &&
        Math.abs(data[i + 1] - target[1]) < 8 &&
        Math.abs(data[i + 2] - target[2]) < 8 &&
        Math.abs(data[i + 3] - target[3]) < 8;

      const stack = [[sx, sy]];
      const visited = new Uint8Array(w * h);

      while (stack.length) {
        const [x, y] = stack.pop();
        if (x < 0 || y < 0 || x >= w || y >= h) continue;
        const idx = y * w + x;
        if (visited[idx]) continue;
        const i = idx * 4;
        if (!match(i)) continue;
        visited[idx] = 1;
        data[i] = fill[0];
        data[i + 1] = fill[1];
        data[i + 2] = fill[2];
        data[i + 3] = fill[3];
        stack.push([x + 1, y], [x - 1, y], [x, y + 1], [x, y - 1]);
      }
      ctx.putImageData(imageData, 0, 0);
    }

    function startPaint(e) {
      const rect = canvas.getBoundingClientRect();
      const scaleX = canvas.width / rect.width;
      const scaleY = canvas.height / rect.height;
      const x = ((e.clientX || e.touches[0].clientX) - rect.left) * scaleX;
      const y = ((e.clientY || e.touches[0].clientY) - rect.top) * scaleY;

      // Tarro de pintura: un clic rellena el área
      if (state.brushType === 'bucket') {
        floodFill(x, y, state.color);
        SFX.paint();
        return;
      }

      painting = true;
      document.getElementById('canvas-wrap').classList.add('painting-active');
      lastX = x;
      lastY = y;
    }

    function draw(e) {
      if (!painting) return;
      e.preventDefault();
      const rect = canvas.getBoundingClientRect();
      const scaleX = canvas.width / rect.width;
      const scaleY = canvas.height / rect.height;
      const x = ((e.clientX || e.touches[0].clientX) - rect.left) * scaleX;
      const y = ((e.clientY || e.touches[0].clientY) - rect.top) * scaleY;

      if (state.brushType === 'eraser') {
        // Goma: pinta de blanco con el tamaño elegido
        ctx.globalCompositeOperation = 'source-over';
        ctx.strokeStyle = '#ffffff';
        ctx.fillStyle = '#ffffff';
        ctx.lineWidth = state.brushSize;
        ctx.lineCap = 'round';
        ctx.lineJoin = 'round';
        ctx.globalAlpha = 1;
        ctx.beginPath();
        ctx.moveTo(lastX, lastY);
        ctx.lineTo(x, y);
        ctx.stroke();
      } else {
        ctx.globalCompositeOperation = 'source-over';
        ctx.strokeStyle = state.color;
        ctx.fillStyle = state.color;
        ctx.lineWidth = state.brushSize;
        ctx.lineCap = state.brushType === 'square' ? 'butt' : 'round';
        ctx.lineJoin = 'round';

        if (state.brushType === 'soft') {
          ctx.globalAlpha = 0.35;
        } else {
          ctx.globalAlpha = 1;
        }

        if (state.brushType === 'square') {
          ctx.fillRect(x - state.brushSize/2, y - state.brushSize/2, state.brushSize, state.brushSize);
        } else {
          ctx.beginPath();
          ctx.moveTo(lastX, lastY);
          ctx.lineTo(x, y);
          ctx.stroke();
        }
      }

      // sonido ocasional de pincel
      const now = Date.now();
      if (now - lastPaintSound > 80) {
        SFX.paint();
        lastPaintSound = now;
      }

      lastX = x;
      lastY = y;
      ctx.globalAlpha = 1;
    }

    function stopPaint() {
      painting = false;
      document.getElementById('canvas-wrap').classList.remove('painting-active');
    }

    canvas.addEventListener('mousedown', startPaint);
    canvas.addEventListener('mousemove', draw);
    canvas.addEventListener('mouseup', stopPaint);
    canvas.addEventListener('mouseleave', stopPaint);
    canvas.addEventListener('touchstart', startPaint, {passive:false});
    canvas.addEventListener('touchmove', draw, {passive:false});
    canvas.addEventListener('touchend', stopPaint);

    document.getElementById('btn-clear').onclick = () => {
      ctx.fillStyle = '#fff';
      ctx.fillRect(0,0,canvas.width,canvas.height);
      SFX.clear();
      toast('Lienzo limpio');
    };

    document.getElementById('btn-save').onclick = () => {
      const dataUrl = canvas.toDataURL('image/png');
      const id = Date.now();
      const name = 'Obra #' + (state.works.length + 1);
      state.works.push({ id, dataUrl, rating: 0, name, w: state.canvasW, h: state.canvasH });
      state.currentPainting = state.works[state.works.length - 1];
      updateStats();
      renderGallery();
      SFX.save();
      popStat('stat-money');
      toast('✅ ¡Obra guardada! Ahora puedes exhibirla en la calle.');
      showOnEasel(dataUrl);
      document.getElementById('btn-start-exhibit').disabled = false;
      // animar caballete
      const board = document.getElementById('easel-board');
      board.classList.remove('highlight');
      void board.offsetWidth;
      board.classList.add('highlight');
    };

    function showOnEasel(dataUrl) {
      const img = new Image();
      img.onload = () => {
        easelCtx.clearRect(0,0,120,90);
        easelCtx.fillStyle = '#fff';
        easelCtx.fillRect(0,0,120,90);
        const scale = Math.min(120 / img.width, 90 / img.height);
        const w = img.width * scale;
        const h = img.height * scale;
        easelCtx.drawImage(img, (120-w)/2, (90-h)/2, w, h);
      };
      img.src = dataUrl;
    }

    // ==================== CALLE / EXHIBICIÓN ====================
    function ratePainting() {
      // Gatitos más exigentes: la mayoría da 1–3 estrellas
      // 4–5 solo con suerte + lienzo grande + estilos desbloqueados
      let score = 0.8 + Math.random() * 2.2; // ~0.8 a 3.0

      // Bonus pequeño por lienzo más grande
      const area = state.currentPainting.w * state.currentPainting.h;
      const sizeBonus = Math.min(0.8, (area / (400 * 300) - 1) * 0.5);
      score += Math.max(0, sizeBonus);

      // Estilos de maestros ayudan un poco
      let styleBonus = 0;
      if (state.styles.vangogh) styleBonus += 0.35;
      if (state.styles.frida) styleBonus += 0.35;
      if (state.styles.picasso) styleBonus += 0.35;
      score += styleBonus;

      // Probabilidad baja de un "crítico generoso"
      if (Math.random() < 0.08) score += 1.2;

      // Clamp y redondeo a enteros 1–5 (más exigente, sin medios puntos)
      let rating = Math.round(score);
      rating = Math.max(1, Math.min(5, rating));
      return rating;
    }

    function spawnCitizen() {
      if (!state.exhibiting || !state.currentPainting) return;
      const layer = document.getElementById('citizens-layer');
      const el = document.createElement('div');
      el.className = 'citizen';
      const colorSet = CITIZEN_COLORS[Math.floor(Math.random() * CITIZEN_COLORS.length)];
      el.innerHTML = makeCatSVG(colorSet.body, colorSet.stripe, 56);
      el.style.left = '-50px';
      layer.appendChild(el);
      SFX.citizen();

      let x = -50;
      const speed = 1.2 + Math.random() * 1.5;
      const stopAt = 180 + Math.random() * 300;
      // Estados: 'walking' → 'looking' → 'leaving'
      let phase = 'walking';
      let lookTimer = 0;
      let hasRated = false; // garantiza una sola calificación

      const move = setInterval(() => {
        if (!state.exhibiting) {
          clearInterval(move);
          el.remove();
          return;
        }

        if (phase === 'walking') {
          x += speed;
          el.style.left = x + 'px';
          // Solo se detiene si aún no ha calificado
          if (!hasRated && x >= stopAt) {
            phase = 'looking';
            el.classList.add('looking');
            lookTimer = 0;

            // 35% de probabilidad de comentario encima del gatito
            if (Math.random() < 0.35) {
              const comment = document.createElement('div');
              comment.className = 'citizen-comment';
              comment.textContent = CITIZEN_COMMENTS[Math.floor(Math.random() * CITIZEN_COMMENTS.length)];
              el.style.position = 'absolute'; // ya lo es, pero asegura contexto
              el.appendChild(comment);
              setTimeout(() => comment.remove(), 2200);
            }

            // Calificar UNA sola vez
            hasRated = true;
            const rating = ratePainting();
            const money = rating * 5;
            state.money += money;
            if (rating > state.bestRating) state.bestRating = rating;
            state.currentPainting.rating = Math.max(state.currentPainting.rating, rating);
            updateStats();
            popStat('stat-money');

            SFX.rating(rating);
            setTimeout(() => SFX.coin(), 200);

            const cat = document.getElementById('cat-artist');
            cat.classList.remove('happy');
            void cat.offsetWidth;
            cat.classList.add('happy');

            const bubble = document.createElement('div');
            bubble.className = 'rating-bubble';
            bubble.innerHTML = `${'⭐'.repeat(Math.round(rating))} ${rating} · +${money}💰`;
            document.getElementById('rating-area').appendChild(bubble);
            setTimeout(() => bubble.remove(), 1800);

            spawnCoinParticles(x, 80, 3 + Math.floor(rating));

            // Contador de ciudadanos: cada 5, 25% de chance de comprador
            state.citizensRated++;
            if (state.citizensRated % 5 === 0 && !state.offerPending) {
              if (Math.random() < 0.25) {
                showBuyOffer();
              }
            }
          }
        } else if (phase === 'looking') {
          // Se queda quieto mirando la obra
          lookTimer++;
          if (lookTimer > 50) {
            phase = 'leaving';
            el.classList.remove('looking');
          }
        } else if (phase === 'leaving') {
          // Se va y no vuelve a parar
          x += speed * 1.4;
          el.style.left = x + 'px';
        }

        if (x > 720) {
          clearInterval(move);
          el.remove();
        }
      }, 30);
    }

    document.getElementById('btn-start-exhibit').onclick = () => {
      if (!state.currentPainting) {
        toast('Primero guarda una obra en el estudio');
        SFX.error();
        return;
      }
      state.exhibiting = true;
      state.citizensRated = 0;
      document.getElementById('btn-start-exhibit').disabled = true;
      document.getElementById('btn-stop-exhibit').disabled = false;
      document.getElementById('exhibit-status').textContent = 'Exhibiendo... los gatitos se acercan';
      SFX.startExhibit();
      state.exhibitInterval = setInterval(spawnCitizen, 2200 + Math.random()*1500);
      spawnCitizen();
    };

    document.getElementById('btn-stop-exhibit').onclick = () => {
      state.exhibiting = false;
      clearInterval(state.exhibitInterval);
      document.getElementById('btn-start-exhibit').disabled = false;
      document.getElementById('btn-stop-exhibit').disabled = true;
      document.getElementById('exhibit-status').textContent = 'Exhibición detenida';
      document.getElementById('citizens-layer').innerHTML = '';
      document.getElementById('rating-area').innerHTML = '';
      document.getElementById('particles-layer').innerHTML = '';
      // Cerrar oferta si estaba abierta
      hideBuyOffer();
      SFX.stopExhibit();
    };

    // ===== OFERTA DE COMPRA =====
    let currentOfferAmount = 0;

    function showBuyOffer() {
      state.offerPending = true;
      // Oferta totalmente aleatoria entre 10 y 50
      currentOfferAmount = 10 + Math.floor(Math.random() * 41); // 10–50
      document.getElementById('offer-amount').textContent = currentOfferAmount + ' 💰';
      document.getElementById('buy-offer-overlay').classList.add('show');
      SFX.buy();
      // Pausar aparición de nuevos ciudadanos mientras hay oferta
      clearInterval(state.exhibitInterval);
    }

    function hideBuyOffer(resumeExhibit = true) {
      state.offerPending = false;
      document.getElementById('buy-offer-overlay').classList.remove('show');
      // Reanudar exhibición solo si sigue activa y aún hay obra
      if (resumeExhibit && state.exhibiting && state.currentPainting) {
        state.exhibitInterval = setInterval(spawnCitizen, 2200 + Math.random() * 1500);
      }
    }

    function sellCurrentPainting() {
      if (!state.currentPainting) return;

      const soldId = state.currentPainting.id;
      // Quitar la obra de la colección (desaparece de la galería)
      state.works = state.works.filter(w => w.id !== soldId);
      state.currentPainting = null;

      // Vaciar el caballete (el comprador se la lleva)
      easelCtx.clearRect(0, 0, 120, 90);
      easelCtx.fillStyle = '#f5f5dc';
      easelCtx.fillRect(0, 0, 120, 90);

      // Reiniciar el lienzo del estudio
      ctx.fillStyle = '#ffffff';
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      // Animación rápida de “se la llevan”
      const board = document.getElementById('easel-board');
      board.classList.remove('highlight');
      void board.offsetWidth;
      board.classList.add('highlight');

      // Detener la exhibición: ya no hay obra que mostrar
      state.exhibiting = false;
      clearInterval(state.exhibitInterval);
      stopAmbientMusic();
      document.getElementById('btn-start-exhibit').disabled = true;
      document.getElementById('btn-stop-exhibit').disabled = true;
      document.getElementById('exhibit-status').textContent = 'Obra vendida — el comprador se la llevó';
      document.getElementById('citizens-layer').innerHTML = '';
      document.getElementById('rating-area').innerHTML = '';
      document.getElementById('particles-layer').innerHTML = '';

      updateStats();
      renderGallery();
    }

    document.getElementById('btn-accept-offer').onclick = () => {
      state.money += currentOfferAmount;
      updateStats();
      popStat('stat-money');
      SFX.coin();
      sellCurrentPainting();
      toast('🎉 ¡Obra vendida por ' + currentOfferAmount + ' monedas! El comprador se la llevó.');
      hideBuyOffer(false); // no reanudar exhibición
    };

    document.getElementById('btn-reject-offer').onclick = () => {
      SFX.click();
      toast('Oferta rechazada. La obra sigue en exhibición.');
      hideBuyOffer(true);
    };

    // ==================== TIENDA ====================
    function renderShop() {
      const grid = document.getElementById('shop-grid');
      grid.innerHTML = '';
      SHOP_ITEMS.forEach(item => {
        const owned = isOwned(item);
        const locked = item.reqMuseum && !state.museumOpen;
        const div = document.createElement('div');
        div.className = 'shop-item';
        div.innerHTML = `
          <h3>${item.name}</h3>
          <p>${item.desc}</p>
          <div class="price">${owned ? '✅ Poseído' : locked ? '🔒 Necesitas museo' : item.price.toLocaleString('es-ES') + ' 💰'}</div>
          <button class="btn btn-primary" ${owned || locked ? 'disabled' : ''} data-id="${item.id}">
            ${owned ? 'Comprado' : locked ? 'Bloqueado' : 'Comprar'}
          </button>
        `;
        grid.appendChild(div);
      });

      grid.querySelectorAll('button[data-id]').forEach(btn => {
        btn.onclick = () => buyItem(btn.dataset.id);
      });
    }

    function isOwned(item) {
      if (item.type === 'brush') return state.owned.brushes.includes(item.value);
      if (item.type === 'size') return state.owned.sizes.includes(item.value);
      if (item.type === 'canvas') return state.owned.canvases.some(c => c.w === item.value.w);
      if (item.type === 'colors') return item.value.every(c => state.owned.colors.includes(c));
      if (item.type === 'style') return state.styles[item.value];
      if (item.type === 'tool') return !!state.owned[item.value];
      return false;
    }

    function buyItem(id) {
      const item = SHOP_ITEMS.find(i => i.id === id);
      if (!item || isOwned(item)) return;
      if (item.reqMuseum && !state.museumOpen) {
        toast('Necesitas abrir el museo primero');
        SFX.error();
        return;
      }
      if (state.money < item.price) {
        toast('No tienes suficiente dinero 😿');
        SFX.error();
        return;
      }
      state.money -= item.price;

      if (item.type === 'brush') state.owned.brushes.push(item.value);
      if (item.type === 'size') state.owned.sizes.push(item.value);
      if (item.type === 'canvas') {
        state.owned.canvases.push(item.value);
        state.canvasW = item.value.w;
        state.canvasH = item.value.h;
        initCanvas();
      }
      if (item.type === 'colors') {
        item.value.forEach(c => {
          if (!state.owned.colors.includes(c)) state.owned.colors.push(c);
        });
        renderColors();
      }
      if (item.type === 'style') {
        state.styles[item.value] = true;
        toast(`🎨 ¡Estilo ${item.name} desbloqueado! Tus obras ganan más estrellas.`);
        updateStylesList();
      }
      if (item.type === 'tool') {
        state.owned[item.value] = true;
      }

      updateStats();
      renderShop();
      renderBrushes();
      SFX.buy();
      popStat('stat-money');
      toast(`Compraste: ${item.name}`);

      // flash en el item
      setTimeout(() => {
        const items = document.querySelectorAll('.shop-item');
        items.forEach(el => {
          if (el.querySelector(`[data-id="${id}"]`)) {
            el.classList.add('just-bought');
          }
        });
      }, 50);
    }

    function updateStylesList() {
      const ul = document.getElementById('styles-list');
      ul.innerHTML = `
        <li>Vincent van Gogh — ${state.styles.vangogh ? '✅ Desbloqueado' : '5.000 💰'}</li>
        <li>Frida Kahlo — ${state.styles.frida ? '✅ Desbloqueado' : '7.500 💰'}</li>
        <li>Pablo Picasso — ${state.styles.picasso ? '✅ Desbloqueado' : '10.000 💰'}</li>
      `;
    }

    // ==================== GALERÍA ====================
    function renderGallery() {
      const g = document.getElementById('gallery');
      g.innerHTML = '';
      state.works.forEach((w, idx) => {
        const div = document.createElement('div');
        div.className = 'gallery-item';
        div.style.animationDelay = (idx * 0.05) + 's';
        const c = document.createElement('canvas');
        c.width = 140; c.height = 100;
        const cx = c.getContext('2d');
        const img = new Image();
        img.onload = () => {
          const s = Math.min(140/img.width, 100/img.height);
          const ww = img.width*s, hh = img.height*s;
          cx.drawImage(img, (140-ww)/2, (100-hh)/2, ww, hh);
        };
        img.src = w.dataUrl;
        div.appendChild(c);
        const label = document.createElement('div');
        label.className = 'label';
        label.textContent = `${w.name} ${w.rating ? '⭐'+w.rating : ''}`;
        div.appendChild(label);
        div.onclick = () => {
          state.currentPainting = w;
          showOnEasel(w.dataUrl);
          SFX.click();
          toast('Obra seleccionada para exhibir');
          document.getElementById('btn-start-exhibit').disabled = false;
          const board = document.getElementById('easel-board');
          board.classList.remove('highlight');
          void board.offsetWidth;
          board.classList.add('highlight');
        };
        g.appendChild(div);
      });
    }

    // ==================== NAVEGACIÓN ====================
    document.querySelectorAll('.nav button').forEach(btn => {
      btn.onclick = () => {
        document.querySelectorAll('.nav button').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
        document.getElementById('panel-' + btn.dataset.panel).classList.add('active');
        SFX.click();
        if (btn.dataset.panel === 'shop') renderShop();
        if (btn.dataset.panel === 'museum') renderGallery();
      };
    });

    // ==================== MENÚ PRINCIPAL ====================
    function renderMenuGallery() {
      const box = document.getElementById('menu-gallery-content');
      if (state.works.length === 0) {
        box.innerHTML = `
          <div class="menu-gallery-empty">
            <div class="empty-cat" id="empty-gallery-cat"></div>
            <p>Aún no has creado ninguna obra.</p>
            <p style="font-size:0.85rem;margin-top:6px;">¡Pulsa <strong>Empezar</strong> y pinta tu primera obra!</p>
          </div>`;
        document.getElementById('empty-gallery-cat').innerHTML = makeCatSVG('#ffffff', '#b0b0b0', 80);
        return;
      }
      let html = '<div class="gallery">';
      state.works.forEach((w, idx) => {
        html += `<div class="gallery-item" style="animation-delay:${idx * 0.05}s" data-work="${idx}">
          <canvas width="140" height="100"></canvas>
          <div class="label">${w.name} ${w.rating ? '⭐' + w.rating : ''}</div>
        </div>`;
      });
      html += '</div>';
      box.innerHTML = html;
      // dibujar miniaturas
      box.querySelectorAll('.gallery-item').forEach((div, idx) => {
        const c = div.querySelector('canvas');
        const cx = c.getContext('2d');
        const img = new Image();
        img.onload = () => {
          const s = Math.min(140 / img.width, 100 / img.height);
          const ww = img.width * s, hh = img.height * s;
          cx.drawImage(img, (140 - ww) / 2, (100 - hh) / 2, ww, hh);
        };
        img.src = state.works[idx].dataUrl;
      });
    }

    document.getElementById('btn-start-game').onclick = () => {
      SFX.click();
      ensureAudio();
      const menu = document.getElementById('main-menu');
      menu.classList.add('hidden');
      setTimeout(() => {
        menu.style.display = 'none';
        document.getElementById('app').classList.add('visible');
        toast('¡Bienvenido, pequeño artista ! Pinta tu primera obra');
      }, 450);
    };

    document.getElementById('btn-menu-gallery').onclick = () => {
      SFX.click();
      ensureAudio();
      renderMenuGallery();
      document.getElementById('menu-gallery-overlay').classList.add('show');
    };

    document.getElementById('btn-close-gallery').onclick = () => {
      SFX.click();
      document.getElementById('menu-gallery-overlay').classList.remove('show');
    };

    // Cerrar galería con clic fuera
    document.getElementById('menu-gallery-overlay').addEventListener('click', (e) => {
      if (e.target.id === 'menu-gallery-overlay') {
        document.getElementById('menu-gallery-overlay').classList.remove('show');
      }
    });

    // ==================== INICIO ====================
    initCanvas();
    renderColors();
    renderBrushes();
    setInspiration();
    updateStats();
    renderShop();
    updateStylesList();

    // Gato artista (estilo de la imagen: blanco con rayas grises)
    document.getElementById('cat-artist').innerHTML = makeCatSVG('#ffffff', '#b0b0b0', 72);

    // Gato del menú
    document.getElementById('menu-cat').innerHTML = makeCatSVG('#ffffff', '#b0b0b0', 110);
  </script>
</body>
</html>
