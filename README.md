<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>HARSH RAVALIYA // SECURE_CORE</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;600;700&display=swap" rel="stylesheet" />
<style>
  :root {
    --lime: #bef264;
    --lime-dim: #86efac44;
    --bg: #000000;
    --bg2: #0a0a0a;
    --bg3: #111111;
    --border: #1a1a1a;
    --text: #e2e8f0;
    --muted: #64748b;
    --red: #ef4444;
    --cyan: #22d3ee;
    --glow: 0 0 20px #bef26455, 0 0 40px #bef26422;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Rajdhani', sans-serif;
    overflow-x: hidden;
    cursor: none;
  }

  /* Custom cursor */
  #cursor {
    position: fixed;
    width: 12px; height: 12px;
    background: var(--lime);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    transform: translate(-50%, -50%);
    transition: transform 0.05s;
    box-shadow: var(--glow);
  }
  #cursor-trail {
    position: fixed;
    width: 30px; height: 30px;
    border: 1px solid var(--lime);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9998;
    transform: translate(-50%, -50%);
    transition: all 0.15s ease;
    opacity: 0.5;
  }

  /* Scanline overlay */
  body::before {
    content: '';
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.03) 2px,
      rgba(0,0,0,0.03) 4px
    );
    pointer-events: none;
    z-index: 1000;
  }

  /* Noise texture */
  body::after {
    content: '';
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 999;
    opacity: 0.3;
  }

  /* Grid background */
  .grid-bg {
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background-image:
      linear-gradient(rgba(190,242,100,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(190,242,100,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  /* Nav */
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 500;
    background: rgba(0,0,0,0.85);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 14px 40px;
  }

  .nav-logo {
    font-family: 'Orbitron', monospace;
    font-size: 14px;
    font-weight: 900;
    color: var(--lime);
    letter-spacing: 3px;
    text-shadow: var(--glow);
  }

  .nav-links {
    display: flex;
    gap: 30px;
    list-style: none;
  }

  .nav-links a {
    font-family: 'Share Tech Mono', monospace;
    color: var(--muted);
    text-decoration: none;
    font-size: 12px;
    letter-spacing: 2px;
    transition: color 0.2s;
  }
  .nav-links a:hover { color: var(--lime); }

  .nav-status {
    display: flex;
    align-items: center;
    gap: 8px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--lime);
  }
  .status-dot {
    width: 7px; height: 7px;
    border-radius: 50%;
    background: var(--lime);
    animation: pulse 2s infinite;
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; box-shadow: 0 0 6px var(--lime); }
    50% { opacity: 0.4; box-shadow: none; }
  }

  /* Hero */
  #hero {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    position: relative;
    z-index: 1;
    padding: 100px 40px 60px;
    text-align: center;
  }

  .hero-banner {
    width: 100%;
    max-width: 900px;
    height: 220px;
    background: linear-gradient(135deg, #0a0a0a 0%, #111 50%, #0d1a0a 100%);
    border: 1px solid var(--border);
    border-radius: 12px;
    margin-bottom: 40px;
    position: relative;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .banner-city {
    position: absolute;
    bottom: 0;
    left: 0; right: 0;
    height: 140px;
    background: linear-gradient(to top, #0a120a, transparent);
  }

  /* Building silhouettes */
  .buildings {
    position: absolute;
    bottom: 0;
    left: 0; right: 0;
    display: flex;
    align-items: flex-end;
    gap: 3px;
    padding: 0 20px;
    opacity: 0.6;
  }
  .building {
    background: #111;
    border: 1px solid #1a2a1a;
    flex-shrink: 0;
    position: relative;
  }
  .building::before {
    content: '';
    position: absolute;
    top: 10px; left: 50%;
    width: 4px; height: 4px;
    background: var(--lime);
    border-radius: 50%;
    transform: translateX(-50%);
    animation: blink 3s infinite;
  }
  @keyframes blink {
    0%, 90%, 100% { opacity: 1; }
    95% { opacity: 0; }
  }

  .banner-title {
    font-family: 'Orbitron', monospace;
    font-size: clamp(28px, 5vw, 52px);
    font-weight: 900;
    color: var(--lime);
    text-shadow: var(--glow);
    letter-spacing: 4px;
    z-index: 2;
    position: relative;
  }

  /* Terminal typing effect */
  .terminal-line {
    font-family: 'Share Tech Mono', monospace;
    font-size: 16px;
    color: var(--lime);
    margin: 10px 0;
    position: relative;
  }

  .typed-text {
    font-family: 'Share Tech Mono', monospace;
    font-size: 18px;
    color: var(--lime);
    text-shadow: var(--glow);
    min-height: 28px;
  }
  .typed-cursor {
    display: inline-block;
    width: 10px;
    background: var(--lime);
    animation: blink-cursor 0.7s infinite;
    margin-left: 2px;
    vertical-align: bottom;
    height: 18px;
  }
  @keyframes blink-cursor {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }

  /* Badges row */
  .badges {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    justify-content: center;
    margin: 30px 0;
  }
  .badge {
    display: flex;
    align-items: center;
    gap: 8px;
    background: #000;
    border: 1px solid var(--lime);
    border-radius: 6px;
    padding: 8px 16px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    color: var(--lime);
    letter-spacing: 1px;
    text-decoration: none;
    transition: all 0.2s;
    cursor: pointer;
  }
  .badge:hover {
    background: var(--lime);
    color: #000;
    box-shadow: var(--glow);
    transform: translateY(-2px);
  }
  .badge svg { width: 16px; height: 16px; flex-shrink: 0; }

  .views-badge {
    background: #000;
    border: 1px solid #222;
    border-radius: 6px;
    padding: 8px 16px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    color: var(--muted);
    letter-spacing: 1px;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .views-badge span {
    background: var(--lime);
    color: #000;
    padding: 2px 8px;
    border-radius: 4px;
    font-weight: bold;
  }

  /* Divider */
  .divider {
    width: 100%;
    max-width: 900px;
    height: 2px;
    background: linear-gradient(90deg, transparent, var(--lime), transparent);
    margin: 40px auto;
    position: relative;
    z-index: 1;
  }

  /* Sections */
  section {
    max-width: 900px;
    margin: 0 auto;
    padding: 20px 40px;
    position: relative;
    z-index: 1;
  }

  .section-title {
    font-family: 'Orbitron', monospace;
    font-size: 18px;
    color: var(--lime);
    letter-spacing: 3px;
    margin-bottom: 30px;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .section-title::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, var(--lime), transparent);
    opacity: 0.3;
  }

  /* About Me - YAML style */
  .yaml-block {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-left: 3px solid var(--lime);
    border-radius: 8px;
    padding: 28px 32px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 14px;
    line-height: 2;
  }
  .yaml-key { color: var(--cyan); }
  .yaml-val { color: var(--text); }
  .yaml-arr { color: var(--lime); }
  .yaml-str { color: #f97316; }
  .yaml-comment { color: var(--muted); }

  /* Tech Stack */
  .tech-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 12px;
  }
  .tech-card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 14px 18px;
    display: flex;
    align-items: center;
    gap: 12px;
    transition: all 0.2s;
    cursor: default;
  }
  .tech-card:hover {
    border-color: var(--lime);
    background: #0a120a;
    transform: translateY(-2px);
    box-shadow: 0 4px 20px rgba(190,242,100,0.1);
  }
  .tech-icon {
    width: 36px; height: 36px;
    border-radius: 6px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
    background: #111;
    flex-shrink: 0;
  }
  .tech-name {
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    color: var(--text);
  }
  .tech-level {
    font-size: 11px;
    color: var(--muted);
    margin-top: 2px;
  }

  .tech-section-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 3px;
    margin: 24px 0 12px;
    text-transform: uppercase;
  }

  /* Security Arsenal */
  .arsenal {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-top: 8px;
  }
  .tool-tag {
    background: #0a0a0a;
    border: 1px solid #1e2e1e;
    color: var(--lime);
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    padding: 6px 14px;
    border-radius: 4px;
    letter-spacing: 1px;
    transition: all 0.2s;
    cursor: default;
  }
  .tool-tag:hover {
    background: var(--lime);
    color: #000;
    border-color: var(--lime);
  }

  /* GitHub Stats */
  .stats-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 16px;
  }
  @media (max-width: 640px) {
    .stats-grid { grid-template-columns: 1fr; }
  }

  .stat-card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 24px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.2s;
  }
  .stat-card:hover { border-color: var(--lime); }
  .stat-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, transparent, var(--lime), transparent);
    opacity: 0;
    transition: opacity 0.2s;
  }
  .stat-card:hover::before { opacity: 1; }

  .stat-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-bottom: 8px;
  }
  .stat-value {
    font-family: 'Orbitron', monospace;
    font-size: 32px;
    font-weight: 900;
    color: var(--lime);
    text-shadow: var(--glow);
  }
  .stat-sub {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--muted);
    margin-top: 6px;
  }

  /* Lang bar */
  .lang-bars { display: flex; flex-direction: column; gap: 10px; }
  .lang-row {
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .lang-name {
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    color: var(--text);
    width: 80px;
    flex-shrink: 0;
  }
  .lang-bar-bg {
    flex: 1;
    height: 6px;
    background: #111;
    border-radius: 3px;
    overflow: hidden;
  }
  .lang-bar-fill {
    height: 100%;
    border-radius: 3px;
    background: var(--lime);
    width: 0;
    transition: width 1.5s ease;
    box-shadow: 0 0 8px var(--lime);
  }
  .lang-pct {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--muted);
    width: 36px;
    text-align: right;
  }

  /* Contribution grid */
  .contrib-grid {
    display: grid;
    grid-template-columns: repeat(52, 1fr);
    gap: 3px;
    margin-top: 12px;
  }
  .contrib-week { display: flex; flex-direction: column; gap: 3px; }
  .contrib-day {
    width: 100%;
    aspect-ratio: 1;
    border-radius: 2px;
    background: #111;
    transition: all 0.2s;
    cursor: default;
    position: relative;
  }
  .contrib-day:hover { transform: scale(1.5); z-index: 10; }
  .contrib-day.l1 { background: #1a2e0a; }
  .contrib-day.l2 { background: #3d6b15; }
  .contrib-day.l3 { background: #7ab533; }
  .contrib-day.l4 { background: var(--lime); box-shadow: 0 0 4px var(--lime); }

  /* Currently table */
  .focus-table { width: 100%; border-collapse: collapse; }
  .focus-table td {
    padding: 14px 20px;
    border-bottom: 1px solid var(--border);
    font-family: 'Rajdhani', sans-serif;
    font-size: 16px;
  }
  .focus-table td:first-child {
    color: var(--lime);
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    white-space: nowrap;
    width: 200px;
  }
  .focus-table tr:hover td { background: #0a0a0a; }
  .focus-table tr:last-child td { border-bottom: none; }

  /* Terminal widget */
  .terminal {
    background: #050505;
    border: 1px solid var(--border);
    border-radius: 10px;
    overflow: hidden;
    font-family: 'Share Tech Mono', monospace;
  }
  .terminal-bar {
    background: #111;
    padding: 10px 16px;
    display: flex;
    align-items: center;
    gap: 8px;
    border-bottom: 1px solid var(--border);
  }
  .term-dot {
    width: 10px; height: 10px;
    border-radius: 50%;
  }
  .term-dot.red { background: #ef4444; }
  .term-dot.yellow { background: #eab308; }
  .term-dot.green { background: var(--lime); }
  .term-title {
    font-size: 11px;
    color: var(--muted);
    margin-left: 8px;
    letter-spacing: 1px;
  }
  .terminal-body {
    padding: 20px 24px;
    font-size: 13px;
    line-height: 1.9;
  }
  .term-prompt { color: var(--lime); }
  .term-cmd { color: var(--cyan); }
  .term-out { color: #94a3b8; }
  .term-success { color: var(--lime); }
  .term-input {
    background: transparent;
    border: none;
    outline: none;
    color: var(--cyan);
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    width: 300px;
    caret-color: var(--lime);
  }
  .term-output { margin-top: 4px; }

  /* Connect section */
  .connect-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
    gap: 14px;
  }
  .connect-card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 20px;
    text-decoration: none;
    display: flex;
    align-items: center;
    gap: 14px;
    transition: all 0.2s;
  }
  .connect-card:hover {
    border-color: var(--lime);
    background: #0a120a;
    transform: translateY(-3px);
    box-shadow: 0 8px 30px rgba(190,242,100,0.1);
  }
  .connect-icon {
    width: 42px; height: 42px;
    border-radius: 8px;
    background: #111;
    border: 1px solid #222;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 20px;
    flex-shrink: 0;
  }
  .connect-name {
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    color: var(--text);
  }
  .connect-handle {
    font-size: 11px;
    color: var(--muted);
    margin-top: 3px;
  }

  /* Quote */
  .quote-block {
    text-align: center;
    padding: 40px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 15px;
    color: var(--muted);
    font-style: italic;
  }
  .quote-block::before, .quote-block::after {
    content: '"';
    color: var(--lime);
    font-size: 24px;
    vertical-align: -4px;
  }

  /* Footer wave */
  .footer-wave {
    width: 100%;
    height: 80px;
    background: linear-gradient(180deg, var(--lime) 0%, transparent 100%);
    clip-path: polygon(0 100%, 100% 100%, 100% 30%, 80% 0%, 60% 40%, 40% 10%, 20% 50%, 0 20%);
    margin-top: 60px;
    opacity: 0.15;
  }

  /* Scroll animations */
  .fade-in {
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.6s ease, transform 0.6s ease;
  }
  .fade-in.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* Glitch effect on hover */
  .glitch {
    position: relative;
  }
  .glitch:hover::before, .glitch:hover::after {
    content: attr(data-text);
    position: absolute;
    top: 0; left: 0;
    width: 100%;
    height: 100%;
  }
  .glitch:hover::before {
    color: var(--cyan);
    clip-path: polygon(0 0, 100% 0, 100% 45%, 0 45%);
    transform: translateX(-2px);
    animation: glitch-anim 0.3s infinite;
  }
  .glitch:hover::after {
    color: var(--red);
    clip-path: polygon(0 55%, 100% 55%, 100% 100%, 0 100%);
    transform: translateX(2px);
    animation: glitch-anim2 0.3s infinite;
  }
  @keyframes glitch-anim {
    0% { transform: translateX(-2px); }
    50% { transform: translateX(2px); }
    100% { transform: translateX(-2px); }
  }
  @keyframes glitch-anim2 {
    0% { transform: translateX(2px); }
    50% { transform: translateX(-2px); }
    100% { transform: translateX(2px); }
  }

  /* Streak counter */
  .streak-display {
    text-align: center;
    padding: 20px;
  }
  .streak-num {
    font-family: 'Orbitron', monospace;
    font-size: 64px;
    font-weight: 900;
    color: var(--lime);
    text-shadow: var(--glow);
    display: block;
  }
  .streak-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    color: var(--muted);
    letter-spacing: 3px;
    margin-top: 8px;
  }

  /* Mobile */
  @media (max-width: 768px) {
    nav { padding: 12px 20px; }
    .nav-links { display: none; }
    section { padding: 20px; }
    .stats-grid { grid-template-columns: 1fr; }
    .contrib-grid { display: none; }
  }
</style>
</head>
<body>

<div id="cursor"></div>
<div id="cursor-trail"></div>
<div class="grid-bg"></div>

<!-- NAV -->
<nav>
  <div class="nav-logo">HARSH // RAVALIYA</div>
  <ul class="nav-links">
    <li><a href="#about">ABOUT</a></li>
    <li><a href="#stack">STACK</a></li>
    <li><a href="#stats">STATS</a></li>
    <li><a href="#terminal">TERMINAL</a></li>
    <li><a href="#connect">CONNECT</a></li>
  </ul>
  <div class="nav-status">
    <div class="status-dot"></div>
    SYSTEM ONLINE
  </div>
</nav>

<!-- HERO -->
<div id="hero">
  <!-- Banner with pixel city -->
  <div class="hero-banner">
    <div class="buildings" id="buildings"></div>
    <div class="banner-title glitch" data-text="HARSH RAVALIYA">HARSH RAVALIYA</div>
  </div>

  <!-- Typing animation -->
  <div class="typed-text">
    <span id="typed"></span><span class="typed-cursor"></span>
  </div>

  <!-- Badges -->
  <div class="badges">
    <a class="badge" href="https://harshsec.xyz" target="_blank">
      <svg viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z"/></svg>
      PORTFOLIO
    </a>
    <a class="badge" href="https://instagram.com/root_harsh" target="_blank">
      <svg viewBox="0 0 24 24" fill="currentColor"><path d="M7.8 2h8.4C19.4 2 22 4.6 22 7.8v8.4a5.8 5.8 0 0 1-5.8 5.8H7.8C4.6 22 2 19.4 2 16.2V7.8A5.8 5.8 0 0 1 7.8 2m-.2 2A3.6 3.6 0 0 0 4 7.6v8.8C4 18.39 5.61 20 7.6 20h8.8a3.6 3.6 0 0 0 3.6-3.6V7.6C20 5.61 18.39 4 16.4 4H7.6m9.65 1.5a1.25 1.25 0 0 1 1.25 1.25A1.25 1.25 0 0 1 17.25 8 1.25 1.25 0 0 1 16 6.75a1.25 1.25 0 0 1 1.25-1.25M12 7a5 5 0 0 1 5 5 5 5 0 0 1-5 5 5 5 0 0 1-5-5 5 5 0 0 1 5-5m0 2a3 3 0 0 0-3 3 3 3 0 0 0 3 3 3 3 0 0 0 3-3 3 3 0 0 0-3-3z"/></svg>
      @ROOT_HARSH
    </a>
    <a class="badge" href="https://github.com/harshravaliya" target="_blank">
      <svg viewBox="0 0 24 24" fill="currentColor"><path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/></svg>
      GITHUB
    </a>
    <div class="views-badge">
      PROFILE VIEWS <span id="view-count">95</span>
    </div>
  </div>
</div>

<div class="divider"></div>

<!-- ABOUT -->
<section id="about" class="fade-in">
  <div class="section-title">// ABOUT_ME</div>
  <div class="yaml-block">
    <div><span class="yaml-key">name        </span><span class="yaml-val">: </span><span class="yaml-str">Harsh Ravaliya</span></div>
    <div><span class="yaml-key">age         </span><span class="yaml-val">: </span><span class="yaml-arr">20</span></div>
    <div><span class="yaml-key">location    </span><span class="yaml-val">: </span><span class="yaml-str">India 🇮🇳</span></div>
    <div><span class="yaml-key">role        </span><span class="yaml-val">: </span><span class="yaml-str">Student &amp; Cybersecurity Enthusiast</span></div>
    <div><span class="yaml-key">interests   </span><span class="yaml-val">: </span><span class="yaml-arr">[</span><span class="yaml-str">Ethical Hacking</span><span class="yaml-val">, </span><span class="yaml-str">Web Dev</span><span class="yaml-val">, </span><span class="yaml-str">CTF Challenges</span><span class="yaml-arr">]</span></div>
    <div><span class="yaml-key">learning    </span><span class="yaml-val">: </span><span class="yaml-arr">[</span><span class="yaml-str">Python</span><span class="yaml-val">, </span><span class="yaml-str">Linux</span><span class="yaml-val">, </span><span class="yaml-str">Network Security</span><span class="yaml-val">, </span><span class="yaml-str">Bash</span><span class="yaml-arr">]</span></div>
    <div><span class="yaml-key">hobbies     </span><span class="yaml-val">: </span><span class="yaml-arr">[</span><span class="yaml-str">Guitar 🎸</span><span class="yaml-val">, </span><span class="yaml-str">Gaming 🎮</span><span class="yaml-val">, </span><span class="yaml-str">Music 🎧</span><span class="yaml-arr">]</span></div>
    <div><span class="yaml-key">website     </span><span class="yaml-val">: </span><span class="yaml-str">harshsec.xyz</span></div>
    <div><span class="yaml-key">status      </span><span class="yaml-val">: </span><span class="yaml-arr">"</span><span class="yaml-str">Always learning, always breaking things legally</span><span class="yaml-arr">"</span></div>
  </div>
</section>

<div class="divider"></div>

<!-- TECH STACK -->
<section id="stack" class="fade-in">
  <div class="section-title">// TECH_STACK</div>

  <div class="tech-section-label">— Languages &amp; Core</div>
  <div class="tech-grid">
    <div class="tech-card">
      <div class="tech-icon">🐍</div>
      <div><div class="tech-name">Python</div><div class="tech-level">Intermediate</div></div>
    </div>
    <div class="tech-card">
      <div class="tech-icon">💲</div>
      <div><div class="tech-name">Bash</div><div class="tech-level">Intermediate</div></div>
    </div>
    <div class="tech-card">
      <div class="tech-icon">🌐</div>
      <div><div class="tech-name">HTML / CSS</div><div class="tech-level">Proficient</div></div>
    </div>
    <div class="tech-card">
      <div class="tech-icon">⚡</div>
      <div><div class="tech-name">JavaScript</div><div class="tech-level">Learning</div></div>
    </div>
  </div>

  <div class="tech-section-label">— Tools &amp; Platforms</div>
  <div class="tech-grid">
    <div class="tech-card">
      <div class="tech-icon">🐧</div>
      <div><div class="tech-name">Linux / Kali</div><div class="tech-level">Daily Driver</div></div>
    </div>
    <div class="tech-card">
      <div class="tech-icon">🔧</div>
      <div><div class="tech-name">Git / GitHub</div><div class="tech-level">Proficient</div></div>
    </div>
    <div class="tech-card">
      <div class="tech-icon">💻</div>
      <div><div class="tech-name">VS Code</div><div class="tech-level">Daily Driver</div></div>
    </div>
    <div class="tech-card">
      <div class="tech-icon">▲</div>
      <div><div class="tech-name">Vercel</div><div class="tech-level">Beginner</div></div>
    </div>
  </div>

  <div class="tech-section-label">— Security Arsenal</div>
  <div class="arsenal">
    <div class="tool-tag">Nmap</div>
    <div class="tool-tag">Wireshark</div>
    <div class="tool-tag">Metasploit</div>
    <div class="tool-tag">Burp Suite</div>
    <div class="tool-tag">Aircrack-ng</div>
    <div class="tool-tag">Kali Linux</div>
    <div class="tool-tag">Netcat</div>
    <div class="tool-tag">SQLmap</div>
    <div class="tool-tag">Hydra</div>
    <div class="tool-tag">John the Ripper</div>
  </div>
</section>

<div class="divider"></div>

<!-- GITHUB STATS -->
<section id="stats" class="fade-in">
  <div class="section-title">// GITHUB_STATS</div>

  <div class="stats-grid">
    <div class="stat-card">
      <div class="stat-label">Total Commits</div>
      <div class="stat-value" data-target="127" id="commits">0</div>
      <div class="stat-sub">across all repositories</div>
    </div>
    <div class="stat-card">
      <div class="stat-label">Current Streak</div>
      <div class="stat-value" data-target="12" id="streak">0</div>
      <div class="stat-sub">days in a row 🔥</div>
    </div>
    <div class="stat-card">
      <div class="stat-label">Stars Earned</div>
      <div class="stat-value" data-target="23" id="stars">0</div>
      <div class="stat-sub">on public repos</div>
    </div>
    <div class="stat-card">
      <div class="stat-label">Repos</div>
      <div class="stat-value" data-target="18" id="repos">0</div>
      <div class="stat-sub">public projects</div>
    </div>
  </div>

  <!-- Top Languages -->
  <div class="stat-card" style="margin-bottom:16px">
    <div class="stat-label" style="margin-bottom:16px">Top Languages</div>
    <div class="lang-bars">
      <div class="lang-row">
        <div class="lang-name">Python</div>
        <div class="lang-bar-bg"><div class="lang-bar-fill" data-width="60%"></div></div>
        <div class="lang-pct">60%</div>
      </div>
      <div class="lang-row">
        <div class="lang-name">HTML</div>
        <div class="lang-bar-bg"><div class="lang-bar-fill" data-width="20%"></div></div>
        <div class="lang-pct">20%</div>
      </div>
      <div class="lang-row">
        <div class="lang-name">Bash</div>
        <div class="lang-bar-bg"><div class="lang-bar-fill" data-width="15%"></div></div>
        <div class="lang-pct">15%</div>
      </div>
      <div class="lang-row">
        <div class="lang-name">JavaScript</div>
        <div class="lang-bar-bg"><div class="lang-bar-fill" data-width="5%"></div></div>
        <div class="lang-pct">5%</div>
      </div>
    </div>
  </div>

  <!-- Contribution Grid -->
  <div class="stat-card">
    <div class="stat-label" style="margin-bottom:12px">Contribution Graph — 2024</div>
    <div class="contrib-grid" id="contrib-grid"></div>
  </div>
</section>

<div class="divider"></div>

<!-- CURRENTLY -->
<section class="fade-in">
  <div class="section-title">// CURRENTLY_FOCUSED</div>
  <div class="stat-card">
    <table class="focus-table">
      <tr>
        <td>🔐 SECURITY</td>
        <td>Learning penetration testing & CTF challenges</td>
      </tr>
      <tr>
        <td>🐍 PYTHON</td>
        <td>Building automation tools & security scripts</td>
      </tr>
      <tr>
        <td>🌐 WEB DEV</td>
        <td>Crafting personal projects & portfolio</td>
      </tr>
      <tr>
        <td>📡 NETWORKING</td>
        <td>Understanding protocols & packet analysis</td>
      </tr>
      <tr>
        <td>🎸 OFF-SCREEN</td>
        <td>Shredding guitar riffs</td>
      </tr>
    </table>
  </div>
</section>

<div class="divider"></div>

<!-- TERMINAL -->
<section id="terminal" class="fade-in">
  <div class="section-title">// INTERACTIVE_TERMINAL</div>
  <div class="terminal">
    <div class="terminal-bar">
      <div class="term-dot red"></div>
      <div class="term-dot yellow"></div>
      <div class="term-dot green"></div>
      <div class="term-title">harsh@kali:~$</div>
    </div>
    <div class="terminal-body" id="term-body">
      <div><span class="term-prompt">harsh@kali:~$ </span><span class="term-cmd">whoami</span></div>
      <div class="term-out">Harsh Ravaliya — Cybersecurity Student | Ethical Hacker in Training</div>
      <div style="margin-top:8px"><span class="term-prompt">harsh@kali:~$ </span><span class="term-cmd">cat /etc/status</span></div>
      <div class="term-success">✔ Always learning. Always breaking things legally.</div>
      <div style="margin-top:8px"><span class="term-prompt">harsh@kali:~$ </span>
        <input type="text" id="term-input" class="term-input" placeholder="type a command..." spellcheck="false" autocomplete="off" />
      </div>
      <div id="term-output" class="term-output"></div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- CONNECT -->
<section id="connect" class="fade-in">
  <div class="section-title">// CONNECT</div>
  <div class="connect-grid">
    <a class="connect-card" href="https://harshsec.xyz" target="_blank">
      <div class="connect-icon">🌐</div>
      <div>
        <div class="connect-name">Portfolio</div>
        <div class="connect-handle">harshsec.xyz</div>
      </div>
    </a>
    <a class="connect-card" href="https://instagram.com/root_harsh" target="_blank">
      <div class="connect-icon">📸</div>
      <div>
        <div class="connect-name">Instagram</div>
        <div class="connect-handle">@root_harsh</div>
      </div>
    </a>
    <a class="connect-card" href="https://github.com/harshravaliya" target="_blank">
      <div class="connect-icon">🐙</div>
      <div>
        <div class="connect-name">GitHub</div>
        <div class="connect-handle">@harshravaliya</div>
      </div>
    </a>
  </div>
</section>

<!-- QUOTE -->
<div class="quote-block fade-in">
  The quieter you become, the more you are able to hear.
  <div style="margin-top:12px; font-size:12px; color: #333; letter-spacing: 2px;">— Kali Linux motto</div>
</div>

<!-- Footer wave -->
<div class="footer-wave"></div>

<script>
// Cursor
const cursor = document.getElementById('cursor');
const trail = document.getElementById('cursor-trail');
document.addEventListener('mousemove', e => {
  cursor.style.left = e.clientX + 'px';
  cursor.style.top = e.clientY + 'px';
  setTimeout(() => {
    trail.style.left = e.clientX + 'px';
    trail.style.top = e.clientY + 'px';
  }, 80);
});

// Typing animation
const lines = [
  "Hey, I'm Harsh Ravaliya 👋",
  "Cybersecurity Student 🛡️",
  "Ethical Hacker in Training 🔐",
  "Guitarist & Gamer 🎸",
  "Always Learning, Never Stopping ✨"
];
let lineIdx = 0, charIdx = 0, deleting = false;
const typedEl = document.getElementById('typed');
function typeLoop() {
  const current = lines[lineIdx];
  if (!deleting) {
    typedEl.textContent = current.slice(0, ++charIdx);
    if (charIdx === current.length) { deleting = true; setTimeout(typeLoop, 1800); return; }
  } else {
    typedEl.textContent = current.slice(0, --charIdx);
    if (charIdx === 0) { deleting = false; lineIdx = (lineIdx + 1) % lines.length; }
  }
  setTimeout(typeLoop, deleting ? 40 : 70);
}
typeLoop();

// Pixel city buildings
const buildingContainer = document.getElementById('buildings');
const widths = [18,28,14,34,20,12,40,16,26,22,30,18,24,38,14,20,32,16,28,22,36,12,24,18,30,20,26,14,34,22];
const heights = [70,110,55,140,90,50,160,65,105,85,120,75,95,155,60,80,130,65,115,88,145,52,98,72,125,82,108,58,138,90];
widths.forEach((w, i) => {
  const b = document.createElement('div');
  b.className = 'building';
  b.style.cssText = `width:${w}px; height:${heights[i]}px; flex-shrink:0;`;
  // Add windows
  const winCols = Math.floor(w / 8);
  const winRows = Math.floor(heights[i] / 16);
  for (let r = 0; r < winRows; r++) {
    for (let c = 0; c < winCols; c++) {
      if (Math.random() > 0.4) {
        const win = document.createElement('div');
        win.style.cssText = `position:absolute; width:4px; height:4px; background:rgba(190,242,100,${Math.random()*0.6+0.1}); left:${c*8+3}px; top:${r*12+8}px; border-radius:1px;`;
        b.appendChild(win);
      }
    }
  }
  buildingContainer.appendChild(b);
});

// Contribution grid
const grid = document.getElementById('contrib-grid');
for (let w = 0; w < 52; w++) {
  const week = document.createElement('div');
  week.className = 'contrib-week';
  for (let d = 0; d < 7; d++) {
    const day = document.createElement('div');
    day.className = 'contrib-day';
    const r = Math.random();
    if (r > 0.85) day.classList.add('l4');
    else if (r > 0.65) day.classList.add('l3');
    else if (r > 0.45) day.classList.add('l2');
    else if (r > 0.3) day.classList.add('l1');
    week.appendChild(day);
  }
  grid.appendChild(week);
}

// Counter animation
function animateCount(el, target, duration = 1500) {
  let start = 0;
  const step = (timestamp) => {
    if (!start) start = timestamp;
    const progress = Math.min((timestamp - start) / duration, 1);
    el.textContent = Math.floor(progress * target);
    if (progress < 1) requestAnimationFrame(step);
    else el.textContent = target;
  };
  requestAnimationFrame(step);
}

// Language bars
function animateBars() {
  document.querySelectorAll('.lang-bar-fill').forEach(bar => {
    bar.style.width = bar.dataset.width;
  });
}

// Intersection Observer for fade-in + counters
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      // Animate counters in stats section
      if (entry.target.contains(document.getElementById('commits'))) {
        animateCount(document.getElementById('commits'), 127);
        animateCount(document.getElementById('streak'), 12);
        animateCount(document.getElementById('stars'), 23);
        animateCount(document.getElementById('repos'), 18);
        setTimeout(animateBars, 400);
      }
    }
  });
}, { threshold: 0.1 });

