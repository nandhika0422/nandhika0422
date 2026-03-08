<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>NANDHIKA — The AI Issue</title>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,300;1,400;1,500&family=IM+Fell+English:ital@0;1&family=Cinzel:wght@400;500;600&display=swap" rel="stylesheet"/>
<style>
  :root {
    --cream:    #fdf6ee;
    --blush:    #f2c4ce;
    --rose:     #d4849a;
    --dusty:    #c9a0b4;
    --sage:     #b5c4b1;
    --gold:     #c9a96e;
    --deep:     #3d2b35;
    --ink:      #2a1a22;
    --petal1:   #f9d5dc;
    --petal2:   #f2b8c4;
    --petal3:   #e8909c;
    --petal4:   #fce8d0;
    --leaf:     #8aab82;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--cream);
    color: var(--ink);
    font-family: 'Cormorant Garamond', serif;
    overflow-x: hidden;
    cursor: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='20' height='20'%3E%3Ccircle cx='10' cy='10' r='4' fill='%23d4849a' opacity='0.8'/%3E%3C/svg%3E") 10 10, auto;
  }

  /* ── FALLING PETALS BACKGROUND ── */
  #petals-canvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    pointer-events: none;
    z-index: 0;
  }

  /* ── GRAIN TEXTURE ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.03'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 1;
    opacity: 0.4;
  }

  .page { position: relative; z-index: 2; }

  /* ── MASTHEAD / COVER ── */
  .cover {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 60px 40px;
    text-align: center;
    position: relative;
    background: linear-gradient(180deg, #fdf0e8 0%, #fdf6ee 40%, #f9eef4 100%);
    border-bottom: 1px solid var(--blush);
  }

  .cover-ornament {
    font-size: 13px;
    letter-spacing: 0.35em;
    color: var(--gold);
    margin-bottom: 18px;
    opacity: 0;
    animation: fadeUp 1.2s ease forwards 0.3s;
  }

  .cover-rule {
    width: 120px;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--gold), transparent);
    margin: 0 auto 28px;
    opacity: 0;
    animation: fadeUp 1s ease forwards 0.5s;
  }

  .cover-name {
    font-family: 'Cinzel', serif;
    font-size: clamp(52px, 12vw, 110px);
    font-weight: 500;
    letter-spacing: 0.18em;
    color: var(--ink);
    line-height: 1;
    opacity: 0;
    animation: fadeUp 1.4s ease forwards 0.6s;
  }

  .cover-tagline {
    font-family: 'IM Fell English', serif;
    font-style: italic;
    font-size: clamp(14px, 2.5vw, 20px);
    color: var(--rose);
    letter-spacing: 0.12em;
    margin-top: 18px;
    opacity: 0;
    animation: fadeUp 1.2s ease forwards 1s;
  }

  .cover-vol {
    font-size: 11px;
    letter-spacing: 0.4em;
    color: var(--dusty);
    margin-top: 30px;
    opacity: 0;
    animation: fadeUp 1s ease forwards 1.3s;
  }

  .cover-badges {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    justify-content: center;
    margin-top: 36px;
    opacity: 0;
    animation: fadeUp 1s ease forwards 1.6s;
  }

  .badge {
    padding: 6px 18px;
    border: 1px solid var(--blush);
    font-size: 10px;
    letter-spacing: 0.3em;
    color: var(--deep);
    background: rgba(253,246,238,0.7);
    backdrop-filter: blur(4px);
  }

  /* ── BLOOMING FLOWERS SVG ── */
  .flowers-wrap {
    position: absolute;
    width: 100%; height: 100%;
    top: 0; left: 0;
    pointer-events: none;
    overflow: hidden;
  }

  .flower {
    position: absolute;
    transform-origin: center bottom;
  }

  .flower-tl { bottom: 0; left: -20px; width: 320px; animation: bloomIn 2.5s ease forwards 0.2s; opacity: 0; }
  .flower-tr { bottom: 0; right: -20px; width: 300px; animation: bloomIn 2.5s ease forwards 0.5s; opacity: 0; transform: scaleX(-1); }
  .flower-bl { top: 0; left: 30px; width: 200px; animation: bloomIn 2s ease forwards 1s; opacity: 0; transform: rotate(180deg); }
  .flower-br { top: 10px; right: 30px; width: 180px; animation: bloomIn 2s ease forwards 1.2s; opacity: 0; transform: rotate(180deg) scaleX(-1); }

  @keyframes bloomIn {
    0%   { opacity: 0; transform: scale(0.3) translateY(40px); }
    60%  { opacity: 0.9; }
    100% { opacity: 1; transform: scale(1) translateY(0); }
  }

  @keyframes sway {
    0%, 100% { transform: rotate(-2deg); }
    50%       { transform: rotate(2deg); }
  }

  .petal-sway { animation: sway 6s ease-in-out infinite; transform-origin: center bottom; }

  /* ── SECTIONS ── */
  section {
    max-width: 860px;
    margin: 0 auto;
    padding: 80px 40px;
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.9s ease, transform 0.9s ease;
  }

  section.visible { opacity: 1; transform: translateY(0); }

  .section-label {
    font-size: 10px;
    letter-spacing: 0.45em;
    color: var(--gold);
    text-align: center;
    margin-bottom: 6px;
  }

  .section-title {
    font-family: 'Cinzel', serif;
    font-size: clamp(22px, 4vw, 36px);
    font-weight: 400;
    text-align: center;
    color: var(--deep);
    letter-spacing: 0.15em;
    margin-bottom: 10px;
  }

  .section-rule {
    width: 80px;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--rose), transparent);
    margin: 0 auto 48px;
  }

  /* ── EDITOR'S LETTER ── */
  .letter-box {
    border: 1px solid var(--blush);
    padding: 48px 52px;
    position: relative;
    background: rgba(255,255,255,0.5);
    backdrop-filter: blur(6px);
  }

  .letter-box::before, .letter-box::after {
    content: '✦';
    position: absolute;
    color: var(--gold);
    font-size: 18px;
  }
  .letter-box::before { top: 16px; left: 20px; }
  .letter-box::after  { bottom: 16px; right: 20px; }

  .letter-text {
    font-family: 'IM Fell English', serif;
    font-size: clamp(15px, 2vw, 18px);
    line-height: 2;
    color: var(--deep);
    font-style: italic;
    text-align: center;
  }

  .letter-sign {
    display: block;
    text-align: right;
    margin-top: 28px;
    font-size: 12px;
    letter-spacing: 0.3em;
    color: var(--rose);
  }

  /* ── PROJECT FEATURE ── */
  .feature-card {
    border: 1px solid var(--blush);
    padding: 52px;
    background: linear-gradient(135deg, rgba(255,255,255,0.6), rgba(242,196,206,0.15));
    backdrop-filter: blur(8px);
    position: relative;
    overflow: hidden;
  }

  .feature-card::before {
    content: '';
    position: absolute;
    top: -60px; right: -60px;
    width: 200px; height: 200px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(242,196,206,0.3), transparent 70%);
    pointer-events: none;
  }

  .feature-quote {
    font-family: 'IM Fell English', serif;
    font-style: italic;
    font-size: clamp(18px, 3vw, 26px);
    color: var(--rose);
    text-align: center;
    line-height: 1.6;
    border-top: 1px solid var(--blush);
    border-bottom: 1px solid var(--blush);
    padding: 24px 0;
    margin-bottom: 40px;
  }

  .feature-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 28px;
    margin-bottom: 36px;
  }

  @media (max-width: 600px) { .feature-grid { grid-template-columns: 1fr; } }

  .feature-block h4 {
    font-family: 'Cinzel', serif;
    font-size: 11px;
    letter-spacing: 0.3em;
    color: var(--gold);
    margin-bottom: 12px;
    padding-bottom: 6px;
    border-bottom: 1px solid var(--blush);
  }

  .feature-block p, .feature-block li {
    font-size: 14px;
    line-height: 1.9;
    color: var(--deep);
    list-style: none;
  }

  .feature-block li::before { content: '— '; color: var(--rose); }

  .progress-wrap { margin-top: 32px; }
  .progress-label {
    font-size: 11px;
    letter-spacing: 0.3em;
    color: var(--dusty);
    margin-bottom: 8px;
  }
  .progress-bar {
    height: 3px;
    background: var(--blush);
    border-radius: 2px;
    overflow: hidden;
  }
  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--rose), var(--gold));
    border-radius: 2px;
    width: 0;
    transition: width 1.8s cubic-bezier(0.4,0,0.2,1) 0.4s;
  }
  section.visible .progress-fill { width: 80%; }

  /* ── SKILL PILLS ── */
  .pill-group { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 10px; }
  .pill {
    padding: 5px 14px;
    border: 1px solid var(--blush);
    font-size: 11px;
    letter-spacing: 0.2em;
    color: var(--deep);
    background: rgba(255,255,255,0.6);
    transition: background 0.3s, border-color 0.3s;
  }
  .pill:hover { background: var(--blush); border-color: var(--rose); }

  /* ── WARDROBE / TECH ── */
  .wardrobe-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
    gap: 1px;
    background: var(--blush);
    border: 1px solid var(--blush);
  }

  .wardrobe-cell {
    background: var(--cream);
    padding: 20px 16px;
    text-align: center;
    transition: background 0.3s;
  }

  .wardrobe-cell:hover { background: rgba(242,196,206,0.3); }

  .wardrobe-cell .wc-name {
    font-size: 11px;
    letter-spacing: 0.2em;
    color: var(--deep);
    margin-top: 6px;
  }

  .wardrobe-cell img { width: 32px; height: 32px; }

  /* ── CAPABILITY BARS ── */
  .capability-list { display: flex; flex-direction: column; gap: 18px; }
  .cap-item {}
  .cap-head {
    display: flex;
    justify-content: space-between;
    margin-bottom: 6px;
    font-size: 12px;
    letter-spacing: 0.25em;
    color: var(--deep);
  }
  .cap-pct { color: var(--rose); }
  .cap-bar { height: 2px; background: var(--blush); }
  .cap-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--rose), var(--gold));
    width: 0;
    transition: width 1.6s cubic-bezier(0.4,0,0.2,1);
  }
  section.visible .cap-fill { width: var(--pct); }

  /* ── QUOTE PULLOUT ── */
  .pullquote {
    text-align: center;
    padding: 60px 40px;
    border-top: 1px solid var(--blush);
    border-bottom: 1px solid var(--blush);
    margin: 20px 0;
  }

  .pullquote p {
    font-family: 'IM Fell English', serif;
    font-style: italic;
    font-size: clamp(20px, 3.5vw, 32px);
    color: var(--rose);
    line-height: 1.7;
  }

  .pullquote cite {
    display: block;
    font-size: 11px;
    letter-spacing: 0.4em;
    color: var(--gold);
    margin-top: 20px;
    font-style: normal;
  }

  /* ── CONTACT / MASTHEAD ── */
  .masthead {
    text-align: center;
    padding: 80px 40px 60px;
    border-top: 1px solid var(--blush);
  }

  .contact-links {
    display: flex;
    justify-content: center;
    gap: 32px;
    flex-wrap: wrap;
    margin: 32px 0;
  }

  .contact-link {
    font-size: 11px;
    letter-spacing: 0.35em;
    color: var(--deep);
    text-decoration: none;
    padding-bottom: 3px;
    border-bottom: 1px solid var(--blush);
    transition: border-color 0.3s, color 0.3s;
  }

  .contact-link:hover { color: var(--rose); border-color: var(--rose); }

  /* ── FOOTER ── */
  footer {
    text-align: center;
    padding: 32px;
    font-size: 10px;
    letter-spacing: 0.4em;
    color: var(--dusty);
    border-top: 1px solid var(--blush);
    position: relative;
    z-index: 2;
  }

  /* ── DIVIDER ORNAMENT ── */
  .ornament-divider {
    text-align: center;
    margin: 0;
    color: var(--gold);
    letter-spacing: 0.5em;
    font-size: 13px;
    padding: 20px 0;
    opacity: 0.7;
  }

  /* ── ANIMATIONS ── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(22px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  @keyframes floatPetal {
    0%   { transform: translateY(-10px) rotate(0deg) translateX(0); opacity: 0; }
    10%  { opacity: 0.8; }
    90%  { opacity: 0.6; }
    100% { transform: translateY(110vh) rotate(720deg) translateX(60px); opacity: 0; }
  }

  /* ── SCROLLBAR ── */
  ::-webkit-scrollbar { width: 6px; }
  ::-webkit-scrollbar-track { background: var(--cream); }
  ::-webkit-scrollbar-thumb { background: var(--blush); border-radius: 3px; }
  ::-webkit-scrollbar-thumb:hover { background: var(--rose); }
