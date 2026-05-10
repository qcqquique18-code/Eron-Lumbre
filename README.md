<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ERON LUMBRE — Visual Artist</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;700;800&family=Space+Mono:wght@400;700&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
/* ═══════════════════════════════════════
   VARIABLES & BASE
═══════════════════════════════════════ */
:root {
  --black: #050505;
  --platinum: #D9D9D9;
  --cold-white: #F5F8FF;
  --electric: #6A5BFF;
  --electric-dim: rgba(106,91,255,0.15);
  --glow: rgba(106,91,255,0.4);
  --metal: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 50%, #111 100%);
  --font-title: 'Syne', sans-serif;
  --font-body: 'DM Sans', sans-serif;
  --font-mono: 'Space Mono', monospace;
}

*, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }

html { scroll-behavior: smooth; }

body {
  background: var(--black);
  color: var(--platinum);
  font-family: var(--font-body);
  overflow-x: hidden;
  cursor: none;
}

/* ═══════════════════════════════════════
   CUSTOM CURSOR
═══════════════════════════════════════ */
#cursor {
  width: 12px; height: 12px;
  background: var(--electric);
  border-radius: 50%;
  position: fixed; top: 0; left: 0;
  pointer-events: none; z-index: 9999;
  transition: transform 0.1s, opacity 0.2s;
  mix-blend-mode: screen;
}
#cursor-trail {
  width: 36px; height: 36px;
  border: 1px solid rgba(106,91,255,0.5);
  border-radius: 50%;
  position: fixed; top: 0; left: 0;
  pointer-events: none; z-index: 9998;
  transition: transform 0.35s cubic-bezier(0.23,1,0.32,1), opacity 0.3s;
}
body:hover #cursor { opacity: 1; }

/* ═══════════════════════════════════════
   CANVAS BACKGROUND
═══════════════════════════════════════ */
#bg-canvas {
  position: fixed; top:0; left:0;
  width:100%; height:100%;
  z-index: 0;
  pointer-events: none;
}

/* ═══════════════════════════════════════
   NAVBAR
═══════════════════════════════════════ */
nav {
  position: fixed; top: 0; left: 0; right: 0;
  z-index: 1000;
  padding: 20px 48px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  backdrop-filter: blur(20px) saturate(180%);
  background: rgba(5,5,5,0.6);
  border-bottom: 1px solid rgba(217,217,217,0.05);
  transition: all 0.4s;
}
nav.scrolled {
  padding: 14px 48px;
  background: rgba(5,5,5,0.85);
}
.nav-logo {
  font-family: var(--font-title);
  font-weight: 800;
  font-size: 1.1rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--cold-white);
  text-decoration: none;
}
.nav-links {
  display: flex; gap: 36px; list-style: none;
}
.nav-links a {
  font-family: var(--font-mono);
  font-size: 0.65rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(217,217,217,0.5);
  text-decoration: none;
  transition: color 0.3s;
  position: relative;
}
.nav-links a::after {
  content:'';
  position:absolute; bottom:-4px; left:0;
  width:0; height:1px;
  background: var(--electric);
  transition: width 0.3s;
}
.nav-links a:hover { color: var(--cold-white); }
.nav-links a:hover::after { width: 100%; }
.nav-cta {
  font-family: var(--font-mono);
  font-size: 0.6rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--electric);
  border: 1px solid var(--electric);
  padding: 8px 20px;
  text-decoration: none;
  transition: all 0.3s;
}
.nav-cta:hover {
  background: var(--electric);
  color: var(--black);
}

/* ═══════════════════════════════════════
   HERO
═══════════════════════════════════════ */
#hero {
  position: relative; z-index: 1;
  height: 100vh;
  display: flex; flex-direction: column;
  align-items: center; justify-content: center;
  text-align: center;
  overflow: hidden;
}

.hero-eyebrow {
  font-family: var(--font-mono);
  font-size: 0.6rem;
  letter-spacing: 0.4em;
  text-transform: uppercase;
  color: var(--electric);
  margin-bottom: 28px;
  opacity: 0;
  animation: fadeUp 0.8s 0.3s forwards;
}

