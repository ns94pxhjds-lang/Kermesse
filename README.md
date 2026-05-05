<!DOCTYPE html>
<html lang="fr" data-theme="light">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>BulletinPro CI_messou – Simulateur de Bulletin Ivoirien</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600;700&family=DM+Serif+Display:ital@0;1&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
/* ═══════════════════════════════════════════
   VARIABLES & RESET
═══════════════════════════════════════════ */
:root {
  --bg: #f4f6fb;
  --bg2: #ffffff;
  --bg3: #eef1f8;
  --surface: #ffffff;
  --border: #e2e8f0;
  --border2: #cbd5e1;
  --text: #0f172a;
  --text2: #475569;
  --text3: #94a3b8;
  --accent: #1a56db;
  --accent2: #1e429f;
  --accent-light: #eff6ff;
  --accent-soft: #bfdbfe;
  --green: #059669;
  --green-light: #ecfdf5;
  --red: #dc2626;
  --red-light: #fef2f2;
  --orange: #d97706;
  --orange-light: #fffbeb;
  --purple: #7c3aed;
  --purple-light: #f5f3ff;
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.05);
  --shadow: 0 4px 16px rgba(0,0,0,.08), 0 2px 6px rgba(0,0,0,.04);
  --shadow-lg: 0 10px 40px rgba(0,0,0,.1), 0 4px 16px rgba(0,0,0,.06);
  --r: 12px;
  --r-sm: 8px;
  --r-lg: 16px;
  --r-xl: 20px;
}
[data-theme="dark"] {
  --bg: #0a0f1e;
  --bg2: #111827;
  --bg3: #1a2235;
  --surface: #1e2a3a;
  --border: #263548;
  --border2: #334155;
  --text: #f1f5f9;
  --text2: #94a3b8;
  --text3: #475569;
  --accent: #3b82f6;
  --accent2: #60a5fa;
  --accent-light: rgba(59,130,246,.1);
  --accent-soft: rgba(59,130,246,.25);
  --green: #10b981;
  --green-light: rgba(16,185,129,.1);
  --red: #f87171;
  --red-light: rgba(248,113,113,.1);
  --orange: #fbbf24;
  --orange-light: rgba(251,191,36,.1);
  --purple: #a78bfa;
  --purple-light: rgba(167,139,250,.1);
  --shadow-sm: 0 1px 3px rgba(0,0,0,.3);
  --shadow: 0 4px 16px rgba(0,0,0,.3);
  --shadow-lg: 0 10px 40px rgba(0,0,0,.4);
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: 'DM Sans', sans-serif;
  font-size: 15px;
  line-height: 1.6;
  min-height: 100vh;
  transition: background .3s, color .3s;
}
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: var(--bg); }
::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 3px; }

/* ═══════════════════════════════════════════
   TYPOGRAPHY
═══════════════════════════════════════════ */
h1, h2, h3 { font-family: 'DM Serif Display', serif; letter-spacing: -.02em; }
h1 { font-size: clamp(1.6rem, 3vw, 2.2rem); }
h2 { font-size: clamp(1.2rem, 2vw, 1.5rem); }
h3 { font-size: 1.1rem; }
code, .mono { font-family: 'JetBrains Mono', monospace; }

/* ═══════════════════════════════════════════
   LAYOUT
═══════════════════════════════════════════ */
.app-header {
  background: var(--bg2);
  border-bottom: 1px solid var(--border);
  padding: 0 24px;
  position: sticky;
  top: 0;
  z-index: 100;
  box-shadow: var(--shadow-sm);
}
.header-inner {
  max-width: 1100px;
  margin: 0 auto;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
}
.logo {
  display: flex;
  align-items: center;
  gap: 10px;
  font-family: 'DM Serif Display', serif;
  font-size: 1.2rem;
  color: var(--text);
  text-decoration: none;
}
.logo-icon {
  width: 36px;
  height: 36px;
  background: linear-gradient(135deg, var(--accent), var(--purple));
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.1rem;
  color: #fff;
  flex-shrink: 0;
}
.logo span { color: var(--accent); }
.header-actions { display: flex; gap: 8px; align-items: center; }

.main { max-width: 1100px; margin: 0 auto; padding: 32px 20px 80px; }

