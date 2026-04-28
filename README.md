<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ServerPanel — Admin Dashboard</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/bootstrap/5.3.3/css/bootstrap.min.css">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono&display=swap" rel="stylesheet">
<style>

  /* ── BACKGROUND COLOR TRANSITION ── */
  @keyframes bgShift {
    0%   { background-position: 0% 50%; }
    50%  { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Inter', sans-serif;
    color: #cbd5e1;
    min-height: 100vh;
    background: linear-gradient(135deg, #0f172a, #0d1f3c, #0f2744, #0a1f38, #111827, #1a1035, #0f172a);
    background-size: 400% 400%;
    animation: bgShift 16s ease infinite;
    display: flex;
    flex-direction: column;
  }

  /* ── ANIMATIONS ── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  @keyframes slideIn {
    from { opacity: 0; transform: translateX(-14px); }
    to   { opacity: 1; transform: translateX(0); }
  }
  @keyframes fillBar {
    from { width: 0; }
  }
  @keyframes blink {
    0%, 100% { opacity: 1; }
    50%       { opacity: 0.25; }
  }
  @keyframes sideIn {
    from { opacity: 0; transform: translateX(-20px); }
    to   { opacity: 1; transform: translateX(0); }
  }

  .fade-up  { animation: fadeUp  0.5s ease both; }
  .slide-in { animation: slideIn 0.4s ease both; }
  .d1 { animation-delay: 0.05s; }
  .d2 { animation-delay: 0.10s; }
  .d3 { animation-delay: 0.15s; }
  .d4 { animation-delay: 0.20s; }
  .d5 { animation-delay: 0.25s; }
  .d6 { animation-delay: 0.30s; }
  .d7 { animation-delay: 0.35s; }
  .d8 { animation-delay: 0.40s; }

  /* ── TOP NAVBAR ── */
  .topbar {
    background: rgba(15, 23, 42, 0.9);
    backdrop-filter: blur(14px);
    height: 56px;
    display: flex;
    align-items: center;
    padding: 0 20px;
    border-bottom: 1px solid rgba(255,255,255,0.07);
    position: sticky;
    top: 0;
    z-index: 200;
    animation: fadeUp 0.4s ease;
    flex-shrink: 0;
  }
  .brand {
    font-size: 18px; font-weight: 700;
    color: #ffffff; text-decoration: none;
  }
  .brand span { color: #38bdf8; }

  .topbar-right { margin-left: auto; display: flex; align-items: center; gap: 14px; }
  .live-dot { width: 8px; height: 8px; border-radius: 50%; background: #10b981; animation: blink 2s infinite; }
  .live-text { font-size: 13px; color: #10b981; font-weight: 500; }
  .avatar {
    width: 34px; height: 34px; border-radius: 50%;
    background: linear-gradient(135deg, #38bdf8, #818cf8);
    display: flex; align-items: center; justify-content: center;
    font-size: 13px; font-weight: 700; color: #fff;
  }

  /* ── WRAPPER (sidebar + main) ── */
  .wrapper {
    display: flex;
    flex: 1;
    min-height: 0;
  }

  /* ── SIDEBAR ── */
  .sidebar {
    width: 220px;
    flex-shrink: 0;
    background: rgba(15, 23, 42, 0.88);
    backdrop-filter: blur(14px);
    border-right: 1px solid rgba(255,255,255,0.07);
    padding: 20px 12px;
    display: flex;
    flex-direction: column;
    gap: 4px;
    animation: sideIn 0.4s ease 0.1s both;
    position: sticky;
    top: 56px;
    height: calc(100vh - 56px);
    overflow-y: auto;
  }

  .sb-section { margin-bottom: 6px; }

  .sb-label {
    font-size: 10px; font-weight: 600;
    text-transform: uppercase; letter-spacing: 0.08em;
    color: #334155; padding: 0 10px;
    margin-bottom: 4px; margin-top: 14px;
  }

  .sb-link {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 9px 10px;
    border-radius: 8px;
    color: #64748b;
    font-size: 14px; font-weight: 500;
    text-decoration: none;
    cursor: pointer;
    transition: background 0.18s, color 0.18s;
    position: relative;
  }
  .sb-link:hover { background: rgba(255,255,255,0.06); color: #cbd5e1; }
  .sb-link.active { background: rgba(56,189,248,0.12); color: #38bdf8; }
  .sb-link.active::before {
    content: '';
    position: absolute;
    left: 0; top: 20%; bottom: 20%;
    width: 3px; border-radius: 0 3px 3px 0;
    background: #38bdf8;
  }

  .sb-icon {
    width: 18px; height: 18px;
    flex-shrink: 0; opacity: 0.7;
  }
  .sb-link.active .sb-icon { opacity: 1; }
  .sb-link:hover .sb-icon { opacity: 0.9; }

  .sb-badge {
    margin-left: auto;
    font-size: 10px; font-weight: 600;
    padding: 2px 7px; border-radius: 20px;
    background: rgba(56,189,248,0.12); color: #38bdf8;
  }
  .sb-badge.red {
    background: rgba(239,68,68,0.12); color: #f87171;
  }

  .sb-divider {
    height: 1px;
    background: rgba(255,255,255,0.06);
    margin: 10px 0;
  }

  /* ── CONTENT AREA ── */
  .content {
    flex: 1;
    min-width: 0;
    display: flex;
    flex-direction: column;
  }

  /* ── PAGE HEADER ── */
  .page-head {
    background: rgba(15, 23, 42, 0.75);
    backdrop-filter: blur(10px);
    border-bottom: 1px solid rgba(255,255,255,0.07);
    padding: 18px 24px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 12px;
    animation: fadeUp 0.4s ease 0.15s both;
    flex-shrink: 0;
  }
  .page-head h1 { font-size: 19px; font-weight: 700; color: #f1f5f9; margin: 0; }
  .page-head p  { font-size: 13px; color: #64748b; margin: 3px 0 0; }

  .btn-outline {
    font-size: 13px; font-weight: 500;
    padding: 7px 15px; border-radius: 8px;
    border: 1px solid rgba(255,255,255,0.1);
    background: transparent; color: #94a3b8;
    cursor: pointer; transition: background 0.2s, color 0.2s;
  }
  .btn-outline:hover { background: rgba(255,255,255,0.07); color: #f1f5f9; }

  .btn-primary {
    font-size: 13px; font-weight: 600;
    padding: 7px 17px; border-radius: 8px;
    border: none; background: #3b82f6;
    color: #ffffff; cursor: pointer;
    transition: background 0.2s;
  }
  .btn-primary:hover { background: #2563eb; }

  /* ── ALERT ── */
  .alert-bar {
    background: rgba(239,68,68,0.1);
    border-left: 4px solid #ef4444;
    padding: 10px 24px;
    font-size: 13px; color: #fca5a5;
    display: flex; align-items: center; gap: 10px;
    animation: slideIn 0.4s ease 0.2s both;
    flex-shrink: 0;
  }
  .alert-dot { width: 8px; height: 8px; border-radius: 50%; background: #ef4444; flex-shrink: 0; animation: blink 1.5s infinite; }

  /* ── MAIN ── */
  .main { padding: 22px 24px; display: flex; flex-direction: column; gap: 18px; flex: 1; }

  /* ── METRICS ── */
  .metrics { display: grid; grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); gap: 12px; }
  .mcard {
    background: rgba(30, 41, 59, 0.7);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255,255,255,0.08);
    border-radius: 12px; padding: 16px;
    transition: transform 0.2s, box-shadow 0.2s;
  }
  .mcard:hover { transform: translateY(-3px); box-shadow: 0 8px 28px rgba(0,0,0,0.4); }
  .mlabel { font-size: 11px; font-weight: 500; color: #64748b; margin-bottom: 6px; }
  .mval   { font-size: 24px; font-weight: 700; line-height: 1; margin-bottom: 4px; }
  .msub   { font-size: 11px; color: #475569; }
  .tag {
    display: inline-block; font-size: 10px; font-weight: 600;
    padding: 2px 6px; border-radius: 20px; margin-left: 4px; vertical-align: middle;
  }
  .tg { background: rgba(16,185,129,0.15); color: #34d399; }
  .tr { background: rgba(239,68,68,0.15);  color: #f87171; }

  /* ── BOX ── */
  .two-col   { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
  .three-col { display: grid; grid-template-columns: 3fr 2fr; gap: 16px; }

  .box {
    background: rgba(30, 41, 59, 0.7);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255,255,255,0.08);
    border-radius: 12px; padding: 18px;
  }
  .box-title {
    font-size: 11px; font-weight: 600;
    text-transform: uppercase; letter-spacing: 0.07em;
    color: #475569; margin-bottom: 14px;
  }

  /* ── PROGRESS ── */
  .prog { margin-bottom: 13px; }
  .prog:last-child { margin-bottom: 0; }
  .prog-row { display: flex; justify-content: space-between; margin-bottom: 5px; }
  .prog-row span  { font-size: 13px; font-weight: 500; color: #94a3b8; }
  .prog-row small { font-size: 11px; color: #475569; }
  .prog-row b     { font-size: 12px; font-weight: 700; font-family: 'JetBrains Mono', monospace; }
  .bar-bg { height: 6px; background: rgba(0,0,0,0.3); border-radius: 20px; overflow: hidden; }
  .bar-fg { height: 100%; border-radius: 20px; animation: fillBar 1s ease both; }

  /* ── SERVICES ── */
  .svc {
    display: flex; align-items: center; gap: 9px;
    padding: 8px 0; border-bottom: 1px solid rgba(255,255,255,0.05);
    font-size: 13px;
  }
  .svc:last-child { border-bottom: none; }
  .dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }
  .dg { background: #10b981; }
  .dr { background: #ef4444; }
  .da { background: #f59e0b; }
  .svc-name { font-weight: 600; color: #ffffff; width: 100px; }
  .svc-port { font-family: 'JetBrains Mono', monospace; font-size: 11px; color: #475569; flex: 1; }
  .svc-mem  { font-family: 'JetBrains Mono', monospace; font-size: 11px; color: #64748b; min-width: 52px; text-align: right; }
  .pill { font-size: 10px; font-weight: 600; padding: 2px 9px; border-radius: 20px; }
  .pg { background: rgba(16,185,129,0.12); color: #34d399; }
  .pr { background: rgba(239,68,68,0.12);  color: #f87171; }
  .pa { background: rgba(245,158,11,0.12); color: #fbbf24; }

  /* ── TABLE ── */
  table { width: 100%; border-collapse: collapse; font-size: 12px; }
  th {
    font-size: 10px; font-weight: 600;
    text-transform: uppercase; letter-spacing: 0.05em;
    color: #475569; padding: 0 8px 9px;
    text-align: left; border-bottom: 1px solid rgba(255,255,255,0.07);
  }
  td {
    padding: 8px 8px;
    border-bottom: 1px solid rgba(255,255,255,0.04);
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; color: #64748b;
  }
  tr:last-child td { border-bottom: none; }
  tr:hover td { background: rgba(255,255,255,0.02); }

  /* ── EVENTS ── */
  .ev {
    display: flex; gap: 10px; padding: 8px 0;
    border-bottom: 1px solid rgba(255,255,255,0.05);
  }
  .ev:last-child { border-bottom: none; }
  .ev-time  { font-family: 'JetBrains Mono', monospace; font-size: 10px; color: #475569; min-width: 40px; padding-top: 2px; }
  .ev-dot   { width: 7px; height: 7px; border-radius: 50%; margin-top: 5px; flex-shrink: 0; }
  .ev-title { font-size: 12px; font-weight: 500; color: #cbd5e1; }
  .ev-sub   { font-size: 11px; color: #475569; margin-top: 1px; }

  /* ── LOG ── */
  .log-chips { display: flex; gap: 7px; margin-bottom: 9px; flex-wrap: wrap; }
  .chip { font-size: 10px; font-weight: 600; padding: 2px 9px; border-radius: 20px; }
  .cr { background: rgba(239,68,68,0.12); color: #f87171; }
  .ca { background: rgba(245,158,11,0.12); color: #fbbf24; }
  .cb { background: rgba(59,130,246,0.12); color: #60a5fa; }

  textarea {
    width: 100%; height: 180px; resize: none;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; line-height: 1.8;
    background: rgba(5,10,20,0.8);
    color: #7dd3fc;
    border: 1px solid rgba(255,255,255,0.08);
    border-radius: 8px;
    padding: 10px 12px; outline: none;
  }

  /* ── STORAGE ── */
  .stor { display: flex; align-items: center; gap: 11px; padding: 9px 0; border-bottom: 1px solid rgba(255,255,255,0.05); }
  .stor:last-child { border-bottom: none; }
  .stor-ico  { font-size: 18px; width: 28px; text-align: center; }
  .stor-info { flex: 1; }
  .stor-name { font-size: 12px; font-weight: 600; color: #ffffff; }
  .stor-path { font-family: 'JetBrains Mono', monospace; font-size: 10px; color: #475569; }
  .stor-bar  { height: 3px; background: rgba(0,0,0,0.3); border-radius: 20px; margin-top: 4px; overflow: hidden; }
  .stor-fill { height: 100%; border-radius: 20px; animation: fillBar 1s ease both; }
  .stor-sz   { font-family: 'JetBrains Mono', monospace; font-size: 10px; color: #64748b; min-width: 66px; text-align: right; }

  /* ── SYS INFO ── */
  .sysinfo { display: grid; grid-template-columns: repeat(auto-fit, minmax(100px, 1fr)); gap: 8px; margin-top: 12px; }
  .si {
    background: rgba(246, 244, 244, 0.25);
    border: 1px solid rgba(255,255,255,0.06);
    border-radius: 8px; padding: 9px 11px;
  }
  .si-label { font-size: 10px; color: #475569; margin-bottom: 2px; }
  .si-val   { font-size: 13px; font-weight: 600; color: #e2e8f0; }
  .si-sub   { font-size: 10px; color: #475569; }

  /* ── FOOTER ── */
  footer {
    background: rgba(15, 23, 42, 0.9);
    backdrop-filter: blur(14px);
    border-top: 1px solid rgba(255,255,255,0.07);
    color: #64748b;
    padding: 32px 24px 18px;
  }
  .footer-grid { display: grid; grid-template-columns: 2fr 1fr 1fr 1fr; gap: 28px; margin-bottom: 20px; }
  .ft-brand { font-size: 17px; font-weight: 700; color: #ffffff; margin-bottom: 7px; }
  .ft-brand span { color: #38bdf8; }
  .ft-desc  { font-size: 13px; color: #475569; line-height: 1.7; max-width: 240px; }
  .ft-head  { font-size: 10px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.07em; color: #475569; margin-bottom: 9px; }
  .ft-links { list-style: none; padding: 0; display: flex; flex-direction: column; gap: 7px; }
  .ft-links a { font-size: 13px; color: #475569; text-decoration: none; transition: color 0.15s; }
  .ft-links a:hover { color: #ffffff; }
  .ft-bottom {
    border-top: 1px solid rgba(255,255,255,0.06);
    padding-top: 14px;
    display: flex; justify-content: space-between;
    flex-wrap: wrap; gap: 8px; font-size: 12px; color: #334155;
  }
  .ft-tags { display: flex; gap: 6px; }
  .ft-tag { font-size: 10px; padding: 2px 8px; border-radius: 20px; border: 1px solid rgba(255,255,255,0.07); color: #475569; }

  /* ── RESPONSIVE ── */
  @media (max-width: 900px) {
    .sidebar   { width: 60px; padding: 16px 8px; }
    .sb-label  { display: none; }
    .sb-link span { display: none; }
    .sb-badge  { display: none; }
    .sb-link   { justify-content: center; padding: 10px; }
    .sb-link::before { display: none; }
    .two-col   { grid-template-columns: 1fr; }
    .three-col { grid-template-columns: 1fr; }
    .footer-grid { grid-template-columns: 1fr 1fr; }
  }
  @media (max-width: 600px) {
    .sidebar   { display: none; }
    .main      { padding: 14px; }
    .page-head { padding: 14px; }
    footer     { padding: 22px 14px 14px; }
    .footer-grid { grid-template-columns: 1fr; }
    .metrics   { grid-template-columns: 1fr 1fr; }
    .hide-sm   { display: none; }
  }

</style>
</head>
<body>

<!-- TOP NAVBAR -->
<div class="topbar">
  <a class="brand" href="#">Server<span>Panel</span></a>
  <div class="topbar-right">
    <div class="live-dot"></div>
    <span class="live-text">All systems up</span>
    <div class="avatar">AK</div>
  </div>
</div>

<!-- WRAPPER -->
<div class="wrapper">

  <!-- SIDE NAV -->
  <div class="sidebar">
    <div class="sb-label">Main</div>
    <a class="sb-link active" href="#">
      <svg class="sb-icon" viewBox="0 0 20 20" fill="currentColor"><path d="M3 4a1 1 0 011-1h4a1 1 0 011 1v4a1 1 0 01-1 1H4a1 1 0 01-1-1V4zm0 8a1 1 0 011-1h4a1 1 0 011 1v4a1 1 0 01-1 1H4a1 1 0 01-1-1v-4zm8-8a1 1 0 011-1h4a1 1 0 011 1v4a1 1 0 01-1 1h-4a1 1 0 01-1-1V4zm0 8a1 1 0 011-1h4a1 1 0 011 1v4a1 1 0 01-1 1h-4a1 1 0 01-1-1v-4z"/></svg>
      <span>Dashboard</span>
    </a>
    <a class="sb-link" href="#">
      <svg class="sb-icon" viewBox="0 0 20 20" fill="currentColor"><path d="M2 5a2 2 0 012-2h12a2 2 0 012 2v2a2 2 0 01-2 2H4a2 2 0 01-2-2V5zm0 8a2 2 0 012-2h12a2 2 0 012 2v2a2 2 0 01-2 2H4a2 2 0 01-2-2v-2z"/></svg>
      <span>Servers</span>
      <span class="sb-badge">4</span>
    </a>
    <a class="sb-link" href="#">
      <svg class="sb-icon" viewBox="0 0 20 20" fill="currentColor"><path d="M10 2a6 6 0 00-6 6v3.586l-.707.707A1 1 0 004 14h12a1 1 0 00.707-1.707L16 11.586V8a6 6 0 00-6-6zM10 18a3 3 0 01-3-3h6a3 3 0 01-3 3z"/></svg>
      <span>Alerts</span>
      <span class="sb-badge red">3</span>
    </a>
    <div class="sb-divider"></div>
    <div class="sb-label">Infrastructure</div>
    <a class="sb-link" href="#">
      <svg class="sb-icon" viewBox="0 0 20 20" fill="currentColor"><path d="M3 12v3c0 1.657 3.134 3 7 3s7-1.343 7-3v-3c0 1.657-3.134 3-7 3s-7-1.343-7-3z"/><path d="M3 7v3c0 1.657 3.134 3 7 3s7-1.343 7-3V7c0 1.657-3.134 3-7 3S3 8.657 3 7z"/><path d="M17 5c0 1.657-3.134 3-7 3S3 6.657 3 5s3.134-3 7-3 7 1.343 7 3z"/></svg>
      <span>Storage</span>
    </a>
    <a class="sb-link" href="#">
      <svg class="sb-icon" viewBox="0 0 20 20" fill="currentColor"><path d="M5 3a2 2 0 00-2 2v2a2 2 0 002 2h2a2 2 0 002-2V5a2 2 0 00-2-2H5zM5 11a2 2 0 00-2 2v2a2 2 0 002 2h2a2 2 0 002-2v-2a2 2 0 00-2-2H5zM11 5a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V5zM14 11a1 1 0 011 1v1h1a1 1 0 110 2h-1v1a1 1 0 11-2 0v-1h-1a1 1 0 110-2h1v-1a1 1 0 011-1z"/></svg>
      <span>Containers</span>
      <span class="sb-badge">5</span>
    </a>
    <a class="sb-link" href="#">
      <svg class="sb-icon" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M12.316 3.051a1 1 0 01.633 1.265l-4 12a1 1 0 11-1.898-.632l4-12a1 1 0 011.265-.633zM5.707 6.293a1 1 0 010 1.414L3.414 10l2.293 2.293a1 1 0 11-1.414 1.414l-3-3a1 1 0 010-1.414l3-3a1 1 0 011.414 0zm8.586 0a1 1 0 011.414 0l3 3a1 1 0 010 1.414l-3 3a1 1 0 11-1.414-1.414L16.586 10l-2.293-2.293a1 1 0 010-1.414z"/></svg>
      <span>Network</span>
    </a>
    <a class="sb-link" href="#">
      <svg class="sb-icon" viewBox="0 0 20 20" fill="currentColor"><path d="M10 12a2 2 0 100-4 2 2 0 000 4z"/><path fill-rule="evenodd" d="M.458 10C1.732 5.943 5.522 3 10 3s8.268 2.943 9.542 7c-1.274 4.057-5.064 7-9.542 7S1.732 14.057.458 10zM14 10a4 4 0 11-8 0 4 4 0 018 0z"/></svg>
      <span>Services</span>
    </a>
    <div class="sb-divider"></div>
    <div class="sb-label">Tools</div>
    <a class="sb-link" href="#">
      <svg class="sb-icon" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M3 5a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 10a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 15a1 1 0 011-1h6a1 1 0 110 2H4a1 1 0 01-1-1z"/></svg>
      <span>Log Viewer</span>
    </a>
    <a class="sb-link" href="#">
      <svg class="sb-icon" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M11.49 3.17c-.38-1.56-2.6-1.56-2.98 0a1.532 1.532 0 01-2.286.948c-1.372-.836-2.942.734-2.106 2.106.54.886.061 2.042-.947 2.287-1.561.379-1.561 2.6 0 2.978a1.532 1.532 0 01.947 2.287c-.836 1.372.734 2.942 2.106 2.106a1.532 1.532 0 012.287.947c.379 1.561 2.6 1.561 2.978 0a1.533 1.533 0 012.287-.947c1.372.836 2.942-.734 2.106-2.106a1.533 1.533 0 01.947-2.287c1.561-.379 1.561-2.6 0-2.978a1.532 1.532 0 01-.947-2.287c.836-1.372-.734-2.942-2.106-2.106a1.532 1.532 0 01-2.287-.947zM10 13a3 3 0 100-6 3 3 0 000 6z"/></svg>
      <span>Settings</span>
    </a>
    <a class="sb-link" href="#">
      <svg class="sb-icon" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z"/></svg>
      <span>Help</span>
    </a>

  </div>
  <!-- CONTENT -->
  <div class="content">
    <!-- PAGE HEADER -->
    <div class="page-head">
      <div>
        <h1>Server Dashboard</h1>
        <p>prod-server-01 &nbsp;·&nbsp; Ubuntu 22.04 LTS &nbsp;·&nbsp; 2026-04-20 14:32 UTC</p>
      </div>
      <div style="display:flex;gap:8px;">
        <button class="btn-outline">Export</button>
        <button class="btn-primary">Run Diagnostics</button>
      </div>
    </div>
    <!-- ALERT -->
    <div class="alert-bar">
      <div class="alert-dot"></div>
      <div><b style="color:#f87171">Warning:</b> Redis stopped unexpectedly (OOM) &nbsp;·&nbsp; /data disk at 86% — near critical</div>
      <div style="margin-left:auto;"><span class="pill pr">2 alerts</span></div>
    </div>
    <!-- MAIN -->
    <div class="main">
      <!-- METRICS -->
      <div class="metrics">
        <div class="mcard fade-up d1"><div class="mlabel">CPU Usage</div><div class="mval" style="color:#60a5fa">63%</div><div class="msub">8 cores · load 1.84</div></div>
        <div class="mcard fade-up d2"><div class="mlabel">RAM Used</div><div class="mval" style="color:#34d399">11.2 GB</div><div class="msub">of 16 GB <span class="tag tg">70%</span></div></div>
        <div class="mcard fade-up d3"><div class="mlabel">Disk /root</div><div class="mval" style="color:#fbbf24">68 GB</div><div class="msub">of 100 GB · 68%</div></div>
        <div class="mcard fade-up d4"><div class="mlabel">Disk /data</div><div class="mval" style="color:#f87171">430 GB</div><div class="msub">of 500 GB <span class="tag tr">86%</span></div></div>
        <div class="mcard fade-up d5"><div class="mlabel">Network In</div><div class="mval" style="color:#a78bfa">9.2 MB/s</div><div class="msub">Out · 3.7 MB/s</div></div>
        <div class="mcard fade-up d6"><div class="mlabel">Uptime</div><div class="mval" style="color:#38bdf8">312d</div><div class="msub">7h 14m · stable</div></div>
      </div>
      <!-- RESOURCES + SERVICES -->
      <div class="two-col fade-up d2">
        <div class="box">
          <div class="box-title">Resource Usage</div>
          <div class="prog"><div class="prog-row"><span>CPU</span><small>8 cores</small><b style="color:#60a5fa">63%</b></div><div class="bar-bg"><div class="bar-fg" style="width:63%;background:#3b82f6;animation-delay:.1s"></div></div></div>
          <div class="prog"><div class="prog-row"><span>RAM</span><small>11.2 / 16 GB</small><b style="color:#34d399">70%</b></div><div class="bar-bg"><div class="bar-fg" style="width:70%;background:#10b981;animation-delay:.2s"></div></div></div>
          <div class="prog"><div class="prog-row"><span>Disk /</span><small>68 / 100 GB</small><b style="color:#fbbf24">68%</b></div><div class="bar-bg"><div class="bar-fg" style="width:68%;background:#f59e0b;animation-delay:.3s"></div></div></div>
          <div class="prog"><div class="prog-row"><span>Disk /data</span><small>430 / 500 GB</small><b style="color:#f87171">86%</b></div><div class="bar-bg"><div class="bar-fg" style="width:86%;background:#ef4444;animation-delay:.4s"></div></div></div>
          <div class="prog"><div class="prog-row"><span>Swap</span><small>1.1 / 8 GB</small><b style="color:#a78bfa">14%</b></div><div class="bar-bg"><div class="bar-fg" style="width:14%;background:#8b5cf6;animation-delay:.5s"></div></div></div>
          <div class="prog"><div class="prog-row"><span>GPU Mem</span><small>6.4 / 12 GB</small><b style="color:#22d3ee">53%</b></div><div class="bar-bg"><div class="bar-fg" style="width:53%;background:#06b6d4;animation-delay:.6s"></div></div></div>
        </div>
        <div class="box">
          <div class="box-title">Services</div>
          <div class="svc slide-in d1"><div class="dot dg"></div><div class="svc-name">nginx</div><div class="svc-port">:80/:443</div><div class="svc-mem">34 MB</div><div class="pill pg">Running</div></div>
          <div class="svc slide-in d2"><div class="dot dg"></div><div class="svc-name">postgres</div><div class="svc-port">:5432</div><div class="svc-mem">512 MB</div><div class="pill pg">Running</div></div>
          <div class="svc slide-in d3"><div class="dot dr"></div><div class="svc-name">redis</div><div class="svc-port">:6379</div><div class="svc-mem">—</div><div class="pill pr">Stopped</div></div>
          <div class="svc slide-in d4"><div class="dot dg"></div><div class="svc-name">node</div><div class="svc-port">:3000</div><div class="svc-mem">210 MB</div><div class="pill pg">Running</div></div>
          <div class="svc slide-in d5"><div class="dot da"></div><div class="svc-name">elasticsearch</div><div class="svc-port">:9200</div><div class="svc-mem">1.2 GB</div><div class="pill pa">Degraded</div></div>
          <div class="svc slide-in d6"><div class="dot dg"></div><div class="svc-name">docker</div><div class="svc-port">daemon</div><div class="svc-mem">88 MB</div><div class="pill pg">Running</div></div>
          <div class="svc slide-in d7"><div class="dot dg"></div><div class="svc-name">prometheus</div><div class="svc-port">:9090</div><div class="svc-mem">156 MB</div><div class="pill pg">Running</div></div>
          <div class="svc slide-in d8"><div class="dot dg"></div><div class="svc-name">cron</div><div class="svc-port">system</div><div class="svc-mem">4 MB</div><div class="pill pg">Running</div></div>
        </div>
      </div>
      <!-- NETWORK + STORAGE -->
      <div class="three-col fade-up d3">
        <div class="box">
          <div class="box-title">Network Interfaces</div>
          <div style="overflow-x:auto">
            <table>
              <thead><tr><th>Interface</th><th>IP Address</th><th class="hide-sm">In</th><th class="hide-sm">Out</th><th>Conn</th><th>Status</th></tr></thead>
              <tbody>
                <tr><td style="color:#ffffff;font-weight:600">eth0</td><td>10.0.1.12</td><td class="hide-sm" style="color:#34d399">9.2 MB/s</td><td class="hide-sm" style="color:#60a5fa">3.7 MB/s</td><td>214</td><td><span class="pill pg">Up</span></td></tr>
                <tr><td style="color:#ffffff;font-weight:600">eth1</td><td>192.168.1.5</td><td class="hide-sm" style="color:#34d399">0.4 MB/s</td><td class="hide-sm" style="color:#60a5fa">0.1 MB/s</td><td>18</td><td><span class="pill pg">Up</span></td></tr>
                <tr><td style="color:#ffffff;font-weight:600">lo</td><td>127.0.0.1</td><td class="hide-sm" style="color:#34d399">12.1 MB/s</td><td class="hide-sm" style="color:#60a5fa">12.1 MB/s</td><td>842</td><td><span class="pill pg">Up</span></td></tr>
                <tr><td style="color:#ffffff;font-weight:600">docker0</td><td>172.17.0.1</td><td class="hide-sm" style="color:#34d399">1.8 MB/s</td><td class="hide-sm" style="color:#60a5fa">0.9 MB/s</td><td>56</td><td><span class="pill pa">Warn</span></td></tr>
              </tbody>
            </table>
          </div>
        </div>
        <div class="box">
          <div class="box-title">Storage</div>
          <div class="stor"><div class="stor-ico">💾</div><div class="stor-info"><div class="stor-name">System /</div><div class="stor-path">ext4 · SSD</div><div class="stor-bar"><div class="stor-fill" style="width:68%;background:#f59e0b"></div></div></div><div class="stor-sz">68/100 GB</div></div>
          <div class="stor"><div class="stor-ico">🗄️</div><div class="stor-info"><div class="stor-name">Data /data</div><div class="stor-path">xfs · RAID</div><div class="stor-bar"><div class="stor-fill" style="width:86%;background:#ef4444"></div></div></div><div class="stor-sz">430/500 GB</div></div>
          <div class="stor"><div class="stor-ico">📦</div><div class="stor-info"><div class="stor-name">Backups</div><div class="stor-path">ext4 · NAS</div><div class="stor-bar"><div class="stor-fill" style="width:41%;background:#10b981"></div></div></div><div class="stor-sz">820GB/2TB</div></div>
          <div class="stor"><div class="stor-ico">🐳</div><div class="stor-info"><div class="stor-name">Docker</div><div class="stor-path">/var/lib/docker</div><div class="stor-bar"><div class="stor-fill" style="width:29%;background:#8b5cf6"></div></div></div><div class="stor-sz">29/100 GB</div></div>
        </div>
      </div>
      <!-- EVENTS + LOG -->
      <div class="two-col fade-up d4">
        <div class="box">
          <div class="box-title">Recent Events</div>
          <div class="ev slide-in d1"><div class="ev-time">14:32</div><div class="ev-dot" style="background:#60a5fa"></div><div><div class="ev-title">Health check passed</div><div class="ev-sub">nginx · /api/health · 3ms</div></div></div>
          <div class="ev slide-in d2"><div class="ev-time">14:31</div><div class="ev-dot" style="background:#f87171"></div><div><div class="ev-title">Redis service stopped</div><div class="ev-sub">OOM killed · exit code 1</div></div></div>
          <div class="ev slide-in d3"><div class="ev-time">14:31</div><div class="ev-dot" style="background:#fbbf24"></div><div><div class="ev-title">Slow query detected</div><div class="ev-sub">postgres · 1842ms</div></div></div>
          <div class="ev slide-in d4"><div class="ev-time">14:30</div><div class="ev-dot" style="background:#fbbf24"></div><div><div class="ev-title">/data usage &gt; 85%</div><div class="ev-sub">disk threshold alert</div></div></div>
          <div class="ev slide-in d5"><div class="ev-time">14:30</div><div class="ev-dot" style="background:#34d399"></div><div><div class="ev-title">Backup completed</div><div class="ev-sub">cron · 4.2s · success</div></div></div>
          <div class="ev slide-in d6"><div class="ev-time">14:29</div><div class="ev-dot" style="background:#34d399"></div><div><div class="ev-title">Worker recycled</div><div class="ev-sub">node · PID 29441</div></div></div>
        </div>
        <div class="box">
          <div class="box-title">System Log</div>
          <div class="log-chips"><span class="chip cr">2 ERR</span><span class="chip ca">3 WARN</span><span class="chip cb">9 INFO</span></div>
          <textarea readonly>[14:32:05]  INFO   [nginx]      200 GET /api/health — 3ms
[14:32:01]  INFO   [postgres]   checkpoint: 842 buffers
[14:31:58]  ERROR  [redis]      exited (OOM, exit 1)
[14:31:44]  INFO   [nginx]      200 GET /dashboard — 12ms
[14:31:39]  INFO   [cron]       backup-db done in 4.2s
[14:31:22]  ERROR  [postgres]   slow query (1842ms)
[14:31:10]  WARN   [disk]       /data crossed 85% (86%)
[14:30:57]  INFO   [node]       worker PID 29441 recycled
[14:30:41]  INFO   [nginx]      304 GET /static/main.js
[14:30:29]  INFO   [node]       active connections: 214
[14:30:18]  WARN   [cron]       maintenance in 6h
[14:30:02]  INFO   [postgres]   autovacuum: public.events
[14:29:51]  INFO   [nginx]      200 POST /api/data — 28ms
[14:29:44]  INFO   [node]       cache hit: 94.3%</textarea>
          <div class="sysinfo">
            <div class="si"><div class="si-label">OS</div><div class="si-val">Ubuntu 22.04</div><div class="si-sub">LTS Jammy</div></div>
            <div class="si"><div class="si-label">Kernel</div><div class="si-val">6.5.0-21</div><div class="si-sub">x86_64</div></div>
            <div class="si"><div class="si-label">Load avg</div><div class="si-val">1.84</div><div class="si-sub">1m average</div></div>
            <div class="si"><div class="si-label">CPU Temp</div><div class="si-val" style="color:#fbbf24">58°C</div><div class="si-sub">safe range</div></div>
          </div>
        </div>
      </div>
      <!-- DOCKER -->
      <div class="box fade-up d5">
        <div class="box-title">Docker Containers</div>
        <div style="overflow-x:auto">
          <table>
            <thead><tr><th>Container</th><th>Image</th><th class="hide-sm">Ports</th><th>CPU</th><th>Memory</th><th>Uptime</th><th>Status</th></tr></thead>
            <tbody>
              <tr><td style="color:#ffffff;font-weight:600">app_web_1</td><td>node:20-alpine</td><td class="hide-sm">3000→3000</td><td style="color:#34d399">4.2%</td><td>210 MB</td><td>6d 12h</td><td><span class="pill pg">Running</span></td></tr>
              <tr><td style="color:#ffffff;font-weight:600">app_worker_3</td><td>node:20-alpine</td><td class="hide-sm">—</td><td style="color:#fbbf24">18.7%</td><td>340 MB</td><td>6d 12h</td><td><span class="pill pg">Running</span></td></tr>
              <tr><td style="color:#ffffff;font-weight:600">pg_primary</td><td>postgres:16</td><td class="hide-sm">5432→5432</td><td style="color:#34d399">9.1%</td><td>512 MB</td><td>12d 3h</td><td><span class="pill pg">Running</span></td></tr>
              <tr><td style="color:#ffffff;font-weight:600">redis_cache</td><td>redis:7-alpine</td><td class="hide-sm">6379→6379</td><td>—</td><td>—</td><td>Exited</td><td><span class="pill pr">Exited</span></td></tr>
              <tr><td style="color:#ffffff;font-weight:600">prometheus</td><td>prom/prometheus</td><td class="hide-sm">9090→9090</td><td style="color:#34d399">1.4%</td><td>156 MB</td><td>30d 0h</td><td><span class="pill pg">Running</span></td></tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <!-- FOOTER -->
    <footer>
      <div class="footer-grid">
        <div>
          <div class="ft-brand">Server<span>Panel</span></div>
          <p class="ft-desc">A clean server administration dashboard for infrastructure teams. Real-time visibility into CPU, memory, storage, services and logs.</p>
        </div>
        <div>
          <div class="ft-head">Platform</div>
          <ul class="ft-links">
            <li><a href="Dashboard">Dashboard</a></li>
            <li><a href="Servers">Servers</a></li>
            <li><a href="Logs">Logs</a></li>
            <li><a href="Alerts">Alerts</a></li>
            <li><a href="Deployments">Deployments</a></li>
          </ul>
        </div>
        <div>
          <div class="ft-head">Resources</div>
          <ul class="ft-links">
            <li><a href="Documentation">Documentation</a></li>
            <li><a href="API Reference">API Reference</a></li>
            <li><a href="CLI Tools">CLI Tools</a></li>
            <li><a href="Status Page">Status Page</a></li>
            <li><a href="Community">Community</a></li>
          </ul>
        </div>
        <div>
          <div class="ft-head">Company</div>
          <ul class="ft-links">
            <li><a href="About">About</a></li>
            <li><a href="Security">Security</a></li>
            <li><a href="Privacy">Privacy</a></li>
            <li><a href="Terms">Terms</a></li>
            <li><a href="Contact">Contact</a></li>
          </ul>
        </div>
      </div>
      <div class="ft-bottom">
        <span>© 2026 ServerPanel Inc. · Built for production infrastructure teams.</span>
        <div class="ft-tags">
          <span class="ft-tag">v4.2.1</span>
          <span class="ft-tag">Bootstrap 5</span>
          <span class="ft-tag">No JS</span>
        </div>
      </div>
    </footer>

  </div><!-- end content -->
</div><!-- end wrapper -->

</body>
</html>
