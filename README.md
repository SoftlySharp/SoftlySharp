<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<style>
@import url('https://fonts.googleapis.com/css2?family=Fira+Code:wght@300;400;500;600;700&family=Inter:wght@300;400;500;600;700;800;900&display=swap');
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0;}
:root{
  --web:#38BDF8;--fl:#02ABFF;--purple:#C084FC;--pink:#F472B6;
  --dark:#080b14;--dark2:#0d1120;
}
body{background:var(--dark);font-family:'Inter',sans-serif;color:#e2e8f0;overflow-x:hidden;min-height:100vh;cursor:none;}
canvas#bg{position:fixed;top:0;left:0;width:100%;height:100%;z-index:0;pointer-events:none;}

/* ── CUSTOM CURSOR ── */
#cursor{
  position:fixed;width:14px;height:14px;
  background:radial-gradient(circle,rgba(56,189,248,1),rgba(192,132,252,.8));
  border-radius:50%;pointer-events:none;z-index:9999;
  transform:translate(-50%,-50%);
  transition:transform .08s ease,width .2s,height .2s,opacity .2s;
  mix-blend-mode:screen;
}
#cursor-ring{
  position:fixed;width:38px;height:38px;
  border:1.5px solid rgba(56,189,248,.5);
  border-radius:50%;pointer-events:none;z-index:9998;
  transform:translate(-50%,-50%);
  transition:transform .18s ease,width .2s,height .2s,border-color .2s;
}
#cursor-trail{position:fixed;pointer-events:none;z-index:9997;}
.trail-dot{position:fixed;width:5px;height:5px;border-radius:50%;background:rgba(56,189,248,.4);pointer-events:none;z-index:9997;transform:translate(-50%,-50%);}
body:hover #cursor{opacity:1;}

.wrap{position:relative;z-index:1;}

/* blobs */
.hero{min-height:460px;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:60px 24px 36px;position:relative;overflow:hidden;}
.blob{position:absolute;border-radius:50%;filter:blur(90px);pointer-events:none;}
.b1{width:320px;height:320px;background:radial-gradient(circle,rgba(56,189,248,.32),transparent);top:-60px;left:-100px;animation:bf 9s ease-in-out infinite;}
.b2{width:270px;height:270px;background:radial-gradient(circle,rgba(192,132,252,.26),transparent);bottom:-40px;right:-80px;animation:bf 9s ease-in-out infinite;animation-delay:-4.5s;}
.b3{width:190px;height:190px;background:radial-gradient(circle,rgba(2,171,255,.18),transparent);top:50%;left:55%;animation:bf 7s ease-in-out infinite;animation-delay:-2s;}
@keyframes bf{0%,100%{transform:translateY(0)scale(1);}50%{transform:translateY(-18px)scale(1.06);}}

