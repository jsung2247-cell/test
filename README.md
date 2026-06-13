<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Omkar Sawant — Penetration Tester</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Inter:wght@300;400;500;600;700&family=Orbitron:wght@700;900&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #080b10;
    --surface: #0d1117;
    --surface2: #161b22;
    --border: #21262d;
    --green: #00ff9d;
    --green-dim: #00cc7d;
    --green-glow: rgba(0,255,157,0.15);
    --cyan: #00d4ff;
    --red: #ff3e5e;
    --text: #e6edf3;
    --text-muted: #7d8590;
    --mono: 'Share Tech Mono', monospace;
    --sans: 'Inter', sans-serif;
    --display: 'Orbitron', sans-serif;
  }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--sans);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── MATRIX CANVAS ── */
  #matrix-canvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: 0;
    opacity: 0.04;
    pointer-events: none;
  }

  /* ── SCAN LINE ── */
  body::after {
    content: '';
    position: fixed;
    top: -100%;
    left: 0;
    width: 100%;
    height: 2px;
    background: linear-gradient(90deg, transparent, var(--green), transparent);
    animation: scanLine 8s linear infinite;
    z-index: 1;
    pointer-events: none;
  }

  @keyframes scanLine {
    0% { top: -2px; }
    100% { top: 100%; }
  }

  /* ── LAYOUT ── */
  .container {
    max-width: 900px;
    margin: 0 auto;
    padding: 0 24px;
    position: relative;
    z-index: 2;
  }

  /* ── HERO ── */
  .hero {
    padding: 80px 0 48px;
    position: relative;
  }

  .terminal-prompt {
    font-family: var(--mono);
    color: var(--green);
    font-size: 13px;
    margin-bottom: 12px;
    opacity: 0;
    animation: fadeInUp 0.6s ease 0.2s forwards;
  }

  .terminal-prompt span { color: var(--cyan); }

  .hero-name {
    font-family: var(--display);
    font-size: clamp(36px, 7vw, 68px);
    font-weight: 900;
    letter-spacing: -1px;
    line-height: 1;
    margin-bottom: 4px;
    opacity: 0;
    animation: fadeInUp 0.6s ease 0.4s forwards;
  }

  .hero-name .accent {
    color: var(--green);
    text-shadow: 0 0 30px var(--green), 0 0 60px rgba(0,255,157,0.3);
  }

  .hero-title {
    font-family: var(--mono);
    color: var(--text-muted);
    font-size: 14px;
    margin-bottom: 28px;
    opacity: 0;
    animation: fadeInUp 0.6s ease 0.6s forwards;
  }

  .hero-title .sep { color: var(--border); margin: 0 8px; }

  .status-bar {
    display: flex;
    align-items: center;
    gap: 8px;
    font-family: var(--mono);
    font-size: 12px;
    color: var(--green);
    margin-bottom: 40px;
    opacity: 0;
    animation: fadeInUp 0.6s ease 0.8s forwards;
  }

  .status-dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: var(--green);
    box-shadow: 0 0 10px var(--green);
    animation: pulse 2s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; box-shadow: 0 0 10px var(--green); }
    50% { opacity: 0.4; box-shadow: 0 0 4px var(--green); }
  }

  /* ── STAT CHIPS ── */
  .stat-row {
    display: flex; flex-wrap: wrap; gap: 12px;
    margin-bottom: 48px;
    opacity: 0;
    animation: fadeInUp 0.6s ease 1s forwards;
  }

  .stat-chip {
    border: 1px solid var(--border);
    background: var(--surface2);
    border-radius: 6px;
    padding: 10px 18px;
    text-align: center;
    transition: border-color 0.3s, transform 0.3s;
  }

  .stat-chip:hover {
    border-color: var(--green);
    transform: translateY(-3px);
    box-shadow: 0 8px 24px var(--green-glow);
  }

  .stat-chip .num {
    font-family: var(--display);
    font-size: 22px;
    color: var(--green);
    display: block;
  }

  .stat-chip .label {
    font-size: 11px;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  /* ── SECTION ── */
  section {
    margin-bottom: 56px;
    opacity: 0;
    transform: translateY(24px);
    transition: opacity 0.7s ease, transform 0.7s ease;
  }

  section.visible {
    opacity: 1;
    transform: none;
  }

  .section-label {
    font-family: var(--mono);
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 3px;
    color: var(--green);
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .section-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, var(--border), transparent);
  }

  /* ── ABOUT ── */
  .about-text {
    font-size: 15px;
    line-height: 1.8;
    color: var(--text-muted);
    border-left: 2px solid var(--green);
    padding-left: 20px;
    max-width: 680px;
  }

  .about-text strong { color: var(--text); }

  /* ── SKILLS GRID ── */
  .skills-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
  }

  .skill-tag {
    font-family: var(--mono);
    font-size: 12px;
    padding: 6px 14px;
    border: 1px solid var(--border);
    border-radius: 4px;
    color: var(--text-muted);
    background: var(--surface2);
    cursor: default;
    transition: all 0.2s;
    position: relative;
    overflow: hidden;
  }

  .skill-tag::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--green-glow);
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.3s ease;
  }

  .skill-tag:hover {
    border-color: var(--green);
    color: var(--green);
  }

  .skill-tag:hover::before { transform: scaleX(1); }

  /* ── LEARNING ── */
  .learning-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 12px;
  }

  .learning-item {
    border: 1px solid var(--border);
    background: var(--surface2);
    border-radius: 8px;
    padding: 16px;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
  }

  .learning-item::after {
    content: '';
    position: absolute;
    bottom: 0; left: 0;
    width: 0; height: 2px;
    background: var(--green);
    transition: width 0.4s ease;
  }

  .learning-item:hover::after { width: 100%; }

  .learning-item:hover {
    border-color: rgba(0,255,157,0.3);
    transform: translateY(-2px);
  }

  .learning-icon {
    font-size: 20px;
    margin-bottom: 8px;
    display: block;
  }

  .learning-item h4 {
    font-size: 13px;
    font-weight: 600;
    color: var(--text);
    margin-bottom: 4px;
  }

  .learning-item p {
    font-size: 11px;
    color: var(--text-muted);
  }

  /* ── PROJECTS ── */
  .project-cards {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
    gap: 16px;
  }

  .project-card {
    border: 1px solid var(--border);
    background: var(--surface2);
    border-radius: 10px;
    padding: 24px;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
  }

  .project-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--green), var(--cyan));
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.4s ease;
  }

  .project-card:hover::before { transform: scaleX(1); }

  .project-card:hover {
    border-color: rgba(0,255,157,0.2);
    transform: translateY(-4px);
    box-shadow: 0 12px 40px rgba(0,0,0,0.4);
  }

  .project-icon {
    font-size: 28px;
    margin-bottom: 12px;
  }

  .project-card h3 {
    font-family: var(--display);
    font-size: 13px;
    color: var(--green);
    margin-bottom: 8px;
    letter-spacing: 1px;
  }

  .project-card p {
    font-size: 13px;
    color: var(--text-muted);
    line-height: 1.6;
  }

  .project-tags {
    display: flex; flex-wrap: wrap; gap: 6px;
    margin-top: 14px;
  }

  .project-tags span {
    font-family: var(--mono);
    font-size: 10px;
    color: var(--cyan);
    border: 1px solid rgba(0,212,255,0.2);
    border-radius: 3px;
    padding: 2px 8px;
  }

  /* ── CERTS ── */
  .cert-list {
    display: flex; flex-direction: column; gap: 10px;
  }

  .cert-item {
    display: flex;
    align-items: center;
    gap: 14px;
    padding: 14px 18px;
    border: 1px solid var(--border);
    background: var(--surface2);
    border-radius: 8px;
    transition: all 0.25s;
    font-size: 14px;
  }

  .cert-item:hover {
    border-color: var(--green);
    padding-left: 24px;
    background: rgba(0,255,157,0.03);
  }

  .cert-badge {
    width: 32px; height: 32px;
    border-radius: 6px;
    background: linear-gradient(135deg, var(--green), var(--cyan));
    display: flex; align-items: center; justify-content: center;
    font-size: 14px;
    flex-shrink: 0;
  }

  /* ── CONNECT ── */
  .connect-grid {
    display: flex; gap: 12px; flex-wrap: wrap;
  }

  .connect-link {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 12px 20px;
    border: 1px solid var(--border);
    border-radius: 8px;
    background: var(--surface2);
    color: var(--text-muted);
    text-decoration: none;
    font-size: 13px;
    font-family: var(--mono);
    transition: all 0.3s;
  }

  .connect-link:hover {
    border-color: var(--green);
    color: var(--green);
    transform: translateY(-2px);
    box-shadow: 0 6px 20px var(--green-glow);
  }

  .connect-link .icon { font-size: 16px; }

  /* ── QUOTE ── */
  .quote-block {
    border: 1px solid var(--border);
    background: var(--surface2);
    border-radius: 10px;
    padding: 28px 32px;
    position: relative;
    overflow: hidden;
    margin-top: 56px;
    margin-bottom: 48px;
  }

  .quote-block::before {
    content: '"';
    position: absolute;
    top: -20px; left: 16px;
    font-family: var(--display);
    font-size: 100px;
    color: var(--green);
    opacity: 0.08;
    line-height: 1;
  }

  .quote-block p {
    font-family: var(--mono);
    font-size: 15px;
    color: var(--text);
    line-height: 1.7;
    position: relative;
  }

  .quote-block cite {
    font-family: var(--sans);
    font-size: 12px;
    color: var(--green);
    display: block;
    margin-top: 12px;
  }

  /* ── ANIMATIONS ── */
  @keyframes fadeInUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  /* ── PLATFORMS ── */
  .platform-row {
    display: flex; gap: 10px; flex-wrap: wrap;
  }

  .platform-chip {
    font-family: var(--mono);
    font-size: 12px;
    padding: 8px 16px;
    border-radius: 20px;
    border: 1px solid var(--border);
    color: var(--text-muted);
    background: var(--surface2);
    transition: all 0.3s;
  }

  .platform-chip:hover {
    border-color: var(--cyan);
    color: var(--cyan);
    box-shadow: 0 0 16px rgba(0,212,255,0.15);
  }

  /* ── GLITCH effect on name hover ── */
  .hero-name:hover .accent {
    animation: glitch 0.3s linear;
  }

  @keyframes glitch {
    0%   { text-shadow: 0 0 30px var(--green); clip-path: inset(0 0 80% 0); }
    20%  { clip-path: inset(30% 0 40% 0); transform: skewX(-4deg); }
    40%  { clip-path: inset(60% 0 10% 0); transform: skewX(3deg); }
    60%  { clip-path: inset(10% 0 60% 0); transform: skewX(-2deg); }
    80%  { clip-path: inset(80% 0 0 0); }
    100% { text-shadow: 0 0 30px var(--green), 0 0 60px rgba(0,255,157,0.3); clip-path: inset(0); transform: none; }
  }

  @media (max-width: 600px) {
    .hero { padding: 48px 0 32px; }
    .project-cards { grid-template-columns: 1fr; }
    .learning-grid { grid-template-columns: 1fr 1fr; }
  }

  @media (prefers-reduced-motion: reduce) {
    *, *::after { animation: none !important; transition: none !important; }
  }
