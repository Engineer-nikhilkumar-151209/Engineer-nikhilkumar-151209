<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NIKHIL KUMAR // KAI LABORATORIES</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600;700;900&family=IBM+Plex+Mono:wght@300;400;500&family=Rajdhani:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --kai-green: #00ff88;
    --kai-green-dim: #00cc6a;
    --kai-cyan: #00eeff;
    --kai-red: #ff003c;
    --bg-void: #010508;
    --bg-deep: #050d10;
    --bg-panel: rgba(0,255,136,0.04);
    --border: rgba(0,255,136,0.2);
    --text-main: #c8ffe8;
    --text-dim: #5a8a70;
    --glow: 0 0 20px rgba(0,255,136,0.4), 0 0 60px rgba(0,255,136,0.1);
    --glow-strong: 0 0 30px rgba(0,255,136,0.8), 0 0 80px rgba(0,255,136,0.3);
  }

  *, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg-void);
    color: var(--text-main);
    font-family: 'IBM Plex Mono', monospace;
    cursor: none;
    overflow-x: hidden;
  }

  /* ── CUSTOM CURSOR ── */
  #cursor {
    position: fixed;
    width: 12px; height: 12px;
    background: var(--kai-green);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    transform: translate(-50%,-50%);
    transition: transform 0.1s, width 0.2s, height 0.2s, opacity 0.2s;
    box-shadow: var(--glow-strong);
    mix-blend-mode: screen;
  }
  #cursor-ring {
    position: fixed;
    width: 36px; height: 36px;
    border: 1px solid rgba(0,255,136,0.5);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9998;
    transform: translate(-50%,-50%);
    transition: transform 0.15s ease, width 0.3s, height 0.3s;
  }

  /* ── SCANLINES OVERLAY ── */
  body::before {
    content: '';
    position: fixed; inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.08) 2px,
      rgba(0,0,0,0.08) 4px
    );
    pointer-events: none;
    z-index: 1000;
  }

  /* ── NOISE TEXTURE ── */
  body::after {
    content: '';
    position: fixed; inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 999;
    opacity: 0.4;
  }

  /* ── PARTICLE CANVAS ── */
  #particle-canvas {
    position: fixed; inset: 0;
    z-index: 0;
    opacity: 0.6;
  }

  /* ── NAV ── */
  nav {
    position: fixed; top: 0; left: 0; right: 0;
    z-index: 500;
    padding: 16px 40px;
    display: flex; align-items: center; justify-content: space-between;
    background: linear-gradient(180deg, rgba(1,5,8,0.95) 0%, transparent 100%);
    border-bottom: 1px solid rgba(0,255,136,0.08);
    backdrop-filter: blur(10px);
  }
  .nav-logo {
    font-family: 'Orbitron', monospace;
    font-size: 13px;
    font-weight: 900;
    color: var(--kai-green);
    letter-spacing: 4px;
    text-shadow: var(--glow);
  }
  .nav-logo span { color: var(--text-dim); }
  .nav-links { display: flex; gap: 32px; }
  .nav-links a {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    color: var(--text-dim);
    text-decoration: none;
    letter-spacing: 2px;
    text-transform: uppercase;
    transition: color 0.3s, text-shadow 0.3s;
    position: relative;
  }
  .nav-links a::after {
    content: '';
    position: absolute; bottom: -4px; left: 0;
    width: 0; height: 1px;
    background: var(--kai-green);
    transition: width 0.3s;
    box-shadow: var(--glow);
  }
  .nav-links a:hover { color: var(--kai-green); text-shadow: var(--glow); }
  .nav-links a:hover::after { width: 100%; }

  /* ── HERO ── */
  #hero {
    position: relative;
    min-height: 100vh;
    display: flex; align-items: center; justify-content: center;
    z-index: 1;
    overflow: hidden;
  }

  .hero-grid {
    position: absolute; inset: 0;
    background-image:
      linear-gradient(rgba(0,255,136,0.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,255,136,0.04) 1px, transparent 1px);
    background-size: 60px 60px;
    mask-image: radial-gradient(ellipse 70% 70% at 50% 50%, black, transparent);
  }

  .hero-content {
    text-align: center;
    position: relative;
    z-index: 2;
  }

  .hero-label {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    letter-spacing: 6px;
    color: var(--kai-green);
    text-transform: uppercase;
    margin-bottom: 24px;
    opacity: 0;
    animation: fadeUp 0.8s 0.2s forwards;
  }

  .hero-name {
    font-family: 'Orbitron', monospace;
    font-size: clamp(36px, 7vw, 88px);
    font-weight: 900;
    line-height: 1;
    letter-spacing: -1px;
    color: #fff;
    text-shadow: 0 0 40px rgba(0,255,136,0.3);
    opacity: 0;
    animation: fadeUp 0.8s 0.4s forwards;
    position: relative;
  }
  .hero-name .green { color: var(--kai-green); text-shadow: var(--glow-strong); }

  .hero-title {
    font-family: 'Rajdhani', sans-serif;
    font-size: clamp(16px, 2.5vw, 26px);
    font-weight: 300;
    color: var(--text-dim);
    letter-spacing: 8px;
    text-transform: uppercase;
    margin-top: 16px;
    opacity: 0;
    animation: fadeUp 0.8s 0.6s forwards;
  }

  .hero-tagline {
    font-size: 13px;
    color: rgba(0,255,136,0.6);
    margin-top: 32px;
    letter-spacing: 2px;
    opacity: 0;
    animation: fadeUp 0.8s 0.8s forwards;
  }

  .hero-tagline .blink {
    animation: blink 1s infinite;
  }

  .hero-cta {
    margin-top: 48px;
    display: flex; gap: 16px; justify-content: center;
    opacity: 0;
    animation: fadeUp 0.8s 1s forwards;
  }

  .btn {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    letter-spacing: 3px;
    text-transform: uppercase;
    padding: 14px 32px;
    border: 1px solid var(--kai-green);
    background: transparent;
    color: var(--kai-green);
    text-decoration: none;
    cursor: none;
    position: relative;
    overflow: hidden;
    transition: color 0.3s;
  }
  .btn::before {
    content: '';
    position: absolute; inset: 0;
    background: var(--kai-green);
    transform: translateX(-100%);
    transition: transform 0.3s;
    z-index: -1;
  }
  .btn:hover { color: #000; }
  .btn:hover::before { transform: translateX(0); }
  .btn-ghost {
    border-color: rgba(0,255,136,0.3);
    color: var(--text-dim);
  }
  .btn-ghost:hover { color: #000; }
  .btn-ghost::before { background: rgba(0,255,136,0.6); }

  .hero-scroll {
    position: absolute; bottom: 40px; left: 50%;
    transform: translateX(-50%);
    display: flex; flex-direction: column; align-items: center; gap: 8px;
    opacity: 0;
    animation: fadeUp 0.8s 1.4s forwards;
  }
  .scroll-text { font-size: 9px; letter-spacing: 4px; color: var(--text-dim); }
  .scroll-line {
    width: 1px; height: 60px;
    background: linear-gradient(var(--kai-green), transparent);
    animation: scrollPulse 2s infinite;
  }

  /* ── GLITCH EFFECT ── */
  .glitch {
    position: relative;
  }
  .glitch::before, .glitch::after {
    content: attr(data-text);
    position: absolute; top: 0; left: 0;
    width: 100%; height: 100%;
  }
  .glitch::before {
    color: var(--kai-cyan);
    animation: glitch1 4s infinite;
    clip-path: polygon(0 0, 100% 0, 100% 35%, 0 35%);
  }
  .glitch::after {
    color: var(--kai-red);
    animation: glitch2 4s infinite;
    clip-path: polygon(0 65%, 100% 65%, 100% 100%, 0 100%);
  }

  /* ── SECTIONS ── */
  section {
    position: relative;
    z-index: 1;
    padding: 120px 40px;
    max-width: 1200px;
    margin: 0 auto;
  }

  .section-label {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 10px;
    letter-spacing: 6px;
    color: var(--kai-green);
    text-transform: uppercase;
    margin-bottom: 12px;
    display: flex; align-items: center; gap: 16px;
  }
  .section-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, var(--border), transparent);
    max-width: 200px;
  }

  .section-title {
    font-family: 'Orbitron', monospace;
    font-size: clamp(28px, 4vw, 52px);
    font-weight: 700;
    color: #fff;
    margin-bottom: 60px;
    line-height: 1.1;
  }
  .section-title .accent { color: var(--kai-green); }

  /* ── ABOUT ── */
  #about { border-top: 1px solid var(--border); }
  .about-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 80px;
    align-items: center;
  }
  .about-text p {
    font-size: 14px;
    line-height: 1.9;
    color: var(--text-dim);
    margin-bottom: 20px;
  }
  .about-text p strong { color: var(--kai-green); font-weight: 500; }

  .about-stats {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2px;
  }
  .stat-card {
    background: var(--bg-panel);
    border: 1px solid var(--border);
    padding: 28px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.3s, background 0.3s;
  }
  .stat-card::before {
    content: '';
    position: absolute; top: 0; left: 0;
    width: 3px; height: 0;
    background: var(--kai-green);
    transition: height 0.4s;
    box-shadow: var(--glow);
  }
  .stat-card:hover { border-color: rgba(0,255,136,0.4); background: rgba(0,255,136,0.06); }
  .stat-card:hover::before { height: 100%; }
  .stat-num {
    font-family: 'Orbitron', monospace;
    font-size: 36px;
    font-weight: 900;
    color: var(--kai-green);
    line-height: 1;
    text-shadow: var(--glow);
  }
  .stat-label { font-size: 10px; letter-spacing: 3px; color: var(--text-dim); margin-top: 8px; text-transform: uppercase; }

  /* ── SKILLS ── */
  #skills { border-top: 1px solid var(--border); }
  .skill-bars { display: flex; flex-direction: column; gap: 24px; }
  .skill-item { position: relative; }
  .skill-header {
    display: flex; justify-content: space-between;
    margin-bottom: 10px;
  }
  .skill-name { font-size: 12px; letter-spacing: 2px; color: var(--text-main); text-transform: uppercase; }
  .skill-pct { font-family: 'Orbitron', monospace; font-size: 12px; color: var(--kai-green); }
  .skill-track {
    height: 3px;
    background: rgba(0,255,136,0.1);
    position: relative;
    overflow: hidden;
  }
  .skill-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--kai-green-dim), var(--kai-green));
    width: 0;
    transition: width 1.2s cubic-bezier(0.16, 1, 0.3, 1);
    box-shadow: var(--glow);
    position: relative;
  }
  .skill-fill::after {
    content: '';
    position: absolute; right: 0; top: -3px;
    width: 8px; height: 8px;
    background: var(--kai-green);
    border-radius: 50%;
    box-shadow: var(--glow-strong);
  }

  .tech-grid {
    display: flex; flex-wrap: wrap; gap: 10px;
    margin-top: 48px;
  }
  .tech-tag {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    padding: 8px 16px;
    border: 1px solid var(--border);
    color: var(--text-dim);
    letter-spacing: 1px;
    transition: all 0.3s;
    cursor: none;
  }
  .tech-tag:hover {
    border-color: var(--kai-green);
    color: var(--kai-green);
    background: rgba(0,255,136,0.05);
    box-shadow: var(--glow);
    transform: translateY(-2px);
  }

  /* ── PROJECTS ── */
  #projects { border-top: 1px solid var(--border); }
  .projects-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(340px, 1fr)); gap: 2px; }
  .project-card {
    background: var(--bg-panel);
    border: 1px solid var(--border);
    padding: 36px;
    position: relative;
    overflow: hidden;
    transition: transform 0.3s, border-color 0.3s, background 0.3s;
    cursor: none;
  }
  .project-card::before {
    content: '';
    position: absolute; inset: 0;
    background: radial-gradient(ellipse 80% 60% at 50% 0%, rgba(0,255,136,0.06), transparent);
    opacity: 0;
    transition: opacity 0.4s;
  }
  .project-card:hover { transform: translateY(-4px); border-color: rgba(0,255,136,0.4); background: rgba(0,255,136,0.04); }
  .project-card:hover::before { opacity: 1; }
  .project-num {
    font-family: 'Orbitron', monospace;
    font-size: 11px;
    color: rgba(0,255,136,0.3);
    letter-spacing: 4px;
    margin-bottom: 20px;
  }
  .project-icon {
    font-size: 32px;
    margin-bottom: 16px;
    display: block;
  }
  .project-name {
    font-family: 'Orbitron', monospace;
    font-size: 18px;
    font-weight: 700;
    color: #fff;
    margin-bottom: 12px;
  }
  .project-desc { font-size: 13px; line-height: 1.8; color: var(--text-dim); margin-bottom: 24px; }
  .project-tags { display: flex; flex-wrap: wrap; gap: 8px; }
  .project-tag {
    font-size: 10px;
    padding: 4px 10px;
    border: 1px solid rgba(0,255,136,0.2);
    color: rgba(0,255,136,0.6);
    letter-spacing: 1px;
  }
  .project-arrow {
    position: absolute; bottom: 28px; right: 28px;
    width: 32px; height: 32px;
    border: 1px solid var(--border);
    display: flex; align-items: center; justify-content: center;
    font-size: 14px;
    color: var(--text-dim);
    transition: all 0.3s;
  }
  .project-card:hover .project-arrow {
    border-color: var(--kai-green);
    color: var(--kai-green);
    box-shadow: var(--glow);
    transform: translate(2px, -2px);
  }

  /* ── CONTACT ── */
  #contact {
    border-top: 1px solid var(--border);
    text-align: center;
    padding-bottom: 160px;
  }
  .contact-tagline { font-size: 14px; color: var(--text-dim); margin-bottom: 48px; letter-spacing: 1px; line-height: 1.8; }
  .contact-links { display: flex; justify-content: center; gap: 24px; flex-wrap: wrap; }
  .contact-link {
    display: flex; align-items: center; gap: 12px;
    padding: 16px 28px;
    border: 1px solid var(--border);
    color: var(--text-dim);
    text-decoration: none;
    font-size: 12px;
    letter-spacing: 2px;
    transition: all 0.3s;
    cursor: none;
  }
  .contact-link:hover {
    border-color: var(--kai-green);
    color: var(--kai-green);
    background: rgba(0,255,136,0.05);
    box-shadow: var(--glow);
  }
  .contact-link svg { width: 18px; height: 18px; fill: currentColor; }

  /* ── FOOTER ── */
  footer {
    position: relative; z-index: 1;
    border-top: 1px solid var(--border);
    padding: 32px 40px;
    display: flex; justify-content: space-between; align-items: center;
    font-size: 10px;
    letter-spacing: 2px;
    color: var(--text-dim);
    text-transform: uppercase;
  }
  .footer-logo { font-family: 'Orbitron', monospace; color: var(--kai-green); font-size: 11px; text-shadow: var(--glow); }

  /* ── SCROLL REVEAL ── */
  .reveal {
    opacity: 0;
    transform: translateY(32px);
    transition: opacity 0.8s cubic-bezier(0.16,1,0.3,1), transform 0.8s cubic-bezier(0.16,1,0.3,1);
  }
  .reveal.visible { opacity: 1; transform: translateY(0); }

  /* ── TERMINAL WIDGET ── */
  .terminal {
    background: rgba(0,0,0,0.6);
    border: 1px solid rgba(0,255,136,0.2);
    padding: 20px;
    font-size: 12px;
    line-height: 2;
    position: relative;
    margin-top: 48px;
  }
  .terminal-bar {
    position: absolute; top: 0; left: 0; right: 0;
    height: 32px;
    background: rgba(0,255,136,0.06);
    border-bottom: 1px solid rgba(0,255,136,0.1);
    display: flex; align-items: center; padding: 0 12px; gap: 8px;
  }
  .dot { width: 10px; height: 10px; border-radius: 50%; }
  .dot-r { background: #ff5f57; }
  .dot-y { background: #ffbd2e; }
  .dot-g { background: #28c840; }
  .terminal-body { margin-top: 28px; }
  .t-line { color: var(--text-dim); }
  .t-line .prompt { color: var(--kai-green); }
  .t-line .cmd { color: #fff; }
  .t-line .out { color: rgba(0,255,136,0.5); }

  /* ── ANIMATIONS ── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }
  @keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0; } }
  @keyframes scrollPulse {
    0%, 100% { opacity: 0.3; transform: scaleY(1); }
    50% { opacity: 1; transform: scaleY(1.1); }
  }
  @keyframes glitch1 {
    0%, 90%, 100% { transform: none; opacity: 0; }
    92% { transform: translate(-2px, 0); opacity: 0.8; }
    96% { transform: translate(2px, 0); opacity: 0.8; }
  }
  @keyframes glitch2 {
    0%, 90%, 100% { transform: none; opacity: 0; }
    93% { transform: translate(2px, 0); opacity: 0.8; }
    97% { transform: translate(-2px, 0); opacity: 0.8; }
  }
  @keyframes float {
    0%, 100% { transform: translateY(0px); }
    50% { transform: translateY(-12px); }
  }
  @keyframes spin { to { transform: rotate(360deg); } }
  @keyframes scanH {
    0% { top: -2px; } 100% { top: 100%; }
  }

  /* ── HEXAGON BADGE ── */
  .hex-badge {
    display: inline-flex;
    align-items: center; justify-content: center;
    width: 80px; height: 92px;
    background: var(--bg-panel);
    clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
    border: none;
    position: relative;
    animation: float 4s ease-in-out infinite;
  }
  .hex-badge span { font-family: 'Orbitron', monospace; font-size: 22px; color: var(--kai-green); text-shadow: var(--glow); }

  /* ── SCAN LINE ── */
  .scan-container {
    position: relative;
    overflow: hidden;
  }
  .scan-container::after {
    content: '';
    position: absolute; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, transparent, var(--kai-green), transparent);
    animation: scanH 3s linear infinite;
    opacity: 0.4;
  }

  /* ── RESPONSIVE ── */
  @media (max-width: 768px) {
    nav { padding: 16px 20px; }
    .nav-links { display: none; }
    section { padding: 80px 20px; }
    .about-grid { grid-template-columns: 1fr; gap: 40px; }
    .projects-grid { grid-template-columns: 1fr; }
    footer { flex-direction: column; gap: 12px; text-align: center; }
  }
</style>
</head>
<body>

<canvas id="particle-canvas"></canvas>
<div id="cursor"></div>
<div id="cursor-ring"></div>

<!-- NAV -->
<nav>
  <div class="nav-logo">KAI<span> // </span>LAB</div>
  <div class="nav-links">
    <a href="#about">About</a>
    <a href="#skills">Skills</a>
    <a href="#projects">Projects</a>
    <a href="#contact">Contact</a>
  </div>
</nav>

<!-- HERO -->
<div id="hero">
  <div class="hero-grid"></div>
  <div class="hero-content">
    <div class="hero-label">// KAI Laboratories — Portfolio v2.0</div>
    <h1 class="hero-name glitch" data-text="NIKHIL KUMAR">
      <span class="green">NIKHIL</span> KUMAR
    </h1>
    <div class="hero-title">Dhavaleshwarapu</div>
    <div class="hero-tagline">
      Engineer &nbsp;·&nbsp; Quantum Tech &nbsp;·&nbsp; AI Developer &nbsp;·&nbsp; Builder
      <span class="blink">_</span>
    </div>
    <div class="hero-cta">
      <a href="#projects" class="btn">View Projects</a>
      <a href="#contact" class="btn btn-ghost">Get In Touch</a>
    </div>
  </div>
  <div class="hero-scroll">
    <span class="scroll-text">SCROLL</span>
    <div class="scroll-line"></div>
  </div>
</div>

<!-- ABOUT -->
<section id="about">
  <div class="section-label reveal">// 01 &nbsp; About</div>
  <h2 class="section-title reveal">Who I <span class="accent">Am</span></h2>
  <div class="about-grid">
    <div class="about-text reveal">
      <p>I'm an engineer from <strong>Vizianagaram, Andhra Pradesh, India</strong> — building at the intersection of emerging technology and real-world impact.</p>
      <p>My work spans <strong>AI-powered web apps</strong>, <strong>hardware prototyping</strong>, <strong>peer-to-peer systems</strong>, and assistive technology. Every project under the <strong>KAI Laboratories</strong> brand pushes boundaries — cyberpunk aesthetics meet serious engineering.</p>
      <p>Deeply passionate about <strong>quantum computing</strong> and the next frontier of AI development. Currently building <strong>MagicDrop.io</strong> — a zero-setup P2P file transfer tool that lives entirely in your browser.</p>
      <div class="terminal scan-container" style="margin-top:32px;">
        <div class="terminal-bar">
          <div class="dot dot-r"></div>
          <div class="dot dot-y"></div>
          <div class="dot dot-g"></div>
        </div>
        <div class="terminal-body">
          <div class="t-line"><span class="prompt">nikhil@kai:~$</span> <span class="cmd">whoami</span></div>
          <div class="t-line"><span class="out">> Engineer · Builder · Quantum Enthusiast</span></div>
          <div class="t-line"><span class="prompt">nikhil@kai:~$</span> <span class="cmd">cat location.txt</span></div>
          <div class="t-line"><span class="out">> Vizianagaram, Andhra Pradesh, India 🇮🇳</span></div>
          <div class="t-line"><span class="prompt">nikhil@kai:~$</span> <span class="cmd">status</span></div>
          <div class="t-line"><span class="out">> [ BUILDING ] MagicDrop.io &nbsp;<span class="blink" style="color:var(--kai-green)">_</span></span></div>
        </div>
      </div>
    </div>
    <div class="about-stats reveal">
      <div class="stat-card">
        <div class="stat-num" data-count="3">0</div>
        <div class="stat-label">Live Projects</div>
      </div>
      <div class="stat-card">
        <div class="stat-num" data-count="21">0</div>
        <div class="stat-label">Followers</div>
      </div>
      <div class="stat-card">
        <div class="stat-num" data-count="4">0</div>
        <div class="stat-label">Starred Repos</div>
      </div>
      <div class="stat-card">
        <div class="stat-num" data-count="∞" style="font-size:28px;">∞</div>
        <div class="stat-label">Lines of Ambition</div>
      </div>
    </div>
  </div>
</section>

<!-- SKILLS -->
<section id="skills">
  <div class="section-label reveal">// 02 &nbsp; Skills</div>
  <h2 class="section-title reveal">Tech <span class="accent">Arsenal</span></h2>
  <div style="display:grid;grid-template-columns:1fr 1fr;gap:80px;" class="reveal">
    <div class="skill-bars">
      <div class="skill-item">
        <div class="skill-header"><span class="skill-name">Quantum Computing</span><span class="skill-pct">80%</span></div>
        <div class="skill-track"><div class="skill-fill" data-width="80"></div></div>
      </div>
      <div class="skill-item">
        <div class="skill-header"><span class="skill-name">AI / Claude API</span><span class="skill-pct">75%</span></div>
        <div class="skill-track"><div class="skill-fill" data-width="75"></div></div>
      </div>
      <div class="skill-item">
        <div class="skill-header"><span class="skill-name">Web Engineering</span><span class="skill-pct">85%</span></div>
        <div class="skill-track"><div class="skill-fill" data-width="85"></div></div>
      </div>
      <div class="skill-item">
        <div class="skill-header"><span class="skill-name">Embedded Systems</span><span class="skill-pct">65%</span></div>
        <div class="skill-track"><div class="skill-fill" data-width="65"></div></div>
      </div>
      <div class="skill-item">
        <div class="skill-header"><span class="skill-name">P2P / WebRTC</span><span class="skill-pct">70%</span></div>
        <div class="skill-track"><div class="skill-fill" data-width="70"></div></div>
      </div>
    </div>
    <div>
      <p style="font-size:13px;color:var(--text-dim);line-height:1.9;margin-bottom:32px;">From quantum circuits to circuits on a breadboard — here's the full stack I work with:</p>
      <div class="tech-grid">
        <span class="tech-tag">HTML5</span>
        <span class="tech-tag">CSS3</span>
        <span class="tech-tag">JavaScript</span>
        <span class="tech-tag">WebRTC</span>
        <span class="tech-tag">PeerJS</span>
        <span class="tech-tag">Arduino</span>
        <span class="tech-tag">C++</span>
        <span class="tech-tag">Python</span>
        <span class="tech-tag">Claude API</span>
        <span class="tech-tag">GitHub Pages</span>
        <span class="tech-tag">HC-SR04</span>
        <span class="tech-tag">Bluetooth HC-05</span>
        <span class="tech-tag">MIT App Inventor</span>
        <span class="tech-tag">SVG Animation</span>
        <span class="tech-tag">Quantum SDK</span>
      </div>
    </div>
  </div>
</section>

<!-- PROJECTS -->
<section id="projects">
  <div class="section-label reveal">// 03 &nbsp; Projects</div>
  <h2 class="section-title reveal">Featured <span class="accent">Work</span></h2>
  <div class="projects-grid">

    <div class="project-card reveal">
      <div class="project-num">01 ——</div>
      <span class="project-icon">🪄</span>
      <div class="project-name">MagicDrop.io</div>
      <p class="project-desc">Zero-setup browser-to-browser P2P file & message transfer. No servers. No sign-up. Just share a code and drop files — powered by WebRTC via PeerJS. Deployed as a single HTML file on GitHub Pages under the KAI Laboratories brand.</p>
      <div class="project-tags">
        <span class="project-tag">WebRTC</span>
        <span class="project-tag">PeerJS</span>
        <span class="project-tag">P2P</span>
        <span class="project-tag">Cyberpunk UI</span>
        <span class="project-tag">GitHub Pages</span>
      </div>
      <div class="project-arrow">↗</div>
    </div>

    <div class="project-card reveal">
      <div class="project-num">02 ——</div>
      <span class="project-icon">🌞</span>
      <div class="project-name">Project Helios</div>
      <p class="project-desc">AI-powered content detection web app with a cyberpunk aesthetic. Features an animated SVG sun logo, user-supplied API key panel (solving CORS for GitHub Pages), and Claude API integration for real-time AI text detection.</p>
      <div class="project-tags">
        <span class="project-tag">Claude API</span>
        <span class="project-tag">AI Detection</span>
        <span class="project-tag">SVG Animation</span>
        <span class="project-tag">Vanilla JS</span>
      </div>
      <div class="project-arrow">↗</div>
    </div>

    <div class="project-card reveal">
      <div class="project-num">03 ——</div>
      <span class="project-icon">🦯</span>
      <div class="project-name">Smart Blind Stick</div>
      <p class="project-desc">Arduino-based assistive navigation device using HC-SR04 ultrasonic sensor for obstacle detection and HC-05 Bluetooth to relay audio alerts to a paired Android phone via a custom MIT App Inventor companion app.</p>
      <div class="project-tags">
        <span class="project-tag">Arduino</span>
        <span class="project-tag">HC-SR04</span>
        <span class="project-tag">Bluetooth</span>
        <span class="project-tag">MIT App Inventor</span>
        <span class="project-tag">Assistive Tech</span>
      </div>
      <div class="project-arrow">↗</div>
    </div>

    <div class="project-card reveal">
      <div class="project-num">04 ——</div>
      <span class="project-icon">💬</span>
      <div class="project-name">WABlast</div>
      <p class="project-desc">WhatsApp bulk messaging tool using Excel/CSV input and the wa.me/ URL scheme. Supports personalized messages with variable delay between sends, loaded with pre-configured contacts. Built for real deployment use.</p>
      <div class="project-tags">
        <span class="project-tag">JavaScript</span>
        <span class="project-tag">CSV Parsing</span>
        <span class="project-tag">WhatsApp API</span>
        <span class="project-tag">Automation</span>
      </div>
      <div class="project-arrow">↗</div>
    </div>

    <div class="project-card reveal">
      <div class="project-num">05 ——</div>
      <span class="project-icon">🏛️</span>
      <div class="project-name">SEEDAP Placement Pages</div>
      <p class="project-desc">Official government placement drive web pages for SEEDAP — Andhra Pradesh's employment & enterprise development organization. Features responsive design, recruitment registration flows, and both dark and light theme variants.</p>
      <div class="project-tags">
        <span class="project-tag">HTML/CSS</span>
        <span class="project-tag">Government</span>
        <span class="project-tag">SEEDAP</span>
        <span class="project-tag">Responsive</span>
      </div>
      <div class="project-arrow">↗</div>
    </div>

    <div class="project-card reveal" style="display:flex;flex-direction:column;justify-content:center;align-items:center;text-align:center;border-style:dashed;opacity:0.5;">
      <span style="font-size:40px;margin-bottom:16px;">⚛️</span>
      <div class="project-name" style="font-size:14px;">Next: Quantum Project</div>
      <p class="project-desc" style="font-size:12px;">Something in the quantum realm is brewing... Stay tuned.</p>
    </div>

  </div>
</section>

<!-- CONTACT -->
<section id="contact">
  <div class="section-label reveal" style="justify-content:center;">// 04 &nbsp; Contact</div>
  <h2 class="section-title reveal" style="text-align:center;">Let's <span class="accent">Connect</span></h2>
  <p class="contact-tagline reveal">Building something interesting? Want to collaborate on AI, quantum,<br>hardware, or web tools? Let's talk.</p>
  <div class="contact-links reveal">
    <a href="https://github.com/Engineer-nikhilkumar-151209" class="contact-link" target="_blank">
      <svg viewBox="0 0 24 24"><path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z"/></svg>
      GitHub
    </a>
    <a href="https://www.instagram.com/thedhavaleswarapu" class="contact-link" target="_blank">
      <svg viewBox="0 0 24 24"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.881 1.44 1.44 0 000-2.881z"/></svg>
      Instagram
    </a>
    <a href="https://github.com/Engineer-nikhilkumar-151209?tab=repositories" class="contact-link" target="_blank">
      <svg viewBox="0 0 24 24"><path d="M3 3h18v18H3V3zm16 16V5H5v14h14zM7 7h4v4H7V7zm0 6h10v2H7v-2zm0-3h10v2H7v-2zm6-3h4v4h-4V7z"/></svg>
      Repositories
    </a>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="footer-logo">KAI LABORATORIES</div>
  <div>© 2026 Nikhil Kumar Dhavaleshwarapu — Vizianagaram, AP, India</div>
  <div style="color:var(--kai-green);font-family:'Orbitron',monospace;font-size:10px;">SYSTEM ONLINE <span class="blink">▮</span></div>
</footer>

<script>
// ── CURSOR ──
const cursor = document.getElementById('cursor');
const ring = document.getElementById('cursor-ring');
let mx = 0, my = 0, rx = 0, ry = 0;
document.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY; cursor.style.left = mx+'px'; cursor.style.top = my+'px'; });
(function animRing() {
  rx += (mx - rx) * 0.12;
  ry += (my - ry) * 0.12;
  ring.style.left = rx+'px'; ring.style.top = ry+'px';
  requestAnimationFrame(animRing);
})();
document.querySelectorAll('a,button,.project-card,.stat-card,.tech-tag,.contact-link').forEach(el => {
  el.addEventListener('mouseenter', () => { cursor.style.width='20px'; cursor.style.height='20px'; ring.style.width='60px'; ring.style.height='60px'; });
  el.addEventListener('mouseleave', () => { cursor.style.width='12px'; cursor.style.height='12px'; ring.style.width='36px'; ring.style.height='36px'; });
});

// ── PARTICLE CANVAS ──
const canvas = document.getElementById('particle-canvas');
const ctx = canvas.getContext('2d');
let W, H, particles = [];
function resize() { W = canvas.width = window.innerWidth; H = canvas.height = window.innerHeight; }
resize(); window.addEventListener('resize', resize);

class Particle {
  constructor() { this.reset(); }
  reset() {
    this.x = Math.random() * W; this.y = Math.random() * H;
    this.size = Math.random() * 1.5 + 0.3;
    this.vx = (Math.random() - 0.5) * 0.3; this.vy = (Math.random() - 0.5) * 0.3;
    this.alpha = Math.random() * 0.4 + 0.1;
    this.pulse = Math.random() * Math.PI * 2;
  }
  update() {
    this.x += this.vx; this.y += this.vy;
    this.pulse += 0.02;
    if (this.x < 0 || this.x > W || this.y < 0 || this.y > H) this.reset();
  }
  draw() {
    ctx.save();
    ctx.globalAlpha = this.alpha * (0.6 + 0.4 * Math.sin(this.pulse));
    ctx.fillStyle = '#00ff88';
    ctx.shadowBlur = 6; ctx.shadowColor = '#00ff88';
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
  }
}

for (let i = 0; i < 120; i++) particles.push(new Particle());

function drawLines() {
  for (let i = 0; i < particles.length; i++) {
    for (let j = i+1; j < particles.length; j++) {
      const dx = particles[i].x - particles[j].x, dy = particles[i].y - particles[j].y;
      const dist = Math.sqrt(dx*dx + dy*dy);
      if (dist < 120) {
        ctx.save();
        ctx.globalAlpha = (1 - dist/120) * 0.06;
        ctx.strokeStyle = '#00ff88'; ctx.lineWidth = 0.5;
        ctx.beginPath(); ctx.moveTo(particles[i].x, particles[i].y); ctx.lineTo(particles[j].x, particles[j].y); ctx.stroke();
        ctx.restore();
      }
    }
  }
}

function animParticles() {
  ctx.clearRect(0, 0, W, H);
  particles.forEach(p => { p.update(); p.draw(); });
  drawLines();
  requestAnimationFrame(animParticles);
}
animParticles();

// ── SCROLL REVEAL ──
const revealEls = document.querySelectorAll('.reveal');
const io = new IntersectionObserver(entries => {
  entries.forEach((e, i) => {
    if (e.isIntersecting) {
      e.target.style.transitionDelay = (i % 4) * 0.1 + 's';
      e.target.classList.add('visible');
    }
  });
}, { threshold: 0.15 });
revealEls.forEach(el => io.observe(el));

// ── SKILL BAR ANIMATION ──
const skillFills = document.querySelectorAll('.skill-fill');
const skillIO = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      setTimeout(() => { e.target.style.width = e.target.dataset.width + '%'; }, 300);
    }
  });
}, { threshold: 0.5 });
skillFills.forEach(el => skillIO.observe(el));

// ── COUNTER ANIMATION ──
function animCount(el, target, duration=1200) {
  if (isNaN(target)) return;
  let start = 0, step = target / (duration / 16);
  const timer = setInterval(() => {
    start = Math.min(start + step, target);
    el.textContent = Math.round(start);
    if (start >= target) clearInterval(timer);
  }, 16);
}
const countEls = document.querySelectorAll('.stat-num[data-count]');
const countIO = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      const val = e.target.dataset.count;
      animCount(e.target, parseInt(val));
      countIO.unobserve(e.target);
    }
  });
}, { threshold: 0.5 });
countEls.forEach(el => countIO.observe(el));

// ── GLITCH RETRIGGER ──
setInterval(() => {
  const h = document.querySelector('.hero-name');
  if (h) { h.style.animation = 'none'; void h.offsetWidth; h.style.animation = ''; }
}, 6000);
</script>
</body>
</html>
