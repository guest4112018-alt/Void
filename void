const http  = require('http');
const https = require('https');

const PORT = process.env.PORT || 3000;

// ── Inlined frontend (no external file needed) ──
const HTML = `<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Untitled document - Google Docs</title>
<link rel="icon" href="https://ssl.gstatic.com/docs/documents/images/kix-favicon7.ico">
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  :root {
    --gdocs-blue:#1a73e8; --gdocs-blue-hover:#1557b0;
    --gdocs-toolbar-bg:#f8f9fa; --gdocs-border:#e0e0e0;
    --gdocs-text:#202124; --gdocs-gray:#5f6368; --gdocs-hover:#f1f3f4;
    --proxy-bg:#0f0f1a; --proxy-accent:#00d4ff; --proxy-accent2:#7c3aed;
    --proxy-surface:#1a1a2e; --proxy-border:#2d2d4e;
    --proxy-text:#e2e8f0; --proxy-muted:#64748b;
  }
  body { font-family:'Roboto',sans-serif; background:#f0f4f9; color:var(--gdocs-text); height:100vh; overflow:hidden; }

  /* ── DOCS ── */
  #docs-shell { display:flex; flex-direction:column; height:100vh; }
  .docs-header { display:flex; align-items:center; padding:6px 12px; background:white; border-bottom:1px solid var(--gdocs-border); gap:8px; height:56px; }
  .docs-title-area { flex:1; display:flex; flex-direction:column; margin-left:4px; }
  .docs-title-input { font-size:18px; font-family:'Roboto',sans-serif; font-weight:400; border:none; outline:none; color:var(--gdocs-text); padding:2px 4px; border-radius:4px; width:240px; }
  .docs-title-input:hover,.docs-title-input:focus { background:var(--gdocs-hover); }
  .docs-meta { display:flex; align-items:center; gap:2px; margin-top:2px; }
  .docs-menu-item { font-size:13px; color:var(--gdocs-text); padding:3px 8px; border-radius:4px; cursor:pointer; }
  .docs-menu-item:hover { background:var(--gdocs-hover); }
  .docs-header-actions { display:flex; align-items:center; gap:8px; margin-left:auto; }
  .docs-share-btn { background:var(--gdocs-blue); color:white; border:none; padding:8px 20px; border-radius:20px; font-size:14px; font-weight:500; cursor:pointer; }
  .docs-share-btn:hover { background:var(--gdocs-blue-hover); }
  .docs-avatar { width:32px; height:32px; border-radius:50%; background:#ea4335; display:flex; align-items:center; justify-content:center; color:white; font-size:14px; font-weight:500; cursor:pointer; }
  .docs-toolbar { display:flex; align-items:center; padding:4px 8px; background:var(--gdocs-toolbar-bg); border-bottom:1px solid var(--gdocs-border); gap:2px; height:40px; overflow:hidden; }
  .toolbar-divider { width:1px; height:20px; background:var(--gdocs-border); margin:0 4px; }
  .toolbar-btn { padding:4px 6px; border:none; background:none; border-radius:4px; cursor:pointer; font-size:13px; color:var(--gdocs-gray); display:flex; align-items:center; min-width:28px; justify-content:center; }
  .toolbar-btn:hover { background:var(--gdocs-hover); }
  .toolbar-select { border:none; background:none; font-size:13px; color:var(--gdocs-text); cursor:pointer; padding:2px 4px; border-radius:4px; }
  .toolbar-select:hover { background:var(--gdocs-hover); }
  .docs-page-area { flex:1; overflow-y:auto; display:flex; justify-content:center; padding:20px 0 40px; background:#f0f4f9; }
  .docs-page { width:816px; min-height:1056px; background:white; box-shadow:0 1px 3px rgba(0,0,0,0.15); padding:72px 96px; outline:none; }
  .docs-page p { margin-bottom:12px; font-size:11pt; line-height:1.5; }
  .docs-page h1 { font-size:20pt; margin-bottom:16px; font-weight:400; }
  .docs-status { display:flex; align-items:center; padding:4px 16px; background:white; border-top:1px solid var(--gdocs-border); font-size:12px; color:var(--gdocs-gray); gap:16px; height:28px; }
  .docs-page-area::-webkit-scrollbar { width:8px; }
  .docs-page-area::-webkit-scrollbar-track { background:#f0f4f9; }
  .docs-page-area::-webkit-scrollbar-thumb { background:#bdc1c6; border-radius:4px; }

  /* ── PROXY ── */
  #proxy-shell { display:none; position:fixed; inset:0; z-index:9999; background:var(--proxy-bg); flex-direction:column; }
  #proxy-shell.active { display:flex; animation:fadeIn .2s ease; }
  @keyframes fadeIn { from{opacity:0;transform:translateY(4px)} to{opacity:1;transform:none} }

  .proxy-topbar { display:flex; align-items:center; padding:0 16px; height:48px; background:var(--proxy-surface); border-bottom:1px solid var(--proxy-border); gap:12px; flex-shrink:0; }
  .proxy-logo { display:flex; align-items:center; gap:8px; color:var(--proxy-accent); font-size:13px; font-weight:500; letter-spacing:.05em; text-transform:uppercase; }
  .proxy-logo-dot { width:8px; height:8px; border-radius:50%; background:var(--proxy-accent); animation:pulse 2s infinite; }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:.3} }
  .proxy-nav-btns { display:flex; gap:2px; }
  .proxy-nav-btn { width:32px; height:32px; background:none; border:1px solid var(--proxy-border); border-radius:6px; color:var(--proxy-muted); font-size:18px; cursor:pointer; display:flex; align-items:center; justify-content:center; transition:all .15s; }
  .proxy-nav-btn:hover:not(:disabled) { background:var(--proxy-border); color:var(--proxy-text); }
  .proxy-nav-btn:disabled { opacity:.3; cursor:default; }
  .proxy-url-bar { flex:1; display:flex; align-items:center; background:#0d0d1f; border:1px solid var(--proxy-border); border-radius:8px; padding:0 12px; gap:8px; height:34px; transition:border-color .15s; }
  .proxy-url-bar:focus-within { border-color:var(--proxy-accent); }
  .proxy-url-icon { color:var(--proxy-muted); font-size:12px; }
  .proxy-url-input { flex:1; background:none; border:none; outline:none; color:var(--proxy-text); font-size:13px; font-family:monospace; }
  .proxy-url-input::placeholder { color:var(--proxy-muted); }
  .proxy-go-btn { background:var(--proxy-accent); color:#0f0f1a; border:none; padding:6px 16px; border-radius:6px; font-size:13px; font-weight:700; cursor:pointer; }
  .proxy-go-btn:hover { background:#00b8d9; }
  .proxy-close-btn { width:32px; height:32px; background:rgba(239,68,68,.1); border:1px solid rgba(239,68,68,.3); border-radius:6px; color:#ef4444; font-size:14px; cursor:pointer; display:flex; align-items:center; justify-content:center; flex-shrink:0; }
  .proxy-close-btn:hover { background:rgba(239,68,68,.2); }

  .proxy-tabs { display:flex; align-items:center; padding:0 16px; gap:2px; background:var(--proxy-surface); border-bottom:1px solid var(--proxy-border); height:36px; flex-shrink:0; }
  .proxy-tab { display:flex; align-items:center; gap:6px; padding:4px 12px; border-radius:6px 6px 0 0; font-size:12px; color:var(--proxy-muted); cursor:pointer; border:1px solid transparent; border-bottom:none; max-width:180px; overflow:hidden; white-space:nowrap; text-overflow:ellipsis; }
  .proxy-tab.active { background:var(--proxy-bg); color:var(--proxy-text); border-color:var(--proxy-border); }
  .proxy-new-tab { width:24px; height:24px; border:1px solid var(--proxy-border); border-radius:4px; background:none; color:var(--proxy-muted); font-size:16px; cursor:pointer; display:flex; align-items:center; justify-content:center; margin-left:4px; }
  .proxy-new-tab:hover { background:var(--proxy-border); }

  .proxy-content { flex:1; position:relative; overflow:hidden; display:flex; flex-direction:column; }

  #proxy-loadbar-wrap { position:absolute; top:0; left:0; right:0; height:3px; z-index:5; display:none; }
  #proxy-loadbar-wrap.show { display:block; }
  #proxy-loadbar { height:100%; width:0%; background:var(--proxy-accent); box-shadow:0 0 6px var(--proxy-accent); transition:width .25s ease; border-radius:0 2px 2px 0; }

  .proxy-home { display:flex; flex-direction:column; align-items:center; justify-content:center; flex:1; color:var(--proxy-text); gap:28px; padding:32px; }
  .proxy-orb { width:70px; height:70px; border-radius:50%; background:radial-gradient(circle at 35% 35%,var(--proxy-accent),var(--proxy-accent2)); box-shadow:0 0 40px rgba(0,212,255,.3); animation:orb 3s ease-in-out infinite; }
  @keyframes orb { 0%,100%{transform:scale(1)} 50%{transform:scale(1.06)} }
  .proxy-home h1 { font-size:26px; font-weight:300; letter-spacing:.1em; text-transform:uppercase; }
  .proxy-home p { font-size:13px; color:var(--proxy-muted); text-align:center; max-width:420px; line-height:1.6; }
  .proxy-quick-links { display:flex; gap:8px; flex-wrap:wrap; justify-content:center; max-width:460px; }
  .proxy-quick-link { background:var(--proxy-surface); border:1px solid var(--proxy-border); border-radius:8px; padding:8px 14px; font-size:13px; color:var(--proxy-text); cursor:pointer; transition:all .15s; }
  .proxy-quick-link:hover { border-color:var(--proxy-accent); color:var(--proxy-accent); }
  .proxy-hint { font-size:11px; color:var(--proxy-muted); background:var(--proxy-surface); border:1px solid var(--proxy-border); border-radius:20px; padding:6px 16px; display:flex; align-items:center; gap:6px; flex-wrap:wrap; justify-content:center; }
  .proxy-kbd { background:var(--proxy-border); border-radius:4px; padding:1px 6px; font-size:11px; font-family:monospace; color:var(--proxy-accent); }

  #render-host { flex:1; overflow:auto; background:white; display:none; }
  #render-host.show { display:block; }

  .proxy-error { display:none; flex-direction:column; align-items:center; justify-content:center; flex:1; gap:14px; color:var(--proxy-text); padding:32px; text-align:center; }
  .proxy-error.show { display:flex; }
  .proxy-error-icon { font-size:36px; }
  .proxy-error h2 { font-size:17px; font-weight:400; }
  .proxy-error p { font-size:12px; color:var(--proxy-muted); max-width:460px; line-height:1.7; white-space:pre-wrap; font-family:monospace; background:rgba(255,255,255,.04); padding:12px; border-radius:8px; border:1px solid var(--proxy-border); }
  .proxy-retry-btn { background:var(--proxy-surface); border:1px solid var(--proxy-border); color:var(--proxy-text); padding:7px 18px; border-radius:8px; cursor:pointer; font-size:13px; }
  .proxy-retry-btn:hover { border-color:var(--proxy-accent); }
</style>
</head>
<body>

<!-- ══ GOOGLE DOCS DISGUISE ══ -->
<div id="docs-shell">
  <div class="docs-header">
    <a class="docs-logo" href="#" style="text-decoration:none">
      <svg width="30" height="40" viewBox="0 0 28 38" fill="none">
        <path d="M0 4C0 1.79 1.79 0 4 0H18L28 10V34C28 36.21 26.21 38 24 38H4C1.79 38 0 36.21 0 34V4Z" fill="#4285F4"/>
        <path d="M18 0L28 10H20C18.9 10 18 9.1 18 8V0Z" fill="#A8C7FA"/>
        <path d="M6 17H22M6 22H22M6 27H16" stroke="white" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
    </a>
    <div class="docs-title-area">
      <input class="docs-title-input" type="text" value="Untitled document" spellcheck="false">
      <div class="docs-meta">
        <span class="docs-menu-item">File</span><span class="docs-menu-item">Edit</span>
        <span class="docs-menu-item">View</span><span class="docs-menu-item">Insert</span>
        <span class="docs-menu-item">Format</span><span class="docs-menu-item">Tools</span>
        <span class="docs-menu-item">Extensions</span><span class="docs-menu-item">Help</span>
      </div>
    </div>
    <div class="docs-header-actions">
      <button class="docs-share-btn">Share</button>
      <div class="docs-avatar">U</div>
    </div>
  </div>
  <div class="docs-toolbar">
    <button class="toolbar-btn">↩</button><button class="toolbar-btn">↪</button><button class="toolbar-btn">🖨</button>
    <div class="toolbar-divider"></div>
    <select class="toolbar-select"><option>Normal text</option><option>Title</option><option>Heading 1</option></select>
    <div class="toolbar-divider"></div>
    <select class="toolbar-select" style="width:120px"><option>Arial</option><option>Times New Roman</option></select>
    <div class="toolbar-divider"></div>
    <button class="toolbar-btn" style="width:28px">−</button>
    <span style="font-size:13px;padding:0 4px">11</span>
    <button class="toolbar-btn" style="width:28px">+</button>
    <div class="toolbar-divider"></div>
    <button class="toolbar-btn"><strong>B</strong></button>
    <button class="toolbar-btn"><em>I</em></button>
    <button class="toolbar-btn"><u>U</u></button>
    <div class="toolbar-divider"></div>
    <button class="toolbar-btn">≡</button><button class="toolbar-btn">≡</button><button class="toolbar-btn">≡</button>
  </div>
  <div class="docs-page-area">
    <div class="docs-page" contenteditable="true" spellcheck="true" id="docs-page">
      <h1>Untitled document</h1>
      <p>Start typing here...</p>
      <p>&nbsp;</p><p>&nbsp;</p>
    </div>
  </div>
  <div class="docs-status">
    <span id="docs-save">All changes saved to Drive</span>
    <span>•</span><span>Page 1 of 1</span><span>•</span>
    <span id="docs-words">Word count: 4</span>
  </div>
</div>

<!-- ══ PROXY SHELL ══ -->
<div id="proxy-shell">
  <div class="proxy-topbar">
    <div class="proxy-logo"><div class="proxy-logo-dot"></div>VOID</div>
    <div class="proxy-nav-btns">
      <button class="proxy-nav-btn" id="btn-back" onclick="proxyBack()" disabled>&#8249;</button>
      <button class="proxy-nav-btn" id="btn-fwd"  onclick="proxyForward()" disabled>&#8250;</button>
      <button class="proxy-nav-btn" onclick="proxyReload()">&#8635;</button>
    </div>
    <div class="proxy-url-bar">
      <span class="proxy-url-icon">🔒</span>
      <input class="proxy-url-input" id="proxy-url-input" type="text"
        placeholder="Enter URL or search..."
        onkeydown="if(event.key==='Enter') proxyNavigate()">
    </div>
    <button class="proxy-go-btn" onclick="proxyNavigate()">GO</button>
    <button class="proxy-close-btn" onclick="hideProxy()">✕</button>
  </div>

  <div class="proxy-tabs" id="proxy-tabs">
    <div class="proxy-tab active"><span>🏠</span><span id="tab-title-0">New Tab</span></div>
    <button class="proxy-new-tab" onclick="addTab()">+</button>
  </div>

  <div class="proxy-content">
    <div id="proxy-loadbar-wrap"><div id="proxy-loadbar"></div></div>

    <div class="proxy-home" id="proxy-home">
      <div style="display:flex;flex-direction:column;align-items:center;gap:12px">
        <div class="proxy-orb"></div>
        <h1>Void Proxy</h1>
      </div>
      <p>Fetches pages through the local Node.js backend — no CORS issues, no iframe blocks.</p>
      <div class="proxy-quick-links">
        <div class="proxy-quick-link" onclick="loadQuick('https://www.google.com')">🔍 Google</div>
        <div class="proxy-quick-link" onclick="loadQuick('https://www.reddit.com')">🟠 Reddit</div>
        <div class="proxy-quick-link" onclick="loadQuick('https://en.wikipedia.org')">📖 Wikipedia</div>
        <div class="proxy-quick-link" onclick="loadQuick('https://news.ycombinator.com')">🔶 Hacker News</div>
        <div class="proxy-quick-link" onclick="loadQuick('https://github.com')">🐱 GitHub</div>
        <div class="proxy-quick-link" onclick="loadQuick('https://www.youtube.com')">▶ YouTube</div>
      </div>
      <div class="proxy-hint">
        Unlock: type <span class="proxy-kbd">openup</span> in the doc &nbsp;|&nbsp; Hide: <span class="proxy-kbd">Esc</span>
      </div>
    </div>

    <div id="render-host"></div>

    <div class="proxy-error" id="proxy-error">
      <div class="proxy-error-icon">⚠️</div>
      <h2>Could not load page</h2>
      <p id="proxy-error-msg"></p>
      <button class="proxy-retry-btn" onclick="proxyReload()">Try again</button>
    </div>
  </div>
</div>

<script>
// ─── CONFIG ───────────────────────────────────────
const SECRET   = 'openup';
const PROXY_API = '/proxy?url=';   // hits our local server.js

// ─── STATE ────────────────────────────────────────
let proxyVisible = false;
let currentURL   = '';
let hist         = [];
let histPos      = -1;

// ─── SHOW / HIDE ──────────────────────────────────
function showProxy() {
  proxyVisible = true;
  document.getElementById('proxy-shell').classList.add('active');
  setTimeout(() => document.getElementById('proxy-url-input').focus(), 60);
}
function hideProxy() {
  proxyVisible = false;
  document.getElementById('proxy-shell').classList.remove('active');
}
document.addEventListener('keydown', e => {
  if (e.key === 'Escape' && proxyVisible) hideProxy();
});

// ─── SECRET CODE ──────────────────────────────────
let buf = '', bufTimer = null;
document.getElementById('docs-page').addEventListener('keydown', e => {
  if (proxyVisible) return;
  if (e.key.length !== 1 || e.ctrlKey || e.altKey || e.metaKey) return;
  buf += e.key.toLowerCase();
  if (buf.length > SECRET.length) buf = buf.slice(-SECRET.length);
  clearTimeout(bufTimer);
  bufTimer = setTimeout(() => { buf = ''; }, 3000);
  if (buf === SECRET) {
    buf = '';
    setTimeout(() => {
      const sel = window.getSelection();
      if (sel && sel.rangeCount) {
        const range = sel.getRangeAt(0).cloneRange();
        const node  = range.startContainer;
        const end   = range.startOffset;
        const start = Math.max(0, end - SECRET.length);
        if (node.nodeType === Node.TEXT_NODE) node.deleteData(start, end - start);
      }
      showProxy();
    }, 30);
  }
});

// ─── URL HELPERS ──────────────────────────────────
function normalizeURL(raw) {
  raw = (raw || '').trim();
  if (!raw) return null;
  if (/^https?:\\/\\//i.test(raw)) return raw;
  if (raw.includes('.') && !raw.includes(' ')) return 'https://' + raw;
  return 'https://www.google.com/search?q=' + encodeURIComponent(raw);
}
function resolveURL(href, base) {
  try { return new URL(href, base).href; } catch { return href; }
}

// ─── NAVIGATION ───────────────────────────────────
function proxyNavigate() {
  const url = normalizeURL(document.getElementById('proxy-url-input').value);
  if (!url) return;
  document.getElementById('proxy-url-input').value = url;
  loadURL(url, true);
}
function loadQuick(url) {
  document.getElementById('proxy-url-input').value = url;
  loadURL(url, true);
}
function proxyBack()    { if (histPos > 0)               { histPos--; _goto(hist[histPos]); } }
function proxyForward() { if (histPos < hist.length - 1) { histPos++; _goto(hist[histPos]); } }
function proxyReload()  { if (currentURL) loadURL(currentURL, false); }
function _goto(url)     { document.getElementById('proxy-url-input').value = url; loadURL(url, false); }
function updateNavBtns() {
  document.getElementById('btn-back').disabled = histPos <= 0;
  document.getElementById('btn-fwd').disabled  = histPos >= hist.length - 1;
}

// ─── LOADING BAR ──────────────────────────────────
let loadTimer = null, loadPct = 0;
function startLoad() {
  document.getElementById('proxy-loadbar-wrap').classList.add('show');
  loadPct = 0;
  document.getElementById('proxy-loadbar').style.width = '0%';
  clearInterval(loadTimer);
  loadTimer = setInterval(() => {
    loadPct += loadPct < 70 ? Math.random() * 10 : loadPct < 90 ? 1 : 0.2;
    if (loadPct > 93) loadPct = 93;
    document.getElementById('proxy-loadbar').style.width = loadPct + '%';
  }, 200);
}
function stopLoad() {
  clearInterval(loadTimer);
  document.getElementById('proxy-loadbar').style.width = '100%';
  setTimeout(() => {
    document.getElementById('proxy-loadbar-wrap').classList.remove('show');
    document.getElementById('proxy-loadbar').style.width = '0%';
  }, 350);
}

// ─── FETCH ────────────────────────────────────────
async function loadURL(url, push) {
  currentURL = url;
  if (push) { hist = hist.slice(0, histPos + 1); hist.push(url); histPos = hist.length - 1; }
  updateNavBtns();

  const home   = document.getElementById('proxy-home');
  const host   = document.getElementById('render-host');
  const errDiv = document.getElementById('proxy-error');

  home.style.display = 'none';
  host.classList.remove('show');
  errDiv.classList.remove('show');
  startLoad();

  let result = null;

  try {
    const res  = await fetch(PROXY_API + encodeURIComponent(url));
    result = await res.json();
  } catch(e) {
    stopLoad();
    document.getElementById('proxy-error-msg').textContent =
      \`Fetch failed: \${e.message}\\n\\nMake sure server.js is running:\\n  node server.js\`;
    errDiv.classList.add('show');
    return;
  }

  // Follow redirect
  let finalURL = url;
  let html = result.body;

  if (result.redirect) {
    // Server told us there's a redirect — follow it transparently
    try {
      const res2 = await fetch(PROXY_API + encodeURIComponent(result.redirect));
      const r2   = await res2.json();
      html = r2.body;
      finalURL = result.redirect;
      document.getElementById('proxy-url-input').value = finalURL;
      currentURL = finalURL;
    } catch(e) { /* use what we have */ }
  }

  if (result.error) {
    stopLoad();
    document.getElementById('proxy-error-msg').textContent = result.error;
    errDiv.classList.add('show');
    return;
  }

  if (!html || html.trim().length < 20) {
    stopLoad();
    document.getElementById('proxy-error-msg').textContent = 'Empty response from server.';
    errDiv.classList.add('show');
    return;
  }

  stopLoad();
  renderPage(html, finalURL);
}

// ─── RENDER ───────────────────────────────────────
function proxyAssetURL(url) {
  // Route every asset through our local proxy server
  if (!url || url.startsWith('data:') || url.startsWith('blob:') || url.startsWith('javascript:') || url.startsWith('#') || url.startsWith('/proxy?')) return url;
  return '/proxy?url=' + encodeURIComponent(url) + '&asset=1';
}

function rewriteCSS(css, baseURL) {
  // Rewrite url(...) inside CSS through the proxy
  var CSS_URL_RE = new RegExp("url\\\\(\\\\s*(['\"]?)([^'\"\\\\)\\\\s]+)\\\\1\\\\s*\\\\)", "gi");
  return css.replace(CSS_URL_RE, function(match, quote, u) {
    if (u.indexOf('data:') === 0 || u.indexOf('blob:') === 0) return match;
    var abs = resolveURL(u, baseURL);
    return 'url(' + quote + proxyAssetURL(abs) + quote + ')';
  });
}

function renderPage(html, baseURL) {
  const host  = document.getElementById('render-host');
  const fresh = document.createElement('div');
  fresh.id    = 'render-host';
  host.replaceWith(fresh);

  const shadow = fresh.attachShadow({ mode: 'open' });

  const parser = new DOMParser();
  const doc    = parser.parseFromString(html, 'text/html');

  // ── Rewrite all src/href attributes through proxy ──
  const srcEls = doc.querySelectorAll('[src]');
  srcEls.forEach(el => {
    const v = el.getAttribute('src');
    if (v && !v.startsWith('data:') && !v.startsWith('blob:') && v.trim()) {
      el.setAttribute('src', proxyAssetURL(resolveURL(v, baseURL)));
    }
  });

  // srcset (images with responsive sources)
  doc.querySelectorAll('[srcset]').forEach(el => {
    const parts = el.getAttribute('srcset').split(',').map(part => {
      const [u, ...rest] = part.trim().split(/\s+/);
      const abs = resolveURL(u, baseURL);
      return [proxyAssetURL(abs), ...rest].join(' ');
    });
    el.setAttribute('srcset', parts.join(', '));
  });

  // ── Rewrite inline <style> tags — url() inside CSS ──
  const styleTags = [...doc.querySelectorAll('style')].map(s => {
    const rewritten = rewriteCSS(s.textContent, baseURL);
    return \`<style>\${rewritten}</style>\`;
  }).join('\\n');

  // ── Rewrite <link rel="stylesheet"> through proxy ──
  const linkTags = [...doc.querySelectorAll('link[rel~="stylesheet"]')].map(l => {
    const href = l.getAttribute('href');
    if (!href) return '';
    const abs = resolveURL(href, baseURL);
    return \`<link rel="stylesheet" href="\${proxyAssetURL(abs)}">\`;
  }).join('\\n');

  // ── Rewrite inline style attributes ──
  doc.querySelectorAll('[style]').forEach(el => {
    const s = el.getAttribute('style');
    if (s && s.includes('url(')) {
      el.setAttribute('style', rewriteCSS(s, baseURL));
    }
  });

  // ── Rewrite <script src> through proxy ──
  const scriptTags = [...doc.querySelectorAll('script[src]')].map(s => {
    const src = s.getAttribute('src');
    if (!src) return '';
    const abs = resolveURL(src, baseURL);
    return \`<script src="\${proxyAssetURL(abs)}"><\/script>\`;
  }).join('\\n');

  // ── Build shadow DOM ──
  shadow.innerHTML = \`
    <style>:host{display:block;min-height:100%} *{box-sizing:border-box}</style>
    \${linkTags}
    \${styleTags}
    \${scriptTags}
    <div id="__root__">\${doc.body ? doc.body.innerHTML : html}</div>\`;

  // ── Intercept links ──
  shadow.querySelectorAll('a').forEach(a => {
    const href = a.getAttribute('href');
    if (!href || href.startsWith('#') || href.startsWith('javascript:')) return;
    a.dataset.voidHref = resolveURL(href, baseURL);
    a.setAttribute('href','#');
  });
  shadow.addEventListener('click', e => {
    const a = e.composedPath().find(n => n.tagName === 'A' && n.dataset && n.dataset.voidHref);
    if (!a) return;
    e.preventDefault();
    document.getElementById('proxy-url-input').value = a.dataset.voidHref;
    loadURL(a.dataset.voidHref, true);
  });

  // ── Intercept forms ──
  shadow.querySelectorAll('form').forEach(form => {
    form.dataset.voidAction = resolveURL(form.getAttribute('action') || baseURL, baseURL);
    form.removeAttribute('action');
  });
  shadow.addEventListener('submit', e => {
    e.preventDefault();
    const form   = e.target;
    const action = form.dataset.voidAction || baseURL;
    const method = (form.getAttribute('method') || 'get').toLowerCase();
    if (method === 'get') {
      const qs   = new URLSearchParams(new FormData(form)).toString();
      const dest = action + (action.includes('?') ? '&' : '?') + qs;
      document.getElementById('proxy-url-input').value = dest;
      loadURL(dest, true);
    }
  });

  const title = doc.title || baseURL;
  document.getElementById('tab-title-0').textContent = title.length > 22 ? title.slice(0,22)+'…' : title;
  fresh.classList.add('show');
}

// ─── TABS ─────────────────────────────────────────
let tabCount = 1;
function addTab() {
  if (tabCount >= 6) return; tabCount++;
  const tabs = document.getElementById('proxy-tabs');
  const btn  = tabs.querySelector('.proxy-new-tab');
  const tab  = document.createElement('div');
  tab.className = 'proxy-tab';
  tab.innerHTML = \`<span>🏠</span><span>New Tab</span><span style="margin-left:4px;opacity:.5;font-size:10px" onclick="this.parentNode.remove()">✕</span>\`;
  tabs.insertBefore(tab, btn);
}

// ─── DOCS AUTOSAVE ────────────────────────────────
const docsPage = document.getElementById('docs-page');
docsPage.addEventListener('input', () => {
  const words = docsPage.innerText.trim().split(/\\s+/).filter(Boolean).length;
  document.getElementById('docs-words').textContent = \`Word count: \${words}\`;
  document.getElementById('docs-save').textContent  = 'Saving…';
  clearTimeout(window._st);
  window._st = setTimeout(() => {
    document.getElementById('docs-save').textContent = 'All changes saved to Drive';
  }, 1200);
});
</script>
</body>
</html>
`;