.hero-title {
  font-family: var(--font-title);
  font-weight: 800;
  font-size: clamp(4rem, 12vw, 11rem);
  line-height: 0.9;
  letter-spacing: -0.03em;
  text-transform: uppercase;
  color: var(--cold-white);
  opacity: 0;
  animation: fadeUp 0.9s 0.5s forwards;
  position: relative;
}
.hero-title span {
  display: block;
  background: linear-gradient(135deg, #fff 0%, var(--platinum) 40%, var(--electric) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-subtitle {
  font-family: var(--font-mono);
  font-size: clamp(0.55rem, 1.2vw, 0.75rem);
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: rgba(217,217,217,0.45);
  margin-top: 28px;
  opacity: 0;
  animation: fadeUp 0.9s 0.75s forwards;
}

.hero-tagline {
  font-family: var(--font-body);
  font-weight: 300;
  font-size: clamp(0.85rem, 1.6vw, 1.1rem);
  color: rgba(217,217,217,0.6);
  margin-top: 20px;
  font-style: italic;
  opacity: 0;
  animation: fadeUp 0.9s 0.95s forwards;
}

.hero-cta {
  margin-top: 52px;
  opacity: 0;
  animation: fadeUp 0.9s 1.2s forwards;
}
.btn-primary {
  display: inline-block;
  font-family: var(--font-mono);
  font-size: 0.65rem;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: var(--black);
  background: var(--cold-white);
  padding: 18px 44px;
  text-decoration: none;
  position: relative;
  overflow: hidden;
  transition: all 0.4s;
}
.btn-primary::before {
  content:'';
  position:absolute; inset:0;
  background: var(--electric);
  transform: translateX(-100%);
  transition: transform 0.4s cubic-bezier(0.23,1,0.32,1);
}
.btn-primary:hover::before { transform: translateX(0); }
.btn-primary:hover { color: var(--cold-white); }
.btn-primary span { position: relative; z-index:1; }

.hero-scroll {
  position: absolute;
  bottom: 40px;
  left: 50%;
  transform: translateX(-50%);
  display: flex; flex-direction: column;
  align-items: center; gap: 8px;
  opacity: 0;
  animation: fadeUp 1s 1.6s forwards;
}
.hero-scroll span {
  font-family: var(--font-mono);
  font-size: 0.5rem;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: rgba(217,217,217,0.3);
}
.scroll-line {
  width: 1px; height: 60px;
  background: linear-gradient(to bottom, rgba(106,91,255,0.7), transparent);
  animation: scrollPulse 2s infinite;
}

/* Hero orbital rings */
.hero-rings {
  position: absolute;
  width: 600px; height: 600px;
  left: 50%; top: 50%;
  transform: translate(-50%,-50%);
  pointer-events: none;
}
.ring {
  position: absolute;
  border-radius: 50%;
  border: 1px solid rgba(106,91,255,0.12);
  top:50%; left:50%;
  transform: translate(-50%,-50%);
  animation: rotateRing linear infinite;
}
.ring:nth-child(1) { width:300px; height:300px; animation-duration:20s; border-color: rgba(106,91,255,0.2); }
.ring:nth-child(2) { width:450px; height:450px; animation-duration:35s; animation-direction: reverse; border-style: dashed; }
.ring:nth-child(3) { width:600px; height:600px; animation-duration:50s; }

/* ═══════════════════════════════════════
   SECTION STRUCTURE
═══════════════════════════════════════ */
section {
  position: relative; z-index: 1;
  padding: 120px 48px;
}
.section-header {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  margin-bottom: 72px;
  border-bottom: 1px solid rgba(217,217,217,0.08);
  padding-bottom: 24px;
}
.section-label {
  font-family: var(--font-mono);
  font-size: 0.55rem;
  letter-spacing: 0.35em;
  text-transform: uppercase;
  color: var(--electric);
  display: block;
  margin-bottom: 10px;
}
.section-title {
  font-family: var(--font-title);
  font-weight: 800;
  font-size: clamp(2.5rem, 6vw, 5rem);
  text-transform: uppercase;
  line-height: 0.9;
  letter-spacing: -0.02em;
  color: var(--cold-white);
}
.section-count {
  font-family: var(--font-mono);
  font-size: 0.55rem;
  letter-spacing: 0.2em;
  color: rgba(217,217,217,0.25);
}

/* ═══════════════════════════════════════
   ARCHIVE / PORTFOLIO GRID
═══════════════════════════════════════ */
#archive { background: transparent; }

.filter-bar {
  display: flex; gap: 4px;
  margin-bottom: 48px;
  flex-wrap: wrap;
}
.filter-btn {
  font-family: var(--font-mono);
  font-size: 0.55rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  padding: 10px 20px;
  background: transparent;
  border: 1px solid rgba(217,217,217,0.1);
  color: rgba(217,217,217,0.4);
  cursor: none;
  transition: all 0.3s;
}
.filter-btn:hover, .filter-btn.active {
  border-color: var(--electric);
  color: var(--cold-white);
  background: var(--electric-dim);
}

.portfolio-grid {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 16px;
}

.artwork-card {
  position: relative;
  overflow: hidden;
  background: #0d0d0d;
  cursor: none;
  border: 1px solid rgba(217,217,217,0.05);
  transition: border-color 0.4s;
}
.artwork-card:hover { border-color: rgba(106,91,255,0.4); }

/* Grid layout irregolare */
.artwork-card:nth-child(1)  { grid-column: span 7; grid-row: span 2; aspect-ratio: auto; min-height: 560px; }
.artwork-card:nth-child(2)  { grid-column: span 5; }
.artwork-card:nth-child(3)  { grid-column: span 5; }
.artwork-card:nth-child(4)  { grid-column: span 4; }
.artwork-card:nth-child(5)  { grid-column: span 4; }
.artwork-card:nth-child(6)  { grid-column: span 4; }
.artwork-card:nth-child(7)  { grid-column: span 8; }
.artwork-card:nth-child(8)  { grid-column: span 4; }
.artwork-card:nth-child(n+9){ grid-column: span 4; }

.artwork-img {
  width: 100%; height: 100%;
  min-height: 280px;
  object-fit: cover;
  display: block;
  transition: transform 0.7s cubic-bezier(0.23,1,0.32,1), filter 0.5s;
  filter: brightness(0.85) saturate(0.9);
}
.artwork-card:hover .artwork-img {
  transform: scale(1.05);
  filter: brightness(1) saturate(1.1);
}

.artwork-overlay {
  position: absolute; inset: 0;
  background: linear-gradient(to top, rgba(5,5,5,0.95) 0%, rgba(5,5,5,0.1) 60%, transparent 100%);
  opacity: 0;
  transition: opacity 0.4s;
  display: flex; flex-direction: column;
  justify-content: flex-end;
  padding: 28px;
}
.artwork-card:hover .artwork-overlay { opacity: 1; }

.artwork-cat {
  font-family: var(--font-mono);
  font-size: 0.5rem;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: var(--electric);
  margin-bottom: 6px;
}
.artwork-name {
  font-family: var(--font-title);
  font-weight: 700;
  font-size: 1.2rem;
  color: var(--cold-white);
  line-height: 1.1;
}
.artwork-year {
  font-family: var(--font-mono);
  font-size: 0.5rem;
  color: rgba(217,217,217,0.4);
  margin-top: 6px;
  letter-spacing: 0.2em;
}

.artwork-glow {
  position: absolute; inset: 0;
  box-shadow: inset 0 0 60px rgba(106,91,255,0);
  transition: box-shadow 0.4s;
  pointer-events: none;
}
.artwork-card:hover .artwork-glow {
  box-shadow: inset 0 0 60px rgba(106,91,255,0.12);
}

/* Placeholder when no image */
.artwork-placeholder {
  width: 100%; height: 100%;
  min-height: 280px;
  display: flex; flex-direction: column;
  align-items: center; justify-content: center;
  background: linear-gradient(135deg, #0d0d0d 0%, #111 50%, #0a0a0a 100%);
  color: rgba(217,217,217,0.15);
  font-family: var(--font-mono);
  font-size: 0.6rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  gap: 12px;
}
.artwork-placeholder svg { opacity: 0.2; }

/* ═══════════════════════════════════════
   ABOUT
═══════════════════════════════════════ */
#about { overflow: hidden; }

.about-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 80px;
  align-items: center;
}
.about-text p {
  font-size: 1.05rem;
  line-height: 1.8;
  color: rgba(217,217,217,0.65);
  margin-bottom: 20px;
  font-weight: 300;
}
.about-text p strong {
  color: var(--cold-white);
  font-weight: 500;
}

.skills-grid {
  margin-top: 48px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 32px;
}
.skill-group h4 {
  font-family: var(--font-mono);
  font-size: 0.55rem;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: var(--electric);
  margin-bottom: 14px;
}
.skill-item {
  display: flex; align-items: center;
  gap: 12px;
  margin-bottom: 10px;
}
.skill-bar-wrap {
  flex: 1;
  height: 1px;
  background: rgba(217,217,217,0.1);
  position: relative;
  overflow: hidden;
}
.skill-bar {
  height: 100%;
  background: linear-gradient(to right, var(--electric), rgba(106,91,255,0.3));
  width: 0;
  transition: width 1.5s cubic-bezier(0.23,1,0.32,1);
}
.skill-name {
  font-family: var(--font-mono);
  font-size: 0.55rem;
  letter-spacing: 0.1em;
  color: rgba(217,217,217,0.5);
  min-width: 100px;
}

.about-visual {
  position: relative;
}
.about-img-wrap {
  position: relative;
  aspect-ratio: 3/4;
  overflow: hidden;
  border: 1px solid rgba(217,217,217,0.08);
}
.about-img-wrap img {
  width:100%; height:100%;
  object-fit: cover;
  filter: saturate(0.7) brightness(0.8);
}
.about-img-frame {
  position: absolute; inset: -1px;
  border: 1px solid rgba(106,91,255,0.3);
  pointer-events: none;
}
.about-img-frame::before, .about-img-frame::after {
  content: '';
  position: absolute;
  width: 24px; height: 24px;
  border-color: var(--electric);
  border-style: solid;
}
.about-img-frame::before { top:-1px; left:-1px; border-width: 2px 0 0 2px; }
.about-img-frame::after  { bottom:-1px; right:-1px; border-width: 0 2px 2px 0; }

.about-stats {
  position: absolute;
  bottom: -24px; right: -24px;
  background: rgba(5,5,5,0.9);
  border: 1px solid rgba(106,91,255,0.2);
  padding: 24px 28px;
  backdrop-filter: blur(20px);
}
.stat-num {
  font-family: var(--font-title);
  font-weight: 800;
  font-size: 2.2rem;
  color: var(--cold-white);
  line-height: 1;
}
.stat-label {
  font-family: var(--font-mono);
  font-size: 0.5rem;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: rgba(217,217,217,0.4);
  margin-top: 4px;
}

/* ═══════════════════════════════════════
   CONTACT
═══════════════════════════════════════ */
#contact { max-width: 800px; margin: 0 auto; }

.contact-headline {
  font-family: var(--font-title);
  font-weight: 800;
  font-size: clamp(3rem, 8vw, 7rem);
  text-transform: uppercase;
  line-height: 0.9;
  letter-spacing: -0.03em;
  color: var(--cold-white);
  margin-bottom: 16px;
}
.contact-sub {
  font-family: var(--font-body);
  font-size: 1rem;
  color: rgba(217,217,217,0.4);
  font-style: italic;
  margin-bottom: 60px;
}

.contact-form { display: grid; gap: 20px; }
.form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }

.field-wrap { position: relative; }
.field-wrap label {
  display: block;
  font-family: var(--font-mono);
  font-size: 0.5rem;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: var(--electric);
  margin-bottom: 10px;
}
.field-wrap input,
.field-wrap select,
.field-wrap textarea {
  width: 100%;
  background: transparent;
  border: none;
  border-bottom: 1px solid rgba(217,217,217,0.15);
  padding: 12px 0;
  color: var(--cold-white);
  font-family: var(--font-body);
  font-size: 0.9rem;
  outline: none;
  transition: border-color 0.3s;
  cursor: none;
}
.field-wrap select option { background: #111; }
.field-wrap input:focus,
.field-wrap select:focus,
.field-wrap textarea:focus { border-color: var(--electric); }
.field-wrap textarea { resize: none; height: 100px; }

.btn-transmit {
  display: inline-block;
  font-family: var(--font-mono);
  font-size: 0.65rem;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  padding: 20px 52px;
  background: transparent;
  border: 1px solid var(--electric);
  color: var(--electric);
  cursor: none;
  position: relative;
  overflow: hidden;
  transition: color 0.4s;
  margin-top: 12px;
  width: 100%;
  text-align: center;
}
.btn-transmit::before {
  content:'';
  position:absolute; inset:0;
  background: var(--electric);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.4s cubic-bezier(0.23,1,0.32,1);
}
.btn-transmit:hover::before { transform: scaleX(1); }
.btn-transmit:hover { color: var(--black); }
.btn-transmit span { position: relative; z-index:1; }

.contact-socials {
  display: flex; gap: 32px;
  margin-top: 52px;
  padding-top: 32px;
  border-top: 1px solid rgba(217,217,217,0.06);
}
.social-link {
  font-family: var(--font-mono);
  font-size: 0.55rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(217,217,217,0.3);
  text-decoration: none;
  transition: color 0.3s;
}
.social-link:hover { color: var(--electric); }

/* ═══════════════════════════════════════
   FOOTER
═══════════════════════════════════════ */
footer {
  position: relative; z-index:1;
  padding: 40px 48px;
  border-top: 1px solid rgba(217,217,217,0.05);
  display: flex; align-items: center;
  justify-content: space-between;
}
.footer-logo {
  font-family: var(--font-title);
  font-weight: 800;
  font-size: 0.9rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(217,217,217,0.3);
}
.footer-signal {
  font-family: var(--font-mono);
  font-size: 0.5rem;
  letter-spacing: 0.2em;
  color: rgba(217,217,217,0.2);
  font-style: italic;
}
.footer-copy {
  font-family: var(--font-mono);
  font-size: 0.5rem;
  letter-spacing: 0.15em;
  color: rgba(217,217,217,0.15);
}

/* ═══════════════════════════════════════
   LIGHTBOX
═══════════════════════════════════════ */
#lightbox {
  position: fixed; inset: 0;
  z-index: 5000;
  background: rgba(5,5,5,0.97);
  display: flex; align-items: center; justify-content: center;
  opacity: 0; pointer-events: none;
  transition: opacity 0.4s;
  backdrop-filter: blur(20px);
}
#lightbox.open { opacity: 1; pointer-events: all; }
#lightbox img {
  max-width: 85vw;
  max-height: 85vh;
  object-fit: contain;
  border: 1px solid rgba(106,91,255,0.2);
}
#lightbox-close {
  position: absolute;
  top: 32px; right: 40px;
  font-family: var(--font-mono);
  font-size: 0.6rem;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: rgba(217,217,217,0.4);
  cursor: none;
  transition: color 0.3s;
  background: none; border: none;
}
#lightbox-close:hover { color: var(--electric); }
#lightbox-info {
  position: absolute;
  bottom: 40px; left: 50%;
  transform: translateX(-50%);
  text-align: center;
}
#lightbox-title {
  font-family: var(--font-title);
  font-weight: 700;
  font-size: 1.4rem;
  color: var(--cold-white);
}
#lightbox-meta {
  font-family: var(--font-mono);
  font-size: 0.55rem;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: rgba(217,217,217,0.35);
  margin-top: 6px;
}