/* avatar */
.av-wrap{position:relative;width:148px;height:148px;margin:0 auto 24px;animation:float 4s ease-in-out infinite;}
@keyframes float{0%,100%{transform:translateY(0);}50%{transform:translateY(-10px);}}
.av-wrap::before{content:'';position:absolute;inset:-5px;border-radius:50%;background:conic-gradient(from 0deg,var(--web),var(--purple),var(--fl),var(--pink),var(--web));animation:spin 3.5s linear infinite;z-index:0;}
.av-wrap::after{content:'';position:absolute;inset:-10px;border-radius:50%;background:conic-gradient(from 180deg,rgba(56,189,248,.25),transparent,rgba(192,132,252,.25));animation:spin 7s linear infinite reverse;filter:blur(5px);z-index:-1;}
@keyframes spin{to{transform:rotate(360deg);}}
.av-inner{position:absolute;inset:4px;border-radius:50%;background:linear-gradient(135deg,#101827,#0d1829);border:2px solid rgba(56,189,248,.4);z-index:1;display:flex;align-items:center;justify-content:center;font-size:60px;overflow:hidden;}

/* name */
.hero-name{font-size:clamp(30px,5vw,46px);font-weight:900;letter-spacing:-1.5px;background:linear-gradient(120deg,var(--web) 0%,var(--purple) 45%,var(--fl) 100%);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;background-size:200% 200%;animation:shimmer 5s linear infinite;text-align:center;line-height:1.1;margin-bottom:8px;}
@keyframes shimmer{0%{background-position:0% 50%;}50%{background-position:100% 50%;}100%{background-position:0% 50%;}}

/* typing */
.type-row{height:28px;display:flex;align-items:center;justify-content:center;margin-bottom:18px;}
.type-txt{font-family:'Fira Code',monospace;font-size:clamp(13px,2.2vw,15px);color:var(--web);font-weight:500;}
.caret{display:inline-block;width:2px;height:1.1em;background:var(--web);margin-left:2px;vertical-align:middle;animation:blink .75s infinite;}
@keyframes blink{0%,50%{opacity:1;}51%,100%{opacity:0;}}

/* badges */
.badge-row{display:flex;gap:9px;flex-wrap:wrap;justify-content:center;margin-bottom:18px;}
.badge{padding:6px 15px;border-radius:100px;font-size:12px;font-weight:600;border:1px solid;letter-spacing:.4px;transition:transform .2s;}
.badge:hover{transform:translateY(-3px) scale(1.05);}
.b-web{background:rgba(56,189,248,.1);border-color:rgba(56,189,248,.35);color:#7DD3FC;}
.b-fl{background:rgba(2,171,255,.1);border-color:rgba(2,171,255,.35);color:#67E8F9;}
.b-code{background:rgba(192,132,252,.1);border-color:rgba(192,132,252,.35);color:#D8B4FE;}

/* PROFILE VIEWS */
.views-row{display:flex;gap:10px;align-items:center;justify-content:center;margin-top:6px;}
.views-card{
  display:inline-flex;align-items:center;gap:10px;
  padding:9px 20px;
  background:linear-gradient(135deg,rgba(56,189,248,.12),rgba(192,132,252,.08));
  border:1px solid rgba(56,189,248,.3);
  border-radius:100px;
  font-size:13px;color:var(--web);font-weight:600;
  position:relative;overflow:hidden;
}
.views-card::before{
  content:'';position:absolute;inset:0;
  background:linear-gradient(90deg,transparent,rgba(56,189,248,.08),transparent);
  transform:translateX(-100%);animation:scanline 2.5s linear infinite;
}
@keyframes scanline{to{transform:translateX(100%);}}
.live-dot{width:8px;height:8px;background:#4ADE80;border-radius:50%;flex-shrink:0;position:relative;}
.live-dot::after{content:'';position:absolute;inset:-3px;border-radius:50%;background:rgba(74,222,128,.4);animation:ping 1.5s ease-out infinite;}
@keyframes ping{0%{transform:scale(1);opacity:.8;}100%{transform:scale(2.8);opacity:0;}}
.views-num{font-family:'Fira Code',monospace;font-size:15px;font-weight:700;}
.eye-icon{font-size:16px;}

/* ── SECTIONS ── */
.section{padding:8px 20px 48px;max-width:880px;margin:0 auto;}
.sec-label{font-size:11px;font-weight:700;letter-spacing:3px;color:rgba(56,189,248,.5);text-transform:uppercase;text-align:center;margin-bottom:6px;}
.sec-title{font-size:clamp(19px,3vw,25px);font-weight:700;text-align:center;margin-bottom:8px;color:#e2e8f0;}
.sec-title span{background:linear-gradient(90deg,var(--web),var(--fl));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;}
.divider{width:56px;height:3px;background:linear-gradient(90deg,var(--web),var(--fl));border-radius:100px;margin:0 auto 30px;}

/* DUAL FOCUS */
.dual{display:grid;grid-template-columns:1fr 1fr;gap:18px;}
@media(max-width:560px){.dual{grid-template-columns:1fr;}}
.focus-card{border-radius:20px;padding:26px 22px;position:relative;overflow:hidden;border:1px solid;transition:transform .3s,box-shadow .3s;cursor:none;}
.focus-card::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;border-radius:20px 20px 0 0;}
.focus-card.web{background:linear-gradient(135deg,rgba(56,189,248,.07),rgba(14,165,233,.02));border-color:rgba(56,189,248,.22);}
.focus-card.web::before{background:linear-gradient(90deg,var(--web),var(--purple));}
.focus-card.web:hover{transform:translateY(-6px);box-shadow:0 14px 40px rgba(56,189,248,.18);}
.focus-card.flutter{background:linear-gradient(135deg,rgba(2,171,255,.07),rgba(1,117,194,.02));border-color:rgba(2,171,255,.22);}
.focus-card.flutter::before{background:linear-gradient(90deg,var(--fl),var(--web));}
.focus-card.flutter:hover{transform:translateY(-6px);box-shadow:0 14px 40px rgba(2,171,255,.18);}
.fc-icon{font-size:36px;margin-bottom:12px;}
.fc-title{font-size:19px;font-weight:700;margin-bottom:5px;}
.focus-card.web .fc-title{color:var(--web);}
.focus-card.flutter .fc-title{color:var(--fl);}
.fc-sub{font-size:13px;color:rgba(226,232,240,.5);line-height:1.6;margin-bottom:16px;}
.stags{display:flex;flex-wrap:wrap;gap:7px;}
.stag{padding:4px 12px;border-radius:100px;font-size:11.5px;font-weight:600;border:1px solid;letter-spacing:.3px;transition:all .2s;cursor:none;}
.stag:hover{transform:translateY(-2px);filter:brightness(1.2);}
.stag.w{background:rgba(56,189,248,.1);border-color:rgba(56,189,248,.3);color:#7DD3FC;}
.stag.f{background:rgba(2,171,255,.1);border-color:rgba(2,171,255,.3);color:#67E8F9;}

/* SKILLS GRID */
.skills-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:13px;}
.skill-card{
  background:rgba(255,255,255,.025);
  border:1px solid rgba(255,255,255,.07);
  border-radius:14px;padding:18px 16px;
  transition:all .3s;cursor:none;position:relative;overflow:hidden;
}
.skill-card::after{content:'';position:absolute;inset:0;opacity:0;background:linear-gradient(135deg,rgba(56,189,248,.06),transparent);transition:opacity .3s;}
.skill-card:hover::after{opacity:1;}
.skill-card:hover{transform:translateY(-4px);border-color:rgba(56,189,248,.2);box-shadow:0 8px 26px rgba(56,189,248,.1);}
.sk-label{font-size:10.5px;font-weight:700;letter-spacing:2px;color:rgba(56,189,248,.5);text-transform:uppercase;margin-bottom:10px;}
.sk-tags{display:flex;flex-wrap:wrap;gap:6px;}
.sk-tag{
  padding:3px 10px;border-radius:100px;font-size:11px;font-weight:500;
  background:rgba(56,189,248,.08);border:1px solid rgba(56,189,248,.2);color:#BAE6FD;
}

/* STATS */
.stats-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(165px,1fr));gap:13px;}
.stat-card{background:linear-gradient(135deg,rgba(56,189,248,.08),rgba(2,171,255,.04));border:1px solid rgba(56,189,248,.14);border-radius:16px;padding:22px 18px;text-align:center;position:relative;overflow:hidden;transition:all .3s;cursor:none;}
.stat-card::after{content:'';position:absolute;bottom:0;left:0;right:0;height:2px;background:linear-gradient(90deg,var(--web),var(--fl));transform:scaleX(0);transition:transform .3s;}
.stat-card:hover::after{transform:scaleX(1);}
.stat-card:hover{transform:translateY(-5px);box-shadow:0 10px 32px rgba(56,189,248,.15);border-color:rgba(56,189,248,.28);}
.stat-num{font-size:34px;font-weight:800;line-height:1.1;margin-bottom:5px;background:linear-gradient(135deg,var(--web),var(--fl));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;}
.stat-lbl{font-size:11.5px;color:rgba(226,232,240,.42);font-weight:500;letter-spacing:.4px;}

/* lang bars */
.lang-box{background:rgba(255,255,255,.02);border:1px solid rgba(56,189,248,.1);border-radius:16px;padding:22px;margin-top:18px;}
.lang-hd{font-size:13px;color:rgba(226,232,240,.38);margin-bottom:16px;font-weight:500;}
.lang-row{display:flex;align-items:center;gap:10px;margin-bottom:11px;}
.lang-name{font-family:'Fira Code',monospace;font-size:12px;color:rgba(226,232,240,.6);width:82px;flex-shrink:0;}
.lang-track{flex:1;height:7px;background:rgba(255,255,255,.05);border-radius:100px;overflow:hidden;}
.lang-fill{height:100%;border-radius:100px;transform:scaleX(0);transform-origin:left;animation:barG 1.4s cubic-bezier(.16,1,.3,1) forwards;}
@keyframes barG{to{transform:scaleX(1);}}
.lang-pct{font-family:'Fira Code',monospace;font-size:11.5px;color:rgba(56,189,248,.7);width:32px;text-align:right;flex-shrink:0;}

/* commit grid */
.commit-box{background:rgba(255,255,255,.02);border:1px solid rgba(56,189,248,.1);border-radius:16px;padding:22px;margin-top:14px;}
.commit-lbl{font-size:13px;color:rgba(226,232,240,.38);margin-bottom:13px;font-weight:500;}
.cgrid{display:grid;grid-template-columns:repeat(52,1fr);gap:2.5px;}
.cc{aspect-ratio:1;border-radius:2px;background:rgba(255,255,255,.04);}
.cc.l1{background:rgba(56,189,248,.2);}
.cc.l2{background:rgba(56,189,248,.45);}
.cc.l3{background:rgba(56,189,248,.7);}
.cc.l4{background:rgba(56,189,248,1);}

/* CONNECT */
.connect-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(185px,1fr));gap:13px;}
.conn-card{background:rgba(255,255,255,.02);border:1px solid rgba(255,255,255,.06);border-radius:14px;padding:17px 18px;display:flex;align-items:center;gap:13px;text-decoration:none;color:inherit;transition:all .3s;cursor:none;position:relative;overflow:hidden;}
.conn-card::before{content:'';position:absolute;inset:0;opacity:0;transition:opacity .3s;}
.conn-card.li::before{background:linear-gradient(135deg,rgba(10,102,194,.14),transparent);}
.conn-card.gh::before{background:linear-gradient(135deg,rgba(255,255,255,.05),transparent);}
.conn-card:hover::before{opacity:1;}
.conn-card:hover{transform:translateY(-4px);border-color:rgba(56,189,248,.25);box-shadow:0 8px 26px rgba(0,0,0,.3);color:inherit;}
.conn-icon{font-size:25px;flex-shrink:0;}
.conn-name{font-size:13.5px;font-weight:600;color:#e2e8f0;}
.conn-sub{font-size:11.5px;color:rgba(226,232,240,.38);margin-top:2px;}

/* footer */
.footer{text-align:center;padding:26px 20px;border-top:1px solid rgba(255,255,255,.04);}
.footer p{font-size:12px;color:rgba(226,232,240,.22);}
.heart{color:#F472B6;animation:hb 1.2s ease-in-out infinite;display:inline-block;}
@keyframes hb{0%,100%{transform:scale(1);}15%{transform:scale(1.35);}30%{transform:scale(1);}45%{transform:scale(1.15);}}

.fu{opacity:0;transform:translateY(18px);transition:opacity .65s ease,transform .65s ease;}
.fu.vis{opacity:1;transform:translateY(0);}
</style>

<!-- CURSOR -->
<div id="cursor"></div>
<div id="cursor-ring"></div>

<canvas id="bg"></canvas>
<div class="wrap">

<!-- HERO -->
<div class="hero">
  <div class="blob b1"></div><div class="blob b2"></div><div class="blob b3"></div>
  <div class="av-wrap"><div class="av-inner">👩‍💻</div></div>
  <h1 class="hero-name">Khushi Umrao</h1>
  <div class="type-row">
    <span class="type-txt" id="typ"></span><span class="caret"></span>
  </div>
  <div class="badge-row">
    <span class="badge b-web">🌐 Web Developer</span>
    <span class="badge b-fl">🦋 Flutter Dev</span>
    <span class="badge b-code">💻 Problem Solver</span>
  </div>
  <div class="views-row">
    <div class="views-card">
      <span class="live-dot"></span>
      <span class="eye-icon">👁</span>
      <span class="views-num" id="view-count">0</span>
      <span style="font-size:12px;color:rgba(56,189,248,.7);font-weight:500;">profile views</span>
    </div>
  </div>
</div>

<!-- FOCUS CARDS -->
<div class="section fu">
  <p class="sec-label">Core Focus</p>
  <h2 class="sec-title">🎯 What I <span>Specialise In</span></h2>
  <div class="divider"></div>
  <div class="dual">
    <div class="focus-card web">
      <div class="fc-icon">🌐</div>
      <div class="fc-title">Web Development</div>
      <div class="fc-sub">Building fast, responsive, pixel-perfect websites and web apps that feel great on any screen.</div>
      <div class="stags">
        <span class="stag w">HTML5</span>
        <span class="stag w">CSS3</span>
        <span class="stag w">JavaScript</span>
        <span class="stag w">Responsive Design</span>
      </div>
    </div>
    <div class="focus-card flutter">
      <div class="fc-icon">🦋</div>
      <div class="fc-title">Flutter & Dart</div>
      <div class="fc-sub">Beautiful cross-platform mobile apps with smooth animations and native-level performance.</div>
      <div class="stags">
        <span class="stag f">Flutter</span>
        <span class="stag f">Dart</span>
        <span class="stag f">Mobile UI</span>
        <span class="stag f">Cross-Platform</span>
      </div>
    </div>
  </div>
</div>

<!-- SKILLS (from resume) -->
<div class="section fu">
  <p class="sec-label">My Arsenal</p>
  <h2 class="sec-title">⚡ Skills & <span>Expertise</span></h2>
  <div class="divider"></div>
  <div class="skills-grid">
    <div class="skill-card">
      <div class="sk-label">Web Development</div>
      <div class="sk-tags">
        <span class="sk-tag">HTML</span>
        <span class="sk-tag">CSS</span>
        <span class="sk-tag">JavaScript</span>
      </div>
    </div>
    <div class="skill-card">
      <div class="sk-label">Mobile / Software</div>
      <div class="sk-tags">
        <span class="sk-tag">Flutter</span>
        <span class="sk-tag">Dart</span>
      </div>
    </div>
    <div class="skill-card">
      <div class="sk-label">Programming</div>
      <div class="sk-tags">
        <span class="sk-tag">C++ (Basic)</span>
        <span class="sk-tag">Python (Basic)</span>
      </div>
    </div>
    <div class="skill-card">
      <div class="sk-label">Soft Skills</div>
      <div class="sk-tags">
        <span class="sk-tag">Teamwork</span>
        <span class="sk-tag">Communication</span>
        <span class="sk-tag">Analytical</span>
        <span class="sk-tag">Time Mgmt</span>
      </div>
    </div>
    <div class="skill-card" style="grid-column:span 2;">
      <div class="sk-label">Research Skills</div>
      <div class="sk-tags">
        <span class="sk-tag">Technical Documentation</span>
        <span class="sk-tag">Literature Review</span>
        <span class="sk-tag">Experimentation</span>
        <span class="sk-tag">Data Analysis</span>
        <span class="sk-tag">Prototype Testing</span>
        <span class="sk-tag">Report Writing</span>
      </div>
    </div>
  </div>
</div>

<!-- STATS -->
<div class="section fu" id="stats-sec">
  <p class="sec-label">By the Numbers</p>
  <h2 class="sec-title">📈 GitHub <span>Analytics</span></h2>
  <div class="divider"></div>
  <div class="stats-grid">
    <div class="stat-card"><div class="stat-num" id="c1">0</div><div class="stat-lbl">Total Commits</div></div>
    <div class="stat-card"><div class="stat-num" id="c2">0</div><div class="stat-lbl">Repositories</div></div>
    <div class="stat-card"><div class="stat-num" id="c3">0</div><div class="stat-lbl">Day Streak 🔥</div></div>
    <div class="stat-card"><div class="stat-num" id="c4">0</div><div class="stat-lbl">Stars Earned</div></div>
  </div>
  <div class="lang-box">
    <div class="lang-hd">Top Languages</div>
    <div class="lang-row"><span class="lang-name">Dart</span><div class="lang-track"><div class="lang-fill" style="width:48%;background:linear-gradient(90deg,#54C5F8,#02ABFF);animation-delay:.1s"></div></div><span class="lang-pct">48%</span></div>
    <div class="lang-row"><span class="lang-name">JavaScript</span><div class="lang-track"><div class="lang-fill" style="width:30%;background:linear-gradient(90deg,#38BDF8,#818CF8);animation-delay:.25s"></div></div><span class="lang-pct">30%</span></div>
    <div class="lang-row"><span class="lang-name">HTML</span><div class="lang-track"><div class="lang-fill" style="width:14%;background:linear-gradient(90deg,#7DD3FC,#38BDF8);animation-delay:.4s"></div></div><span class="lang-pct">14%</span></div>
    <div class="lang-row"><span class="lang-name">CSS</span><div class="lang-track"><div class="lang-fill" style="width:8%;background:linear-gradient(90deg,#67E8F9,#02ABFF);animation-delay:.55s"></div></div><span class="lang-pct">8%</span></div>
  </div>
  <div class="commit-box">
    <div class="commit-lbl">Contribution activity — past year</div>
    <div class="cgrid" id="cgrid"></div>
  </div>
</div>

<!-- CONNECT -->
<div class="section fu">
  <p class="sec-label">Let's Talk</p>
  <h2 class="sec-title">🌐 Connect <span>With Me</span></h2>
  <div class="divider"></div>
  <div class="connect-grid">
    <a href="https://www.linkedin.com/in/khushi-umrao/" class="conn-card li" target="_blank">
      <span class="conn-icon">💼</span><div><div class="conn-name">LinkedIn</div><div class="conn-sub">Khushi Umrao</div></div>
    </a>
    <a href="https://github.com/LunaKhushi" class="conn-card gh" target="_blank">
      <span class="conn-icon">🐙</span><div><div class="conn-name">GitHub</div><div class="conn-sub">@LunaKhushi</div></div>
    </a>
    <div class="conn-card">
      <span class="conn-icon">✉️</span><div><div class="conn-name">Email</div><div class="conn-sub">Open for collabs</div></div>
    </div>
    <div class="conn-card">
      <span class="conn-icon">🚀</span><div><div class="conn-name">Portfolio</div><div class="conn-sub">Coming soon...</div></div>
    </div>
  </div>
</div>

<!-- FOOTER -->
<div class="footer">
  <p>Made with <span class="heart">♥</span> by Khushi Umrao · Web Dev & Flutter · Thanks for visiting ✨</p>
</div>

</div>

<script>
/* ── CURSOR ── */
const cur = document.getElementById('cursor');
const ring = document.getElementById('cursor-ring');
let mx=0,my=0,rx=0,ry=0;
const trails = [];
const TRAIL = 8;
for(let i=0;i<TRAIL;i++){
  const d=document.createElement('div');
  d.className='trail-dot';
  d.style.opacity=(1-i/TRAIL)*0.5;
  d.style.width=(5-i*0.4)+'px';
  d.style.height=(5-i*0.4)+'px';
  document.body.appendChild(d);
  trails.push({el:d,x:0,y:0});
}
document.addEventListener('mousemove',e=>{mx=e.clientX;my=e.clientY;});
document.addEventListener('mousedown',()=>{cur.style.transform='translate(-50%,-50%) scale(0.7)';ring.style.transform='translate(-50%,-50%) scale(1.4)';ring.style.borderColor='rgba(192,132,252,.8)';});
document.addEventListener('mouseup',()=>{cur.style.transform='translate(-50%,-50%) scale(1)';ring.style.transform='translate(-50%,-50%) scale(1)';ring.style.borderColor='rgba(56,189,248,.5)';});
const hoverEls = document.querySelectorAll('a,.focus-card,.skill-card,.stat-card,.conn-card,.stag,.badge,.build-card');
hoverEls.forEach(el=>{
  el.addEventListener('mouseenter',()=>{cur.style.width='20px';cur.style.height='20px';ring.style.width='52px';ring.style.height='52px';ring.style.borderColor='rgba(192,132,252,.7)';});
  el.addEventListener('mouseleave',()=>{cur.style.width='14px';cur.style.height='14px';ring.style.width='38px';ring.style.height='38px';ring.style.borderColor='rgba(56,189,248,.5)';});
});
let trailPositions=Array(TRAIL).fill({x:0,y:0});
function animCursor(){
  cur.style.left=mx+'px';cur.style.top=my+'px';
  rx+=(mx-rx)*.12;ry+=(my-ry)*.12;
  ring.style.left=rx+'px';ring.style.top=ry+'px';
  trailPositions=[{x:mx,y:my},...trailPositions.slice(0,-1)];
  trails.forEach((t,i)=>{
    t.el.style.left=trailPositions[i].x+'px';
    t.el.style.top=trailPositions[i].y+'px';
  });
  requestAnimationFrame(animCursor);
}
animCursor();

/* ── STARS ── */
const cv=document.getElementById('bg');const cx=cv.getContext('2d');
let stars=[];
function resize(){cv.width=window.innerWidth;cv.height=Math.max(document.body.scrollHeight,window.innerHeight*3);}
function mkStars(){stars=[];for(let i=0;i<200;i++)stars.push({x:Math.random()*cv.width,y:Math.random()*cv.height,r:Math.random()*1.4+.2,o:Math.random()*.6+.15,s:Math.random()*.01+.003,t:Math.random()*Math.PI*2});}
function drawStars(){
  cx.clearRect(0,0,cv.width,cv.height);
  const now=Date.now()*.001;
  stars.forEach(s=>{const a=s.o*(.5+.5*Math.sin(now*s.s*8+s.t));cx.beginPath();cx.arc(s.x,s.y,s.r,0,Math.PI*2);cx.fillStyle=`rgba(56,189,248,${a*.7})`;cx.fill();});
  requestAnimationFrame(drawStars);
}
resize();mkStars();drawStars();
window.addEventListener('resize',()=>{resize();mkStars();});

/* ── TYPING ── */
const phrases=["Web Developer ✦ Flutter Dev","HTML · CSS · JS · Dart","Building for Web & Mobile","Code. Design. Ship. Repeat.","Transforming Ideas into Apps"];
let pi=0,ci=0,del=false,txt='';
const tel=document.getElementById('typ');
function type(){
  const p=phrases[pi];
  if(!del){txt=p.slice(0,++ci);if(ci===p.length)setTimeout(()=>{del=true;},2200);}
  else{txt=p.slice(0,--ci);if(ci===0){del=false;pi=(pi+1)%phrases.length;}}
  tel.textContent=txt;setTimeout(type,del?42:74);
}
type();

/* ── PROFILE VIEWS COUNTER ── */
let viewTarget = 1247;
const vEl = document.getElementById('view-count');
function animViews(){
  const s=Date.now();const dur=2000;
  function t(){const p=Math.min((Date.now()-s)/dur,1);const e=1-Math.pow(1-p,3);vEl.textContent=Math.round(e*viewTarget).toLocaleString();if(p<1)requestAnimationFrame(t);}
  t();
}
setTimeout(animViews,600);
setInterval(()=>{
  viewTarget+=Math.floor(Math.random()*3)+1;
  vEl.textContent=viewTarget.toLocaleString();
},8000);

/* ── COUNTERS ── */
function count(id,target,dur){
  const el=document.getElementById(id);const s=Date.now();
  function t(){const p=Math.min((Date.now()-s)/dur,1);const e=1-Math.pow(1-p,3);el.textContent=Math.round(e*target);if(p<1)requestAnimationFrame(t);}
  t();
}

/* ── COMMIT GRID ── */
function buildGrid(){
  const g=document.getElementById('cgrid');
  const lv=['','l1','l2','l3','l4'];const w=[.42,.26,.16,.1,.06];
  for(let i=0;i<364;i++){
    const r=Math.random();let cum=0,cl='';
    for(let j=0;j<w.length;j++){cum+=w[j];if(r<cum){cl=lv[j];break;}}
    const c=document.createElement('div');c.className='cc'+(cl?' '+cl:'');g.appendChild(c);
  }
}
buildGrid();

/* ── OBSERVER ── */
let counted=false;
const obs=new IntersectionObserver(entries=>{
  entries.forEach(e=>{
    if(e.isIntersecting){
      e.target.classList.add('vis');
      if(e.target.id==='stats-sec'&&!counted){
        counted=true;count('c1',247,1400);count('c2',18,1100);count('c3',42,900);count('c4',89,1200);
      }
      obs.unobserve(e.target);
    }
  });
},{threshold:.1});
document.querySelectorAll('.fu').forEach(el=>obs.observe(el));
document.getElementById('stats-sec')&&obs.observe(document.getElementById('stats-sec'));
</script>