</style>
</head>
<body>

<canvas id="matrix-canvas"></canvas>

<div class="container">

  <!-- HERO -->
  <div class="hero">
    <p class="terminal-prompt">root@kali:~# <span>whoami</span></p>
    <h1 class="hero-name">Omkar <span class="accent">Sawant</span></h1>
    <p class="hero-title">
      Penetration Tester
      <span class="sep">|</span>
      Cybersecurity Analyst
      <span class="sep">|</span>
      Security Researcher
    </p>
    <div class="status-bar">
      <span class="status-dot"></span>
      <span>AVAILABLE FOR ENGAGEMENTS</span>
    </div>

    <div class="stat-row">
      <div class="stat-chip"><span class="num">6+</span><span class="label">Certs</span></div>
      <div class="stat-chip"><span class="num">OWASP</span><span class="label">Top 10</span></div>
      <div class="stat-chip"><span class="num">CWE</span><span class="label">Top 25</span></div>
      <div class="stat-chip"><span class="num">CTF</span><span class="label">Player</span></div>
      <div class="stat-chip"><span class="num">VAPT</span><span class="label">Specialist</span></div>
    </div>
  </div>

  <!-- ABOUT -->
  <section>
    <p class="section-label">// about</p>
    <p class="about-text">
      Cybersecurity professional specializing in <strong>Web Application Security</strong>, <strong>Network Penetration Testing</strong>, and <strong>VAPT</strong>. I enjoy discovering vulnerabilities, tracing attack paths, and documenting findings with clear, actionable remediation guidance — turning complexity into clarity for development teams.
    </p>
  </section>

  <!-- SKILLS -->
  <section>
    <p class="section-label">// skills &amp; expertise</p>
    <div class="skills-grid">
      <span class="skill-tag">Web App Pentesting</span>
      <span class="skill-tag">Network Security</span>
      <span class="skill-tag">Vulnerability Assessment</span>
      <span class="skill-tag">OWASP Top 10</span>
      <span class="skill-tag">CWE Top 25</span>
      <span class="skill-tag">Linux Priv Esc</span>
      <span class="skill-tag">SQLi</span>
      <span class="skill-tag">XSS</span>
      <span class="skill-tag">File Upload Exploits</span>
      <span class="skill-tag">Directory Traversal</span>
      <span class="skill-tag">Auth Bypass</span>
      <span class="skill-tag">Service Enumeration</span>
      <span class="skill-tag">Traffic Analysis</span>
      <span class="skill-tag">Security Reporting</span>
      <span class="skill-tag">CTF Challenges</span>
      <span class="skill-tag">Burp Suite</span>
      <span class="skill-tag">Nmap</span>
      <span class="skill-tag">Metasploit</span>
    </div>
  </section>

  <!-- CURRENTLY LEARNING -->
  <section>
    <p class="section-label">// currently learning</p>
    <div class="learning-grid">
      <div class="learning-item">
        <span class="learning-icon">🌐</span>
        <h4>Advanced Web Security</h4>
        <p>Server-side vulns, deserialization, SSRF</p>
      </div>
      <div class="learning-item">
        <span class="learning-icon">🔌</span>
        <h4>API Security Testing</h4>
        <p>REST/GraphQL attack surfaces</p>
      </div>
      <div class="learning-item">
        <span class="learning-icon">🏰</span>
        <h4>Active Directory</h4>
        <p>Kerberoasting, lateral movement</p>
      </div>
      <div class="learning-item">
        <span class="learning-icon">🎭</span>
        <h4>Red Team Ops</h4>
        <p>TTPs, evasion, C2 frameworks</p>
      </div>
      <div class="learning-item">
        <span class="learning-icon">💥</span>
        <h4>Exploit Development</h4>
        <p>Buffer overflows, shellcoding</p>
      </div>
      <div class="learning-item">
        <span class="learning-icon">☁️</span>
        <h4>Cloud Security</h4>
        <p>AWS/Azure misconfigurations</p>
      </div>
    </div>
  </section>

  <!-- PROJECTS -->
  <section>
    <p class="section-label">// featured projects</p>
    <div class="project-cards">

      <div class="project-card">
        <div class="project-icon">🔥</div>
        <h3>VulnHub Walkthroughs</h3>
        <p>End-to-end machine walkthroughs — enumeration, exploitation, privilege escalation, post-exploitation, and professional reporting.</p>
        <div class="project-tags">
          <span>Enumeration</span><span>Privesc</span><span>Reporting</span>
        </div>
      </div>

      <div class="project-card">
        <div class="project-icon">🌐</div>
        <h3>Web App Pentest Labs</h3>
        <p>Hands-on assessments against DVWA and custom labs covering the most critical injection and authentication vulnerabilities.</p>
        <div class="project-tags">
          <span>SQLi</span><span>XSS</span><span>Auth Flaws</span>
        </div>
      </div>

      <div class="project-card">
        <div class="project-icon">🛡️</div>
        <h3>Network Security Labs</h3>
        <p>Service enumeration, traffic analysis, network discovery, and identification of exploitable network-layer vulnerabilities.</p>
        <div class="project-tags">
          <span>Nmap</span><span>Wireshark</span><span>Recon</span>
        </div>
      </div>

    </div>
  </section>

  <!-- PLATFORMS -->
  <section>
    <p class="section-label">// practice platforms</p>
    <div class="platform-row">
      <span class="platform-chip">PortSwigger Web Security Academy</span>
      <span class="platform-chip">TryHackMe</span>
      <span class="platform-chip">VulnHub</span>
      <span class="platform-chip">DVWA</span>
      <span class="platform-chip">Custom Pentesting Labs</span>
    </div>
  </section>

  <!-- CERTIFICATIONS -->
  <section>
    <p class="section-label">// certifications</p>
    <div class="cert-list">
      <div class="cert-item"><div class="cert-badge">🎓</div> Advance Diploma in Information Security</div>
      <div class="cert-item"><div class="cert-badge">🌐</div> Certified Web Penetration Tester</div>
      <div class="cert-item"><div class="cert-badge">🛡️</div> Certified Cyber Security &amp; Ethical Hacker</div>
      <div class="cert-item"><div class="cert-badge">🔌</div> Certified Network Penetration Tester</div>
      <div class="cert-item"><div class="cert-badge">💥</div> Exploit Writing</div>
      <div class="cert-item"><div class="cert-badge">🔒</div> Network Security</div>
    </div>
  </section>

  <!-- CONNECT -->
  <section>
    <p class="section-label">// connect</p>
    <div class="connect-grid">
      <a href="mailto:omkarsawant116@gmail.com" class="connect-link">
        <span class="icon">📧</span> omkarsawant116@gmail.com
      </a>
      <a href="https://linkedin.com/in/omkar-sawant-37bb5228b" target="_blank" class="connect-link">
        <span class="icon">💼</span> LinkedIn
      </a>
      <a href="https://github.com/omkarsawant1337" target="_blank" class="connect-link">
        <span class="icon">🐙</span> GitHub
      </a>
    </div>
  </section>

  <!-- QUOTE -->
  <div class="quote-block">
    <p>"Security is not a product, but a process."</p>
    <cite>— Bruce Schneier</cite>
  </div>