/* ═══════════════════════════════════════
   ADMIN PANEL
═══════════════════════════════════════ */
#admin-toggle {
  position: fixed;
  bottom: 32px; right: 32px;
  z-index: 4000;
  width: 48px; height: 48px;
  background: rgba(5,5,5,0.8);
  border: 1px solid rgba(106,91,255,0.3);
  display: flex; align-items: center; justify-content: center;
  cursor: none;
  transition: all 0.3s;
  backdrop-filter: blur(10px);
}
#admin-toggle:hover { border-color: var(--electric); background: var(--electric-dim); }
#admin-toggle svg { pointer-events: none; }

#admin-panel {
  position: fixed;
  bottom: 96px; right: 32px;
  z-index: 4000;
  width: 380px;
  background: rgba(8,8,8,0.97);
  border: 1px solid rgba(106,91,255,0.25);
  backdrop-filter: blur(30px);
  transform: translateY(20px) scale(0.96);
  opacity: 0;
  pointer-events: none;
  transition: all 0.35s cubic-bezier(0.23,1,0.32,1);
}
#admin-panel.open {
  transform: translateY(0) scale(1);
  opacity: 1;
  pointer-events: all;
}

.admin-header {
  padding: 20px 24px 16px;
  border-bottom: 1px solid rgba(217,217,217,0.06);
  display: flex; align-items: center; justify-content: space-between;
}
.admin-title {
  font-family: var(--font-mono);
  font-size: 0.6rem;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: var(--electric);
}
.admin-badge {
  font-family: var(--font-mono);
  font-size: 0.45rem;
  letter-spacing: 0.2em;
  color: rgba(217,217,217,0.25);
}

.admin-body { padding: 20px 24px; }

.admin-section-label {
  font-family: var(--font-mono);
  font-size: 0.5rem;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: rgba(217,217,217,0.35);
  margin-bottom: 12px;
  margin-top: 20px;
}
.admin-section-label:first-child { margin-top: 0; }

/* Upload drop zone */
.drop-zone {
  border: 1px dashed rgba(106,91,255,0.3);
  padding: 28px 20px;
  text-align: center;
  cursor: none;
  transition: all 0.3s;
  position: relative;
}
.drop-zone.drag-over {
  border-color: var(--electric);
  background: var(--electric-dim);
}
.drop-zone-icon { margin-bottom: 10px; opacity: 0.4; }
.drop-zone p {
  font-family: var(--font-mono);
  font-size: 0.55rem;
  letter-spacing: 0.15em;
  color: rgba(217,217,217,0.4);
  line-height: 1.7;
}
.drop-zone strong { color: var(--electric); }
#file-input { display: none; }

.admin-input {
  width: 100%;
  background: rgba(255,255,255,0.03);
  border: 1px solid rgba(217,217,217,0.1);
  padding: 10px 14px;
  color: var(--cold-white);
  font-family: var(--font-mono);
  font-size: 0.6rem;
  letter-spacing: 0.1em;
  outline: none;
  transition: border-color 0.3s;
  margin-bottom: 8px;
  cursor: none;
}
.admin-input:focus { border-color: var(--electric); }