const server = http.createServer((req, res) => {
  const reqURL = new URL(req.url, "http://localhost:65535");

  // Serve frontend
  if (reqURL.pathname === '/' || reqURL.pathname === '/index.html') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    return res.end(HTML);
  }

  // CORS preflight
  if (req.method === 'OPTIONS') {
    res.writeHead(204, { 'Access-Control-Allow-Origin': '*' });
    return res.end();
  }

  // Proxy endpoint
  if (reqURL.pathname === '/proxy') {
    const target = reqURL.searchParams.get('url');
    if (!target) {
      res.writeHead(400, { 'Content-Type': 'application/json' });
      return res.end(JSON.stringify({ error: 'Missing url parameter' }));
    }

    let targetURL;
    try { targetURL = new URL(target); }
    catch(e) {
      res.writeHead(400, { 'Content-Type': 'application/json' });
      return res.end(JSON.stringify({ error: 'Invalid URL: ' + target }));
    }

    const lib = targetURL.protocol === 'https:' ? https : http;

    const options = {
      hostname: targetURL.hostname,
      port:     targetURL.port || (targetURL.protocol === 'https:' ? 443 : 80),
      path:     targetURL.pathname + (targetURL.search || ''),
      method:   'GET',
      headers: {
        'User-Agent':      'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 Chrome/120 Safari/537.36',
        'Accept':          'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
        'Accept-Language': 'en-US,en;q=0.9',
        'Accept-Encoding': 'identity',
        'Cache-Control':   'no-cache',
      },
      timeout: 20000,
    };

    const isHTML = !reqURL.searchParams.get('asset');

    const proxyReq = lib.request(options, proxyRes => {
      // Handle redirects
      if ([301, 302, 303, 307, 308].includes(proxyRes.statusCode)) {
        const location = proxyRes.headers['location'];
        if (location) {
          const redirected = location.startsWith('http')
            ? location : new URL(location, target).href;
          if (isHTML) {
            res.writeHead(200, { 'Content-Type': 'application/json', 'Access-Control-Allow-Origin': '*' });
            return res.end(JSON.stringify({ redirect: redirected }));
          } else {
            res.writeHead(302, { 'Location': '/proxy?url=' + encodeURIComponent(redirected) + '&asset=1', 'Access-Control-Allow-Origin': '*' });
            return res.end();
          }
        }
      }

      const ct = proxyRes.headers['content-type'] || '';
      const chunks = [];
      proxyRes.on('data', c => chunks.push(c));
      proxyRes.on('end', () => {
        const buf = Buffer.concat(chunks);

        if (isHTML) {
          // HTML page fetch — return as JSON for the frontend to parse
          res.writeHead(200, { 'Content-Type': 'application/json', 'Access-Control-Allow-Origin': '*' });
          res.end(JSON.stringify({ status: proxyRes.statusCode, contentType: ct, body: buf.toString('utf-8') }));
        } else {
          // Asset fetch (CSS, JS, image, font) — stream directly with correct content-type
          // Rewrite CSS url() references on the fly
          let outBuf = buf;
          if (ct.includes('text/css')) {
            let css = buf.toString('utf-8');
            var CSS_RE1 = new RegExp("url\\(\\s*(['\"]?)([^'\"\\)\\s]+)\\1\\s*\\)", "gi");
            css = css.replace(CSS_RE1, function(match, quote, u) {
              if (u.indexOf('data:') === 0 || u.indexOf('blob:') === 0 || u.indexOf('http') === 0) return match;
              try {
                var abs = new URL(u, target).href;
                return 'url(' + quote + '/proxy?url=' + encodeURIComponent(abs) + '&asset=1' + quote + ')';
              } catch(e) { return match; }
            });
            var CSS_RE2 = new RegExp("url\\(\\s*(['\"]?)(https?:\\/\\/[^'\"\\)\\s]+)\\1\\s*\\)", "gi");
            css = css.replace(CSS_RE2, function(match, quote, u) {
              return 'url(' + quote + '/proxy?url=' + encodeURIComponent(u) + '&asset=1' + quote + ')';
            });
            outBuf = Buffer.from(css, 'utf-8');
          }
          res.writeHead(200, {
            'Content-Type': ct || 'application/octet-stream',
            'Access-Control-Allow-Origin': '*',
            'Cache-Control': 'public, max-age=3600',
          });
          res.end(outBuf);
        }
      });
    });

    proxyReq.on('timeout', () => {
      proxyReq.destroy();
      res.writeHead(504, { 'Content-Type': 'application/json' });
      res.end(JSON.stringify({ error: 'Request timed out' }));
    });

    proxyReq.on('error', err => {
      res.writeHead(502, { 'Content-Type': 'application/json', 'Access-Control-Allow-Origin': '*' });
      res.end(JSON.stringify({ error: err.message }));
    });

    proxyReq.end();
    return;
  }

  res.writeHead(404);
  res.end('Not found');
});

server.listen(PORT, () => {
  console.log("VOID PROXY running on port " + PORT);
});
