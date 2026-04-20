<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Toto — Developer Profile</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;600;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #05070f;
    --bg2: #090d1a;
    --bg3: #0d1224;
    --purple: #7c3aed;
    --purple-light: #a78bfa;
    --blue: #3b82f6;
    --blue-light: #93c5fd;
    --cyan: #06b6d4;
    --text: #e2e8f0;
    --muted: #64748b;
    --border: rgba(124, 58, 237, 0.2);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Syne', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Canvas particules */
  #canvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    pointer-events: none;
    z-index: 0;
    opacity: 0.5;
  }

  /* Grille de fond */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(124,58,237,0.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba(124,58,237,0.04) 1px, transparent 1px);
    background-size: 40px 40px;
    z-index: 0;
    pointer-events: none;
  }

  .container {
    max-width: 860px;
    margin: 0 auto;
    padding: 60px 24px 100px;
    position: relative;
    z-index: 1;
  }

  /* ─── HERO ─── */
  .hero {
    text-align: center;
    margin-bottom: 80px;
    opacity: 0;
    animation: fadeUp 0.9s 0.2s ease forwards;
  }

  .avatar-ring {
    width: 96px; height: 96px;
    border-radius: 50%;
    margin: 0 auto 24px;
    background: linear-gradient(135deg, var(--purple), var(--blue), var(--cyan));
    padding: 3px;
    animation: spin-ring 6s linear infinite;
    position: relative;
  }

  @keyframes spin-ring {
    0% { filter: hue-rotate(0deg); }
    100% { filter: hue-rotate(360deg); }
  }

  .avatar-inner {
    width: 100%; height: 100%;
    border-radius: 50%;
    background: var(--bg);
    display: flex; align-items: center; justify-content: center;
    font-size: 36px;
  }

  .hero h1 {
    font-family: 'Syne', sans-serif;
    font-size: clamp(2.4rem, 6vw, 4rem);
    font-weight: 800;
    letter-spacing: -1px;
    background: linear-gradient(135deg, #fff 20%, var(--purple-light) 60%, var(--blue-light));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    line-height: 1.1;
    margin-bottom: 12px;
  }

  .hero-subtitle {
    font-family: 'Space Mono', monospace;
    font-size: 0.85rem;
    color: var(--muted);
    margin-bottom: 20px;
    letter-spacing: 0.05em;
  }

  .typing-wrap {
    font-family: 'Space Mono', monospace;
    font-size: 1rem;
    color: var(--purple-light);
    min-height: 1.4em;
    margin-bottom: 32px;
  }

  .cursor {
    display: inline-block;
    width: 2px; height: 1em;
    background: var(--cyan);
    margin-left: 2px;
    vertical-align: middle;
    animation: blink 1s step-end infinite;
  }

  @keyframes blink { 50% { opacity: 0; } }

  .badges {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    justify-content: center;
  }

  .badge {
    font-family: 'Space Mono', monospace;
    font-size: 0.72rem;
    padding: 6px 14px;
    border-radius: 999px;
    border: 1px solid;
    letter-spacing: 0.04em;
    transition: transform 0.2s, box-shadow 0.2s;
    cursor: default;
  }

  .badge:hover {
    transform: translateY(-2px);
    box-shadow: 0 0 16px currentColor;
  }

  .badge-purple { color: var(--purple-light); border-color: rgba(167,139,250,0.35); background: rgba(124,58,237,0.08); }
  .badge-blue   { color: var(--blue-light);   border-color: rgba(147,197,253,0.35); background: rgba(59,130,246,0.08); }
  .badge-cyan   { color: #67e8f9;             border-color: rgba(103,232,249,0.35); background: rgba(6,182,212,0.08); }

  /* ─── QUOTE ─── */
  .quote-block {
    border-left: 2px solid var(--purple);
    padding: 16px 24px;
    margin-bottom: 64px;
    background: rgba(124,58,237,0.05);
    border-radius: 0 8px 8px 0;
    opacity: 0;
    animation: fadeUp 0.9s 0.5s ease forwards;
  }

  .quote-block p {
    font-size: 1.05rem;
    font-style: italic;
    color: #c4b5fd;
    line-height: 1.6;
  }

  /* ─── SECTIONS ─── */
  .section {
    margin-bottom: 64px;
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.7s ease, transform 0.7s ease;
  }

  .section.visible {
    opacity: 1;
    transform: none;
  }

  .section-label {
    font-family: 'Space Mono', monospace;
    font-size: 0.7rem;
    color: var(--cyan);
    letter-spacing: 0.15em;
    text-transform: uppercase;
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .section-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, rgba(6,182,212,0.3), transparent);
  }

  /* ─── STACK GRID ─── */
  .stack-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 14px;
  }

  .stack-card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 18px 20px;
    transition: transform 0.25s, border-color 0.25s, box-shadow 0.25s;
    cursor: default;
    position: relative;
    overflow: hidden;
  }

  .stack-card::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(124,58,237,0.07), transparent);
    opacity: 0;
    transition: opacity 0.3s;
  }

  .stack-card:hover {
    transform: translateY(-4px);
    border-color: rgba(124,58,237,0.5);
    box-shadow: 0 8px 32px rgba(124,58,237,0.15);
  }

  .stack-card:hover::before { opacity: 1; }

  .stack-icon { font-size: 22px; margin-bottom: 8px; }
  .stack-name { font-weight: 600; font-size: 0.95rem; margin-bottom: 4px; }
  .stack-level {
    font-family: 'Space Mono', monospace;
    font-size: 0.68rem;
    color: var(--muted);
    letter-spacing: 0.05em;
  }

  .stack-bar {
    margin-top: 10px;
    height: 3px;
    background: rgba(255,255,255,0.06);
    border-radius: 999px;
    overflow: hidden;
  }

  .stack-fill {
    height: 100%;
    border-radius: 999px;
    background: linear-gradient(90deg, var(--purple), var(--cyan));
    width: 0;
    transition: width 1.2s cubic-bezier(0.4, 0, 0.2, 1);
  }

  /* ─── PROJETS ─── */
  .project-card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 28px 32px;
    margin-bottom: 16px;
    position: relative;
    overflow: hidden;
    transition: transform 0.25s, box-shadow 0.25s, border-color 0.25s;
  }

  .project-card::after {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--purple), var(--blue), var(--cyan));
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.4s ease;
  }

  .project-card:hover {
    transform: translateY(-3px);
    box-shadow: 0 12px 40px rgba(59,130,246,0.12);
    border-color: rgba(59,130,246,0.35);
  }

  .project-card:hover::after { transform: scaleX(1); }

  .project-header {
    display: flex;
    align-items: center;
    gap: 14px;
    margin-bottom: 12px;
  }

  .project-emoji { font-size: 26px; }

  .project-title {
    font-size: 1.15rem;
    font-weight: 700;
    color: #f8fafc;
  }

  .project-desc {
    font-size: 0.9rem;
    color: #94a3b8;
    line-height: 1.65;
    margin-bottom: 16px;
  }

  .project-tags {
    display: flex; gap: 8px; flex-wrap: wrap;
  }

  .tag {
    font-family: 'Space Mono', monospace;
    font-size: 0.68rem;
    padding: 4px 10px;
    border-radius: 6px;
    background: rgba(124,58,237,0.12);
    color: var(--purple-light);
    border: 1px solid rgba(124,58,237,0.2);
    letter-spacing: 0.03em;
  }

  .status-dot {
    display: inline-block;
    width: 7px; height: 7px;
    border-radius: 50%;
    background: #22c55e;
    box-shadow: 0 0 8px #22c55e;
    animation: pulse-dot 2s ease infinite;
    margin-right: 6px;
  }

  @keyframes pulse-dot {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.6; transform: scale(1.3); }
  }

  /* ─── MINDSET ─── */
  .mindset-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 14px;
  }

  .mindset-item {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 20px;
    transition: transform 0.2s, box-shadow 0.2s;
    cursor: default;
  }

  .mindset-item:hover {
    transform: scale(1.03);
    box-shadow: 0 0 24px rgba(124,58,237,0.12);
  }

  .mindset-item .icon { font-size: 20px; margin-bottom: 10px; }
  .mindset-item .label { font-weight: 600; font-size: 0.9rem; margin-bottom: 6px; color: #f1f5f9; }
  .mindset-item .desc { font-size: 0.8rem; color: var(--muted); line-height: 1.5; }

  /* ─── OBJECTIFS ─── */
  .goals-list { list-style: none; display: flex; flex-direction: column; gap: 12px; }

  .goal-item {
    display: flex;
    align-items: center;
    gap: 14px;
    padding: 14px 20px;
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 10px;
    transition: transform 0.2s, border-color 0.2s;
    cursor: default;
  }

  .goal-item:hover {
    transform: translateX(6px);
    border-color: rgba(124,58,237,0.45);
  }

  .goal-check {
    width: 18px; height: 18px;
    border: 1.5px solid var(--purple);
    border-radius: 5px;
    flex-shrink: 0;
    position: relative;
    transition: background 0.3s;
  }

  .goal-item:hover .goal-check {
    background: rgba(124,58,237,0.2);
  }

  .goal-text { font-size: 0.9rem; color: #cbd5e1; }

  /* ─── FOOTER ─── */
  .footer {
    text-align: center;
    margin-top: 80px;
    opacity: 0;
    animation: fadeUp 0.9s 1.2s ease forwards;
  }

  .footer p {
    font-family: 'Space Mono', monospace;
    font-size: 0.75rem;
    color: var(--muted);
    line-height: 2;
  }

  .glow-text {
    color: var(--purple-light);
    text-shadow: 0 0 14px rgba(167,139,250,0.6);
  }

  /* ─── ANIMATIONS ─── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(24px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  /* code block */
  .code-block {
    background: var(--bg3);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 24px 28px;
    font-family: 'Space Mono', monospace;
    font-size: 0.82rem;
    line-height: 1.9;
    overflow-x: auto;
    position: relative;
  }

  .code-block::before {
    content: '● ● ●';
    display: block;
    color: var(--muted);
    font-size: 0.65rem;
    letter-spacing: 0.3em;
    margin-bottom: 16px;
  }

  .kw  { color: #c084fc; }
  .str { color: #86efac; }
  .key { color: #93c5fd; }
  .pun { color: #64748b; }
  .cmt { color: #475569; }
</style>
</head>
<body>

<canvas id="canvas"></canvas>

<div class="container">

  <!-- HERO -->
  <div class="hero">
    <div class="avatar-ring">
      <div class="avatar-inner">🧑‍💻</div>
    </div>
    <h1>Toto</h1>
    <p class="hero-subtitle">Lycéen · 16 ans · France 🇫🇷</p>
    <div class="typing-wrap">
      <span id="typing"></span><span class="cursor"></span>
    </div>
    <div class="badges">
      <span class="badge badge-purple">Python 🐍</span>
      <span class="badge badge-blue">Flutter</span>
      <span class="badge badge-cyan">AI / ML 🤖</span>
      <span class="badge badge-purple">Git & GitHub</span>
      <span class="badge badge-blue">Builder mindset</span>
    </div>
  </div>

  <!-- QUOTE -->
  <div class="quote-block">
    <p>"Discipline > Motivation. Apprendre en construisant. Créer plutôt que consommer."</p>
  </div>

  <!-- STACK -->
  <div class="section" data-section>
    <div class="section-label">Stack technique</div>
    <div class="stack-grid">
      <div class="stack-card" data-fill="90">
        <div class="stack-icon">🐍</div>
        <div class="stack-name">Python</div>
        <div class="stack-level">Principal · avancé</div>
        <div class="stack-bar"><div class="stack-fill"></div></div>
      </div>
      <div class="stack-card" data-fill="40">
        <div class="stack-icon">📱</div>
        <div class="stack-name">Flutter / Dart</div>
        <div class="stack-level">En apprentissage</div>
        <div class="stack-bar"><div class="stack-fill"></div></div>
      </div>
      <div class="stack-card" data-fill="55">
        <div class="stack-icon">🌐</div>
        <div class="stack-name">HTML / CSS</div>
        <div class="stack-level">Bases solides</div>
        <div class="stack-bar"><div class="stack-fill"></div></div>
      </div>
      <div class="stack-card" data-fill="70">
        <div class="stack-icon">🤖</div>
        <div class="stack-name">IA / ML</div>
        <div class="stack-level">Exploration active</div>
        <div class="stack-bar"><div class="stack-fill"></div></div>
      </div>
      <div class="stack-card" data-fill="75">
        <div class="stack-icon">🔧</div>
        <div class="stack-name">Git / GitHub</div>
        <div class="stack-level">Utilisé au quotidien</div>
        <div class="stack-bar"><div class="stack-fill"></div></div>
      </div>
    </div>
  </div>

  <!-- CODE BLOCK -->
  <div class="section" data-section>
    <div class="section-label">À propos (format dev)</div>
    <div class="code-block">
<span class="kw">class</span> <span class="key">Toto</span><span class="pun">:</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="kw">def</span> <span class="key">__init__</span><span class="pun">(</span>self<span class="pun">):</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self<span class="pun">.</span>age&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= <span class="str">16</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self<span class="pun">.</span>statut&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= <span class="str">"Lycéen & Builder"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self<span class="pun">.</span>objectif&nbsp;&nbsp;&nbsp;&nbsp;= <span class="str">"Indépendance financière à 18 ans"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self<span class="pun">.</span>stack&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= <span class="pun">[</span><span class="str">"Python"</span><span class="pun">,</span> <span class="str">"Flutter"</span><span class="pun">,</span> <span class="str">"AI/ML"</span><span class="pun">]</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;self<span class="pun">.</span>mindset&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= <span class="pun">[</span><span class="str">"discipline"</span><span class="pun">,</span> <span class="str">"build fast"</span><span class="pun">,</span> <span class="str">"long terme"</span><span class="pun">]</span><br><br>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="kw">def</span> <span class="key">current_focus</span><span class="pun">(</span>self<span class="pun">):</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="kw">return</span> <span class="pun">{</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="str">"app"</span><span class="pun">:</span> <span class="str">"Quotidien+ (Flutter)"</span><span class="pun">,</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="str">"learning"</span><span class="pun">:</span> <span class="str">"Machine Learning & automatisation"</span><span class="pun">,</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="str">"goal_2026"</span><span class="pun">:</span> <span class="str">"Premiers revenus via projets tech"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="pun">}</span>
    </div>
  </div>

  <!-- PROJETS -->
  <div class="section" data-section>
    <div class="section-label">Projets en cours</div>

    <div class="project-card">
      <div class="project-header">
        <span class="project-emoji">📱</span>
        <span class="project-title">Quotidien+</span>
        <span style="margin-left:auto; font-family:'Space Mono',monospace; font-size:0.7rem; color:#22c55e;">
          <span class="status-dot"></span>En dev
        </span>
      </div>
      <p class="project-desc">Application Flutter de développement personnel axée sur les routines et la productivité quotidienne. L'objectif : rendre la discipline facile à maintenir sur le long terme.</p>
      <div class="project-tags">
        <span class="tag">Flutter</span>
        <span class="tag">Dart</span>
        <span class="tag">Mobile</span>
        <span class="tag">Productivité</span>
      </div>
    </div>

    <div class="project-card">
      <div class="project-header">
        <span class="project-emoji">🤖</span>
        <span class="project-title">Expérimentations IA & Python</span>
        <span style="margin-left:auto; font-family:'Space Mono',monospace; font-size:0.7rem; color:#22c55e;">
          <span class="status-dot"></span>Continu
        </span>
      </div>
      <p class="project-desc">Scripts et prototypes Python pour explorer le machine learning, l'automatisation et les APIs d'IA. Apprendre en construisant des outils concrets et utiles.</p>
      <div class="project-tags">
        <span class="tag">Python</span>
        <span class="tag">ML</span>
        <span class="tag">Automation</span>
        <span class="tag">API</span>
      </div>
    </div>
  </div>

  <!-- MINDSET -->
  <div class="section" data-section>
    <div class="section-label">Mindset</div>
    <div class="mindset-grid">
      <div class="mindset-item">
        <div class="icon">🎯</div>
        <div class="label">Long terme</div>
        <div class="desc">Chaque décision d'aujourd'hui construit demain.</div>
      </div>
      <div class="mindset-item">
        <div class="icon">🔨</div>
        <div class="label">Learn by building</div>
        <div class="desc">Apprendre en faisant, pas juste en regardant.</div>
      </div>
      <div class="mindset-item">
        <div class="icon">⚡</div>
        <div class="label">Discipline first</div>
        <div class="desc">La motivation fluctue, les habitudes restent.</div>
      </div>
      <div class="mindset-item">
        <div class="icon">🛠️</div>
        <div class="label">Builder mindset</div>
        <div class="desc">Créer plutôt que consommer. Toujours.</div>
      </div>
      <div class="mindset-item">
        <div class="icon">🚫</div>
        <div class="label">Anti-procrastination</div>
        <div class="desc">Action > réflexion sans fin. Ship fast.</div>
      </div>
      <div class="mindset-item">
        <div class="icon">📈</div>
        <div class="label">Évolutif</div>
        <div class="desc">Profil en évolution constante — recheck dans 6 mois.</div>
      </div>
    </div>
  </div>

  <!-- OBJECTIFS 2026 -->
  <div class="section" data-section>
    <div class="section-label">Objectifs 2026</div>
    <ul class="goals-list">
      <li class="goal-item"><span class="goal-check"></span><span class="goal-text">Maîtriser Python solidement</span></li>
      <li class="goal-item"><span class="goal-check"></span><span class="goal-text">Lancer plusieurs projets concrets et utiles</span></li>
      <li class="goal-item"><span class="goal-check"></span><span class="goal-text">Améliorer ma discipline et ma régularité</span></li>
      <li class="goal-item"><span class="goal-check"></span><span class="goal-text">Monter en compétences IA / dev</span></li>
      <li class="goal-item"><span class="goal-check"></span><span class="goal-text">Commencer à générer des revenus via mes projets</span></li>
    </ul>
  </div>

  <!-- FOOTER -->
  <div class="footer">
    <p>Profil en construction — comme moi. 🚀</p>
    <p><span class="glow-text">France</span> · 16 ans · Lycéen · Builder</p>
    <p style="margin-top:8px;">Si tu partages la même vision — <span class="glow-text">follow ou dis bonjour.</span></p>
  </div>

</div>

<script>
// ─── PARTICULES ───
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
let particles = [];

function resize() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}

resize();
window.addEventListener('resize', resize);

function randomBetween(a, b) { return a + Math.random() * (b - a); }

for (let i = 0; i < 80; i++) {
  particles.push({
    x: randomBetween(0, window.innerWidth),
    y: randomBetween(0, window.innerHeight),
    r: randomBetween(0.5, 2),
    dx: randomBetween(-0.2, 0.2),
    dy: randomBetween(-0.3, -0.05),
    alpha: randomBetween(0.2, 0.7),
    color: Math.random() > 0.5 ? '#7c3aed' : Math.random() > 0.5 ? '#3b82f6' : '#06b6d4'
  });
}

function animParticles() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  for (const p of particles) {
    ctx.beginPath();
    ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
    ctx.fillStyle = p.color;
    ctx.globalAlpha = p.alpha;
    ctx.fill();
    p.x += p.dx;
    p.y += p.dy;
    if (p.y < -5) { p.y = canvas.height + 5; p.x = randomBetween(0, canvas.width); }
    if (p.x < -5) p.x = canvas.width + 5;
    if (p.x > canvas.width + 5) p.x = -5;
  }
  ctx.globalAlpha = 1;
  requestAnimationFrame(animParticles);
}

animParticles();

// ─── TYPING EFFECT ───
const phrases = [
  "Futur indie developer 🚀",
  "Python & AI enthusiast 🤖",
  "Discipline > Motivation ⚡",
  "Builder in progress 🔨",
  "Indépendance financière à 18 ans 🎯"
];

let pi = 0, ci = 0, deleting = false, wait = 0;
const el = document.getElementById('typing');

function type() {
  const phrase = phrases[pi];
  if (!deleting) {
    el.textContent = phrase.slice(0, ++ci);
    if (ci === phrase.length) { deleting = true; wait = 60; }
  } else {
    if (wait-- > 0) { setTimeout(type, 40); return; }
    el.textContent = phrase.slice(0, --ci);
    if (ci === 0) { deleting = false; pi = (pi + 1) % phrases.length; }
  }
  setTimeout(type, deleting ? 35 : 65);
}

type();

// ─── SCROLL REVEAL ───
const sections = document.querySelectorAll('[data-section]');
const io = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.classList.add('visible');
      // Animer les barres de stack
      e.target.querySelectorAll('.stack-card').forEach((card, i) => {
        const fill = card.querySelector('.stack-fill');
        const pct = card.dataset.fill || 50;
        setTimeout(() => { fill.style.width = pct + '%'; }, i * 100);
      });
    }
  });
}, { threshold: 0.1 });

sections.forEach(s => io.observe(s));
</script>
</body>
</html>