.admin-select {
  width: 100%;
  background: rgba(255,255,255,0.03);
  border: 1px solid rgba(217,217,217,0.1);
  padding: 10px 14px;
  color: var(--cold-white);
  font-family: var(--font-mono);
  font-size: 0.6rem;
  letter-spacing: 0.1em;
  outline: none;
  margin-bottom: 8px;
  cursor: none;
  appearance: none;
}
.admin-select option { background: #0d0d0d; }

.btn-admin {
  width: 100%;
  padding: 12px;
  background: var(--electric);
  border: none;
  color: var(--black);
  font-family: var(--font-mono);
  font-size: 0.55rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  cursor: none;
  transition: opacity 0.3s;
  margin-top: 8px;
}
.btn-admin:hover { opacity: 0.85; }
.btn-admin.danger {
  background: transparent;
  border: 1px solid rgba(255,80,80,0.3);
  color: rgba(255,80,80,0.6);
}
.btn-admin.danger:hover { border-color: #ff5050; color: #ff5050; }

/* Manage list */
.manage-list {
  max-height: 200px;
  overflow-y: auto;
  scrollbar-width: thin;
  scrollbar-color: rgba(106,91,255,0.3) transparent;
}
.manage-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px 0;
  border-bottom: 1px solid rgba(217,217,217,0.04);
}
.manage-thumb {
  width: 40px; height: 40px;
  object-fit: cover;
  border: 1px solid rgba(217,217,217,0.08);
  flex-shrink: 0;
}
.manage-info { flex: 1; min-width: 0; }
.manage-name {
  font-family: var(--font-mono);
  font-size: 0.5rem;
  letter-spacing: 0.1em;
  color: rgba(217,217,217,0.6);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.manage-cat {
  font-family: var(--font-mono);
  font-size: 0.45rem;
  letter-spacing: 0.15em;
  color: var(--electric);
  text-transform: uppercase;
  margin-top: 2px;
}
.manage-del {
  background: none;
  border: none;
  color: rgba(255,80,80,0.4);
  cursor: none;
  font-size: 0.7rem;
  padding: 4px;
  transition: color 0.2s;
  flex-shrink: 0;
}
.manage-del:hover { color: #ff5050; }

/* Upload preview */
.upload-preview {
  display: flex; gap: 8px;
  flex-wrap: wrap;
  margin-top: 12px;
}
.preview-thumb {
  width: 60px; height: 60px;
  object-fit: cover;
  border: 1px solid rgba(106,91,255,0.3);
}

/* Status */
.status-msg {
  font-family: var(--font-mono);
  font-size: 0.5rem;
  letter-spacing: 0.15em;
  padding: 10px 0;
  text-align: center;
  transition: all 0.3s;
}
.status-msg.success { color: #50ff8c; }
.status-msg.error { color: #ff5050; }

/* ═══════════════════════════════════════
   ANIMATIONS
═══════════════════════════════════════ */
@keyframes fadeUp {
  from { opacity:0; transform: translateY(24px); }
  to   { opacity:1; transform: translateY(0); }
}
@keyframes rotateRing {
  from { transform: translate(-50%,-50%) rotate(0deg); }
  to   { transform: translate(-50%,-50%) rotate(360deg); }
}
@keyframes scrollPulse {
  0%,100% { opacity:0.3; transform: scaleY(1); }
  50% { opacity:0.8; transform: scaleY(1.1); }
}
@keyframes glitch {
  0%,100% { transform: none; opacity:1; }
  7% { transform: skew(-0.5deg,-0.9deg); }
  10% { transform: none; }
  27% { transform: none; }
  30% { transform: skew(0.8deg,-0.1deg); }
  35% { transform: none; }
}
.glitch { animation: glitch 4s infinite; }

/* Fade in on scroll */
.reveal {
  opacity: 0;
  transform: translateY(40px);
  transition: opacity 0.8s ease, transform 0.8s cubic-bezier(0.23,1,0.32,1);
}
.reveal.visible { opacity: 1; transform: translateY(0); }

/* ═══════════════════════════════════════
   MOBILE
═══════════════════════════════════════ */
@media (max-width: 900px) {
  nav { padding: 16px 24px; }
  .nav-links { display: none; }
  section { padding: 80px 24px; }
  .portfolio-grid { grid-template-columns: 1fr 1fr; }
  .artwork-card { grid-column: span 1 !important; grid-row: span 1 !important; min-height: 220px !important; }
  .about-grid { grid-template-columns: 1fr; }
  .about-visual { order: -1; }
  .about-stats { position: static; margin-top: 20px; }
  .form-row { grid-template-columns: 1fr; }
  footer { flex-direction: column; gap: 16px; text-align: center; }
  #admin-panel { width: calc(100vw - 48px); right: 24px; }
}
</style>
</head>
<body>

<!-- CURSOR -->
<div id="cursor"></div>
<div id="cursor-trail"></div>

<!-- BACKGROUND -->
<canvas id="bg-canvas"></canvas>

<!-- LIGHTBOX -->
<div id="lightbox">
  <button id="lightbox-close">[ close ]</button>
  <img id="lightbox-img" src="" alt="">
  <div id="lightbox-info">
    <div id="lightbox-title"></div>
    <div id="lightbox-meta"></div>
  </div>
</div>

<!-- NAVBAR -->
<nav id="navbar">
  <a href="#hero" class="nav-logo">ERON LUMBRE</a>
  <ul class="nav-links">
    <li><a href="#archive">Archive</a></li>
    <li><a href="#about">About</a></li>
    <li><a href="#contact">Contact</a></li>
  </ul>
  <a href="#contact" class="nav-cta">Initiate Contact</a>
</nav>

<!-- HERO -->
<section id="hero">
  <div class="hero-rings">
    <div class="ring"></div>
    <div class="ring"></div>
    <div class="ring"></div>
  </div>
  <div class="hero-eyebrow">Visual Organism // 001</div>
  <h1 class="hero-title glitch"><span>ERON</span><span>LUMBRE</span></h1>
  <div class="hero-subtitle">Visual Artist · Illustrator · Digital Sculptor · Worldbuilder</div>
  <p class="hero-tagline">"Building visual organisms between instinct and precision."</p>
  <div class="hero-cta">
    <a href="#archive" class="btn-primary"><span>ENTER THE ARCHIVE</span></a>
  </div>
  <div class="hero-scroll">
    <span>Scroll</span>
    <div class="scroll-line"></div>
  </div>
</section>

<!-- ARCHIVE -->
<section id="archive">
  <div class="section-header reveal">
    <div>
      <span class="section-label">// 001</span>
      <h2 class="section-title">Archive</h2>
    </div>
    <div class="section-count" id="artwork-count">— works</div>
  </div>

  <div class="filter-bar reveal" id="filter-bar">
    <button class="filter-btn active" data-cat="all">All</button>
    <button class="filter-btn" data-cat="Illustration">Illustration</button>
    <button class="filter-btn" data-cat="Concept Art">Concept Art</button>
    <button class="filter-btn" data-cat="Experimental">Experimental</button>
    <button class="filter-btn" data-cat="3D Sculpting">3D Sculpting</button>
    <button class="filter-btn" data-cat="Motion">Motion</button>
    <button class="filter-btn" data-cat="Hybrid">Hybrid</button>
  </div>

  <div class="portfolio-grid" id="portfolio-grid">
    <!-- Cards rendered by JS -->
  </div>
</section>

<!-- ABOUT -->
<section id="about">
  <div class="section-header reveal">
    <div>
      <span class="section-label">// 002</span>
      <h2 class="section-title">About</h2>
    </div>
  </div>
  <div class="about-grid">
    <div class="about-text reveal">
      <p>
        <strong>Eron Lumbre</strong> is a visual artist working between digital painting, sculptural experimentation and worldbuilding.
      </p>
      <p>
        His work reveals hidden forms through texture, distortion, emergent color and controlled instinct. Blending traditional artistic intuition with digital precision, he constructs <strong>impossible organisms suspended between discipline and chaos.</strong>
      </p>
      <p>
        Available for commissions, collaborations, and creative partnerships with studios, game developers, and collectors seeking genuinely original visual work.
      </p>
      <div class="skills-grid">
        <div class="skill-group">
          <h4>Digital Painting</h4>
          <div class="skill-item"><span class="skill-name">Photoshop</span><div class="skill-bar-wrap"><div class="skill-bar" data-w="92"></div></div></div>
          <div class="skill-item"><span class="skill-name">Procreate</span><div class="skill-bar-wrap"><div class="skill-bar" data-w="88"></div></div></div>
        </div>
        <div class="skill-group">
          <h4>3D & Sculpting</h4>
          <div class="skill-item"><span class="skill-name">Blender</span><div class="skill-bar-wrap"><div class="skill-bar" data-w="85"></div></div></div>
          <div class="skill-item"><span class="skill-name">Nomad Sculpt</span><div class="skill-bar-wrap"><div class="skill-bar" data-w="78"></div></div></div>
        </div>
        <div class="skill-group">
          <h4>Visual Dev</h4>
          <div class="skill-item"><span class="skill-name">Concept Design</span><div class="skill-bar-wrap"><div class="skill-bar" data-w="95"></div></div></div>
          <div class="skill-item"><span class="skill-name">Worldbuilding</span><div class="skill-bar-wrap"><div class="skill-bar" data-w="90"></div></div></div>
        </div>
        <div class="skill-group">
          <h4>Form Language</h4>
          <div class="skill-item"><span class="skill-name">Color Systems</span><div class="skill-bar-wrap"><div class="skill-bar" data-w="87"></div></div></div>
          <div class="skill-item"><span class="skill-name">Experimental</span><div class="skill-bar-wrap"><div class="skill-bar" data-w="93"></div></div></div>
        </div>
      </div>
    </div>
    <div class="about-visual reveal">
      <div class="about-img-wrap" id="about-img-wrap">
        <div style="width:100%;height:100%;min-height:400px;background:linear-gradient(135deg,#0d0d0d,#111);display:flex;align-items:center;justify-content:center;color:rgba(217,217,217,0.1);font-family:var(--font-mono);font-size:0.6rem;letter-spacing:0.2em;flex-direction:column;gap:12px;">
          <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1"><circle cx="12" cy="8" r="4"/><path d="M4 20c0-4 3.6-7 8-7s8 3 8 7"/></svg>
          ARTIST PHOTO
        </div>
        <div class="about-img-frame"></div>
      </div>
      <div class="about-stats">
        <div class="stat-num" id="stat-count">0</div>
        <div class="stat-label">Works in Archive</div>
      </div>
    </div>
  </div>
</section>

<!-- CONTACT -->
<section id="contact">
  <p class="section-label reveal">// 003</p>
  <h2 class="contact-headline reveal">Initiate<br>Contact</h2>
  <p class="contact-sub reveal">"Let's build impossible worlds."</p>
  <div class="contact-form reveal">
    <div class="form-row">
      <div class="field-wrap">
        <label>Name</label>
        <input type="text" placeholder="Your name">
      </div>
      <div class="field-wrap">
        <label>Email</label>
        <input type="email" placeholder="your@email.com">
      </div>
    </div>
    <div class="form-row">
      <div class="field-wrap">
        <label>Project Type</label>
        <select>
          <option value="">Select type</option>
          <option>Concept Art</option>
          <option>Illustration</option>
          <option>3D Sculpting</option>
          <option>Worldbuilding</option>
          <option>Commission</option>
          <option>Collaboration</option>
          <option>Other</option>
        </select>
      </div>
      <div class="field-wrap">
        <label>Budget</label>
        <select>
          <option value="">Select range</option>
          <option>Under $500</option>
          <option>$500 – $2,000</option>
          <option>$2,000 – $5,000</option>
          <option>$5,000+</option>
          <option>Let's discuss</option>
        </select>
      </div>
    </div>
    <div class="field-wrap">
      <label>Message</label>
      <textarea placeholder="Describe your project, vision, or idea..."></textarea>
    </div>
    <button class="btn-transmit"><span>— Transmit Signal —</span></button>
  </div>
  <div class="contact-socials reveal">
    <a href="#" class="social-link">Instagram</a>
    <a href="#" class="social-link">ArtStation</a>
    <a href="mailto:qcqquiqu18@gmail.com" class="social-link">Email</a>
    <a href="#" class="social-link">Behance</a>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="footer-logo">ERON LUMBRE</div>
  <div class="footer-signal">"Signal received across dimensions."</div>
  <div class="footer-copy" id="footer-copy"></div>
</footer>

<!-- ADMIN TOGGLE -->
<button id="admin-toggle" title="Manage Gallery">
  <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="rgba(106,91,255,0.8)" stroke-width="1.5">
    <circle cx="12" cy="12" r="3"/>
    <path d="M12 1v4M12 19v4M4.22 4.22l2.83 2.83M16.95 16.95l2.83 2.83M1 12h4M19 12h4M4.22 19.78l2.83-2.83M16.95 7.05l2.83-2.83"/>
  </svg>
</button>

<!-- ADMIN PANEL -->
<div id="admin-panel">
  <div class="admin-header">
    <span class="admin-title">Gallery Manager</span>
    <span class="admin-badge">ERON LUMBRE</span>
  </div>
  <div class="admin-body">

    <div class="admin-section-label">Upload Images</div>
    <div class="drop-zone" id="drop-zone">
      <div class="drop-zone-icon">
        <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="rgba(106,91,255,0.6)" stroke-width="1.2">
          <path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4M17 8l-5-5-5 5M12 3v12"/>
        </svg>
      </div>
      <p>Drop images here or <strong>click to browse</strong><br>PNG, JPG, WEBP — múltiples a la vez</p>
      <input type="file" id="file-input" accept="image/*" multiple>
    </div>
    <div class="upload-preview" id="upload-preview"></div>

    <div id="upload-fields" style="display:none;">
      <div class="admin-section-label">Artwork Details</div>
      <input type="text" class="admin-input" id="artwork-title" placeholder="Title (e.g. Void Entity)">
      <input type="text" class="admin-input" id="artwork-year" placeholder="Year (e.g. 2024)">
      <select class="admin-select" id="artwork-cat">
        <option value="Illustration">Illustration</option>
        <option value="Concept Art">Concept Art</option>
        <option value="Experimental">Experimental</option>
        <option value="3D Sculpting">3D Sculpting</option>
        <option value="Motion">Motion</option>
        <option value="Hybrid">Hybrid</option>
      </select>
      <button class="btn-admin" id="btn-add">Add to Archive</button>
    </div>
    <div class="status-msg" id="status-msg"></div>

    <div class="admin-section-label">Manage Archive (<span id="manage-count">0</span>)</div>
    <div class="manage-list" id="manage-list"></div>

    <button class="btn-admin danger" id="btn-clear-all" style="margin-top:16px;">Clear All Works</button>
  </div>
</div>

<script>
/* ═══════════════════════════════════════
   GALAXY CANVAS
═══════════════════════════════════════ */
(function() {
  const canvas = document.getElementById('bg-canvas');
  const ctx = canvas.getContext('2d');
  let W, H, particles = [], mouse = {x:0,y:0};
  const COLORS = ['rgba(106,91,255,', 'rgba(217,217,217,', 'rgba(245,248,255,'];

  function resize() {
    W = canvas.width = window.innerWidth;
    H = canvas.height = window.innerHeight;
  }

  function Particle() {
    this.reset = function() {
      this.x = Math.random() * W;
      this.y = Math.random() * H;
      this.z = Math.random() * W;
      this.pz = this.z;
      this.r = Math.random() * 1.5 + 0.3;
      this.col = COLORS[Math.floor(Math.random()*COLORS.length)];
      this.speed = Math.random() * 0.3 + 0.05;
    };
    this.reset();
  }

  function initParticles(n) {
    particles = [];
    for (let i=0;i<n;i++) particles.push(new Particle());
  }

  function draw() {
    ctx.clearRect(0,0,W,H);
    // Nebula glow
    const grad = ctx.createRadialGradient(W/2,H/2,0,W/2,H/2,Math.max(W,H)*0.7);
    grad.addColorStop(0,'rgba(106,91,255,0.04)');
    grad.addColorStop(0.5,'rgba(106,91,255,0.01)');
    grad.addColorStop(1,'transparent');
    ctx.fillStyle = grad;
    ctx.fillRect(0,0,W,H);

    // Mouse influence
    const mx = ctx.createRadialGradient(mouse.x,mouse.y,0,mouse.x,mouse.y,200);
    mx.addColorStop(0,'rgba(106,91,255,0.06)');
    mx.addColorStop(1,'transparent');
    ctx.fillStyle = mx;
    ctx.fillRect(0,0,W,H);

    particles.forEach(p => {
      p.z -= p.speed;
      if (p.z <= 0) p.reset();

      const sx = (p.x - W/2) * (W/p.z) + W/2;
      const sy = (p.y - H/2) * (W/p.z) + H/2;
      const psx = (p.x - W/2) * (W/p.pz) + W/2;
      const psy = (p.y - H/2) * (W/p.pz) + H/2;
      const size = Math.max(0, p.r * (W/p.z));
      const opacity = Math.min(0.9, (W - p.z) / W * 0.8);

      if (sx < -10 || sx > W+10 || sy < -10 || sy > H+10) { p.reset(); return; }

      ctx.beginPath();
      ctx.moveTo(psx, psy);
      ctx.lineTo(sx, sy);
      ctx.strokeStyle = p.col + opacity + ')';
      ctx.lineWidth = size;
      ctx.stroke();

      p.pz = p.z;
    });
    requestAnimationFrame(draw);
  }

  window.addEventListener('resize', () => { resize(); initParticles(Math.floor(W*H/10000)); });
  window.addEventListener('mousemove', e => { mouse.x = e.clientX; mouse.y = e.clientY; });
  resize();
  initParticles(Math.floor(W*H/10000));
  draw();
})();

/* ═══════════════════════════════════════
   CURSOR
═══════════════════════════════════════ */
const cursor = document.getElementById('cursor');
const trail  = document.getElementById('cursor-trail');
let cx=0,cy=0,tx=0,ty=0;

document.addEventListener('mousemove', e => {
  cx = e.clientX; cy = e.clientY;
  cursor.style.transform = `translate(${cx-6}px,${cy-6}px)`;
});

function animateTrail() {
  tx += (cx - tx) * 0.12;
  ty += (cy - ty) * 0.12;
  trail.style.transform = `translate(${tx-18}px,${ty-18}px)`;
  requestAnimationFrame(animateTrail);
}
animateTrail();

document.querySelectorAll('a,button,.artwork-card,.filter-btn,.drop-zone,.manage-del').forEach(el => {
  el.addEventListener('mouseenter', () => {
    cursor.style.transform += ' scale(2)';
    trail.style.transform += ' scale(1.5)';
    trail.style.borderColor = 'rgba(106,91,255,0.8)';
  });
  el.addEventListener('mouseleave', () => {
    trail.style.borderColor = 'rgba(106,91,255,0.5)';
  });
});

/* ═══════════════════════════════════════
   NAVBAR
═══════════════════════════════════════ */
const navbar = document.getElementById('navbar');
window.addEventListener('scroll', () => {
  navbar.classList.toggle('scrolled', window.scrollY > 60);
});

/* ═══════════════════════════════════════
   FOOTER YEAR
═══════════════════════════════════════ */
document.getElementById('footer-copy').textContent = `© ${new Date().getFullYear()} ERON LUMBRE — All rights reserved`;

/* ═══════════════════════════════════════
   SCROLL REVEAL
═══════════════════════════════════════ */
const revealEls = document.querySelectorAll('.reveal');
const revealObs = new IntersectionObserver((entries) => {
  entries.forEach((e, i) => {
    if (e.isIntersecting) {
      setTimeout(() => e.target.classList.add('visible'), i * 60);
      revealObs.unobserve(e.target);
    }
  });
}, { threshold: 0.1 });
revealEls.forEach(el => revealObs.observe(el));

/* ═══════════════════════════════════════
   SKILL BARS
═══════════════════════════════════════ */
const barObs = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.querySelectorAll('.skill-bar').forEach(bar => {
        setTimeout(() => { bar.style.width = bar.dataset.w + '%'; }, 200);
      });
      barObs.unobserve(e.target);
    }
  });
}, { threshold: 0.3 });
document.querySelectorAll('.skills-grid').forEach(el => barObs.observe(el));