document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));

// Profile view count increment
let views = 95;
setInterval(() => {
  views++;
  document.getElementById('view-count').textContent = views;
}, 30000);

// Terminal commands
const commands = {
  whoami: 'Harsh Ravaliya — Cybersecurity Student | Ethical Hacker in Training',
  pwd: '/home/harsh',
  ls: 'projects/  tools/  ctf-writeups/  scripts/  notes/',
  'cat skills.txt': 'Python • Bash • Nmap • Wireshark • Burp Suite • Metasploit • Kali Linux',
  'cat interests.txt': '[Ethical Hacking, Web Dev, CTF Challenges, Guitar, Gaming]',
  uname: 'Linux kali 6.x.x-kali1-amd64 #1 SMP PREEMPT_DYNAMIC x86_64 GNU/Linux',
  date: new Date().toString(),
  help: 'Available: whoami, pwd, ls, cat skills.txt, cat interests.txt, uname, date, clear',
  clear: '__CLEAR__',
  ifconfig: 'eth0: 192.168.1.42  flags=4163  mtu 1500\nlo: 127.0.0.1  flags=73  mtu 65536',
  nmap: 'Starting Nmap 7.94... Target: harshsec.xyz\nOpen ports: 80/tcp (http), 443/tcp (https)',
  python3: 'Python 3.11.6 (main, Oct 3 2023, 18:10:42) on linux',
};

const input = document.getElementById('term-input');
const output = document.getElementById('term-output');

input.addEventListener('keydown', e => {
  if (e.key === 'Enter') {
    const cmd = input.value.trim().toLowerCase();
    input.value = '';
    if (!cmd) return;
    if (cmd === 'clear') { output.innerHTML = ''; return; }
    const res = commands[cmd] || `bash: ${cmd}: command not found. Type 'help' for available commands.`;
    const div = document.createElement('div');
    div.innerHTML = `<div style="margin-top:6px"><span class="term-prompt">harsh@kali:~$ </span><span class="term-cmd">${cmd}</span></div><div class="term-out">${res.replace(/\n/g, '<br>')}</div>`;
    output.appendChild(div);
    div.scrollIntoView({ behavior: 'smooth' });
  }
});
</script>
</body>
</html>
