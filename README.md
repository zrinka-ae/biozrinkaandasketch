<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>The Gear Hog – Sales Portfolio</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet"/>
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --orange: #F4600C;
  --orange-dark: #C94C08;
  --charcoal: #2C2C2C;
  --white: #FFFFFF;
}

html { scroll-behavior: smooth; }
body { font-family: 'Inter', sans-serif; background: #111; overflow-x: hidden; }

/* ─────────────────────────────────────────
   JOKE BANNER — pinned to very top of page
───────────────────────────────────────── */
.joke-banner {
  position: relative;
  z-index: 20;
  background: #000;
  color: #fff;
  text-align: center;
  padding: 0.6rem 1.5rem;
  font-family: 'Inter', sans-serif;
  font-size: clamp(0.78rem, 1.6vw, 0.92rem);
  font-weight: 500;
  letter-spacing: 0.04em;
  animation: fadeIn 0.6s ease both;
}

/* ─────────────────────────────────────────
   HERO WRAPPER — sky + building only
   NO overflow:hidden so nothing is clipped
───────────────────────────────────────── */
.hero {
  position: relative;
  width: 100%;
  /* Tall enough for sky + building, stops at concrete line */
  height: 80vw;
  min-height: 520px;
  max-height: 820px;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: visible; /* allow dropdowns to escape */
}

/* SVG covers the hero box */
.hero-scene {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  overflow: hidden; /* clip the illustration only */
  border-radius: 0;
}

/* ─────────────────────────────────────────
   HERO TEXT — centred inside the sky band.
   Sky occupies the top ~28% of the SVG
   (0–250 of 900). We mirror that with
   top: 0 / bottom: 72% so the flexbox
   centres the text perfectly in that band.
───────────────────────────────────────── */
.hero-text {
  position: absolute;
  top: 0;
  bottom: 58%;          /* sky now occupies ~42% of the taller hero */
  left: 0;
  right: 0;
  z-index: 10;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 0 2rem;
  pointer-events: none;
  animation: fadeDown 0.7s ease both;
}

.hero-title {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(1.6rem, 4.2vw, 3.8rem);
  line-height: 1.05;
  color: #fff;
  text-shadow: 2px 3px 0 rgba(0,0,0,0.55), 0 0 24px rgba(0,0,0,0.3);
  letter-spacing: 0.04em;
}
.hero-title em { color: #FFCC44; font-style: normal; }

.hero-sub {
  margin-top: 0.5rem;
  font-size: clamp(0.78rem, 1.4vw, 1rem);
  color: rgba(255,255,255,0.92);
  font-weight: 500;
  text-shadow: 0 1px 8px rgba(0,0,0,0.75);
  line-height: 1.5;
}

/* ─────────────────────────────────────────
   CONCRETE SECTION — sits below hero image,
   matching the SVG forecourt colour.
   CTAs live here, dropdowns expand freely.
───────────────────────────────────────── */
.concrete {
  background: linear-gradient(180deg, #7A7570 0%, #5C5850 100%);
  width: 100%;
  padding: 2.2rem 1.5rem 3rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  position: relative;
  z-index: 5;
}

/* ─────────────────────────────────────────
   CTA ZONE inside concrete
───────────────────────────────────────── */
.cta-zone {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  align-items: center;
  width: 100%;
  animation: riseUp 0.7s 0.15s ease both;
}

/* ─────────────────────────────────────────
   BUTTONS
───────────────────────────────────────── */
.cta-btn {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.2rem;
  background: var(--orange);
  color: #fff;
  border: none;
  border-radius: 8px;
  padding: 0.85rem 1.8rem;
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(1rem, 2.2vw, 1.25rem);
  letter-spacing: 0.1em;
  cursor: pointer;
  text-decoration: none;
  width: 260px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.4), inset 0 1px 0 rgba(255,255,255,0.12);
  transition: transform 0.16s ease, background 0.16s ease, box-shadow 0.16s ease;
}
.cta-btn .sub {
  font-family: 'Inter', sans-serif;
  font-size: 0.62rem;
  font-weight: 600;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  opacity: 0.8;
}
.cta-btn:hover {
  background: var(--orange-dark);
  transform: translateY(-3px) scale(1.03);
  box-shadow: 0 8px 26px rgba(0,0,0,0.45);
}
.cta-btn:active { transform: scale(0.98); }

/* ─────────────────────────────────────────
   SKILLS DROPDOWN
───────────────────────────────────────── */
.skills-wrap { display: flex; flex-direction: column; align-items: center; width: 260px; }

.skills-panel {
  max-height: 0;
  overflow: hidden;
  opacity: 0;
  transition: max-height 0.5s cubic-bezier(0.4,0,0.2,1), opacity 0.35s ease;
  width: 340px; /* wider than button so 2-col grid fits */
}
.skills-panel.open { max-height: 800px; opacity: 1; }

.skills-inner {
  background: rgba(16,14,10,0.96);
  border: 1px solid rgba(244,96,12,0.4);
  border-radius: 8px;
  padding: 1.2rem 1.5rem;
  margin-top: 0.5rem;
  text-align: left;
}
.skills-inner h3 {
  font-family: 'Bebas Neue', sans-serif;
  font-size: 1rem;
  letter-spacing: 0.18em;
  color: var(--orange);
  margin-bottom: 0.8rem;
}
.skills-list {
  list-style: none;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.5rem 1.4rem;
}
.skills-list li {
  font-size: 0.78rem;
  color: rgba(255,255,255,0.88);
  font-weight: 500;
  display: flex;
  align-items: flex-start;
  gap: 0.4rem;
  line-height: 1.4;
}
.skills-list li::before {
  content: '⚙';
  color: var(--orange);
  font-size: 0.7rem;
  margin-top: 0.05rem;
  flex-shrink: 0;
}

/* ─────────────────────────────────────────
   CONTACT DROPDOWN (Just Tires)
───────────────────────────────────────── */
.contact-wrap { display: flex; flex-direction: column; align-items: center; width: 260px; }

.contact-panel {
  max-height: 0;
  overflow: hidden;
  opacity: 0;
  transition: max-height 0.4s cubic-bezier(0.4,0,0.2,1), opacity 0.3s ease;
  width: 260px;
}
.contact-panel.open { max-height: 200px; opacity: 1; }

.contact-inner {
  background: rgba(16,14,10,0.96);
  border: 1px solid rgba(244,96,12,0.4);
  border-radius: 8px;
  padding: 1rem 1.4rem;
  margin-top: 0.5rem;
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
}
.contact-inner a {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  color: rgba(255,255,255,0.9);
  text-decoration: none;
  font-size: 0.85rem;
  font-weight: 500;
  font-family: 'Inter', sans-serif;
  transition: color 0.15s;
}
.contact-inner a:hover { color: var(--orange); }
.contact-inner a .icon {
  font-size: 1rem;
  flex-shrink: 0;
}

/* ─────────────────────────────────────────
   ABOUT SECTION
───────────────────────────────────────── */
.about {
  background: #111;
  color: rgba(255,255,255,0.8);
  padding: 4rem 2rem;
  text-align: center;
}
.about h2 {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(1.8rem, 4vw, 2.8rem);
  color: #fff;
  letter-spacing: 0.06em;
  margin-bottom: 0.8rem;
}
.about h2 em { color: var(--orange); font-style: normal; }
.about p {
  max-width: 620px;
  margin: 0 auto;
  font-size: clamp(0.95rem, 1.8vw, 1.08rem);
  line-height: 1.8;
  color: rgba(255,255,255,0.65);
}
.gears { font-size: 1.5rem; letter-spacing: 0.5rem; margin: 1rem 0; opacity: 0.4; }

footer {
  background: #0a0a0a;
  padding: 1.2rem;
  text-align: center;
  font-size: 0.75rem;
  color: rgba(255,255,255,0.25);
  letter-spacing: 0.1em;
}
footer span { color: var(--orange); }

/* ─────────────────────────────────────────
   ANIMATIONS
───────────────────────────────────────── */
@keyframes fadeDown {
  from { opacity: 0; transform: translateY(-16px); }
  to   { opacity: 1; transform: translateY(0); }
}
@keyframes riseUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}
@keyframes fadeIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}