/* ═══════════════════════════════════════
   DATA STORE (localStorage)
═══════════════════════════════════════ */
const STORE_KEY = 'eron_lumbre_gallery_v2';

function loadGallery() {
  try { return JSON.parse(localStorage.getItem(STORE_KEY)) || []; }
  catch(e) { return []; }
}

function saveGallery(data) {
  try { localStorage.setItem(STORE_KEY, JSON.stringify(data)); }
  catch(e) { showStatus('Storage full — images too large. Try smaller files.', 'error'); }
}

let gallery = loadGallery();

/* ═══════════════════════════════════════
   PORTFOLIO GRID RENDER
═══════════════════════════════════════ */
function renderGrid(cat='all') {
  const grid = document.getElementById('portfolio-grid');
  const filtered = cat === 'all' ? gallery : gallery.filter(a => a.cat === cat);
  const count = document.getElementById('artwork-count');
  count.textContent = `${gallery.length} works`;
  document.getElementById('stat-count').textContent = gallery.length;
  document.getElementById('manage-count').textContent = gallery.length;

  if (filtered.length === 0) {
    grid.innerHTML = `
      <div style="grid-column:1/-1;padding:80px;text-align:center;color:rgba(217,217,217,0.15);font-family:var(--font-mono);font-size:0.6rem;letter-spacing:0.3em;text-transform:uppercase;border:1px dashed rgba(217,217,217,0.05);">
        <div style="font-size:2rem;margin-bottom:16px;opacity:0.4">◈</div>
        No works in this category yet<br>Use the manager ↘ to upload your art
      </div>`;
    return;
  }

  grid.innerHTML = filtered.map((art, i) => `
    <div class="artwork-card" data-index="${gallery.indexOf(art)}" onclick="openLightbox(${gallery.indexOf(art)})">
      <img class="artwork-img" src="${art.src}" alt="${art.title}" loading="lazy">
      <div class="artwork-glow"></div>
      <div class="artwork-overlay">
        <div class="artwork-cat">${art.cat}</div>
      </div>
    </div>
  `).join('');
}