</style>
</head>
<body>

<!-- Falling petals canvas -->
<canvas id="petals-canvas"></canvas>

<div class="page">

  <!-- ═══════════════ COVER ═══════════════ -->
  <div class="cover">

    <!-- Blooming corner flowers (SVG) -->
    <div class="flowers-wrap">

      <!-- Bottom Left Bouquet -->
      <svg class="flower flower-tl" viewBox="0 0 320 380" xmlns="http://www.w3.org/2000/svg">
        <!-- Stems -->
        <g stroke="#8aab82" stroke-width="2" fill="none">
          <path d="M160 380 Q140 320 120 280 Q100 240 110 200"/>
          <path d="M160 380 Q165 310 180 270 Q195 230 185 185"/>
          <path d="M160 380 Q150 340 130 310 Q110 280 115 250"/>
          <path d="M160 380 Q170 350 190 320 Q210 290 205 260"/>
          <path d="M160 380 Q155 330 145 295 Q135 260 140 230"/>
        </g>
        <!-- Leaves -->
        <ellipse cx="125" cy="240" rx="18" ry="8" fill="#8aab82" opacity="0.7" transform="rotate(-40,125,240)"/>
        <ellipse cx="190" cy="225" rx="18" ry="8" fill="#8aab82" opacity="0.7" transform="rotate(30,190,225)"/>
        <ellipse cx="135" cy="295" rx="14" ry="6" fill="#8aab82" opacity="0.6" transform="rotate(-25,135,295)"/>
        <!-- Roses -->
        <g class="petal-sway">
          <!-- Rose 1 -->
          <g transform="translate(110,195)">
            <circle cx="0" cy="0" r="22" fill="#f9d5dc"/>
            <circle cx="0" cy="0" r="16" fill="#f2b8c4"/>
            <circle cx="0" cy="0" r="10" fill="#e8909c"/>
            <circle cx="0" cy="0" r="5"  fill="#d4849a"/>
            <ellipse cx="-14" cy="-8"  rx="12" ry="8" fill="#f9d5dc" opacity="0.85" transform="rotate(-30,-14,-8)"/>
            <ellipse cx="14"  cy="-8"  rx="12" ry="8" fill="#f9d5dc" opacity="0.85" transform="rotate(30,14,-8)"/>
            <ellipse cx="0"   cy="-18" rx="10" ry="8" fill="#f9d5dc" opacity="0.85"/>
            <ellipse cx="-16" cy="8"   rx="12" ry="8" fill="#f2b8c4" opacity="0.8" transform="rotate(20,-16,8)"/>
            <ellipse cx="16"  cy="8"   rx="12" ry="8" fill="#f2b8c4" opacity="0.8" transform="rotate(-20,16,8)"/>
          </g>
          <!-- Rose 2 -->
          <g transform="translate(185,180)">
            <circle cx="0" cy="0" r="20" fill="#fce8d0"/>
            <circle cx="0" cy="0" r="14" fill="#f9d5dc"/>
            <circle cx="0" cy="0" r="9"  fill="#f2b8c4"/>
            <circle cx="0" cy="0" r="4"  fill="#e8909c"/>
            <ellipse cx="-13" cy="-7"  rx="11" ry="7" fill="#fce8d0" opacity="0.85" transform="rotate(-30,-13,-7)"/>
            <ellipse cx="13"  cy="-7"  rx="11" ry="7" fill="#fce8d0" opacity="0.85" transform="rotate(30,13,-7)"/>
            <ellipse cx="0"   cy="-16" rx="9"  ry="7" fill="#fce8d0" opacity="0.85"/>
            <ellipse cx="-14" cy="7"   rx="11" ry="7" fill="#f9d5dc" opacity="0.8" transform="rotate(20,-14,7)"/>
            <ellipse cx="14"  cy="7"   rx="11" ry="7" fill="#f9d5dc" opacity="0.8" transform="rotate(-20,14,7)"/>
          </g>
          <!-- Rose 3 (small) -->
          <g transform="translate(145,255)">
            <circle cx="0" cy="0" r="15" fill="#f9d5dc"/>
            <circle cx="0" cy="0" r="10" fill="#e8909c"/>
            <circle cx="0" cy="0" r="5"  fill="#d4849a"/>
            <ellipse cx="-10" cy="-6"  rx="8" ry="6" fill="#f9d5dc" opacity="0.85" transform="rotate(-30,-10,-6)"/>
            <ellipse cx="10"  cy="-6"  rx="8" ry="6" fill="#f9d5dc" opacity="0.85" transform="rotate(30,10,-6)"/>
            <ellipse cx="0"   cy="-12" rx="7" ry="6" fill="#f9d5dc" opacity="0.85"/>
          </g>
          <!-- Bud -->
          <g transform="translate(205,255)">
            <ellipse cx="0" cy="0" rx="7" ry="12" fill="#f2b8c4"/>
            <ellipse cx="-5" cy="2" rx="5" ry="9" fill="#e8909c" opacity="0.7" transform="rotate(-15,-5,2)"/>
            <ellipse cx="5"  cy="2" rx="5" ry="9" fill="#e8909c" opacity="0.7" transform="rotate(15,5,2)"/>
            <ellipse cx="-4" cy="-4" rx="4" ry="6" fill="#8aab82" opacity="0.9" transform="rotate(-20,-4,-4)"/>
            <ellipse cx="4"  cy="-4" rx="4" ry="6" fill="#8aab82" opacity="0.9" transform="rotate(20,4,-4)"/>
          </g>
        </g>
        <!-- Baby's breath -->
        <g fill="#fdf6ee" stroke="#c9a0b4" stroke-width="0.5">
          <circle cx="100" cy="215" r="3" fill="#fce4ec"/>
          <circle cx="107" cy="208" r="2.5" fill="#fce4ec"/>
          <circle cx="96"  cy="208" r="2"   fill="#fce4ec"/>
          <circle cx="170" cy="198" r="3"   fill="#fce4ec"/>
          <circle cx="177" cy="192" r="2.5" fill="#fce4ec"/>
          <circle cx="163" cy="192" r="2"   fill="#fce4ec"/>
          <circle cx="165" cy="268" r="2.5" fill="#fce4ec"/>
          <circle cx="172" cy="263" r="2"   fill="#fce4ec"/>
        </g>
      </svg>

      <!-- Bottom Right Bouquet (mirrored) -->
      <svg class="flower flower-tr" viewBox="0 0 320 380" xmlns="http://www.w3.org/2000/svg">
        <g stroke="#8aab82" stroke-width="2" fill="none">
          <path d="M160 380 Q140 320 120 280 Q100 240 110 200"/>
          <path d="M160 380 Q165 310 180 270 Q195 230 185 185"/>
          <path d="M160 380 Q150 340 130 310 Q110 280 115 250"/>
          <path d="M160 380 Q170 350 190 320 Q210 290 205 260"/>
        </g>
        <ellipse cx="125" cy="240" rx="18" ry="8" fill="#8aab82" opacity="0.7" transform="rotate(-40,125,240)"/>
        <ellipse cx="190" cy="225" rx="18" ry="8" fill="#8aab82" opacity="0.7" transform="rotate(30,190,225)"/>
        <g class="petal-sway">
          <g transform="translate(110,195)">
            <circle cx="0" cy="0" r="20" fill="#fce8d0"/>
            <circle cx="0" cy="0" r="14" fill="#f9d5dc"/>
            <circle cx="0" cy="0" r="8"  fill="#f2b8c4"/>
            <circle cx="0" cy="0" r="4"  fill="#e8909c"/>
            <ellipse cx="-13" cy="-7"  rx="11" ry="7" fill="#fce8d0" opacity="0.85" transform="rotate(-30,-13,-7)"/>
            <ellipse cx="13"  cy="-7"  rx="11" ry="7" fill="#fce8d0" opacity="0.85" transform="rotate(30,13,-7)"/>
            <ellipse cx="0"   cy="-16" rx="9"  ry="7" fill="#fce8d0" opacity="0.85"/>
            <ellipse cx="-14" cy="7"   rx="11" ry="7" fill="#f9d5dc" opacity="0.8"  transform="rotate(20,-14,7)"/>
            <ellipse cx="14"  cy="7"   rx="11" ry="7" fill="#f9d5dc" opacity="0.8"  transform="rotate(-20,14,7)"/>
          </g>
          <g transform="translate(185,180)">
            <circle cx="0" cy="0" r="22" fill="#f9d5dc"/>
            <circle cx="0" cy="0" r="16" fill="#f2b8c4"/>
            <circle cx="0" cy="0" r="10" fill="#e8909c"/>
            <circle cx="0" cy="0" r="5"  fill="#d4849a"/>
            <ellipse cx="-14" cy="-8"  rx="12" ry="8" fill="#f9d5dc" opacity="0.85" transform="rotate(-30,-14,-8)"/>
            <ellipse cx="14"  cy="-8"  rx="12" ry="8" fill="#f9d5dc" opacity="0.85" transform="rotate(30,14,-8)"/>
            <ellipse cx="0"   cy="-18" rx="10" ry="8" fill="#f9d5dc" opacity="0.85"/>
            <ellipse cx="-16" cy="8"   rx="12" ry="8" fill="#f2b8c4" opacity="0.8"  transform="rotate(20,-16,8)"/>
            <ellipse cx="16"  cy="8"   rx="12" ry="8" fill="#f2b8c4" opacity="0.8"  transform="rotate(-20,16,8)"/>
          </g>
          <g transform="translate(148,260)">
            <circle cx="0" cy="0" r="14" fill="#f9d5dc"/>
            <circle cx="0" cy="0" r="9"  fill="#e8909c"/>
            <circle cx="0" cy="0" r="4"  fill="#d4849a"/>
            <ellipse cx="-9"  cy="-5" rx="8" ry="5" fill="#f9d5dc" opacity="0.85" transform="rotate(-30,-9,-5)"/>
            <ellipse cx="9"   cy="-5" rx="8" ry="5" fill="#f9d5dc" opacity="0.85" transform="rotate(30,9,-5)"/>
            <ellipse cx="0"   cy="-12" rx="7" ry="5" fill="#f9d5dc" opacity="0.85"/>
          </g>
        </g>
        <g fill="#fce4ec">
          <circle cx="100" cy="215" r="3"/>
          <circle cx="107" cy="208" r="2.5"/>
          <circle cx="96"  cy="208" r="2"/>
          <circle cx="170" cy="198" r="3"/>
          <circle cx="177" cy="192" r="2.5"/>
        </g>
      </svg>

    </div><!-- /flowers-wrap -->

    <div class="cover-ornament">✦ &nbsp; GITHUB PROFILE &nbsp; ✦</div>
    <div class="cover-rule"></div>
    <h1 class="cover-name">NANDHIKA</h1>
    <p class="cover-tagline">Artificial Intelligence · Machine Learning · Python Engineering</p>
    <p class="cover-vol">VOL. I &nbsp;·&nbsp; 2026 &nbsp;·&nbsp; BENGALURU &nbsp;·&nbsp; THE AI ISSUE</p>
    <div class="cover-badges">
      <span class="badge">BCA · BACHELOR OF COMPUTER APPLICATIONS</span><span class="badge">DATA SCIENCE</span>
      <span class="badge">AI ASSISTANT BUILD</span>
      <span class="badge">INDIA 🇮🇳</span>
    </div>
  </div>

  <!-- ═══════════════ EDITOR'S LETTER ═══════════════ -->
  <section id="letter">
    <div class="section-label">✦ &nbsp; THE OPENING &nbsp; ✦</div>
    <h2 class="section-title">Editor's Letter</h2>
    <div class="section-rule"></div>
    <div class="letter-box">
      <p class="letter-text">
        She arrived at the intersection of code and intelligence —<br/>
        a developer who treats every model like a masterpiece<br/>
        and every dataset like a carefully curated mood board.<br/><br/>
        A BCA student (Bachelor of Computer Applications) specialising in Data Science,<br/>
        she builds systems that don't merely process data —<br/>
        they understand it. They feel it.<br/><br/>
        This season's collection: an AI that listens, thinks,<br/>
        speaks, and responds with empathy.<br/>
        Draped in Python. Finished in Generative AI.
      </p>
      <span class="letter-sign">— The Edit &nbsp; ✦</span>
    </div>
  </section>

  <div class="ornament-divider">✦ &nbsp;&nbsp; ✦ &nbsp;&nbsp; ✦</div>

  <!-- ═══════════════ FEATURE PROJECT ═══════════════ -->
  <section id="feature">
    <div class="section-label">✦ &nbsp; THE FEATURE &nbsp; ✦</div>
    <h2 class="section-title">The Project</h2>
    <div class="section-rule"></div>
    <div class="feature-card">
      <div class="feature-quote">" The most intelligent thing she has ever built. "</div>

      <div class="feature-grid">
        <div class="feature-block">
          <h4>Architecture</h4>
          <ul>
            <li>HTML · CSS · JavaScript frontend</li>
            <li>Animated character interface</li>
            <li>Python · FastAPI backend</li>
            <li>REST API — client/server</li>
            <li>pyttsx3 voice synthesis</li>
            <li>Web Speech API output</li>
          </ul>
        </div>
        <div class="feature-block">
          <h4>Intelligence Layer</h4>
          <ul>
            <li>Keyword-based NLP mood engine</li>
            <li>sad → empathetic response</li>
            <li>happy → warm, celebratory</li>
            <li>angry → calm encouragement</li>
            <li>neutral → open conversation</li>
            <li>JSON response pipeline</li>
          </ul>
        </div>
        <div class="feature-block">
          <h4>Endpoints</h4>
          <ul>
            <li>GET / → serves web UI</li>
            <li>POST /detect-mood</li>
            <li>Accepts user text input</li>
            <li>Returns mood + response</li>
            <li>Fetch API communication</li>
          </ul>
        </div>
        <div class="feature-block">
          <h4>Full Stack</h4>
          <ul>
            <li>Python · FastAPI · pyttsx3</li>
            <li>HTML5 · CSS3 · JavaScript</li>
            <li>Web Speech API</li>
            <li>Fetch API · JSON</li>
            <li>REST architecture</li>
          </ul>
        </div>
      </div>

      <div class="progress-wrap">
        <div class="progress-label">DEVELOPMENT PROGRESS &nbsp; · &nbsp; 80%</div>
        <div class="progress-bar"><div class="progress-fill"></div></div>
      </div>
    </div>
  </section>

  <div class="ornament-divider">✦ &nbsp;&nbsp; ✦ &nbsp;&nbsp; ✦</div>

  <!-- ═══════════════ THE WARDROBE ═══════════════ -->
  <section id="wardrobe">
    <div class="section-label">✦ &nbsp; THE WARDROBE &nbsp; ✦</div>
    <h2 class="section-title">Technical Arsenal</h2>
    <div class="section-rule"></div>
    <div class="wardrobe-grid">

      <!-- Python -->
      <div class="wardrobe-cell">
        <svg viewBox="0 0 48 48" width="36" height="36" xmlns="http://www.w3.org/2000/svg">
          <linearGradient id="pyA" x1="0" y1="0" x2="1" y2="1"><stop offset="0" stop-color="#5A9FD4"/><stop offset="1" stop-color="#306998"/></linearGradient>
          <linearGradient id="pyB" x1="0" y1="0" x2="1" y2="1"><stop offset="0" stop-color="#FFD43B"/><stop offset="1" stop-color="#FFE873"/></linearGradient>
          <path fill="url(#pyA)" d="M24 3.5c-5.2 0-9 1.1-9 4.5v3h9v1.5H11.5C8.4 12.5 6 15.2 6 20.5c0 5.4 2.4 8 6.5 8H15v-4c0-3.8 2.7-6 6.5-6h8c3.2 0 5.5-2 5.5-5V8c0-2.8-2.7-4.5-11-4.5zm-3.5 3a1.5 1.5 0 110 3 1.5 1.5 0 010-3z"/>
          <path fill="url(#pyB)" d="M24 44.5c5.2 0 9-1.1 9-4.5v-3h-9V35.5h12.5c3.1 0 5.5-2.7 5.5-8 0-5.4-2.4-8-6.5-8H33v4c0 3.8-2.7 6-6.5 6h-8c-3.2 0-5.5 2-5.5 5v7c0 2.8 2.7 4.5 11 4.5zm3.5-3a1.5 1.5 0 110-3 1.5 1.5 0 010 3z"/>
        </svg>
        <div class="wc-name">Python</div>
      </div>

      <!-- FastAPI -->
      <div class="wardrobe-cell">
        <svg viewBox="0 0 48 48" width="36" height="36" xmlns="http://www.w3.org/2000/svg">
          <circle cx="24" cy="24" r="20" fill="#009688"/>
          <path fill="#fff" d="M25 14l-9 14h8l-1 6 9-14h-8z"/>
        </svg>
        <div class="wc-name">FastAPI</div>
      </div>

      <!-- TensorFlow -->
      <div class="wardrobe-cell">
        <svg viewBox="0 0 48 48" width="36" height="36" xmlns="http://www.w3.org/2000/svg">
          <path fill="#FF6F00" d="M24 4L6 14v8l18-10 18 10v-8z"/>
          <path fill="#FFA000" d="M6 22v8l9 5v-8zM33 27v8l9-5v-8z"/>
          <path fill="#FF6F00" d="M15 27v12l9 5 9-5V27l-9 5z"/>
          <path fill="#FFA000" d="M24 32l-9-5v8l9 5z"/>
        </svg>
        <div class="wc-name">TensorFlow</div>
      </div>

      <!-- PyTorch -->
      <div class="wardrobe-cell">
        <svg viewBox="0 0 48 48" width="36" height="36" xmlns="http://www.w3.org/2000/svg">
          <path fill="#EE4C2C" d="M24 6l-2 8 8 2-2 8 8 2-8 12c-6.6 0-12-5.4-12-12V14c0-4.4 3.6-8 8-8z"/>
          <circle cx="30" cy="12" r="2.5" fill="#EE4C2C"/>
          <path fill="#EE4C2C" d="M16 14c0-4.4 3.6-8 8-8v8l-8 12V14z" opacity=".4"/>
        </svg>
        <div class="wc-name">PyTorch</div>
      </div>

      <!-- Scikit-Learn -->
      <div class="wardrobe-cell">
        <svg viewBox="0 0 48 48" width="36" height="36" xmlns="http://www.w3.org/2000/svg">
          <ellipse cx="24" cy="24" rx="18" ry="12" fill="#F7931E" opacity=".2"/>
          <ellipse cx="24" cy="24" rx="18" ry="12" fill="none" stroke="#F7931E" stroke-width="2.5"/>
          <ellipse cx="24" cy="24" rx="18" ry="12" fill="none" stroke="#3499CD" stroke-width="2.5" transform="rotate(60 24 24)"/>
          <ellipse cx="24" cy="24" rx="18" ry="12" fill="none" stroke="#3499CD" stroke-width="2.5" transform="rotate(120 24 24)"/>
          <circle cx="24" cy="24" r="4" fill="#F7931E"/>
        </svg>
        <div class="wc-name">Scikit-Learn</div>
      </div>

      <!-- HTML5 -->
      <div class="wardrobe-cell">
        <svg viewBox="0 0 48 48" width="36" height="36" xmlns="http://www.w3.org/2000/svg">
          <path fill="#E44D26" d="M8 6l3.6 40L24 50l12.4-4L40 6z"/>
          <path fill="#F16529" d="M24 46.4V9.6H8.4L11.6 44z"/>
          <path fill="#EBEBEB" d="M24 20H15l.6 6H24v6h-8.4l.6 5.6L24 40v6.4l-8.8-2.4L14 36H8.8l1.4 16L24 56V46.4l-7.8-2.1-.6-5.9H24z" transform="scale(.8) translate(3 -4)"/>
          <path fill="#fff" d="M24 20v6h8.4l-.8 8-7.6 2v6.4l8.8-2.4 6.4-16H24z" transform="scale(.8) translate(3 -4)"/>
        </svg>
        <div class="wc-name">HTML5</div>
      </div>

      <!-- CSS3 -->
      <div class="wardrobe-cell">
        <svg viewBox="0 0 48 48" width="36" height="36" xmlns="http://www.w3.org/2000/svg">
          <path fill="#1572B6" d="M8 6l3.6 40L24 50l12.4-4L40 6z"/>
          <path fill="#33A9DC" d="M24 46.4V9.6H8.4L11.6 44z"/>
          <path fill="#fff" d="M24 22h-9l.4 4.5H24V31h-8l.5 4.5 7.5 2v4.8l-8-2.3-1-10.5h-4l1.8 20L24 52V46.4l-7.5-2-.5-4.9H24z" transform="scale(.75) translate(4 -3)"/>
          <path fill="#EBEBEB" d="M24 22v4.5h8.3l-.8 8.5-7.5 2v4.8l8-2.3 5.5-17H24z" transform="scale(.75) translate(4 -3)"/>
        </svg>
        <div class="wc-name">CSS3</div>
      </div>

      <!-- JavaScript -->
      <div class="wardrobe-cell">
        <svg viewBox="0 0 48 48" width="36" height="36" xmlns="http://www.w3.org/2000/svg">
          <rect width="36" height="36" x="6" y="6" fill="#F7DF1E" rx="2"/>
          <path fill="#323330" d="M27 34c0 2.5 1.4 3.4 3.2 3.4 1.7 0 2.8-.9 2.8-2.2 0-1.5-1.1-2-3-2.8l-1-.4c-3-1.3-5-2.9-5-6.3 0-3.1 2.4-5.5 6.1-5.5 2.6 0 4.5 1 5.8 3.4l-3.2 2c-.7-1.2-1.4-1.7-2.6-1.7-1.2 0-2 .8-2 1.7 0 1.2.7 1.7 2.4 2.4l1 .4c3.5 1.5 5.5 3 5.5 6.5 0 3.7-2.9 5.8-6.8 5.8-3.8 0-6.3-1.8-7.5-4.2L27 34zM14 20.5h4V37c0 3.5-1.7 5.1-4.2 5.1-2.2 0-3.5-1.2-4.2-2.5l3.2-2c.4.7.8 1.2 1.6 1.2.8 0 1.4-.4 1.4-2V20.5h-1.8z"/>
        </svg>
        <div class="wc-name">JavaScript</div>
      </div>

      <!-- Git -->
      <div class="wardrobe-cell">
        <svg viewBox="0 0 48 48" width="36" height="36" xmlns="http://www.w3.org/2000/svg">
          <path fill="#F05032" d="M44.5 21.7L26.3 3.5a3.3 3.3 0 00-4.6 0l-4.6 4.6 5.8 5.8a3.9 3.9 0 014.9 4.9l5.6 5.6a3.9 3.9 0 11-2.3 2.3l-5.2-5.2v13.7a3.9 3.9 0 11-3.2 0V21l-6.4-6.4a3.3 3.3 0 000 4.6l18.2 18.2a3.3 3.3 0 004.6 0l11.4-11.4a3.3 3.3 0 000-4.6z"/>
        </svg>
        <div class="wc-name">Git</div>
      </div>

      <!-- GitHub -->
      <div class="wardrobe-cell">
        <svg viewBox="0 0 48 48" width="36" height="36" xmlns="http://www.w3.org/2000/svg">
          <path fill="#3d2b35" d="M24 4C12.95 4 4 12.95 4 24c0 8.84 5.73 16.33 13.67 18.97.1.02.2.03.3.03.8 0 1.1-.6 1.1-1.1v-3.8c-5.56 1.21-6.73-2.68-6.73-2.68-.91-2.3-2.22-2.92-2.22-2.92-1.8-1.24.14-1.22.14-1.22 2 .14 3.05 2.05 3.05 2.05 1.78 3.04 4.66 2.16 5.8 1.65.18-1.28.7-2.16 1.27-2.66-4.44-.5-9.1-2.22-9.1-9.88 0-2.18.78-3.97 2.05-5.37-.2-.5-.89-2.54.2-5.3 0 0 1.67-.53 5.47 2.04A19.07 19.07 0 0124 13.4c1.69.01 3.39.23 4.98.67 3.8-2.57 5.47-2.04 5.47-2.04 1.09 2.76.4 4.8.2 5.3 1.28 1.4 2.05 3.19 2.05 5.37 0 7.68-4.68 9.37-9.14 9.87.72.62 1.36 1.84 1.36 3.7v5.5c0 .5.3 1.12 1.12 1.1C38.28 40.32 44 32.83 44 24 44 12.95 35.05 4 24 4z"/>
        </svg>
        <div class="wc-name">GitHub</div>
      </div>

      <!-- VS Code -->
      <div class="wardrobe-cell">
        <svg viewBox="0 0 48 48" width="36" height="36" xmlns="http://www.w3.org/2000/svg">
          <path fill="#0065A9" d="M34 6L18 22 9 15 4 18v12l5 3 9-7 16 16 8-4V10z"/>
          <path fill="#007ACC" d="M34 6l8 4v28l-8 4-16-16-9 7-5-3V18l5-3 9 7z"/>
          <path fill="#1F9CF0" d="M34 6L18 22v4l16 16 8-4V10z" opacity=".5"/>
          <path fill="#fff" d="M9 21v6l9-3z"/>
        </svg>
        <div class="wc-name">VS Code</div>
      </div>

      <!-- Jupyter -->
      <div class="wardrobe-cell">
        <svg viewBox="0 0 48 48" width="36" height="36" xmlns="http://www.w3.org/2000/svg">
          <path fill="#F37726" d="M24 8C15.2 8 8 15.2 8 24s7.2 16 16 16 16-7.2 16-16S32.8 8 24 8zm0 28c-6.6 0-12-5.4-12-12S17.4 12 24 12s12 5.4 12 12-5.4 12-12 12z"/>
          <circle cx="24" cy="14" r="3" fill="#F37726"/>
          <circle cx="14" cy="30" r="2.5" fill="#9E9E9E"/>
          <circle cx="34" cy="30" r="2" fill="#616161"/>
        </svg>
        <div class="wc-name">Jupyter</div>
      </div>

    </div>
    <br/>
    <div class="pill-group" style="justify-content:center">
      <span class="pill">LangChain</span>
      <span class="pill">HuggingFace</span>
      <span class="pill">OpenAI API</span>
      <span class="pill">Gemini API</span>
      <span class="pill">Pandas</span>
      <span class="pill">NumPy</span>
      <span class="pill">OpenCV</span>
      <span class="pill">Streamlit</span>
      <span class="pill">Google Colab</span>
      
      <span class="pill">SQL</span>
      <span class="pill">Matplotlib</span>
    </div>
  </section>

  <div class="ornament-divider">✦ &nbsp;&nbsp; ✦ &nbsp;&nbsp; ✦</div>

  <!-- ═══════════════ CAPABILITY MATRIX ═══════════════ -->
  <section id="capabilities">
    <div class="section-label">✦ &nbsp; THE MEASURE &nbsp; ✦</div>
    <h2 class="section-title">Capability Index</h2>
    <div class="section-rule"></div>
    <div class="capability-list">
      <div class="cap-item"><div class="cap-head"><span>Python</span><span class="cap-pct">80</span></div><div class="cap-bar"><div class="cap-fill" style="--pct:80%"></div></div></div>
      <div class="cap-item"><div class="cap-head"><span>Data Science</span><span class="cap-pct">70</span></div><div class="cap-bar"><div class="cap-fill" style="--pct:70%"></div></div></div>
      <div class="cap-item"><div class="cap-head"><span>Generative AI</span><span class="cap-pct">60</span></div><div class="cap-bar"><div class="cap-fill" style="--pct:60%"></div></div></div>
      <div class="cap-item"><div class="cap-head"><span>Machine Learning</span><span class="cap-pct">60</span></div><div class="cap-bar"><div class="cap-fill" style="--pct:60%"></div></div></div>
      <div class="cap-item"><div class="cap-head"><span>Deep Learning</span><span class="cap-pct">50</span></div><div class="cap-bar"><div class="cap-fill" style="--pct:50%"></div></div></div>
      <div class="cap-item"><div class="cap-head"><span>NLP</span><span class="cap-pct">40</span></div><div class="cap-bar"><div class="cap-fill" style="--pct:40%"></div></div></div>
    </div>
  </section>

  <!-- ═══════════════ PULLQUOTE ═══════════════ -->
  <div class="pullquote">
    <p>" She doesn't follow the algorithm.<br/>She writes it. "</p>
    <cite>— nandhika0422</cite>
  </div>

  <!-- ═══════════════ MASTHEAD ═══════════════ -->
  <div class="masthead" id="contact">
    <div class="section-label">✦ &nbsp; THE MASTHEAD &nbsp; ✦</div>
    <h2 class="section-title" style="font-size:28px; margin-top:10px;">Establish Contact</h2>
    <div class="section-rule"></div>
    <div class="contact-links">
      <a class="contact-link" href="https://github.com/nandhika0422" target="_blank">GitHub</a>
      <a class="contact-link" href="mailto:nandhikasri0422@gmail.com">nandhikasri0422@gmail.com</a>
      
    </div>
    <p style="font-size:12px; letter-spacing:0.25em; color:var(--dusty); margin-top:12px;">
      Turning raw data into intelligence. One model at a time.
    </p>
  </div>

  <footer>✦ &nbsp; NANDHIKA &nbsp; · &nbsp; nandhika0422 &nbsp; · &nbsp; 2026 &nbsp; ✦</footer>