@media (max-width: 560px) {
  .cta-btn { width: 90vw; max-width: 320px; }
  .skills-panel { width: 90vw; max-width: 340px; }
  .contact-panel { width: 90vw; max-width: 320px; }
  .skills-list { grid-template-columns: 1fr; }
  .hero-title { font-size: 2rem; }
}
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation: none !important; transition: none !important; }
}
</style>
</head>
<body>

<!-- ═══════════════════════
     TOP BANNER
════════════════════════ -->
<div class="joke-banner">
  🔧 &nbsp; You came in for an oil change — and left with 3 more repairs you didn't know you needed &nbsp; 🔧
</div>

<!-- ═══════════════════════
     HERO
════════════════════════ -->
<section class="hero">

  <!--
    SVG SCENE
    ViewBox: 0 0 1400 900
    Layout zones:
      - Sky:      y 0–340  (blue gradient, clouds, sun)
      - Building: y 260–580 (flat-roof commercial garage)
      - Concrete: y 575–900 (forecourt / parking lot)

    Building matches Telle Tire reference:
      - Flat roof with parapet
      - Full-width illuminated sign panel across the top of the facade
      - 4 roll-up bay doors
      - Brick/panel cladding texture
      - Corner pilasters
      - Parking lot markings
  -->
  <svg class="hero-scene" viewBox="0 -180 1400 760" preserveAspectRatio="xMidYMin slice"
       xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
    <defs>
      <!-- Sky -->
      <linearGradient id="gSky" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%"   stop-color="#4E9EC2"/>
        <stop offset="55%"  stop-color="#87C3DC"/>
        <stop offset="100%" stop-color="#C8A858"/>
      </linearGradient>
      <!-- Asphalt / concrete lot -->
      <linearGradient id="gLot" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%"   stop-color="#7A7570"/>
        <stop offset="100%" stop-color="#5C5850"/>
      </linearGradient>
      <!-- Main building wall (light tan/cream like reference) -->
      <linearGradient id="gWall" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%"   stop-color="#E8E0CC"/>
        <stop offset="100%" stop-color="#D4CAB4"/>
      </linearGradient>
      <!-- Sign panel (dark navy / charcoal like reference) -->
      <linearGradient id="gSign" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%"   stop-color="#1C2030"/>
        <stop offset="100%" stop-color="#141820"/>
      </linearGradient>
      <!-- Parapet / roof cap -->
      <linearGradient id="gParapet" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%"   stop-color="#D8D0BC"/>
        <stop offset="100%" stop-color="#BCB4A0"/>
      </linearGradient>
      <!-- Bay door -->
      <linearGradient id="gDoor" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%"   stop-color="#484038"/>
        <stop offset="100%" stop-color="#302820"/>
      </linearGradient>
      <!-- Brick tint overlay -->
      <pattern id="pBrick" x="0" y="0" width="40" height="16" patternUnits="userSpaceOnUse">
        <rect width="40" height="16" fill="none"/>
        <rect x="0"  y="0"  width="19" height="7"  rx="0.5" fill="rgba(0,0,0,0.035)"/>
        <rect x="21" y="0"  width="19" height="7"  rx="0.5" fill="rgba(0,0,0,0.035)"/>
        <rect x="10" y="9"  width="19" height="7"  rx="0.5" fill="rgba(0,0,0,0.025)"/>
        <line x1="0" y1="8"  x2="40" y2="8"  stroke="rgba(0,0,0,0.04)" stroke-width="1"/>
        <line x1="0" y1="16" x2="40" y2="16" stroke="rgba(0,0,0,0.04)" stroke-width="1"/>
      </pattern>
      <!-- Concrete texture on lot -->
      <pattern id="pConcrete" x="0" y="0" width="200" height="200" patternUnits="userSpaceOnUse">
        <rect width="200" height="200" fill="none"/>
        <line x1="0" y1="100" x2="200" y2="100" stroke="rgba(0,0,0,0.08)" stroke-width="1.5"/>
        <line x1="100" y1="0" x2="100" y2="200" stroke="rgba(0,0,0,0.06)" stroke-width="1"/>
        <!-- subtle stain blobs -->
        <ellipse cx="55"  cy="55"  rx="28" ry="12" fill="rgba(0,0,0,0.04)"/>
        <ellipse cx="155" cy="155" rx="20" ry="9"  fill="rgba(0,0,0,0.03)"/>
      </pattern>
      <!-- Sun glow -->
      <radialGradient id="gSun" cx="50%" cy="50%" r="50%">
        <stop offset="0%"   stop-color="#FFF0A0" stop-opacity="1"/>
        <stop offset="40%"  stop-color="#FFDD60" stop-opacity="0.6"/>
        <stop offset="100%" stop-color="#FFB820" stop-opacity="0"/>
      </radialGradient>
      <!-- Soft shadow drop under building -->
      <linearGradient id="gBldgShadow" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%"   stop-color="rgba(0,0,0,0.22)"/>
        <stop offset="100%" stop-color="rgba(0,0,0,0)"/>
      </linearGradient>
      <!-- Cloud blur -->
      <filter id="fCloud"><feGaussianBlur stdDeviation="4"/></filter>
      <!-- Neon glow -->
      <filter id="fGlow" x="-20%" y="-20%" width="140%" height="140%">
        <feGaussianBlur stdDeviation="5" result="blur"/>
        <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
      </filter>
    </defs>

    <!-- ── SKY — extended to cover viewBox expansion ── -->
    <rect x="0" y="-200" width="1400" height="1100" fill="url(#gSky)"/>

    <!-- Sun -->
    <circle cx="185" cy="130" r="52" fill="url(#gSun)"/>
    <circle cx="185" cy="130" r="32" fill="#FFEE88" opacity="0.88"/>

    <!-- Distant hills / tree line -->
    <ellipse cx="0"    cy="295" rx="160" ry="50" fill="#4A7840" opacity="0.45"/>
    <ellipse cx="140"  cy="282" rx="120" ry="55" fill="#3E6C34" opacity="0.5"/>
    <ellipse cx="280"  cy="290" rx="100" ry="45" fill="#4A7840" opacity="0.4"/>
    <ellipse cx="1140" cy="288" rx="110" ry="48" fill="#3E6C34" opacity="0.45"/>
    <ellipse cx="1280" cy="280" rx="130" ry="52" fill="#4A7840" opacity="0.5"/>
    <ellipse cx="1400" cy="295" rx="120" ry="46" fill="#3E6C34" opacity="0.4"/>

    <!-- Clouds -->
    <g opacity="0.85" filter="url(#fCloud)">
      <ellipse cx="560" cy="102" rx="95" ry="28" fill="#FFFFFF"/>
      <ellipse cx="610" cy="86"  rx="65" ry="26" fill="#FFFFFF"/>
      <ellipse cx="510" cy="98"  rx="55" ry="22" fill="#FFFFFF"/>
    </g>
    <g opacity="0.72" filter="url(#fCloud)">
      <ellipse cx="1050" cy="88"  rx="80" ry="24" fill="#FFFFFF"/>
      <ellipse cx="1100" cy="74"  rx="58" ry="22" fill="#FFFFFF"/>
      <ellipse cx="1005" cy="86"  rx="48" ry="20" fill="#FFFFFF"/>
    </g>
    <g opacity="0.55" filter="url(#fCloud)">
      <ellipse cx="820" cy="120" rx="60" ry="20" fill="#FFFFFF"/>
      <ellipse cx="865" cy="108" rx="45" ry="18" fill="#FFFFFF"/>
    </g>

    <!-- ════════════════════════════════════════
         BUILDING — flat-roof commercial style
         matching Telle Tire reference:
         • Full width across scene
         • Tall sign panel at top of facade
         • Cream/tan masonry walls below sign
         • 4 roll-up bay doors
         • Corner pilasters
         • Parapet (flush flat roof edge)
    ═══════════════════════════════════════════ -->

    <!-- Ground shadow cast by building -->
    <rect x="50" y="568" width="1300" height="30" rx="4" fill="rgba(0,0,0,0.18)"/>

    <!-- ── Rear/upper parapet (raised center section, like reference) ── -->
    <!-- Left wing parapet -->
    <rect x="50"  y="278" width="300" height="295" fill="url(#gWall)"/>
    <rect x="50"  y="278" width="300" height="295" fill="url(#pBrick)" opacity="0.6"/>
    <!-- Right wing parapet -->
    <rect x="1050" y="278" width="300" height="295" fill="url(#gWall)"/>
    <rect x="1050" y="278" width="300" height="295" fill="url(#pBrick)" opacity="0.6"/>
    <!-- Center body -->
    <rect x="330" y="295" width="740" height="278" fill="url(#gWall)"/>
    <rect x="330" y="295" width="740" height="278" fill="url(#pBrick)" opacity="0.55"/>

    <!-- ── Parapet cap (flat roof edge) ── -->
    <!-- Center raised parapet -->
    <rect x="295" y="250" width="810" height="52" fill="url(#gParapet)"/>
    <rect x="295" y="246" width="810" height="10" fill="#C8C0A8"/>
    <!-- Wing parapet caps -->
    <rect x="45"  y="268" width="312" height="18" fill="#C0B8A0"/>
    <rect x="1043" y="268" width="312" height="18" fill="#C0B8A0"/>

    <!-- ── SIGN PANEL (full width, dark, prominent — like reference) ── -->
    <!-- Sign runs across the full facade just below parapet -->
    <rect x="48"  y="286" width="304" height="80" fill="url(#gSign)"/>
    <rect x="293" y="270" width="814" height="82" fill="url(#gSign)"/>
    <rect x="1048" y="286" width="304" height="80" fill="url(#gSign)"/>

    <!-- Sign border highlight -->
    <rect x="48"   y="286" width="304" height="2" fill="rgba(255,255,255,0.08)"/>
    <rect x="293"  y="270" width="814" height="2" fill="rgba(255,255,255,0.1)"/>
    <rect x="1048" y="286" width="304" height="2" fill="rgba(255,255,255,0.08)"/>

    <!-- Orange accent stripe at sign bottom -->
    <rect x="48"   y="362" width="304" height="5" fill="#F4600C" opacity="0.9"/>
    <rect x="293"  y="348" width="814" height="5" fill="#F4600C" opacity="0.9"/>
    <rect x="1048" y="362" width="304" height="5" fill="#F4600C" opacity="0.9"/>

    <!-- SIGN TEXT — center main sign -->
    <!-- Gear icons flanking text -->
    <g transform="translate(340,311)" filter="url(#fGlow)">
      <circle r="18" fill="none" stroke="#F4600C" stroke-width="3.5"/>
      <circle r="8" fill="#F4600C"/>
      <g stroke="#F4600C" stroke-width="3.5">
        <line x1="0" y1="-24" x2="0" y2="-18"/><line x1="0" y1="18" x2="0" y2="24"/>
        <line x1="-24" y1="0" x2="-18" y2="0"/><line x1="18" y1="0" x2="24" y2="0"/>
        <line x1="-17" y1="-17" x2="-12.7" y2="-12.7"/>
        <line x1="12.7" y1="12.7" x2="17" y2="17"/>
        <line x1="17" y1="-17" x2="12.7" y2="-12.7"/>
        <line x1="-12.7" y1="12.7" x2="-17" y2="17"/>
      </g>
    </g>
    <g transform="translate(1060,311)" filter="url(#fGlow)">
      <circle r="18" fill="none" stroke="#F4600C" stroke-width="3.5"/>
      <circle r="8" fill="#F4600C"/>
      <g stroke="#F4600C" stroke-width="3.5">
        <line x1="0" y1="-24" x2="0" y2="-18"/><line x1="0" y1="18" x2="0" y2="24"/>
        <line x1="-24" y1="0" x2="-18" y2="0"/><line x1="18" y1="0" x2="24" y2="0"/>
        <line x1="-17" y1="-17" x2="-12.7" y2="-12.7"/>
        <line x1="12.7" y1="12.7" x2="17" y2="17"/>
        <line x1="17" y1="-17" x2="12.7" y2="-12.7"/>
        <line x1="-12.7" y1="12.7" x2="-17" y2="17"/>
      </g>
    </g>
    <!-- Main name -->
    <text x="700" y="324" text-anchor="middle"
          font-family="'Bebas Neue',sans-serif" font-size="48"
          fill="#FFCC44" letter-spacing="10" filter="url(#fGlow)">THE GEAR HOG</text>
    <text x="700" y="345" text-anchor="middle"
          font-family="'Bebas Neue',sans-serif" font-size="14"
          fill="#F4600C" letter-spacing="7" opacity="0.9">AUTO REPAIR  ·  FULL SERVICE  ·  COMPLETE SOLUTIONS</text>

    <!-- Wing sign left: OPEN -->
    <text x="200" y="322" text-anchor="middle"
          font-family="'Bebas Neue',sans-serif" font-size="28"
          fill="#44DD88" letter-spacing="6" filter="url(#fGlow)">OPEN</text>
    <text x="200" y="340" text-anchor="middle"
          font-family="'Bebas Neue',sans-serif" font-size="11"
          fill="rgba(255,255,255,0.5)" letter-spacing="4">MON – SAT  8AM–6PM</text>

    <!-- Wing sign right: phone/tagline -->
    <text x="1200" y="318" text-anchor="middle"
          font-family="'Bebas Neue',sans-serif" font-size="20"
          fill="rgba(255,255,255,0.75)" letter-spacing="4">CALL US</text>
    <text x="1200" y="340" text-anchor="middle"
          font-family="'Bebas Neue',sans-serif" font-size="22"
          fill="#FFCC44" letter-spacing="3">+31 20 123 4567</text>

    <!-- ── PILASTERS / COLUMNS at corners and between doors ── -->
    <!-- Outer left -->
    <rect x="50"  y="366" width="28" height="210" fill="#CABFAA"/>
    <rect x="50"  y="364" width="28" height="6"   fill="#B8AD98"/>
    <!-- Between bay 1 and 2 -->
    <rect x="384" y="366" width="28" height="210" fill="#CABFAA"/>
    <!-- Between bay 2 and 3 -->
    <rect x="688" y="366" width="24" height="210" fill="#CABFAA"/>
    <!-- Between bay 3 and 4 -->
    <rect x="988" y="366" width="28" height="210" fill="#CABFAA"/>
    <!-- Outer right -->
    <rect x="1322" y="366" width="28" height="210" fill="#CABFAA"/>

    <!-- ── 4 BAY DOORS (roll-up style matching reference) ── -->
    <!-- Door 1 -->
    <rect x="80"  y="380" width="298" height="196" fill="url(#gDoor)" rx="1"/>
    <!-- Door 1 horizontal panel lines -->
    <g stroke="#242018" stroke-width="2.2">
      <line x1="80"  y1="412" x2="378" y2="412"/>
      <line x1="80"  y1="444" x2="378" y2="444"/>
      <line x1="80"  y1="476" x2="378" y2="476"/>
      <line x1="80"  y1="508" x2="378" y2="508"/>
      <line x1="80"  y1="540" x2="378" y2="540"/>
    </g>
    <line x1="229" y1="380" x2="229" y2="576" stroke="#242018" stroke-width="2"/>
    <!-- Door 1 handle -->
    <rect x="214" y="474" width="30" height="8" rx="4" fill="#88807A"/>
    <!-- Door 1 frame -->
    <rect x="78"  y="378" width="302" height="200" fill="none" stroke="#888070" stroke-width="5" rx="1"/>

    <!-- Door 2 -->
    <rect x="414" y="380" width="268" height="196" fill="url(#gDoor)" rx="1"/>
    <g stroke="#242018" stroke-width="2.2">
      <line x1="414" y1="412" x2="682" y2="412"/>
      <line x1="414" y1="444" x2="682" y2="444"/>
      <line x1="414" y1="476" x2="682" y2="476"/>
      <line x1="414" y1="508" x2="682" y2="508"/>
      <line x1="414" y1="540" x2="682" y2="540"/>
    </g>
    <line x1="548" y1="380" x2="548" y2="576" stroke="#242018" stroke-width="2"/>
    <rect x="533" y="474" width="30" height="8" rx="4" fill="#88807A"/>
    <rect x="412" y="378" width="272" height="200" fill="none" stroke="#888070" stroke-width="5" rx="1"/>

    <!-- Door 3 -->
    <rect x="714" y="380" width="268" height="196" fill="url(#gDoor)" rx="1"/>
    <g stroke="#242018" stroke-width="2.2">
      <line x1="714" y1="412" x2="982" y2="412"/>
      <line x1="714" y1="444" x2="982" y2="444"/>
      <line x1="714" y1="476" x2="982" y2="476"/>
      <line x1="714" y1="508" x2="982" y2="508"/>
      <line x1="714" y1="540" x2="982" y2="540"/>
    </g>
    <line x1="848" y1="380" x2="848" y2="576" stroke="#242018" stroke-width="2"/>
    <rect x="833" y="474" width="30" height="8" rx="4" fill="#88807A"/>
    <rect x="712" y="378" width="272" height="200" fill="none" stroke="#888070" stroke-width="5" rx="1"/>

    <!-- Door 4 -->
    <rect x="1018" y="380" width="298" height="196" fill="url(#gDoor)" rx="1"/>
    <g stroke="#242018" stroke-width="2.2">
      <line x1="1018" y1="412" x2="1316" y2="412"/>
      <line x1="1018" y1="444" x2="1316" y2="444"/>
      <line x1="1018" y1="476" x2="1316" y2="476"/>
      <line x1="1018" y1="508" x2="1316" y2="508"/>
      <line x1="1018" y1="540" x2="1316" y2="540"/>
    </g>
    <line x1="1167" y1="380" x2="1167" y2="576" stroke="#242018" stroke-width="2"/>
    <rect x="1152" y="474" width="30" height="8" rx="4" fill="#88807A"/>
    <rect x="1016" y="378" width="302" height="200" fill="none" stroke="#888070" stroke-width="5" rx="1"/>

    <!-- Door light strip at top (warm interior glow visible under door) -->
    <rect x="80"  y="575" width="298" height="4" fill="rgba(255,220,100,0.22)" rx="1"/>
    <rect x="414" y="575" width="268" height="4" fill="rgba(255,220,100,0.18)" rx="1"/>
    <rect x="714" y="575" width="268" height="4" fill="rgba(255,220,100,0.20)" rx="1"/>
    <rect x="1018" y="575" width="298" height="4" fill="rgba(255,220,100,0.15)" rx="1"/>

    <!-- ── BUILDING BASE FASCIA ── -->
    <rect x="48" y="572" width="1304" height="12" fill="#888070"/>
    <rect x="48" y="572" width="1304" height="5"  fill="rgba(0,0,0,0.25)"/>

    <!-- ── AWNING / CANOPY over doors ── -->
    <rect x="50"  y="368" width="1300" height="18" fill="#B8B0A0"/>
    <rect x="50"  y="368" width="1300" height="5"  fill="rgba(255,255,255,0.12)"/>
    <rect x="50"  y="382" width="1300" height="3"  fill="rgba(0,0,0,0.2)"/>

    <!-- ── PROPS ── -->

    <!-- Utility box / AC unit on roof (adds realism) -->
    <rect x="400" y="244" width="55" height="22" rx="2" fill="#A8A090"/>
    <rect x="400" y="240" width="55" height="6"  rx="1" fill="#C0B8A0"/>
    <rect x="920" y="244" width="55" height="22" rx="2" fill="#A8A090"/>
    <rect x="920" y="240" width="55" height="6"  rx="1" fill="#C0B8A0"/>

    <!-- Lamp post left -->
    <rect x="42" y="290" width="8"  height="285" rx="2" fill="#5C5850"/>
    <rect x="18" y="285" width="56" height="10"  rx="2" fill="#4A4640"/>
    <ellipse cx="46" cy="282" rx="28" ry="12" fill="#3A3630"/>
    <rect x="30" y="284" width="32" height="10" rx="2" fill="rgba(255,240,180,0.5)"/>

    <!-- Lamp post right -->
    <rect x="1350" y="290" width="8"  height="285" rx="2" fill="#5C5850"/>
    <rect x="1326" y="285" width="56" height="10"  rx="2" fill="#4A4640"/>
    <ellipse cx="1354" cy="282" rx="28" ry="12" fill="#3A3630"/>
    <rect x="1338" y="284" width="32" height="10" rx="2" fill="rgba(255,240,180,0.5)"/>

    <!-- ── FORECOURT / PARKING LOT ── -->
    <rect x="0" y="584" width="1400" height="316" fill="url(#gLot)"/>
    <rect x="0" y="584" width="1400" height="316" fill="url(#pConcrete)"/>

    <!-- Parking lane markings -->
    <g stroke="#E8D840" stroke-width="3" stroke-dasharray="50 25" opacity="0.55">
      <line x1="0"   y1="598" x2="1400" y2="598"/>
    </g>

    <!-- White parking bays -->
    <g stroke="rgba(255,255,255,0.18)" stroke-width="2.5" fill="none">
      <line x1="175"  y1="600" x2="175"  y2="700"/>
      <line x1="345"  y1="600" x2="345"  y2="700"/>
      <line x1="515"  y1="600" x2="515"  y2="700"/>
      <line x1="685"  y1="600" x2="685"  y2="700"/>
      <line x1="855"  y1="600" x2="855"  y2="700"/>
      <line x1="1025" y1="600" x2="1025" y2="700"/>
      <line x1="1195" y1="600" x2="1195" y2="700"/>
    </g>

    <!-- Oil stain near bay 2 -->
    <ellipse cx="548" cy="594" rx="55" ry="10" fill="rgba(20,16,10,0.22)"/>

    <!-- ── CARS ── -->
    <!-- Beaten-up customer car (left bay area) — the before -->
    <g transform="translate(85,608)">
      <!-- Shadow -->
      <ellipse cx="105" cy="78" rx="108" ry="12" fill="rgba(0,0,0,0.22)"/>
      <!-- Body -->
      <rect x="10" y="32" width="190" height="44" rx="10" fill="#7A6855"/>
      <!-- Cabin -->
      <path d="M35,32 Q46,6 78,5 L148,5 Q172,6 178,32 Z" fill="#695A48"/>
      <!-- Windows -->
      <path d="M46,31 Q54,10 78,9 L105,9 Q118,10 122,31 Z" fill="#88B8D0" opacity="0.65"/>
      <path d="M125,31 Q132,10 150,9 L170,10 Q178,14 176,31 Z" fill="#88B8D0" opacity="0.65"/>
      <!-- Wheels -->
      <circle cx="42"  cy="76" r="24" fill="#181610"/>
      <circle cx="42"  cy="76" r="14" fill="#3C3A34"/>
      <circle cx="42"  cy="76" r="6"  fill="#5C5A50"/>
      <circle cx="168" cy="76" r="24" fill="#181610"/>
      <circle cx="168" cy="76" r="14" fill="#3C3A34"/>
      <circle cx="168" cy="76" r="6"  fill="#5C5A50"/>
      <!-- Headlight -->
      <ellipse cx="199" cy="50" rx="6" ry="5" fill="#FFFACC" opacity="0.5"/>
      <!-- Rust patches -->
      <ellipse cx="70"  cy="46" rx="14" ry="7" fill="rgba(100,60,28,0.45)"/>
      <ellipse cx="140" cy="54" rx="8"  ry="5" fill="rgba(100,60,28,0.3)"/>
      <!-- "CUSTOMER" label under car... subtle -->
    </g>

    <!-- Gleaming sports car (right, the upsell — the after) -->
    <g transform="translate(1080,610)">
      <!-- Shadow -->
      <ellipse cx="105" cy="76" rx="112" ry="11" fill="rgba(0,0,0,0.22)"/>
      <!-- Body low-slung -->
      <rect x="8" y="28" width="202" height="42" rx="12" fill="#F4600C"/>
      <!-- Front splitter -->
      <rect x="180" y="62" width="28" height="5" rx="2" fill="#CC4008"/>
      <!-- Rear spoiler -->
      <rect x="2"  y="22" width="10" height="18" rx="2" fill="#D84008"/>
      <rect x="0"  y="20" width="20" height="6"  rx="2" fill="#D84008"/>
      <!-- Cabin -->
      <path d="M38,28 Q54,-2 90,-4 L148,-4 Q172,-2 185,28 Z" fill="#D84008"/>
      <!-- Windows -->
      <path d="M50,27 Q62,2 90,0 L118,0 Q133,2 138,27 Z" fill="#88B8D0" opacity="0.72"/>
      <path d="M140,27 Q149,5 165,3 L180,5 Q187,11 184,27 Z" fill="#88B8D0" opacity="0.72"/>
      <!-- Wheels -->
      <circle cx="42"  cy="70" r="26" fill="#181610"/>
      <circle cx="42"  cy="70" r="16" fill="#3C3A34"/>
      <circle cx="42"  cy="70" r="7"  fill="#C8C0B0"/>
      <circle cx="168" cy="70" r="26" fill="#181610"/>
      <circle cx="168" cy="70" r="16" fill="#3C3A34"/>
      <circle cx="168" cy="70" r="7"  fill="#C8C0B0"/>
      <!-- Brake caliper -->
      <path d="M30,60 A16,16 0 0 1 55,60" stroke="#E02020" stroke-width="5" fill="none"/>
      <path d="M156,60 A16,16 0 0 1 181,60" stroke="#E02020" stroke-width="5" fill="none"/>
      <!-- Headlights -->
      <ellipse cx="198" cy="38" rx="8" ry="5" fill="#FFFAAA" opacity="0.9"/>
      <ellipse cx="200" cy="47" rx="5" ry="3" fill="#FFC040" opacity="0.75"/>
      <!-- Racing stripe -->
      <rect x="8" y="37" width="202" height="6" rx="0" fill="rgba(255,255,255,0.18)"/>
    </g>

    <!-- Tire stack beside building right -->
    <g transform="translate(1350,530)">
      <ellipse cx="24" cy="12" rx="24" ry="9"  fill="#282420"/>
      <rect x="0"  y="12" width="48" height="60" fill="#282420" rx="3"/>
      <ellipse cx="24" cy="12" rx="24" ry="9"  fill="#343028"/>
      <ellipse cx="24" cy="72" rx="24" ry="9"  fill="#1E1C18"/>
      <ellipse cx="24" cy="12" rx="14" ry="5"  fill="#1A1816"/>
      <ellipse cx="24" cy="45" rx="14" ry="5"  fill="#1A1816" opacity="0.6"/>
    </g>

    <!-- Oil drums left of building -->
    <g transform="translate(14,516)">
      <ellipse cx="21" cy="76" rx="21" ry="8"  fill="#221C18"/>
      <rect x="0" y="18" width="42" height="60" rx="6" fill="#C04018"/>
      <ellipse cx="21" cy="18" rx="21" ry="8"  fill="#D85020"/>
      <rect x="0" y="18" width="42" height="9"  rx="6" fill="#E05828"/>
      <rect x="0" y="65" width="42" height="9"  rx="0" fill="#E05828"/>
      <text x="21" y="52" text-anchor="middle" font-size="9" fill="white" font-weight="700" font-family="sans-serif">OIL</text>
    </g>
    <g transform="translate(55,530)">
      <ellipse cx="21" cy="64" rx="21" ry="8"  fill="#221C18"/>
      <rect x="0" y="18" width="42" height="48" rx="6" fill="#C04018"/>
      <ellipse cx="21" cy="18" rx="21" ry="8"  fill="#D85020"/>
      <rect x="0" y="18" width="42" height="9"  rx="6" fill="#E05828"/>
    </g>

  </svg>
  <!-- end SVG -->

  <!-- ── Hero text (sky zone) ── -->
  <div class="hero-text">
    <h1 class="hero-title">Welcome to My<br/><em>GearHog</em> Garage</h1>
    <p class="hero-sub">You came in for a quick fix.<br/>Let's talk about your future.</p>
  </div>