/* ═══════════════════════════════════════
   MANAGE LIST
═══════════════════════════════════════ */
function renderManageList() {
  const list = document.getElementById('manage-list');
  document.getElementById('manage-count').textContent = gallery.length;
  if (gallery.length === 0) {
    list.innerHTML = `<div style="padding:20px;text-align:center;color:rgba(217,217,217,0.2);font-family:var(--font-mono);font-size:0.5rem;letter-spacing:0.2em;">Archive empty</div>`;
    return;
  }
  list.innerHTML = gallery.map((art, i) => `
    <div class="manage-item">
      <img class="manage-thumb" src="${art.src}" alt="${art.title}">
      <div class="manage-info">
        <div class="manage-name">${art.title || 'Untitled'}</div>
        <div class="manage-cat">${art.cat} · ${art.year}</div>
      </div>
      <button class="manage-del" onclick="deleteArtwork(${i})" title="Delete">✕</button>
    </div>
  `).join('');
}

/* ═══════════════════════════════════════
   LIGHTBOX
═══════════════════════════════════════ */
function openLightbox(i) {
  const art = gallery[i];
  if (!art) return;
  document.getElementById('lightbox-img').src = art.src;
  document.getElementById('lightbox-title').textContent = art.title;
  document.getElementById('lightbox-meta').textContent = `${art.cat} · ${art.year}`;
  document.getElementById('lightbox').classList.add('open');
  document.body.style.overflow = 'hidden';
}
document.getElementById('lightbox-close').addEventListener('click', () => {
  document.getElementById('lightbox').classList.remove('open');
  document.body.style.overflow = '';
});
document.getElementById('lightbox').addEventListener('click', e => {
  if (e.target === document.getElementById('lightbox')) {
    document.getElementById('lightbox').classList.remove('open');
    document.body.style.overflow = '';
  }
});
document.addEventListener('keydown', e => {
  if (e.key === 'Escape') {
    document.getElementById('lightbox').classList.remove('open');
    document.body.style.overflow = '';
  }
});