</div><!-- /page -->

<script>
// ── FALLING PETALS ──
const canvas = document.getElementById('petals-canvas');
const ctx    = canvas.getContext('2d');

function resize() { canvas.width = window.innerWidth; canvas.height = window.innerHeight; }
resize();
window.addEventListener('resize', resize);

const COLORS = ['#f9d5dc','#f2b8c4','#e8909c','#fce8d0','#f2c4ce','#fdf6ee','#c9a0b4'];

class Petal {
  constructor() { this.reset(true); }
  reset(initial) {
    this.x    = Math.random() * canvas.width;
    this.y    = initial ? Math.random() * canvas.height : -20;
    this.size = 4 + Math.random() * 7;
    this.speedY = 0.5 + Math.random() * 1.2;
    this.speedX = (Math.random() - 0.5) * 0.8;
    this.rotation = Math.random() * Math.PI * 2;
    this.rotSpeed = (Math.random() - 0.5) * 0.03;
    this.opacity  = 0.4 + Math.random() * 0.5;
    this.color    = COLORS[Math.floor(Math.random() * COLORS.length)];
    this.wobble   = Math.random() * Math.PI * 2;
    this.wobbleSpeed = 0.02 + Math.random() * 0.02;
  }
  update() {
    this.wobble   += this.wobbleSpeed;
    this.x        += this.speedX + Math.sin(this.wobble) * 0.5;
    this.y        += this.speedY;
    this.rotation += this.rotSpeed;
    if (this.y > canvas.height + 20) this.reset(false);
  }
  draw() {
    ctx.save();
    ctx.globalAlpha = this.opacity;
    ctx.translate(this.x, this.y);
    ctx.rotate(this.rotation);
    ctx.fillStyle = this.color;
    // Petal shape
    ctx.beginPath();
    ctx.ellipse(0, 0, this.size, this.size * 1.6, 0, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
  }
}

const petals = Array.from({length: 55}, () => new Petal());

function animatePetals() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  petals.forEach(p => { p.update(); p.draw(); });
  requestAnimationFrame(animatePetals);
}
animatePetals();

// ── SCROLL REVEAL ──
const observer = new IntersectionObserver(entries => {
  entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible'); });
}, { threshold: 0.15 });

document.querySelectorAll('section').forEach(s => observer.observe(s));
</script>
</body>
</html>