/* ═══════════════════════════════════════════
   BUTTONS
═══════════════════════════════════════════ */
.btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 9px 18px;
  border-radius: var(--r-sm);
  font-family: 'DM Sans', sans-serif;
  font-size: .85rem;
  font-weight: 600;
  cursor: pointer;
  border: none;
  transition: all .2s;
  white-space: nowrap;
  text-decoration: none;
}
.btn:active { transform: scale(.97); }
.btn-primary { background: var(--accent); color: #fff; }
.btn-primary:hover { background: var(--accent2); box-shadow: 0 4px 14px rgba(26,86,219,.35); }
.btn-outline {
  background: transparent;
  color: var(--text2);
  border: 1.5px solid var(--border2);
}
.btn-outline:hover { border-color: var(--accent); color: var(--accent); background: var(--accent-light); }
.btn-ghost { background: transparent; color: var(--text2); }
.btn-ghost:hover { background: var(--bg3); color: var(--text); }
.btn-danger { background: var(--red-light); color: var(--red); border: 1.5px solid rgba(220,38,38,.2); }
.btn-danger:hover { background: var(--red); color: #fff; }
.btn-success { background: var(--green); color: #fff; }
.btn-success:hover { background: #047857; box-shadow: 0 4px 14px rgba(5,150,105,.35); }
.btn-purple { background: var(--purple); color: #fff; }
.btn-purple:hover { background: #6d28d9; box-shadow: 0 4px 14px rgba(124,58,237,.35); }
.btn-sm { padding: 6px 12px; font-size: .78rem; border-radius: 6px; }
.btn-lg { padding: 12px 28px; font-size: .95rem; border-radius: var(--r); }
.btn-icon { padding: 8px; border-radius: 8px; }

/* Theme toggle */
.theme-btn {
  width: 40px; height: 40px;
  border-radius: 10px;
  border: 1.5px solid var(--border);
  background: var(--bg3);
  color: var(--text2);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.1rem;
  transition: all .2s;
}
.theme-btn:hover { border-color: var(--accent); color: var(--accent); }

/* ═══════════════════════════════════════════
   CARDS
═══════════════════════════════════════════ */
.card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--r-lg);
  padding: 24px;
  box-shadow: var(--shadow-sm);
  transition: box-shadow .2s;
}
.card:hover { box-shadow: var(--shadow); }
.card-title {
  display: flex;
  align-items: center;
  gap: 10px;
  font-family: 'DM Serif Display', serif;
  font-size: 1.1rem;
  color: var(--text);
  margin-bottom: 20px;
}
.card-title .icon {
  width: 32px; height: 32px;
  border-radius: 8px;
  display: flex; align-items: center; justify-content: center;
  font-size: .9rem;
  flex-shrink: 0;
}
.icon-blue { background: var(--accent-light); color: var(--accent); }
.icon-green { background: var(--green-light); color: var(--green); }
.icon-orange { background: var(--orange-light); color: var(--orange); }
.icon-red { background: var(--red-light); color: var(--red); }
.icon-purple { background: var(--purple-light); color: var(--purple); }

/* ═══════════════════════════════════════════
   STEPS / PROGRESS
═══════════════════════════════════════════ */
.steps-nav {
  display: flex;
  gap: 0;
  margin-bottom: 32px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--r-lg);
  padding: 6px;
  overflow-x: auto;
  box-shadow: var(--shadow-sm);
}
.step-item {
  flex: 1;
  min-width: 120px;
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 16px;
  border-radius: 10px;
  cursor: pointer;
  transition: all .2s;
  font-size: .82rem;
  font-weight: 600;
  color: var(--text3);
  white-space: nowrap;
}
.step-item.active {
  background: var(--accent);
  color: #fff;
}
.step-item.done {
  color: var(--green);
}
.step-num {
  width: 22px; height: 22px;
  border-radius: 50%;
  border: 1.5px solid currentColor;
  display: flex; align-items: center; justify-content: center;
  font-size: .7rem;
  font-family: 'JetBrains Mono', monospace;
  flex-shrink: 0;
}
.step-item.active .step-num { background: rgba(255,255,255,.25); border-color: rgba(255,255,255,.4); }
.step-item.done .step-num { background: var(--green); color: #fff; border-color: var(--green); }

.step-panel { display: none; }
.step-panel.active { display: block; animation: fadeIn .3s ease; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }

/* ═══════════════════════════════════════════
   LEVEL SELECTOR
═══════════════════════════════════════════ */
.level-section-title {
  font-size: .7rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .1em;
  color: var(--text3);
  margin-bottom: 10px;
  padding-left: 4px;
}
.level-group { margin-bottom: 24px; }
.level-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
  gap: 10px;
}
.level-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  padding: 16px 12px;
  background: var(--bg3);
  border: 2px solid var(--border);
  border-radius: var(--r);
  cursor: pointer;
  transition: all .2s;
  text-align: center;
}
.level-btn:hover { border-color: var(--accent-soft); background: var(--accent-light); transform: translateY(-2px); box-shadow: var(--shadow-sm); }
.level-btn.selected { border-color: var(--accent); background: var(--accent-light); }
.level-btn.selected .lb-name { color: var(--accent); }
.lb-emoji { font-size: 1.6rem; }
.lb-name { font-size: .85rem; font-weight: 700; color: var(--text); }
.lb-series { font-size: .72rem; color: var(--text3); }

/* Serie selector */
.serie-selector { margin-top: 20px; display: none; }
.serie-selector.show { display: block; }
.serie-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
  gap: 10px;
}
.serie-btn {
  padding: 12px 16px;
  background: var(--bg3);
  border: 2px solid var(--border);
  border-radius: var(--r-sm);
  cursor: pointer;
  transition: all .2s;
  font-size: .85rem;
  font-weight: 600;
  color: var(--text2);
  text-align: center;
}
.serie-btn:hover { border-color: var(--accent); color: var(--accent); }
.serie-btn.selected { border-color: var(--accent); background: var(--accent-light); color: var(--accent); }

/* ═══════════════════════════════════════════
   GRADES TABLE
═══════════════════════════════════════════ */
.grades-toolbar {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  margin-bottom: 16px;
  align-items: center;
}
.grades-table-wrap {
  overflow-x: auto;
  border-radius: var(--r);
  border: 1px solid var(--border);
}
table {
  width: 100%;
  border-collapse: collapse;
  font-size: .88rem;
}
thead th {
  background: var(--bg3);
  padding: 12px 14px;
  text-align: left;
  font-size: .72rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .07em;
  color: var(--text2);
  border-bottom: 1px solid var(--border);
  white-space: nowrap;
}
tbody td {
  padding: 11px 14px;
  border-bottom: 1px solid var(--border);
  vertical-align: middle;
}
tbody tr:last-child td { border-bottom: none; }
tbody tr:hover td { background: var(--bg3); }
tbody tr { transition: background .15s; }

.matiere-name {
  font-weight: 600;
  color: var(--text);
  display: flex;
  align-items: center;
  gap: 6px;
}
.matiere-cat {
  font-size: .65rem;
  background: var(--bg3);
  color: var(--text3);
  padding: 2px 7px;
  border-radius: 20px;
  font-weight: 600;
  border: 1px solid var(--border);
}
.note-input {
  width: 80px;
  padding: 7px 10px;
  border-radius: 7px;
  border: 1.5px solid var(--border);
  background: var(--bg);
  color: var(--text);
  font-family: 'JetBrains Mono', monospace;
  font-size: .9rem;
  font-weight: 600;
  text-align: center;
  transition: all .2s;
  outline: none;
}
.note-input:focus { border-color: var(--accent); box-shadow: 0 0 0 3px var(--accent-soft); }
.note-input.error { border-color: var(--red); background: var(--red-light); }
.note-input.good { border-color: var(--green); }
.coef-input {
  width: 65px;
  padding: 7px 10px;
  border-radius: 7px;
  border: 1.5px solid var(--border);
  background: var(--bg);
  color: var(--text2);
  font-family: 'JetBrains Mono', monospace;
  font-size: .88rem;
  text-align: center;
  transition: all .2s;
  outline: none;
}
.coef-input:focus { border-color: var(--accent); box-shadow: 0 0 0 3px var(--accent-soft); }
.moy-ponderee {
  font-family: 'JetBrains Mono', monospace;
  font-size: .9rem;
  font-weight: 600;
  color: var(--accent);
}
.moy-ponderee.empty { color: var(--text3); }

/* Note badge */
.note-badge {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 52px;
  height: 28px;
  border-radius: 6px;
  font-family: 'JetBrains Mono', monospace;
  font-size: .85rem;
  font-weight: 700;
  padding: 0 8px;
}
.nb-excellent { background: #dcfce7; color: #15803d; }
.nb-good { background: #d1fae5; color: #065f46; }
.nb-avg { background: var(--accent-light); color: var(--accent); }
.nb-low { background: var(--orange-light); color: var(--orange); }
.nb-fail { background: var(--red-light); color: var(--red); }
.nb-empty { background: var(--bg3); color: var(--text3); }
[data-theme="dark"] .nb-excellent { background: rgba(21,128,61,.2); color: #4ade80; }
[data-theme="dark"] .nb-good { background: rgba(6,95,70,.2); color: #34d399; }

/* Progress bar */
.note-bar { height: 4px; background: var(--bg3); border-radius: 2px; overflow: hidden; min-width: 80px; }
.note-bar-fill { height: 100%; border-radius: 2px; transition: width .5s ease; }

.del-row-btn {
  width: 28px; height: 28px;
  border-radius: 6px;
  border: none;
  background: transparent;
  color: var(--text3);
  cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  font-size: .9rem;
  transition: all .15s;
}
.del-row-btn:hover { background: var(--red-light); color: var(--red); }

/* Add matiere row */
.add-matiere-form {
  background: var(--bg3);
  border: 1.5px dashed var(--border2);
  border-radius: var(--r);
  padding: 16px;
  margin-top: 12px;
  display: none;
  gap: 10px;
  flex-wrap: wrap;
  align-items: flex-end;
}
.add-matiere-form.show { display: flex; }
.form-field { display: flex; flex-direction: column; gap: 4px; flex: 1; min-width: 130px; }
.form-label { font-size: .72rem; font-weight: 700; text-transform: uppercase; letter-spacing: .06em; color: var(--text3); }
.form-input {
  padding: 9px 12px;
  border-radius: 8px;
  border: 1.5px solid var(--border);
  background: var(--surface);
  color: var(--text);
  font-family: 'DM Sans', sans-serif;
  font-size: .88rem;
  outline: none;
  transition: all .2s;
}
.form-input:focus { border-color: var(--accent); box-shadow: 0 0 0 3px var(--accent-soft); }

/* ═══════════════════════════════════════════
   RESULTS SUMMARY
═══════════════════════════════════════════ */
.results-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 16px;
  margin-bottom: 24px;
}
.result-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--r);
  padding: 20px;
  text-align: center;
  position: relative;
  overflow: hidden;
  box-shadow: var(--shadow-sm);
  transition: box-shadow .2s, transform .2s;
}
.result-card:hover { box-shadow: var(--shadow); transform: translateY(-2px); }
.result-card::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 3px;
}
.rc-blue::before { background: linear-gradient(90deg, var(--accent), #60a5fa); }
.rc-green::before { background: linear-gradient(90deg, var(--green), #34d399); }
.rc-purple::before { background: linear-gradient(90deg, var(--purple), #a78bfa); }
.rc-orange::before { background: linear-gradient(90deg, var(--orange), #fcd34d); }
.rc-red::before { background: linear-gradient(90deg, var(--red), #fb7185); }
.result-icon { font-size: 1.8rem; margin-bottom: 8px; }
.result-value {
  font-family: 'DM Serif Display', serif;
  font-size: 2rem;
  color: var(--text);
  line-height: 1;
  margin-bottom: 4px;
}
.result-value.big { font-size: 2.5rem; }
.result-label { font-size: .78rem; color: var(--text3); font-weight: 600; text-transform: uppercase; letter-spacing: .05em; }
.result-sub { font-size: .82rem; color: var(--text2); margin-top: 6px; }

/* Mention badge */
.mention-badge {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 8px 18px;
  border-radius: 30px;
  font-size: .85rem;
  font-weight: 700;
  letter-spacing: .03em;
}
.mention-excellent { background: #dcfce7; color: #15803d; border: 1.5px solid #86efac; }
.mention-tb { background: #d1fae5; color: #065f46; border: 1.5px solid #6ee7b7; }
.mention-b { background: var(--accent-light); color: var(--accent); border: 1.5px solid var(--accent-soft); }
.mention-ab { background: var(--purple-light); color: var(--purple); border: 1.5px solid #c4b5fd; }
.mention-p { background: var(--orange-light); color: var(--orange); border: 1.5px solid #fcd34d; }
.mention-i { background: var(--red-light); color: var(--red); border: 1.5px solid #fca5a5; }
[data-theme="dark"] .mention-excellent { background: rgba(21,128,61,.2); }
[data-theme="dark"] .mention-tb { background: rgba(6,95,70,.2); }
[data-theme="dark"] .mention-b { background: var(--accent-light); }
[data-theme="dark"] .mention-ab { background: var(--purple-light); }
[data-theme="dark"] .mention-p { background: var(--orange-light); }
[data-theme="dark"] .mention-i { background: var(--red-light); }

/* Bilan categories */
.bilan-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  background: var(--bg3);
  border-radius: var(--r-sm);
  margin-bottom: 8px;
}
.bilan-label { font-weight: 700; font-size: .88rem; color: var(--text); }
.bilan-vals { display: flex; gap: 16px; align-items: center; }
.bilan-val { font-family: 'JetBrains Mono', monospace; font-size: .9rem; font-weight: 600; }

/* ═══════════════════════════════════════════
   ANALYSIS
═══════════════════════════════════════════ */
.analysis-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-bottom: 24px; }
@media (max-width: 640px) { .analysis-grid { grid-template-columns: 1fr; } }

.analysis-section-title {
  font-size: .75rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .08em;
  color: var(--text3);
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  gap: 6px;
}
.strength-item, .weakness-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 12px;
  border-radius: var(--r-sm);
  margin-bottom: 6px;
  font-size: .85rem;
  font-weight: 600;
}
.strength-item { background: var(--green-light); color: var(--green); }
.weakness-item { background: var(--red-light); color: var(--red); }
.item-note { font-family: 'JetBrains Mono', monospace; margin-left: auto; font-size: .82rem; }

.advice-list { display: flex; flex-direction: column; gap: 8px; }
.advice-item {
  display: flex;
  align-items: flex-start;
  gap: 10px;
  padding: 12px 14px;
  background: var(--bg3);
  border-radius: var(--r-sm);
  font-size: .85rem;
  color: var(--text2);
  border-left: 3px solid var(--border);
}
.advice-item.positive { border-left-color: var(--green); }
.advice-item.negative { border-left-color: var(--red); }
.advice-item.neutral { border-left-color: var(--accent); }
.advice-icon { font-size: 1rem; flex-shrink: 0; margin-top: 1px; }

/* Chart container */
.chart-container {
  position: relative;
  height: 260px;
  margin-top: 8px;
}

/* ═══════════════════════════════════════════
   BULLETIN FORM
═══════════════════════════════════════════ */
.bulletin-form { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
@media (max-width: 580px) { .bulletin-form { grid-template-columns: 1fr; } }
.bulletin-form .form-field { min-width: 0; }

/* ═══════════════════════════════════════════
   BULLETIN PREVIEW (print-style)
═══════════════════════════════════════════ */
#bulletin-preview {
  display: none;
  margin-top: 32px;
}
.bulletin-doc {
  background: #fff;
  color: #000;
  border: 1px solid #ccc;
  border-radius: var(--r);
  padding: 32px 36px;
  max-width: 820px;
  margin: 0 auto;
  box-shadow: var(--shadow-lg);
  font-family: 'DM Sans', sans-serif;
}

/* Bulletin header */
.bul-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 16px;
  padding-bottom: 16px;
  border-bottom: 2px solid #1a56db;
  gap: 16px;
}
.bul-ministry {
  font-size: .68rem;
  text-align: center;
  line-height: 1.5;
  font-weight: 600;
  color: #1a56db;
  flex: 1;
  text-transform: uppercase;
}
.bul-title-center {
  text-align: center;
  flex: 2;
}
.bul-main-title {
  font-family: 'DM Serif Display', serif;
  font-size: 1.1rem;
  color: #1a56db;
  margin-bottom: 2px;
  font-weight: bold;
}
.bul-subtitle { font-size: .78rem; color: #475569; font-weight: 600; }
.bul-trimestre {
  font-size: .72rem;
  text-align: right;
  flex: 1;
  color: #475569;
  line-height: 1.6;
}

/* Bulletin student info */
.bul-student-grid {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 16px;
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 14px 18px;
  margin-bottom: 16px;
  align-items: start;
}
.bul-student-fields { display: grid; grid-template-columns: 1fr 1fr; gap: 6px 20px; }
.bul-field { display: flex; align-items: baseline; gap: 4px; font-size: .82rem; }
.bul-field-label { font-weight: 700; color: #475569; white-space: nowrap; }
.bul-field-value {
  font-weight: 600;
  color: #0f172a;
  border-bottom: 1px solid #cbd5e1;
  flex: 1;
  min-width: 80px;
  padding-bottom: 1px;
}
.bul-field-value.large { font-size: .9rem; text-transform: uppercase; letter-spacing: .02em; color: #1a56db; }
.bul-photo-box {
  width: 70px; height: 85px;
  border: 1px solid #cbd5e1;
  border-radius: 4px;
  display: flex; align-items: center; justify-content: center;
  font-size: .62rem; color: #94a3b8; text-align: center; font-weight: 500;
  background: #f1f5f9;
}

/* Bulletin table */
.bul-table { width: 100%; border-collapse: collapse; margin-bottom: 16px; font-size: .82rem; }
.bul-table th {
  background: #1a56db;
  color: #fff;
  padding: 8px 10px;
  text-align: center;
  font-size: .72rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .04em;
  border: 1px solid #1e429f;
}
.bul-table th.left { text-align: left; }
.bul-table td {
  padding: 7px 10px;
  border: 1px solid #e2e8f0;
  color: #0f172a;
  font-size: .82rem;
}
.bul-table tr:nth-child(even) td { background: #f8fafc; }
.bul-table tr:hover td { background: #eff6ff; }
.bul-table td.mat-name { font-weight: 600; }
.bul-table td.note-col { text-align: center; font-family: 'JetBrains Mono', monospace; font-weight: 600; }
.bul-table td.coef-col { text-align: center; font-family: 'JetBrains Mono', monospace; color: #475569; }
.bul-table td.total-col { text-align: center; font-family: 'JetBrains Mono', monospace; color: #1a56db; font-weight: 700; }
.bul-table td.appre-col { font-size: .75rem; font-style: italic; color: #475569; }
.bul-bilan-row td {
  background: #1e429f !important;
  color: #fff !important;
  font-weight: 700;
  font-size: .8rem;
  text-align: center;
}
.bul-bilan-row td.left { text-align: left; padding-left: 12px; }

/* Bulletin footer */
.bul-bottom { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 16px; margin-top: 16px; }
.bul-bottom-box {
  border: 1px solid #e2e8f0;
  border-radius: 6px;
  padding: 12px;
  background: #f8fafc;
}
.bul-box-title { font-size: .7rem; font-weight: 700; text-transform: uppercase; letter-spacing: .06em; color: #475569; margin-bottom: 8px; border-bottom: 1px solid #e2e8f0; padding-bottom: 6px; }
.bul-avg-display {
  text-align: center;
  padding: 10px 0;
}
.bul-avg-num { font-family: 'DM Serif Display', serif; font-size: 2.2rem; color: #1a56db; line-height: 1; }
.bul-avg-label { font-size: .7rem; color: #94a3b8; margin-top: 2px; }
.bul-mention-display { text-align: center; padding: 10px 0; font-size: 1.1rem; font-weight: 700; font-style: italic; }
.bul-conseil-line { font-size: .75rem; color: #475569; line-height: 1.6; font-style: italic; }
.bul-sign-area { margin-top: 24px; display: flex; justify-content: space-between; padding-top: 16px; border-top: 1px solid #e2e8f0; }
.bul-sign-block { text-align: center; font-size: .78rem; color: #475569; }
.bul-sign-line { width: 130px; border-bottom: 1px solid #0f172a; height: 40px; margin: 0 auto 6px; }

/* Results de classe box */
.bul-class-results { margin-top: 8px; }
.bul-class-row { display: flex; justify-content: space-between; font-size: .78rem; padding: 3px 0; color: #475569; }
.bul-class-row span:last-child { font-weight: 700; color: #0f172a; font-family: 'JetBrains Mono', monospace; }

/* ═══════════════════════════════════════════
   MISC
═══════════════════════════════════════════ */
.divider {
  height: 1px;
  background: var(--border);
  margin: 24px 0;
}
.empty-state {
  text-align: center;
  padding: 48px 24px;
  color: var(--text3);
}
.empty-icon { font-size: 3rem; margin-bottom: 12px; }
.empty-text { font-size: .9rem; }
.section-intro { font-size: .88rem; color: var(--text2); margin-bottom: 24px; line-height: 1.7; }
.badge-pill {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  padding: 3px 10px;
  border-radius: 20px;
  font-size: .72rem;
  font-weight: 700;
  letter-spacing: .03em;
}
.pill-blue { background: var(--accent-light); color: var(--accent); }
.pill-green { background: var(--green-light); color: var(--green); }
.pill-orange { background: var(--orange-light); color: var(--orange); }
.pill-red { background: var(--red-light); color: var(--red); }

/* Rang simulator */
.rang-sim {
  background: var(--bg3);
  border: 1px solid var(--border);
  border-radius: var(--r);
  padding: 16px;
  margin-top: 16px;
}
.rang-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); gap: 12px; }
.rang-input-group { display: flex; flex-direction: column; gap: 4px; }

.floating-save {
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 50;
  box-shadow: var(--shadow-lg);
  animation: bounceIn .4s ease;
  display: none;
}
@keyframes bounceIn { from { transform: scale(.8) translateY(20px); opacity: 0; } to { transform: scale(1) translateY(0); opacity: 1; } }

/* Notification */
.notif {
  position: fixed;
  bottom: 24px;
  left: 50%;
  transform: translateX(-50%) translateY(100px);
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--r);
  padding: 12px 20px;
  font-size: .88rem;
  font-weight: 600;
  color: var(--text);
  box-shadow: var(--shadow-lg);
  z-index: 200;
  transition: transform .35s cubic-bezier(.34,1.56,.64,1);
  display: flex;
  align-items: center;
  gap: 10px;
  white-space: nowrap;
}
.notif.show { transform: translateX(-50%) translateY(0); }

/* Responsive */
@media (max-width: 768px) {
  .main { padding: 20px 12px 80px; }
  .bul-bottom { grid-template-columns: 1fr; }
  .bul-header { flex-direction: column; text-align: center; }
  .bul-student-fields { grid-template-columns: 1fr; }
  .bul-sign-area { flex-direction: column; gap: 20px; align-items: center; }
}

/* Print */
@media print {
  body * { visibility: hidden; }
  .bulletin-doc, .bulletin-doc * { visibility: visible; }
  .bulletin-doc { position: fixed; top: 0; left: 0; width: 100%; box-shadow: none; border: none; padding: 20px; }
  .no-print { display: none !important; }
}
</style>
</head>
<body>

<!-- HEADER -->
<header class="app-header">
  <div class="header-inner">
    <div class="logo">
      <div class="logo-icon">📋</div>
      <span>Bulletin<span>Pro</span> CI_messou</span>
    </div>
    <div class="header-actions">
      <div style="font-size:.78rem;color:var(--text3);display:none" id="currentLevelBadge"></div>
      <button class="theme-btn" onclick="toggleTheme()" title="Mode sombre/clair" id="themeBtn">🌙</button>
      <button class="btn btn-outline btn-sm no-print" onclick="resetAll()">🔄 Réinitialiser</button>
    </div>
  </div>
</header>

<!-- MAIN -->
<main class="main">

  <!-- WELCOME -->
  <div id="welcomePanel" style="text-align:center;padding:40px 20px 32px;">
    <div style="font-size:3.5rem;margin-bottom:16px;">🎓</div>
    <h1 style="margin-bottom:12px;">Simulateur de Bulletin Scolaire cree par 𝚙𝚛𝚒𝚟𝚊𝚝_𝚖𝚎𝚜𝚜𝚘𝚞-𝚝𝚎𝚌𝚑</h1>
    <p style="font-size:.95rem;color:var(--text2);max-width:540px;margin:0 auto 28px;line-height:1.8;">
      Calculez vos moyennes, analysez vos performances et générez un <strong>bulletin scolaire officiel</strong> selon les programmes du système éducatif ivoirien.
    </p>
    <div style="display:flex;gap:10px;justify-content:center;flex-wrap:wrap;">
      <div style="display:flex;align-items:center;gap:6px;font-size:.82rem;color:var(--text2);background:var(--bg3);padding:8px 14px;border-radius:30px;border:1px solid var(--border);">✅ Coefficients officiels CI</div>
      <div style="display:flex;align-items:center;gap:6px;font-size:.82rem;color:var(--text2);background:var(--bg3);padding:8px 14px;border-radius:30px;border:1px solid var(--border);">📊 Analyse automatique</div>
      <div style="display:flex;align-items:center;gap:6px;font-size:.82rem;color:var(--text2);background:var(--bg3);padding:8px 14px;border-radius:30px;border:1px solid var(--border);">📄 Export PDF</div>
      <div style="display:flex;align-items:center;gap:6px;font-size:.82rem;color:var(--text2);background:var(--bg3);padding:8px 14px;border-radius:30px;border:1px solid var(--border);">💾 Sauvegarde auto</div>
    </div>
  </div>

  <!-- STEPS NAV -->
  <div class="steps-nav no-print">
    <div class="step-item active" id="step-nav-1" onclick="goToStep(1)">
      <div class="step-num">1</div> Niveau
    </div>
    <div class="step-item" id="step-nav-2" onclick="goToStep(2)">
      <div class="step-num">2</div> Notes
    </div>
    <div class="step-item" id="step-nav-3" onclick="goToStep(3)">
      <div class="step-num">3</div> Résultats
    </div>
    <div class="step-item" id="step-nav-4" onclick="goToStep(4)">
      <div class="step-num">4</div> Analyse
    </div>
    <div class="step-item" id="step-nav-5" onclick="goToStep(5)">
      <div class="step-num">5</div> Bulletin
    </div>
  </div>

  <!-- ══════════ STEP 1 : NIVEAU ══════════ -->
  <div class="step-panel active" id="step-1">
    <div class="card">
      <div class="card-title"><div class="icon icon-blue">🏫</div> Choisissez votre niveau scolaire</div>
      <p class="section-intro">Sélectionnez votre classe pour charger automatiquement les matières et coefficients officiels du programme ivoirien.</p>

      <div class="level-group">
        <div class="level-section-title">🏛️ Collège</div>
        <div class="level-grid">
          <div class="level-btn" onclick="selectLevel('6eme',this)">
            <div class="lb-emoji">📚</div>
            <div class="lb-name">6ème</div>
            <div class="lb-series">1er cycle</div>
          </div>
          <div class="level-btn" onclick="selectLevel('5eme',this)">
            <div class="lb-emoji">📚</div>
            <div class="lb-name">5ème</div>
            <div class="lb-series">1er cycle</div>
          </div>
          <div class="level-btn" onclick="selectLevel('4eme',this)">
            <div class="lb-emoji">📖</div>
            <div class="lb-name">4ème</div>
            <div class="lb-series">1er cycle</div>
          </div>
          <div class="level-btn" onclick="selectLevel('3eme',this)">
            <div class="lb-emoji">🎓</div>
            <div class="lb-name">3ème</div>
            <div class="lb-series">BEPC</div>
          </div>
        </div>
      </div>

      <div class="level-group">
        <div class="level-section-title">🎓 Lycée</div>
        <div class="level-grid">
          <div class="level-btn" onclick="selectLevel('2nde',this)">
            <div class="lb-emoji">⭐</div>
            <div class="lb-name">2nde</div>
            <div class="lb-series">2e cycle</div>
          </div>
          <div class="level-btn" onclick="selectLevel('1ere',this)">
            <div class="lb-emoji">🌟</div>
            <div class="lb-name">1ère</div>
            <div class="lb-series">Séries A/B/C/D</div>
          </div>
          <div class="level-btn" onclick="selectLevel('tle',this)">
            <div class="lb-emoji">🏆</div>
            <div class="lb-name">Terminale</div>
            <div class="lb-series">BAC</div>
          </div>
        </div>
      </div>

      <!-- Serie selector -->
      <div class="serie-selector" id="serieSelector">
        <div class="level-section-title">📋 Choisissez votre série</div>
        <div class="serie-grid" id="serieGrid"></div>
      </div>

      <div style="margin-top:24px;display:flex;justify-content:flex-end;">
        <button class="btn btn-primary btn-lg" onclick="confirmLevel()" id="confirmLevelBtn" style="display:none;">
          Continuer vers les notes →
        </button>
      </div>
    </div>
  </div>

  <!-- ══════════ STEP 2 : MOYENNES ══════════ -->
  <div class="step-panel" id="step-2">
    <div class="card">
      <div class="card-title">
        <div class="icon icon-blue">✏️</div>
        <span>Saisie des MOYENNES</span>
        <span id="levelBadge" class="badge-pill pill-blue" style="margin-left:auto;font-size:.75rem;"></span>
      </div>

      <div class="grades-toolbar">
        <button class="btn btn-outline btn-sm" onclick="toggleAddForm()">➕ Ajouter matière</button>
        <button class="btn btn-outline btn-sm" onclick="clearAllNotes()">🗑️ Effacer MOYENNE</button>
        <div style="margin-left:auto;font-size:.8rem;color:var(--text3)">
          Coefficients modifiables • MOYENNE de 0 à 20
        </div>
      </div>

      <div class="grades-table-wrap">
        <table id="gradesTable">
          <thead>
            <tr>
              <th class="left" style="min-width:160px;">Matière</th>
              <th style="text-align:center">MOY /20</th>
              <th style="text-align:center">Coef.</th>
              <th style="text-align:center;min-width:100px;">Moy. pond.</th>
              <th style="text-align:center;min-width:90px;">Progression</th>
              <th style="text-align:center">Appréc.</th>
              <th></th>
            </tr>
          </thead>
          <tbody id="gradesBody"></tbody>
        </table>
      </div>

      <!-- Totals row -->
      <div style="display:flex;justify-content:flex-end;margin-top:12px;">
        <div style="display:flex;gap:24px;align-items:center;background:var(--bg3);border:1px solid var(--border);border-radius:var(--r-sm);padding:12px 20px;">
          <div style="text-align:center;">
            <div style="font-size:.7rem;color:var(--text3);text-transform:uppercase;letter-spacing:.05em;margin-bottom:2px;">Total Coefs</div>
            <div style="font-family:'JetBrains Mono',monospace;font-size:1rem;font-weight:700;color:var(--text)" id="totalCoef">0</div>
          </div>
          <div style="width:1px;height:32px;background:var(--border);"></div>
          <div style="text-align:center;">
            <div style="font-size:.7rem;color:var(--text3);text-transform:uppercase;letter-spacing:.05em;margin-bottom:2px;">Total Pond.</div>
            <div style="font-family:'JetBrains Mono',monospace;font-size:1rem;font-weight:700;color:var(--text)" id="totalPond">0</div>
          </div>
          <div style="width:1px;height:32px;background:var(--border);"></div>
          <div style="text-align:center;">
            <div style="font-size:.7rem;color:var(--text3);text-transform:uppercase;letter-spacing:.05em;margin-bottom:2px;">Moyenne</div>
            <div style="font-family:'JetBrains Mono',monospace;font-size:1.3rem;font-weight:700;color:var(--accent)" id="liveMoyenne">—</div>
          </div>
        </div>
      </div>

      <!-- Add form -->
      <div class="add-matiere-form" id="addMatiereForm">
        <div class="form-field" style="flex:2">
          <div class="form-label">Nom de la matière</div>
          <input type="text" class="form-input" id="newMatiereName" placeholder="Ex: Économie">
        </div>
        <div class="form-field">
          <div class="form-label">Catégorie</div>
          <select class="form-input" id="newMatiereCategorie">
            <option value="Lettres">Lettres</option>
            <option value="Sciences">Sciences</option>
            <option value="Autres">Autres</option>
          </select>
        </div>
        <div class="form-field" style="max-width:90px">
          <div class="form-label">Coefficient</div>
          <input type="number" class="form-input" id="newMatiereCoef" value="2" min="1" max="9" style="text-align:center">
        </div>
        <div style="display:flex;gap:6px;align-items:flex-end;padding-bottom:0">
          <button class="btn btn-primary btn-sm" onclick="addCustomMatiere()">Ajouter</button>
          <button class="btn btn-ghost btn-sm" onclick="toggleAddForm()">Annuler</button>
        </div>
      </div>

      <div style="margin-top:24px;display:flex;justify-content:flex-end;gap:8px;">
        <button class="btn btn-outline" onclick="goToStep(1)">← Retour</button>
        <button class="btn btn-primary btn-lg" onclick="goToStep(3)">Voir les résultats →</button>
      </div>
    </div>
  </div>

  <!-- ══════════ STEP 3 : RÉSULTATS ══════════ -->
  <div class="step-panel" id="step-3">
    <div id="resultsContent">
      <div class="empty-state">
        <div class="empty-icon">📊</div>
        <div class="empty-text">Saisissez vos moyenne à l'étape 2 pour voir vos résultats.</div>
      </div>
    </div>
    <div style="margin-top:24px;display:flex;justify-content:space-between;flex-wrap:wrap;gap:8px;">
      <button class="btn btn-outline" onclick="goToStep(2)">← Modifier les notes</button>
      <button class="btn btn-primary btn-lg" onclick="goToStep(4)">Analyser mes résultats →</button>
    </div>
  </div>

  <!-- ══════════ STEP 4 : ANALYSE ══════════ -->
  <div class="step-panel" id="step-4">
    <div id="analysisContent">
      <div class="empty-state">
        <div class="empty-icon">🔍</div>
        <div class="empty-text">Calculez vos résultats d'abord.</div>
      </div>
    </div>
    <div style="margin-top:24px;display:flex;justify-content:space-between;flex-wrap:wrap;gap:8px;">
      <button class="btn btn-outline" onclick="goToStep(3)">← Résultats</button>
      <button class="btn btn-success btn-lg" onclick="goToStep(5)">Générer mon bulletin →</button>
    </div>
  </div>

  <!-- ══════════ STEP 5 : BULLETIN ══════════ -->
  <div class="step-panel" id="step-5">
    <div class="card" id="bulletinFormCard">
      <div class="card-title"><div class="icon icon-green">📄</div> Informations pour le bulletin</div>
      <p class="section-intro">Renseignez vos informations personnelles pour générer un bulletin scolaire complet et réaliste.</p>

      <div class="bulletin-form">
        <div class="form-field">
          <div class="form-label">Nom de famille *</div>
          <input type="text" class="form-input" id="bNom" placeholder="Ex: MESSOU">
        </div>
        <div class="form-field">
          <div class="form-label">Prénom(s) *</div>
          <input type="text" class="form-input" id="bPrenom" placeholder="Ex: Langeving Privat">
        </div>
        <div class="form-field">
          <div class="form-label">Matricule</div>
          <input type="text" class="form-input" id="bMatricule" placeholder="Ex: 19018466Q">
        </div>
        <div class="form-field">
          <div class="form-label">Classe</div>
          <input type="text" class="form-input" id="bClasse" placeholder="Ex: 1ère D 3">
        </div>
        <div class="form-field">
          <div class="form-label">Établissement</div>
          <input type="text" class="form-input" id="bEtablissement" placeholder="Ex: Lycée Moderne Aboisso">
        </div>
        <div class="form-field">
          <div class="form-label">Année scolaire</div>
          <input type="text" class="form-input" id="bAnnee" value="2025-2026">
        </div>
        <div class="form-field">
          <div class="form-label">Trimestre</div>
          <select class="form-input" id="bTrimestre">
            <option value="1er Trimestre">1er Trimestre</option>
            <option value="2ème Trimestre" selected>2ème Trimestre</option>
            <option value="3ème Trimestre">3ème Trimestre</option>
          </select>
        </div>
        <div class="form-field">
          <div class="form-label">Effectif de la classe</div>
          <input type="number" class="form-input" id="bEffectif" placeholder="Ex: 39" min="1" max="60">
        </div>
      </div>

      <!-- Rang simulator -->
      <div class="rang-sim">
        <div style="font-size:.82rem;font-weight:700;color:var(--text2);margin-bottom:12px;">📈 Simulation du classement</div>
        <div class="rang-grid">
          <div class="rang-input-group">
            <div class="form-label">Rang dans la classe</div>
            <input type="number" class="form-input" id="bRang" placeholder="Ex: 12" min="1">
          </div>
          <div class="rang-input-group">
            <div class="form-label">Moyenne classe</div>
            <input type="number" class="form-input" id="bMoyClasse" placeholder="Ex: 10.03" step="0.01">
          </div>
          <div class="rang-input-group">
            <div class="form-label">Moyenne mini</div>
            <input type="number" class="form-input" id="bMoyMin" placeholder="Ex: 6.71" step="0.01">
          </div>
          <div class="rang-input-group">
            <div class="form-label">Moyenne maxi</div>
            <input type="number" class="form-input" id="bMoyMax" placeholder="Ex: 12.64" step="0.01">
          </div>
        </div>
      </div>

      <div style="margin-top:24px;display:flex;gap:10px;flex-wrap:wrap;justify-content:flex-end;">
        <button class="btn btn-outline" onclick="goToStep(4)">← Analyse</button>
        <button class="btn btn-success btn-lg" onclick="generateBulletin()">✨ Générer le bulletin</button>
      </div>
    </div>

    <!-- BULLETIN PREVIEW -->
    <div id="bulletin-preview">
      <div style="display:flex;gap:10px;margin-bottom:20px;flex-wrap:wrap;" class="no-print">
        <button class="btn btn-primary" onclick="printBulletin()">🖨️ Imprimer</button>
        <button class="btn btn-purple" onclick="exportPDF()">📥 Export PDF</button>
        <button class="btn btn-outline" onclick="hideBulletin()">✏️ Modifier</button>
      </div>

      <div class="bulletin-doc" id="bulletinDoc">
        <!-- Header -->
        <div class="bul-header">
          <div class="bul-ministry">
            République de Côte d'Ivoire<br>
            ──────────────<br>
            Ministère de l'Éducation Nationale,<br>
            de l'Alphabétisation et de<br>
            l'Enseignement Technique<br>
            ──────────────<br>
            <span id="bDREN">DRENAET</span>
          </div>
          <div class="bul-title-center">
            <div class="bul-main-title">BULLETIN TRIMESTRIEL DE NOTES</div>
            <div id="bDocTrimestre" class="bul-subtitle" style="font-size:.9rem;font-weight:700;color:#1a56db;"></div>
            <div style="margin-top:6px;font-size:.75rem;color:#94a3b8;">Union – Discipline – Travail</div>
          </div>
          <div class="bul-trimestre">
            Année scolaire<br>
            <strong id="bDocAnnee" style="font-size:.85rem;color:#0f172a;"></strong><br>
            <br>
            Date : <strong id="bDocDate"></strong><br>
          </div>
        </div>

        <!-- Student info -->
        <div class="bul-student-grid">
          <div>
            <div style="margin-bottom:10px;">
              <span style="font-size:.7rem;font-weight:700;color:#475569;text-transform:uppercase;letter-spacing:.05em;">Établissement :</span>
              <strong id="bDocEtabl" style="color:#1a56db;margin-left:6px;font-size:.88rem;"></strong>
            </div>
            <div class="bul-student-fields">
              <div class="bul-field"><span class="bul-field-label">Élève :</span><span class="bul-field-value large" id="bDocNomEleve"></span></div>
              <div class="bul-field"><span class="bul-field-label">Matricule :</span><span class="bul-field-value mono" id="bDocMatricule"></span></div>
              <div class="bul-field"><span class="bul-field-label">Classe :</span><span class="bul-field-value" id="bDocClasse"></span></div>
              <div class="bul-field"><span class="bul-field-label">Effectif :</span><span class="bul-field-value" id="bDocEffectif"></span></div>
              <div class="bul-field"><span class="bul-field-label">Niveau :</span><span class="bul-field-value" id="bDocNiveau"></span></div>
              <div class="bul-field"><span class="bul-field-label">Régime :</span><span class="bul-field-value">Non-Boursier</span></div>
            </div>
          </div>
          <div class="bul-photo-box">Photo<br>d'identité</div>
        </div>

        <!-- Grades table -->
        <table class="bul-table" id="bulletinTable">
          <thead>
            <tr>
              <th class="left" style="width:28%">DISCIPLINES</th>
              <th>Moy.</th>
              <th>Coef.</th>
              <th>Total</th>
              <th>Rang</th>
              <th>Appréciations</th>
            </tr>
          </thead>
          <tbody id="bulletinBody"></tbody>
        </table>

        <!-- Bottom section -->
        <div class="bul-bottom">
          <div class="bul-bottom-box">
            <div class="bul-box-title">📊 Résultats de classe</div>
            <div class="bul-class-results">
              <div class="bul-class-row"><span>Moy. générale :</span><span id="bDocMoyClasse">—</span></div>
              <div class="bul-class-row"><span>Moy. minimale :</span><span id="bDocMoyMin">—</span></div>
              <div class="bul-class-row"><span>Moy. maximale :</span><span id="bDocMoyMax">—</span></div>
              <div class="bul-class-row"><span>Effectif :</span><span id="bDocEffectifClass">—</span></div>
              <div class="bul-class-row"><span>Rang :</span><span id="bDocRang">—</span></div>
            </div>
          </div>
          <div class="bul-bottom-box" style="text-align:center;">
            <div class="bul-box-title">🎯 Résultat trimestriel</div>
            <div class="bul-avg-display">
              <div class="bul-avg-num" id="bDocMoyenne">—</div>
              <div class="bul-avg-label">/20 — <span id="bDocRangShort">—</span></div>
            </div>
            <div id="bDocMentionBadge" style="margin-top:8px;"></div>
          </div>
          <div class="bul-bottom-box">
            <div class="bul-box-title">💬 Appréciation conseil</div>
            <div class="bul-mention-display" id="bDocMention"></div>
            <div class="bul-conseil-line" id="bDocConseil"></div>
          </div>
        </div>

        <!-- Signatures -->
        <div class="bul-sign-area">
          <div class="bul-sign-block">
            <div class="bul-sign-line"></div>
            <div>Professeur principal</div>
          </div>
          <div class="bul-sign-block">
            <div class="bul-sign-line"></div>
            <div>Fait le <span id="bDocDateSign"></span></div>
            <div style="margin-top:4px;font-weight:700;">Le Proviseur</div>
          </div>
          <div class="bul-sign-block">
            <div class="bul-sign-line"></div>
            <div>Signature des parents</div>
          </div>
        </div>

        <!-- Seal placeholder -->
        <div style="text-align:right;margin-top:8px;">
          <div style="display:inline-flex;align-items:center;gap:6px;font-size:.72rem;color:#94a3b8;border:1px dashed #e2e8f0;padding:6px 12px;border-radius:6px;">
            🔵 Cachet de l'établissement
          </div>
        </div>
      </div>
    </div>
  </div>

</main>

<!-- Notification -->
<div class="notif" id="notif">✅ Sauvegardé</div>

<script>
/* ═══════════════════════════════════════════════════
   DATA – COEFFICIENTS SYSTÈME IVOIRIEN
═══════════════════════════════════════════════════ */
const MATIERES_DATA = {

  '6eme': [
    {nom:'Français',            cat:'Lettres',    coef:4},
    {nom:'Mathématiques',       cat:'Sciences',   coef:4},
    {nom:'Sciences de la vie',  cat:'Sciences',   coef:2},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:2},
    {nom:'Anglais',             cat:'Lettres',    coef:2},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
    {nom:'Arts plastiques',     cat:'Autres',     coef:1},
    {nom:'Musique',             cat:'Autres',     coef:1},
    {nom:'Technologie',         cat:'Sciences',   coef:1},
  ],

  '5eme': [
    {nom:'Français',            cat:'Lettres',    coef:4},
    {nom:'Mathématiques',       cat:'Sciences',   coef:4},
    {nom:'Sciences de la vie',  cat:'Sciences',   coef:2},
    {nom:'Physique-Chimie',     cat:'Sciences',   coef:2},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:2},
    {nom:'Anglais',             cat:'Lettres',    coef:2},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
    {nom:'Arts plastiques',     cat:'Autres',     coef:1},
    {nom:'Musique',             cat:'Autres',     coef:1},
  ],

  '4eme': [
    {nom:'Français',            cat:'Lettres',    coef:4},
    {nom:'Mathématiques',       cat:'Sciences',   coef:4},
    {nom:'Sciences de la vie',  cat:'Sciences',   coef:2},
    {nom:'Physique-Chimie',     cat:'Sciences',   coef:2},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:2},
    {nom:'Anglais',             cat:'Lettres',    coef:2},
    {nom:'Espagnol/Allemand',   cat:'Lettres',    coef:2},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
    {nom:'Arts plastiques',     cat:'Autres',     coef:1},
  ],

  '3eme': [
    {nom:'Français',            cat:'Lettres',    coef:4},
    {nom:'Mathématiques',       cat:'Sciences',   coef:4},
    {nom:'Sciences de la vie',  cat:'Sciences',   coef:2},
    {nom:'Physique-Chimie',     cat:'Sciences',   coef:2},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:2},
    {nom:'Anglais',             cat:'Lettres',    coef:2},
    {nom:'Espagnol/Allemand',   cat:'Lettres',    coef:2},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
  ],

  '2nde': [
    {nom:'Français',            cat:'Lettres',    coef:3},
    {nom:'Mathématiques',       cat:'Sciences',   coef:3},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:2},
    {nom:'Anglais',             cat:'Lettres',    coef:2},
    {nom:'SVT',                 cat:'Sciences',   coef:2},
    {nom:'Physique-Chimie',     cat:'Sciences',   coef:2},
    {nom:'Espagnol/Allemand',   cat:'Lettres',    coef:2},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
    {nom:'Arts plastiques',     cat:'Autres',     coef:1},
    {nom:'Musique',             cat:'Autres',     coef:1},
  ],

  '1ere_A': [
    {nom:'Français',            cat:'Lettres',    coef:5},
    {nom:'Philosophie',         cat:'Lettres',    coef:3},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:3},
    {nom:'Anglais',             cat:'Lettres',    coef:3},
    {nom:'Espagnol/Allemand',   cat:'Lettres',    coef:3},
    {nom:'Mathématiques',       cat:'Sciences',   coef:2},
    {nom:'SVT',                 cat:'Sciences',   coef:1},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
    {nom:'Arts plastiques',     cat:'Autres',     coef:1},
  ],

  '1ere_B': [
    {nom:'Français',            cat:'Lettres',    coef:3},
    {nom:'Philosophie',         cat:'Lettres',    coef:2},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:2},
    {nom:'Mathématiques',       cat:'Sciences',   coef:5},
    {nom:'Sciences économiques',cat:'Sciences',   coef:4},
    {nom:'Anglais',             cat:'Lettres',    coef:2},
    {nom:'Espagnol/Allemand',   cat:'Lettres',    coef:2},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
  ],

  '1ere_C': [
    {nom:'Français',            cat:'Lettres',    coef:3},
    {nom:'Philosophie',         cat:'Lettres',    coef:2},
    {nom:'Mathématiques',       cat:'Sciences',   coef:5},
    {nom:'Physique-Chimie',     cat:'Sciences',   coef:5},
    {nom:'SVT',                 cat:'Sciences',   coef:2},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:2},
    {nom:'Anglais',             cat:'Lettres',    coef:2},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
  ],

  '1ere_D': [
    {nom:'Français',            cat:'Lettres',    coef:3},
    {nom:'Anglais',             cat:'Lettres',    coef:2},
    {nom:'Espagnol/Allemand',   cat:'Lettres',    coef:1},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:2},
    {nom:'Philosophie',         cat:'Lettres',    coef:2},
    {nom:'Mathématiques',       cat:'Sciences',   coef:4},
    {nom:'Physique-Chimie',     cat:'Sciences',   coef:4},
    {nom:'SVT',                 cat:'Sciences',   coef:4},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
    {nom:'Arts plastiques',     cat:'Autres',     coef:1},
    {nom:'Conduite',            cat:'Autres',     coef:1},
  ],

  'tle_A': [
    {nom:'Français',            cat:'Lettres',    coef:5},
    {nom:'Philosophie',         cat:'Lettres',    coef:5},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:4},
    {nom:'Anglais',             cat:'Lettres',    coef:4},
    {nom:'Espagnol/Allemand',   cat:'Lettres',    coef:4},
    {nom:'Mathématiques',       cat:'Sciences',   coef:2},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
  ],

  'tle_B': [
    {nom:'Français',            cat:'Lettres',    coef:3},
    {nom:'Philosophie',         cat:'Lettres',    coef:3},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:2},
    {nom:'Mathématiques',       cat:'Sciences',   coef:5},
    {nom:'Sciences économiques',cat:'Sciences',   coef:5},
    {nom:'Anglais',             cat:'Lettres',    coef:2},
    {nom:'Espagnol/Allemand',   cat:'Lettres',    coef:2},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
  ],

  'tle_C': [
    {nom:'Français',            cat:'Lettres',    coef:3},
    {nom:'Philosophie',         cat:'Lettres',    coef:3},
    {nom:'Mathématiques',       cat:'Sciences',   coef:6},
    {nom:'Physique-Chimie',     cat:'Sciences',   coef:6},
    {nom:'SVT',                 cat:'Sciences',   coef:2},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:2},
    {nom:'Anglais',             cat:'Lettres',    coef:2},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
  ],

  'tle_D': [
    {nom:'Français',            cat:'Lettres',    coef:3},
    {nom:'Anglais',             cat:'Lettres',    coef:2},
    {nom:'Histoire-Géographie', cat:'Lettres',    coef:2},
    {nom:'Philosophie',         cat:'Lettres',    coef:3},
    {nom:'Mathématiques',       cat:'Sciences',   coef:5},
    {nom:'Physique-Chimie',     cat:'Sciences',   coef:5},
    {nom:'SVT',                 cat:'Sciences',   coef:5},
    {nom:'EDHC',                cat:'Autres',     coef:1},
    {nom:'EPS',                 cat:'Autres',     coef:1},
  ],
};

const SERIES = {
  '1ere': [
    {id:'1ere_A', label:'Série A – Lettres & Sciences Humaines'},
    {id:'1ere_B', label:'Série B – Économie & Gestion'},
    {id:'1ere_C', label:'Série C – Mathématiques & Physique'},
    {id:'1ere_D', label:'Série D – Sciences de la Vie (SVT)'},
  ],
  'tle': [
    {id:'tle_A', label:'Série A – Lettres & Sciences Humaines'},
    {id:'tle_B', label:'Série B – Économie & Gestion'},
    {id:'tle_C', label:'Série C – Mathématiques & Physique'},
    {id:'tle_D', label:'Série D – Sciences de la Vie (SVT)'},
  ]
};

const LEVEL_LABELS = {
  '6eme':'6ème','5eme':'5ème','4eme':'4ème','3eme':'3ème',
  '2nde':'2nde','1ere':'1ère','tle':'Terminale',
  '1ere_A':'1ère A','1ere_B':'1ère B','1ere_C':'1ère C','1ere_D':'1ère D',
  'tle_A':'Tle A','tle_B':'Tle B','tle_C':'Tle C','tle_D':'Tle D',
};

/* ═══════════════════════════════════════════════════
   STATE
═══════════════════════════════════════════════════ */
let state = {
  currentLevel: null,
  rows: [],
  currentStep: 1,
};

/* ═══════════════════════════════════════════════════
   INIT
═══════════════════════════════════════════════════ */
window.addEventListener('DOMContentLoaded', () => {
  loadFromStorage();
  applyTheme();
});

/* ═══════════════════════════════════════════════════
   THEME
═══════════════════════════════════════════════════ */
let darkMode = localStorage.getItem('bulletinTheme') === 'dark';
function applyTheme(){
  document.documentElement.setAttribute('data-theme', darkMode ? 'dark' : 'light');
  document.getElementById('themeBtn').textContent = darkMode ? '☀️' : '🌙';
}
function toggleTheme(){
  darkMode = !darkMode;
  localStorage.setItem('bulletinTheme', darkMode ? 'dark' : 'light');
  applyTheme();
  // Redraw charts if on step 4
  if(state.currentStep === 4) buildAnalysis();
}

/* ═══════════════════════════════════════════════════
   STEPS
═══════════════════════════════════════════════════ */
function goToStep(n){
  if(n === 2 && !state.currentLevel){ showNotif('⚠️ Choisissez un niveau d\'abord'); return; }
  if(n >= 3) buildResults();
  if(n === 4) buildAnalysis();

  state.currentStep = n;
  document.querySelectorAll('.step-panel').forEach((p,i) => {
    p.classList.toggle('active', i+1 === n);
  });
  document.querySelectorAll('.step-item').forEach((item, i) => {
    item.classList.remove('active','done');
    if(i+1 === n) item.classList.add('active');
    else if(i+1 < n) item.classList.add('done');
    const numEl = item.querySelector('.step-num');
    if(i+1 < n) numEl.textContent = '✓';
    else numEl.textContent = i+1;
  });
  window.scrollTo({top:0, behavior:'smooth'});
  saveToStorage();
}

/* ═══════════════════════════════════════════════════
   LEVEL SELECTION
═══════════════════════════════════════════════════ */
let selectedLevelBase = null;
let selectedSerie = null;

function selectLevel(level, el){
  selectedLevelBase = level;
  selectedSerie = null;
  document.querySelectorAll('.level-btn').forEach(b => b.classList.remove('selected'));
  el.classList.add('selected');
  document.getElementById('confirmLevelBtn').style.display = 'none';

  const serieDiv = document.getElementById('serieSelector');
  const serieGrid = document.getElementById('serieGrid');

  if(SERIES[level]){
    serieDiv.classList.add('show');
    serieGrid.innerHTML = SERIES[level].map(s =>
      `<div class="serie-btn" onclick="selectSerie('${s.id}',this)">${s.label}</div>`
    ).join('');
  } else {
    serieDiv.classList.remove('show');
    document.getElementById('confirmLevelBtn').style.display = 'inline-flex';
  }
}

function selectSerie(id, el){
  selectedSerie = id;
  document.querySelectorAll('.serie-btn').forEach(b => b.classList.remove('selected'));
  el.classList.add('selected');
  document.getElementById('confirmLevelBtn').style.display = 'inline-flex';
}

function confirmLevel(){
  const level = selectedSerie || selectedLevelBase;
  if(!MATIERES_DATA[level]){ showNotif('⚠️ Données non disponibles pour ce niveau'); return; }
  state.currentLevel = level;
  state.rows = MATIERES_DATA[level].map(m => ({...m, note: ''}));
  renderGradesTable();
  document.getElementById('levelBadge').textContent = LEVEL_LABELS[level] || level;
  document.getElementById('currentLevelBadge').textContent = '📚 ' + (LEVEL_LABELS[level]||level);
  document.getElementById('currentLevelBadge').style.display = 'block';
  goToStep(2);
}

/* ═══════════════════════════════════════════════════
   GRADES TABLE
═══════════════════════════════════════════════════ */
function renderGradesTable(){
  const tbody = document.getElementById('gradesBody');
  tbody.innerHTML = '';
  state.rows.forEach((row, i) => {
    const tr = document.createElement('tr');
    tr.dataset.idx = i;
    tr.innerHTML = `
      <td>
        <div class="matiere-name">
          ${row.nom}
          <span class="matiere-cat">${row.cat}</span>
        </div>
      </td>
      <td style="text-align:center">
        <input type="number" class="note-input" value="${row.note}"
          min="0" max="20" step="0.25" placeholder="—"
          oninput="onNoteChange(${i}, this)"
          onblur="validateNote(${i}, this)">
      </td>
      <td style="text-align:center">
        <input type="number" class="coef-input" value="${row.coef}"
          min="1" max="9" step="1"
          oninput="onCoefChange(${i}, this)">
      </td>
      <td style="text-align:center">
        <span class="moy-ponderee ${row.note===''?'empty':''}" id="pond-${i}">
          ${row.note!=='' ? fmtNote(parseFloat(row.note)*row.coef) : '—'}
        </span>
      </td>
      <td>
        <div class="note-bar">
          <div class="note-bar-fill" id="bar-${i}" style="width:${row.note!==''?parseFloat(row.note)/20*100:0}%;background:${barColor(row.note)};"></div>
        </div>
      </td>
      <td style="font-size:.78rem;color:var(--text3);font-style:italic;" id="appre-${i}">
        ${row.note!==''?getMiniAppre(parseFloat(row.note)):''}
      </td>
      <td>
        <button class="del-row-btn" onclick="deleteRow(${i})" title="Supprimer">✕</button>
      </td>`;
    tbody.appendChild(tr);
  });
  updateTotals();
}

function onNoteChange(i, input){
  const val = input.value.trim();
  if(val !== '' && (isNaN(val) || parseFloat(val) < 0 || parseFloat(val) > 20)){
    input.classList.add('error'); return;
  }
  input.classList.remove('error');
  input.classList.toggle('good', val !== '' && parseFloat(val) >= 10);
  state.rows[i].note = val;
  const note = val !== '' ? parseFloat(val) : '';
  const coef = state.rows[i].coef;
  const pond = document.getElementById('pond-'+i);
  const bar = document.getElementById('bar-'+i);
  const appre = document.getElementById('appre-'+i);
  if(pond){
    pond.textContent = note !== '' ? fmtNote(note * coef) : '—';
    pond.className = 'moy-ponderee' + (note===''?' empty':'');
  }
  if(bar){ bar.style.width = note!==''?(note/20*100)+'%':'0%'; bar.style.background = barColor(val); }
  if(appre){ appre.textContent = note!==''?getMiniAppre(note):''; }
  updateTotals();
  saveToStorage();
}

function validateNote(i, input){
  let val = parseFloat(input.value);
  if(isNaN(val)) return;
  val = Math.max(0, Math.min(20, val));
  input.value = val;
  state.rows[i].note = String(val);
  onNoteChange(i, input);
}

function onCoefChange(i, input){
  const val = parseInt(input.value);
  if(!isNaN(val) && val >= 1 && val <= 9){
    state.rows[i].coef = val;
    onNoteChange(i, {value: state.rows[i].note, classList:{remove:()=>{},add:()=>{},toggle:()=>{}}});
  }
}

function barColor(val){
  if(val==='') return 'var(--border)';
  const n = parseFloat(val);
  if(n >= 16) return '#15803d';
  if(n >= 14) return '#059669';
  if(n >= 12) return '#0284c7';
  if(n >= 10) return 'var(--accent)';
  if(n >= 8) return var_orange();
  return '#dc2626';
}
function var_orange(){ return '#d97706'; }

function updateTotals(){
  let totalCoef = 0, totalPond = 0, count = 0;
  state.rows.forEach(r => {
    if(r.note !== ''){
      const n = parseFloat(r.note);
      totalCoef += r.coef;
      totalPond += n * r.coef;
      count++;
    }
  });
  document.getElementById('totalCoef').textContent = totalCoef || '—';
  document.getElementById('totalPond').textContent = totalPond ? fmtNote(totalPond) : '—';
  const moy = totalCoef > 0 ? totalPond / totalCoef : null;
  const el = document.getElementById('liveMoyenne');
  el.textContent = moy !== null ? fmtNote(moy) + '/20' : '—';
  if(moy !== null){
    el.style.color = moy >= 16?'#15803d':moy>=14?'#059669':moy>=12?'var(--accent)':moy>=10?'var(--orange)':'var(--red)';
  } else el.style.color = 'var(--accent)';
}

function deleteRow(i){
  state.rows.splice(i, 1);
  renderGradesTable();
  saveToStorage();
}

function clearAllNotes(){
  state.rows.forEach(r => r.note = '');
  renderGradesTable();
  showNotif('🗑️ moyenne effacées');
}

function toggleAddForm(){
  const f = document.getElementById('addMatiereForm');
  f.classList.toggle('show');
}

function addCustomMatiere(){
  const nom = document.getElementById('newMatiereName').value.trim();
  const cat = document.getElementById('newMatiereCategorie').value;
  const coef = parseInt(document.getElementById('newMatiereCoef').value) || 2;
  if(!nom){ showNotif('⚠️ Entrez un nom de matière'); return; }
  state.rows.push({nom, cat, coef, note: ''});
  renderGradesTable();
  document.getElementById('newMatiereName').value = '';
  document.getElementById('addMatiereForm').classList.remove('show');
  showNotif('✅ Matière ajoutée : ' + nom);
}

/* ═══════════════════════════════════════════════════
   CALCULATIONS
═══════════════════════════════════════════════════ */
function calcStats(){
  const noted = state.rows.filter(r => r.note !== '');
  if(!noted.length) return null;
  const totalCoef = noted.reduce((s,r) => s + r.coef, 0);
  const totalPond = noted.reduce((s,r) => s + parseFloat(r.note) * r.coef, 0);
  const moyenne = totalCoef > 0 ? totalPond / totalCoef : 0;

  // Bilans par catégorie
  const cats = {};
  noted.forEach(r => {
    if(!cats[r.cat]) cats[r.cat] = {totalC:0, totalP:0};
    cats[r.cat].totalC += r.coef;
    cats[r.cat].totalP += parseFloat(r.note) * r.coef;
  });
  const bilans = {};
  Object.entries(cats).forEach(([cat,d]) => {
    bilans[cat] = d.totalC > 0 ? d.totalP / d.totalC : 0;
  });

  const mention = getMention(moyenne);
  return {noted, totalCoef, totalPond, moyenne, bilans, mention};
}

function getMention(m){
  if(m >= 16) return {label:'Très Bien', class:'mention-tb', emoji:'🏆', conseil:'Résultats excellents ! Continuez sur cette lancée.'};
  if(m >= 14) return {label:'Bien', class:'mention-b', emoji:'🌟', conseil:'Très bons résultats. Quelques efforts et vous atteignez le sommet.'};
  if(m >= 12) return {label:'Assez Bien', class:'mention-ab', emoji:'👍', conseil:'Bons résultats. Consolidez vos acquis et progressez.'};
  if(m >= 10) return {label:'Passable', class:'mention-p', emoji:'📘', conseil:'Niveau suffisant. Des efforts supplémentaires sont nécessaires.'};
  return {label:'Insuffisant', class:'mention-i', emoji:'⚠️', conseil:'Résultats insuffisants. Une remise en question et du travail s\'imposent.'};
}

function getMiniAppre(n){
  if(n >= 16) return 'Excellent';
  if(n >= 14) return 'Très bien';
  if(n >= 12) return 'Bien';
  if(n >= 10) return 'Assez bien';
  if(n >= 8)  return 'Passable';
  if(n >= 6)  return 'Insuffisant';
  return 'Très faible';
}

function fmtNote(n){ return parseFloat(n.toFixed(2)).toString(); }

function getNoteBadgeClass(n){
  if(n >= 16) return 'nb-excellent';
  if(n >= 14) return 'nb-good';
  if(n >= 12) return 'nb-avg';
  if(n >= 10) return 'nb-low';
  return 'nb-fail';
}

/* ═══════════════════════════════════════════════════
   RESULTS PAGE
═══════════════════════════════════════════════════ */
function buildResults(){
  const stats = calcStats();
  const container = document.getElementById('resultsContent');

  if(!stats){
    container.innerHTML = `<div class="empty-state"><div class="empty-icon">📊</div><div class="empty-text">Saisissez au moins une note pour voir vos résultats.</div></div>`;
    return;
  }

  const {moyenne, totalCoef, totalPond, bilans, mention} = stats;

  // Rank simulation
  const classAvg = parseFloat(document.getElementById('bMoyClasse')?.value) || null;

  let html = `
    <div class="results-grid">
      <div class="result-card rc-blue">
        <div class="result-icon">📊</div>
        <div class="result-value big">${fmtNote(moyenne)}</div>
        <div class="result-label">Moyenne générale</div>
        <div class="result-sub">/20</div>
      </div>
      <div class="result-card rc-purple">
        <div class="result-icon">🎯</div>
        <div class="result-value big">${totalCoef}</div>
        <div class="result-label">Total coefficients</div>
        <div class="result-sub">${fmtNote(totalPond)} points pondérés</div>
      </div>
      <div class="result-card rc-green">
        <div class="result-icon">${mention.emoji}</div>
        <div class="result-value" style="font-size:1.3rem">${mention.label}</div>
        <div class="result-label">Mention</div>
        <div class="result-sub" style="margin-top:8px">
          <span class="mention-badge ${mention.class}">${mention.label}</span>
        </div>
      </div>
      <div class="result-card rc-orange">
        <div class="result-icon">📝</div>
        <div class="result-value">${stats.noted.length}</div>
        <div class="result-label">Matières notées</div>
        <div class="result-sub">sur ${state.rows.length} matières</div>
      </div>
    </div>`;

  // Bilans par catégorie
  html += `<div class="card"><div class="card-title"><div class="icon icon-blue">📂</div> Bilans par catégorie</div>`;
  Object.entries(bilans).forEach(([cat, avg]) => {
    const color = avg >= 14 ? 'var(--green)' : avg >= 10 ? 'var(--accent)' : 'var(--red)';
    html += `
      <div class="bilan-row">
        <div class="bilan-label">${cat}</div>
        <div class="bilan-vals">
          <div style="height:6px;width:120px;background:var(--border);border-radius:3px;overflow:hidden">
            <div style="height:100%;width:${avg/20*100}%;background:${color};border-radius:3px;transition:width .8s ease;"></div>
          </div>
          <div class="bilan-val" style="color:${color}">${fmtNote(avg)}/20</div>
          <span class="mention-badge ${getMention(avg).class}" style="font-size:.7rem">${getMention(avg).label}</span>
        </div>
      </div>`;
  });
  html += `</div>`;

  // Detail table
  html += `
    <div class="card" style="margin-top:16px">
      <div class="card-title"><div class="icon icon-blue">📋</div> Détail par matière</div>
      <div class="grades-table-wrap">
        <table>
          <thead>
            <tr>
              <th class="left">Matière</th>
              <th style="text-align:center">Note</th>
              <th style="text-align:center">Coef</th>
              <th style="text-align:center">Pond.</th>
              <th style="text-align:center">Appréciation</th>
              <th style="text-align:center">Contribution %</th>
            </tr>
          </thead>
          <tbody>`;

  stats.noted.forEach(r => {
    const n = parseFloat(r.note);
    const contrib = totalCoef > 0 ? (r.coef / totalCoef * 100).toFixed(1) : 0;
    html += `
      <tr>
        <td class="matiere-name" style="font-weight:600">${r.nom} <span class="matiere-cat">${r.cat}</span></td>
        <td style="text-align:center"><span class="note-badge ${getNoteBadgeClass(n)}">${fmtNote(n)}</span></td>
        <td style="text-align:center;font-family:'JetBrains Mono',monospace;color:var(--text2)">${r.coef}</td>
        <td style="text-align:center;font-family:'JetBrains Mono',monospace;font-weight:700;color:var(--accent)">${fmtNote(n*r.coef)}</td>
        <td style="text-align:center;font-size:.78rem;font-style:italic;color:var(--text2)">${getMiniAppre(n)}</td>
        <td style="text-align:center">
          <div style="display:flex;align-items:center;gap:6px;justify-content:center">
            <div style="height:4px;width:60px;background:var(--border);border-radius:2px;overflow:hidden">
              <div style="height:100%;width:${contrib}%;background:var(--accent);border-radius:2px"></div>
            </div>
            <span style="font-size:.72rem;color:var(--text3)">${contrib}%</span>
          </div>
        </td>
      </tr>`;
  });

  html += `</tbody></table></div></div>`;
  container.innerHTML = html;
}

/* ═══════════════════════════════════════════════════
   ANALYSIS PAGE
═══════════════════════════════════════════════════ */
let chartInstance = null;

function buildAnalysis(){
  const stats = calcStats();
  const container = document.getElementById('analysisContent');
  if(!stats){
    container.innerHTML = `<div class="empty-state"><div class="empty-icon">🔍</div><div class="empty-text">Saisissez des moyenne pour analyser vos résultats.</div></div>`;
    return;
  }

  const {noted, moyenne} = stats;
  const strengths = noted.filter(r => parseFloat(r.note) >= 14);
  const weaknesses = noted.filter(r => parseFloat(r.note) < 10);
  const mid = noted.filter(r => parseFloat(r.note) >= 10 && parseFloat(r.note) < 14);

  // Advices
  const advices = [];
  weaknesses.forEach(r => {
    const n = parseFloat(r.note);
    advices.push({type:'negative', icon:'⚠️', msg:`Tu dois sérieusement améliorer <strong>${r.nom}</strong> (${fmtNote(n)}/20). C'est une matière à coefficient ${r.coef} — chaque point compte !`});
  });
  strengths.forEach(r => {
    advices.push({type:'positive', icon:'✅', msg:`Excellent travail en <strong>${r.nom}</strong> (${fmtNote(parseFloat(r.note))}/20) ! Continue à maintenir ce niveau.`});
  });
  mid.forEach(r => {
    const n = parseFloat(r.note);
    if(n >= 12) advices.push({type:'neutral', icon:'📈', msg:`<strong>${r.nom}</strong> (${fmtNote(n)}/20) est bien. Quelques révisions pourraient porter ta note à 14+.`});
    else advices.push({type:'negative', icon:'💪', msg:`<strong>${r.nom}</strong> (${fmtNote(n)}/20) — Juste passable. Un effort ciblé peut changer la donne.`});
  });

  let html = `
    <div class="analysis-grid">
      <div class="card">
        <div class="card-title"><div class="icon icon-green">💪</div> Points forts (≥14)</div>`;

  if(strengths.length){
    strengths.forEach(r => {
      const n = parseFloat(r.note);
      html += `<div class="strength-item">
        <span>⭐ ${r.nom}</span>
        <span class="item-note">${fmtNote(n)}/20</span>
      </div>`;
    });
  } else {
    html += `<div style="font-size:.85rem;color:var(--text3);padding:8px 0">Aucune matière ≥14 pour l'instant.</div>`;
  }

  html += `</div><div class="card">
    <div class="card-title"><div class="icon icon-red">⚠️</div> Points faibles (<10)</div>`;

  if(weaknesses.length){
    weaknesses.forEach(r => {
      const n = parseFloat(r.note);
      html += `<div class="weakness-item">
        <span>📉 ${r.nom}</span>
        <span class="item-note">${fmtNote(n)}/20</span>
      </div>`;
    });
  } else {
    html += `<div style="font-size:.85rem;color:var(--text3);padding:8px 0">Bravo ! Aucune matière sous 10. 🎉</div>`;
  }

  html += `</div></div>`;

  // Advices
  html += `<div class="card"><div class="card-title"><div class="icon icon-orange">💡</div> Conseils personnalisés</div><div class="advice-list">`;
  advices.forEach(a => {
    html += `<div class="advice-item ${a.type}"><span class="advice-icon">${a.icon}</span><span>${a.msg}</span></div>`;
  });
  if(!advices.length) html += `<div class="advice-item neutral"><span class="advice-icon">📊</span><span>Saisissez vos moyenne pour recevoir des conseils personnalisés.</span></div>`;
  html += `</div></div>`;

  // Chart
  html += `<div class="card" style="margin-top:16px">
    <div class="card-title"><div class="icon icon-purple">📈</div> Graphique des performances</div>
    <div class="chart-container"><canvas id="perfChart"></canvas></div>
  </div>`;

  // Progression simulée
  html += `<div class="card" style="margin-top:16px">
    <div class="card-title"><div class="icon icon-blue">📊</div> Comparaison par matière</div>
    <div style="overflow-x:auto">`;

  const sorted = [...noted].sort((a,b) => parseFloat(b.note) - parseFloat(a.note));
  sorted.forEach(r => {
    const n = parseFloat(r.note);
    const pct = n/20*100;
    const col = barColor(r.note);
    html += `
      <div style="display:flex;align-items:center;gap:12px;padding:8px 0;border-bottom:1px solid var(--border)">
        <div style="min-width:160px;font-size:.82rem;font-weight:600;color:var(--text)">${r.nom}</div>
        <div style="flex:1;height:8px;background:var(--bg3);border-radius:4px;overflow:hidden;min-width:80px">
          <div style="height:100%;width:${pct}%;background:${col};border-radius:4px;transition:width 1s ease"></div>
        </div>
        <div style="min-width:55px;text-align:right;font-family:'JetBrains Mono',monospace;font-size:.85rem;font-weight:700;color:${col}">${fmtNote(n)}/20</div>
        <div style="min-width:70px;text-align:right"><span class="badge-pill ${n>=14?'pill-green':n>=10?'pill-blue':'pill-red'}" style="font-size:.68rem">${getMiniAppre(n)}</span></div>
      </div>`;
  });
  html += `</div></div>`;

  container.innerHTML = html;

  // Draw chart
  setTimeout(() => {
    const ctx = document.getElementById('perfChart');
    if(!ctx) return;
    if(chartInstance) chartInstance.destroy();
    const isDark = document.documentElement.getAttribute('data-theme') === 'dark';
    const gridColor = isDark ? 'rgba(255,255,255,.05)' : 'rgba(0,0,0,.06)';
    const textColor = isDark ? '#94a3b8' : '#475569';
    chartInstance = new Chart(ctx, {
      type: 'bar',
      data: {
        labels: noted.map(r => r.nom.length > 14 ? r.nom.slice(0,13)+'…' : r.nom),
        datasets: [{
          label: 'MOY /20',
          data: noted.map(r => parseFloat(r.note)),
          backgroundColor: noted.map(r => {
            const n = parseFloat(r.note);
            if(n>=16) return 'rgba(21,128,61,.7)';
            if(n>=14) return 'rgba(5,150,105,.7)';
            if(n>=12) return 'rgba(26,86,219,.7)';
            if(n>=10) return 'rgba(59,130,246,.6)';
            if(n>=8) return 'rgba(217,119,6,.7)';
            return 'rgba(220,38,38,.7)';
          }),
          borderRadius: 6,
          borderSkipped: false,
        },{
          label: 'Moyenne /20',
          data: noted.map(() => moyenne),
          type: 'line',
          borderColor: isDark ? '#a78bfa' : '#7c3aed',
          borderWidth: 2,
          borderDash: [6,3],
          pointRadius: 0,
          fill: false,
        }]
      },
      options: {
        responsive: true, maintainAspectRatio: false,
        plugins: {
          legend: { labels: { color: textColor, font: {family:"'DM Sans',sans-serif", size:12} } },
          tooltip: { callbacks: { label: ctx => ` ${ctx.dataset.label}: ${ctx.raw.toFixed(2)}` } }
        },
        scales: {
          y: { min:0, max:20, grid:{color:gridColor}, ticks:{color:textColor, stepSize:2} },
          x: { grid:{display:false}, ticks:{color:textColor, font:{size:10}} }
        }
      }
    });
  }, 100);
}

/* ═══════════════════════════════════════════════════
   BULLETIN GENERATION
═══════════════════════════════════════════════════ */
function generateBulletin(){
  const nom = document.getElementById('bNom').value.trim();
  const prenom = document.getElementById('bPrenom').value.trim();
  if(!nom || !prenom){ showNotif('⚠️ Nom et prénom requis'); return; }

  const stats = calcStats();
  if(!stats){ showNotif('⚠️ Aucune note saisie'); return; }
  const {noted, moyenne, mention, totalCoef, totalPond} = stats;

  // Fill header info
  const etabl = document.getElementById('bEtablissement').value || 'Lycée Moderne Aboisso';
  const annee = document.getElementById('bAnnee').value || '2025-2026';
  const trimestre = document.getElementById('bTrimestre').value;
  const classe = document.getElementById('bClasse').value || (LEVEL_LABELS[state.currentLevel]||'');
  const matricule = document.getElementById('bMatricule').value || '—';
  const effectif = document.getElementById('bEffectif').value || '—';
  const rang = document.getElementById('bRang').value;
  const moyClasse = document.getElementById('bMoyClasse').value;
  const moyMin = document.getElementById('bMoyMin').value;
  const moyMax = document.getElementById('bMoyMax').value;
  const today = new Date().toLocaleDateString('fr-FR');

  setText('bDocTrimestre', trimestre);
  setText('bDocAnnee', annee);
  setText('bDocDate', today);
  setText('bDocDateSign', today);
  setText('bDocEtabl', etabl.toUpperCase());
  setText('bDocNomEleve', `${nom.toUpperCase()} ${prenom}`);
  setText('bDocMatricule', matricule);
  setText('bDocClasse', classe);
  setText('bDocEffectif', effectif);
  setText('bDocNiveau', LEVEL_LABELS[state.currentLevel]||state.currentLevel);
  setText('bDocMoyClasse', moyClasse ? fmtNote(parseFloat(moyClasse)) : '—');
  setText('bDocMoyMin', moyMin ? fmtNote(parseFloat(moyMin)) : '—');
  setText('bDocMoyMax', moyMax ? fmtNote(parseFloat(moyMax)) : '—');
  setText('bDocEffectifClass', effectif !== '—' ? effectif + ' élèves' : '—');
  setText('bDocRang', rang ? `${rang}${ordinal(parseInt(rang))} sur ${effectif}` : '—');
  setText('bDocMoyenne', fmtNote(moyenne));
  setText('bDocRangShort', rang ? `${rang}${ordinal(parseInt(rang))} / ${effectif||'?'}` : 'N/A');
  setText('bDocMention', mention.label);
  setText('bDocConseil', mention.conseil);

  // Mention badge
  document.getElementById('bDocMentionBadge').innerHTML = `<span class="mention-badge ${mention.class}">${mention.emoji} ${mention.label}</span>`;

  // Extract DREN from school name
  const drenMatch = etabl.match(/\b(ABOISSO|ABIDJAN|BOUAKÉ|YAMOUSSOUKRO|SAN PEDRO|DALOA|KORHOGO|MAN)\b/i);
  document.getElementById('bDREN').textContent = drenMatch ? `DRENAET ${drenMatch[0].toUpperCase()}` : 'DRENAET';

  // Build table body
  const tbody = document.getElementById('bulletinBody');
  tbody.innerHTML = '';

  // Group by category
  const cats = {};
  state.rows.forEach(r => {
    if(!cats[r.cat]) cats[r.cat] = [];
    cats[r.cat].push(r);
  });

  const catOrder = ['Lettres','Sciences','Autres'];
  catOrder.forEach(cat => {
    if(!cats[cat]) return;
    let catCoef = 0, catPond = 0;

    cats[cat].forEach(r => {
      const n = r.note !== '' ? parseFloat(r.note) : null;
      tbody.insertAdjacentHTML('beforeend',`
        <tr>
          <td class="mat-name">${r.nom}</td>
          <td class="note-col">${n !== null ? fmtNote(n) : '-'}</td>
          <td class="coef-col">${r.coef}</td>
          <td class="total-col">${n !== null ? fmtNote(n * r.coef) : '-'}</td>
          <td class="note-col" style="color:#475569;">—</td>
          <td class="appre-col">${n !== null ? getMiniAppre(n) : '-'}</td>
        </tr>`);
      if(n !== null){ catCoef += r.coef; catPond += n * r.coef; }
    });

    const catAvg = catCoef > 0 ? catPond / catCoef : 0;
    tbody.insertAdjacentHTML('beforeend',`
      <tr class="bul-bilan-row">
        <td class="left">BILAN ${cat.toUpperCase()}</td>
        <td>${catCoef > 0 ? fmtNote(catAvg) : '-'}</td>
        <td>${catCoef}</td>
        <td>${catCoef > 0 ? fmtNote(catPond) : '-'}</td>
        <td colspan="2">Moy. catégorie : ${catCoef>0?fmtNote(catAvg)+'/20':'-'}</td>
      </tr>`);
  });

  // Total row
  tbody.insertAdjacentHTML('beforeend',`
    <tr style="background:#f1f5f9;font-weight:700;">
      <td style="font-weight:700">TOTAUX</td>
      <td class="note-col" style="color:#1a56db;font-weight:700;font-size:.9rem">${fmtNote(moyenne)}</td>
      <td class="coef-col">${totalCoef}</td>
      <td class="total-col" style="font-size:.9rem">${fmtNote(totalPond)}</td>
      <td colspan="2" style="font-style:italic;font-size:.8rem;color:#475569">${mention.emoji} ${mention.label}</td>
    </tr>`);

  document.getElementById('bulletin-preview').style.display = 'block';
  document.getElementById('bulletinFormCard').style.display = 'none';
  document.getElementById('bulletin-preview').scrollIntoView({behavior:'smooth'});
  showNotif('✅ Bulletin généré !');
}

function hideBulletin(){
  document.getElementById('bulletin-preview').style.display = 'none';
  document.getElementById('bulletinFormCard').style.display = 'block';
}

function printBulletin(){ window.print(); }

function exportPDF(){
  showNotif('⏳ Génération du PDF...');
  const {jsPDF} = window.jspdf;
  const doc = new jsPDF('p','mm','a4');
  const element = document.getElementById('bulletinDoc');
  import('https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js').catch(()=>{});
  // Fallback: use html2canvas if available
  if(typeof html2canvas !== 'undefined'){
    html2canvas(element, {scale:2, useCORS:true, backgroundColor:'#ffffff'}).then(canvas => {
      const imgData = canvas.toDataURL('image/png');
      const w = doc.internal.pageSize.getWidth();
      const h = (canvas.height / canvas.width) * w;
      doc.addImage(imgData,'PNG',0,0,w,h);
      doc.save('bulletin-scolaire.pdf');
      showNotif('✅ PDF téléchargé !');
    });
  } else {
    window.print();
    showNotif('💡 Utilisez Ctrl+P → Enregistrer en PDF');
  }
}

/* ═══════════════════════════════════════════════════
   UTILS
═══════════════════════════════════════════════════ */
function setText(id, text){ const el=document.getElementById(id); if(el) el.textContent = text; }
function ordinal(n){ if(n===1) return 'er'; return 'e'; }

function resetAll(){
  if(!confirm('Réinitialiser toutes les données ?')) return;
  state = { currentLevel: null, rows: [], currentStep: 1 };
  localStorage.removeItem('bulletinState');
  document.querySelectorAll('.level-btn').forEach(b => b.classList.remove('selected'));
  document.querySelectorAll('.serie-btn').forEach(b => b.classList.remove('selected'));
  document.getElementById('serieSelector').classList.remove('show');
  document.getElementById('confirmLevelBtn').style.display = 'none';
  document.getElementById('currentLevelBadge').style.display = 'none';
  document.getElementById('bulletin-preview').style.display = 'none';
  document.getElementById('bulletinFormCard').style.display = 'block';
  selectedLevelBase = null; selectedSerie = null;
  goToStep(1);
  showNotif('🔄 Données réinitialisées');
}

/* ═══════════════════════════════════════════════════
   STORAGE
═══════════════════════════════════════════════════ */
function saveToStorage(){
  try{ localStorage.setItem('bulletinState', JSON.stringify(state)); }catch(e){}
}
function loadFromStorage(){
  try{
    const saved = localStorage.getItem('bulletinState');
    if(!saved) return;
    const s = JSON.parse(saved);
    if(s.currentLevel && s.rows){
      state = s;
      renderGradesTable();
      document.getElementById('levelBadge').textContent = LEVEL_LABELS[s.currentLevel]||s.currentLevel;
      document.getElementById('currentLevelBadge').textContent = '📚 '+(LEVEL_LABELS[s.currentLevel]||s.currentLevel);
      document.getElementById('currentLevelBadge').style.display = 'block';
    }
    if(s.currentStep > 1) goToStep(s.currentStep);
  }catch(e){ console.warn('Storage load error', e); }
}

/* ═══════════════════════════════════════════════════
   NOTIFICATIONS
═══════════════════════════════════════════════════ */
let notifTimer = null;
function showNotif(msg){
  const el = document.getElementById('notif');
  el.textContent = msg;
  el.classList.add('show');
  if(notifTimer) clearTimeout(notifTimer);
  notifTimer = setTimeout(() => el.classList.remove('show'), 3200);
}
</script>
</body>
</html>