</div><!-- /container -->

<script>
/* ── MATRIX RAIN ── */
(function() {
  const canvas = document.getElementById('matrix-canvas');
  const ctx = canvas.getContext('2d');
  let W, H, cols, drops;
  const chars = 'アイウエオカキクケコ0123456789ABCDEF<>{}[]|\\/?!@#$%^&*';

  function resize() {
    W = canvas.width = window.innerWidth;
    H = canvas.height = window.innerHeight;
    cols = Math.floor(W / 16);
    drops = Array(cols).fill(1);
  }

  function draw() {
    ctx.fillStyle = 'rgba(8,11,16,0.05)';
    ctx.fillRect(0, 0, W, H);
    ctx.fillStyle = '#00ff9d';
    ctx.font = '14px Share Tech Mono';
    drops.forEach((y, i) => {
      const char = chars[Math.floor(Math.random() * chars.length)];
      ctx.fillText(char, i * 16, y * 16);
      if (y * 16 > H && Math.random() > 0.975) drops[i] = 0;
      drops[i]++;
    });
  }

  resize();
  window.addEventListener('resize', resize);
  setInterval(draw, 55);
})();

/* ── SCROLL REVEAL ── */
const sections = document.querySelectorAll('section');
const io = new IntersectionObserver((entries) => {
  entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible'); });
}, { threshold: 0.12 });
sections.forEach(s => io.observe(s));

/* ── TYPEWRITER for terminal prompt ── */
(function() {
  const el = document.querySelector('.terminal-prompt');
  const base = 'root@kali:~# ';
  const cmd = 'whoami';
  let i = 0;
  el.innerHTML = base;
  const cursor = document.createElement('span');
  cursor.textContent = '█';
  cursor.style.cssText = 'animation:blink 1s step-end infinite;color:#00ff9d';
  el.appendChild(cursor);

  const style = document.createElement('style');
  style.textContent = '@keyframes blink{0%,100%{opacity:1}50%{opacity:0}}';
  document.head.appendChild(style);

  setTimeout(() => {
    const span = document.createElement('span');
    el.insertBefore(span, cursor);
    const type = setInterval(() => {
      span.textContent += cmd[i++];
      if (i >= cmd.length) clearInterval(type);
    }, 80);
  }, 900);
})();
</script>
</body>
</html>