</section>

<!-- ═══════════════════════
     CONCRETE + CTAs
════════════════════════ -->
<div class="concrete">
  <div class="cta-zone">

    <!-- CTA 1: CV Download -->
    <a class="cta-btn"
       href="https://drive.google.com/file/d/1o1n4bqWZVxgMXwJTlYdS0jLiQ9wRsWTw/view?usp=sharing"
       target="_blank" rel="noopener">
      🛢 Change Oil
      <span class="sub">Download CV</span>
    </a>

    <!-- CTA 2: Skills dropdown -->
    <div class="skills-wrap">
      <button class="cta-btn" id="skillsBtn" onclick="toggleSkills()" aria-expanded="false">
        🔧 Quick Inspection
        <span class="sub">Skills Overview</span>
      </button>
      <div class="skills-panel" id="skillsPanel" aria-hidden="true">
        <div class="skills-inner">
          <h3>⚙ Under the Hood</h3>
          <ul class="skills-list">
            <li>End-to-End Technical &amp; Consultative Sales</li>
            <li>Account Expansion &amp; Cross-Selling</li>
            <li>Executive Relationship Building</li>
            <li>Multi-Threading &amp; Stakeholder Mgmt</li>
            <li>Cross-Functional Collaboration</li>
            <li>Digital Experience Analytics</li>
            <li>Product Analytics</li>
            <li>Customer-Centric Problem Solving</li>
            <li>Ownership Mentality</li>
            <li>Adaptability &amp; Fast Learning</li>
            <li>Salesforce &amp; Outreach</li>
            <li>Zendesk, Clari &amp; Dust</li>
          </ul>
        </div>
      </div>
    </div>

    <!-- CTA 3: Just Tires contact dropdown -->
    <div class="contact-wrap">
      <button class="cta-btn" id="contactBtn" onclick="toggleContact()" aria-expanded="false">
        🛞 Just Tires
        <span class="sub">Contact Me</span>
      </button>
      <div class="contact-panel" id="contactPanel" aria-hidden="true">
        <div class="contact-inner">
          <a href="mailto:zrinka.garcia@gmail.com">
            <span class="icon">✉️</span>
            zrinka.garcia@gmail.com
          </a>
          <a href="tel:+385993306600">
            <span class="icon">📞</span>
            +385 99 330 6600
          </a>
        </div>
      </div>
    </div>

  </div>