/* ═══════════════════════════════════════
   FILTERS
═══════════════════════════════════════ */
let currentCat = 'all';
document.getElementById('filter-bar').addEventListener('click', e => {
  if (!e.target.classList.contains('filter-btn')) return;
  document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
  e.target.classList.add('active');
  currentCat = e.target.dataset.cat;
  renderGrid(currentCat);
});

/* ═══════════════════════════════════════
   ADMIN PANEL
═══════════════════════════════════════ */
const adminPanel = document.getElementById('admin-panel');
document.getElementById('admin-toggle').addEventListener('click', () => {
  adminPanel.classList.toggle('open');
  if (adminPanel.classList.contains('open')) renderManageList();
});

/* FILE INPUT */
const dropZone = document.getElementById('drop-zone');
const fileInput = document.getElementById('file-input');
const uploadPreview = document.getElementById('upload-preview');
const uploadFields = document.getElementById('upload-fields');

let pendingFiles = [];

dropZone.addEventListener('click', () => fileInput.click());
dropZone.addEventListener('dragover', e => { e.preventDefault(); dropZone.classList.add('drag-over'); });
dropZone.addEventListener('dragleave', () => dropZone.classList.remove('drag-over'));
dropZone.addEventListener('drop', e => {
  e.preventDefault();
  dropZone.classList.remove('drag-over');
  handleFiles(e.dataTransfer.files);
});
fileInput.addEventListener('change', () => handleFiles(fileInput.files));

function handleFiles(files) {
  pendingFiles = Array.from(files);
  uploadPreview.innerHTML = '';
  uploadFields.style.display = 'none';
  if (pendingFiles.length === 0) return;

  const readers = pendingFiles.map(file => {
    return new Promise(resolve => {
      const reader = new FileReader();
      reader.onload = e => {
        const img = document.createElement('img');
        img.src = e.target.result;
        img.className = 'preview-thumb';
        uploadPreview.appendChild(img);
        resolve();
      };
      reader.readAsDataURL(file);
    });
  });

  Promise.all(readers).then(() => {
    uploadFields.style.display = 'block';
    // Auto-fill title from filename
    if (pendingFiles.length === 1) {
      const name = pendingFiles[0].name.replace(/\.[^.]+$/, '').replace(/[-_]/g,' ');
      document.getElementById('artwork-title').value = name.charAt(0).toUpperCase() + name.slice(1);
    } else {
      document.getElementById('artwork-title').value = '';
    }
    document.getElementById('artwork-year').value = new Date().getFullYear();
  });
}

/* ADD TO ARCHIVE */
document.getElementById('btn-add').addEventListener('click', () => {
  if (pendingFiles.length === 0) { showStatus('No images selected.', 'error'); return; }

  const title = document.getElementById('artwork-title').value.trim();
  const year  = document.getElementById('artwork-year').value.trim() || new Date().getFullYear();
  const cat   = document.getElementById('artwork-cat').value;

  let loaded = 0;
  pendingFiles.forEach((file, i) => {
    const reader = new FileReader();
    reader.onload = e => {
      gallery.unshift({
        src: e.target.result,
        title: pendingFiles.length > 1 ? (title || file.name.replace(/\.[^.]+$/,'')) : (title || 'Untitled'),
        year: year,
        cat: cat,
        id: Date.now() + i
      });
      loaded++;
      if (loaded === pendingFiles.length) {
        saveGallery(gallery);
        renderGrid(currentCat);
        renderManageList();
        // Reset
        uploadPreview.innerHTML = '';
        uploadFields.style.display = 'none';
        document.getElementById('artwork-title').value = '';
        fileInput.value = '';
        pendingFiles = [];
        showStatus(`✓ ${loaded} work${loaded>1?'s':''} added to the archive`, 'success');
      }
    };
    reader.readAsDataURL(file);
  });
});

/* DELETE */
function deleteArtwork(i) {
  if (!confirm('Remove this work from the archive?')) return;
  gallery.splice(i, 1);
  saveGallery(gallery);
  renderGrid(currentCat);
  renderManageList();
  showStatus('Work removed.', 'success');
}

/* CLEAR ALL */
document.getElementById('btn-clear-all').addEventListener('click', () => {
  if (!confirm('Clear the entire archive? This cannot be undone.')) return;
  gallery = [];
  saveGallery(gallery);
  renderGrid(currentCat);
  renderManageList();
  showStatus('Archive cleared.', 'success');
});

/* STATUS */
function showStatus(msg, type) {
  const el = document.getElementById('status-msg');
  el.textContent = msg;
  el.className = 'status-msg ' + type;
  setTimeout(() => { el.textContent = ''; el.className = 'status-msg'; }, 3500);
}

/* ═══════════════════════════════════════
   INIT
═══════════════════════════════════════ */
renderGrid();
renderManageList();

/* TRANSMIT BUTTON — sends via mailto */
document.querySelector('.btn-transmit').addEventListener('click', () => {
  const fields = document.querySelectorAll('.contact-form input, .contact-form select, .contact-form textarea');
  const name    = fields[0].value.trim();
  const email   = fields[1].value.trim();
  const project = fields[2].value;
  const budget  = fields[3].value;
  const message = fields[4].value.trim();

  if (!name || !email || !message) {
    const btn = document.querySelector('.btn-transmit span');
    btn.textContent = '— Fill in Name, Email & Message —';
    setTimeout(() => { btn.textContent = '— Transmit Signal —'; }, 2500);
    return;
  }

  const subject = encodeURIComponent(`[ERON LUMBRE] New contact from ${name}`);
  const body = encodeURIComponent(
    `Name: ${name}\nEmail: ${email}\nProject Type: ${project || 'Not specified'}\nBudget: ${budget || 'Not specified'}\n\nMessage:\n${message}`
  );
  window.location.href = `mailto:qcqquiqu18@gmail.com?subject=${subject}&body=${body}`;

  const btn = document.querySelector('.btn-transmit span');
  btn.textContent = '— Signal Transmitted ✓ —';
  setTimeout(() => { btn.textContent = '— Transmit Signal —'; }, 3000);
});
</script>
</body>
</html>