</div>

<!-- ═══════════════════════
     ABOUT STRIP
════════════════════════ -->
<section class="about">
  <h2>You Came In For An <em>Oil Change.</em></h2>
  <div class="gears">⚙ ⚙ ⚙</div>
  <p>
    You know how it goes. A customer walks in for a tire rotation and
    leaves with a full performance package — and they're <em style="color:var(--orange)">thrilled</em> about it.
    That's not upselling. That's listening, diagnosing, and delivering
    the right solution at the right moment.
    <br/><br/>
    That's what I do in sales. Every single time.
  </p>
</section>

<footer>
  <span>THE GEAR HOG</span> &nbsp;·&nbsp; All diagnostics are free. Solutions are priceless.
</footer>

<script>
  function toggleSkills() {
    const panel = document.getElementById('skillsPanel');
    const btn   = document.getElementById('skillsBtn');
    const open  = panel.classList.toggle('open');
    btn.setAttribute('aria-expanded', String(open));
    panel.setAttribute('aria-hidden',  String(!open));
  }
  function toggleContact() {
    const panel = document.getElementById('contactPanel');
    const btn   = document.getElementById('contactBtn');
    const open  = panel.classList.toggle('open');
    btn.setAttribute('aria-expanded', String(open));
    panel.setAttribute('aria-hidden',  String(!open));
  }
</script>
</body>
</html>
