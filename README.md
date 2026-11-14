<!DOCTYPE html>
<html lang="en" class="dark">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>GitHub Profile Explorer</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet" />
<script>
  try { if (JSON.parse(localStorage.getItem('ghx_theme')) === 'light') document.documentElement.classList.remove('dark'); } catch (e) {}
</script>
<script src="https://cdn.tailwindcss.com"></script>
<script>
  tailwind.config = {
    darkMode: 'class',
    theme: { extend: { fontFamily: { sans: ['Inter', 'ui-sans-serif', 'system-ui', 'sans-serif'], mono: ['JetBrains Mono', 'ui-monospace', 'monospace'] } } }
  };
</script>
<style>
  :root {
    --hm0:#ebedf0; --hm1:#9be9a8; --hm2:#40c463; --hm3:#30a14e; --hm4:#216e39;
    --code-bg:#f4f4f5; --pl-k:#cf222e; --pl-c:#6e7781; --pl-s:#0a3069; --pl-en:#8250df; --pl-c1:#0550ae;
  }
  .dark {
    --hm0:#18181b; --hm1:#0e4429; --hm2:#006d32; --hm3:#26a641; --hm4:#39d353;
    --code-bg:#18181b; --pl-k:#ff7b72; --pl-c:#8b949e; --pl-s:#a5d6ff; --pl-en:#d2a8ff; --pl-c1:#79c0ff;
  }
  html { scroll-behavior: smooth; }
  body { font-family: 'Inter', ui-sans-serif, system-ui, sans-serif; }
  .bg-glow {
    background-image:
      radial-gradient(60rem 30rem at 50% -10%, rgba(99,102,241,.14), transparent 60%),
      radial-gradient(40rem 25rem at 100% 10%, rgba(34,211,238,.08), transparent 60%);
  }
  button:focus-visible, a:focus-visible, [role=button]:focus-visible, [role=tab]:focus-visible, select:focus-visible {
    outline: 2px solid #6366f1; outline-offset: 2px;
  }
  /* animations */
  @keyframes fadeUp { from { opacity: 0; transform: translateY(14px); } to { opacity: 1; transform: none; } }
  .fade-up { animation: fadeUp .55s cubic-bezier(.22,1,.36,1) backwards; }
  @keyframes pop { from { opacity: 0; transform: scale(.5); } to { opacity: 1; transform: none; } }
  .pop { animation: pop .4s cubic-bezier(.34,1.56,.64,1) backwards; }
  @keyframes spin { to { transform: rotate(360deg); } }
  .spinner { width: 16px; height: 16px; border: 2px solid currentColor; border-right-color: transparent; border-radius: 50%; animation: spin .7s linear infinite; display: inline-block; }
  @keyframes shimmer { 100% { transform: translateX(100%); } }
  .skeleton { position: relative; overflow: hidden; }
  .skeleton::after { content: ''; position: absolute; inset: 0; transform: translateX(-100%); background: linear-gradient(90deg, transparent, rgba(255,255,255,.09), transparent); animation: shimmer 1.4s infinite; }
  @keyframes float { 0%,100% { transform: translateY(0); } 50% { transform: translateY(-6px); } }
  .float { animation: float 3s ease-in-out infinite; }
  @keyframes toastIn { from { opacity: 0; transform: translate(-50%, 16px) scale(.96); } to { opacity: 1; transform: translate(-50%, 0) scale(1); } }
  .toast-in { animation: toastIn .35s cubic-bezier(.34,1.4,.64,1); }
  @keyframes checkPop { 0% { transform: scale(.3); opacity: 0; } 60% { transform: scale(1.25); } 100% { transform: scale(1); opacity: 1; } }
  .check-pop { animation: checkPop .4s ease-out; }
  .lift { transition: transform .2s ease, box-shadow .2s ease, border-color .2s ease; }
  .lift:hover { transform: translateY(-2px); border-color: rgba(99,102,241,.5) !important; box-shadow: 0 10px 30px -14px rgba(99,102,241,.45); }
  /* heatmap */
  .hm { width: 11px; height: 11px; border-radius: 2px; background: var(--hm0); }
  .hm-1 { background: var(--hm1); } .hm-2 { background: var(--hm2); } .hm-3 { background: var(--hm3); } .hm-4 { background: var(--hm4); }
  .hm:hover { outline: 1.5px solid #818cf8; outline-offset: 1px; }
  /* README typography */
  .readme { font-size: 14.5px; line-height: 1.7; word-wrap: break-word; }
  .readme h1, .readme h2, .readme h3, .readme h4 { font-weight: 600; line-height: 1.25; margin: 1.4em 0 .6em; }
  .readme h1 { font-size: 1.7em; padding-bottom: .3em; border-bottom: 1px solid rgba(113,113,122,.3); }
  .readme h2 { font-size: 1.4em; padding-bottom: .3em; border-bottom: 1px solid rgba(113,113,122,.3); }
  .readme h3 { font-size: 1.15em; }
  .readme p, .readme ul, .readme ol, .readme pre, .readme table, .readme blockquote, .readme details { margin: 0 0 1em; }
  .readme ul { list-style: disc; padding-left: 1.6em; }
  .readme ol { list-style: decimal; padding-left: 1.6em; }
  .readme li + li { margin-top: .25em; }
  .readme a { color: #6366f1; text-decoration: none; }
  .readme a:hover { text-decoration: underline; }
  .readme code { font-family: 'JetBrains Mono', ui-monospace, monospace; font-size: .86em; background: var(--code-bg); padding: .15em .4em; border-radius: 6px; }
  .readme pre { background: var(--code-bg); padding: 14px 16px; border-radius: 10px; overflow: auto; border: 1px solid rgba(113,113,122,.25); }
  .readme pre code { background: none; padding: 0; }
  .readme blockquote { border-left: 3px solid rgba(113,113,122,.5); padding: .1em 1em; opacity: .85; }
  .readme img { max-width: 100%; height: auto; border-radius: 6px; }
  .readme table { display: block; overflow: auto; border-collapse: collapse; width: max-content; max-width: 100%; }
  .readme th, .readme td { border: 1px solid rgba(113,113,122,.3); padding: 6px 12px; }
  .readme th { font-weight: 600; background: rgba(113,113,122,.1); }
  .readme hr { border: 0; border-top: 1px solid rgba(113,113,122,.3); margin: 1.5em 0; }
  .readme summary { cursor: pointer; }
  .readme .pl-k { color: var(--pl-k); } .readme .pl-c { color: var(--pl-c); font-style: italic; }
  .readme .pl-s, .readme .pl-pds { color: var(--pl-s); } .readme .pl-en { color: var(--pl-en); } .readme .pl-c1, .readme .pl-smi { color: var(--pl-c1); }
  ::-webkit-scrollbar { height: 8px; width: 8px; } ::-webkit-scrollbar-thumb { background: rgba(113,113,122,.35); border-radius: 8px; }
  @media (prefers-reduced-motion: reduce) { *, *::before, *::after { animation-duration: .001ms !important; transition-duration: .001ms !important; } }
</style>
</head>
<body class="bg-zinc-50 text-zinc-900 dark:bg-zinc-950 dark:text-zinc-100 antialiased">
<div id="root"></div>

<script src="https://unpkg.com/react@18.3.1/umd/react.production.min.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@18.3.1/umd/react-dom.production.min.js" crossorigin></script>
<script src="https://unpkg.com/@babel/standalone@7.25.6/babel.min.js"></script>

<script type="text/babel" data-presets="react">
const { useState, useEffect, useRef, useMemo, useCallback } = React;

/* ───────────────────────── helpers ───────────────────────── */
const API = 'https://api.github.com';
const QUICK_USERS = ['torvalds', 'gaearon', 'yyx990803', 'sindresorhus', 'addyosmani', 'tj'];

const LS = {
  get(k, d) { try { const v = localStorage.getItem(k); return v === null ? d : JSON.parse(v); } catch { return d; } },
  set(k, v) { try { localStorage.setItem(k, JSON.stringify(v)); } catch {} },
};

const LANG_COLORS = {
  JavaScript:'#f1e05a', TypeScript:'#3178c6', Python:'#3572A5', Java:'#b07219', C:'#6e6e6e', 'C++':'#f34b7d', 'C#':'#178600',
  Go:'#00ADD8', Rust:'#dea584', Ruby:'#c0392b', PHP:'#4F5D95', Swift:'#F05138', Kotlin:'#A97BFF', Dart:'#00B4AB',
  HTML:'#e34c26', CSS:'#563d7c', SCSS:'#c6538c', Shell:'#89e051', Vue:'#41b883', Svelte:'#ff3e00', 'Jupyter Notebook':'#DA5B0B',
  'Objective-C':'#438eff', Scala:'#c22d40', Elixir:'#6e4a7e', Haskell:'#5e5086', 'Vim Script':'#199f4b', Makefile:'#427819',
  Dockerfile:'#384d54', PowerShell:'#3b6ea5', R:'#198CE7', Perl:'#0298c3', Zig:'#ec915c', Nix:'#7e7eff', Lua:'#3b4fa0', MDX:'#fcb32c',
};
function langColor(name) {
  if (!name) return '#6366f1';
  if (LANG_COLORS[name]) return LANG_COLORS[name];
  let h = 0; for (const c of name) h = (h * 31 + c.charCodeAt(0)) % 360;
  return `hsl(${h} 60% 55%)`;
}

const rtf = new Intl.RelativeTimeFormat('en', { numeric: 'auto' });
function timeAgo(d) {
  const s = (new Date(d) - Date.now()) / 1000;
  const units = [['year', 31536000], ['month', 2592000], ['week', 604800], ['day', 86400], ['hour', 3600], ['minute', 60]];
  for (const [u, sec] of units) if (Math.abs(s) >= sec) return rtf.format(Math.round(s / sec), u);
  return 'just now';
}
const fmtCount = n => new Intl.NumberFormat('en', { notation: 'compact', maximumFractionDigits: 1 }).format(n || 0);
const cap = s => (s ? s.charAt(0).toUpperCase() + s.slice(1) : '');
const firstLine = s => (s || '').split('\n')[0];
function fmtCountdown(ms) {
  const t = Math.ceil(ms / 1000); const m = Math.floor(t / 60); const s = t % 60;
  return `${m}:${String(s).padStart(2, '0')}`;
}
function parseUsername(input) {
  let s = (input || '').trim();
  s = s.replace(/^https?:\/\/(www\.)?github\.com\//i, '').replace(/^@/, '');
  s = s.split(/[/?#]/)[0];
  return /^[A-Za-z0-9][A-Za-z0-9_-]{0,38}$/.test(s) ? s : '';
}
function syncUrl(name, replace = false) {
  try {
    const u = new URL(window.location.href);
    if (name) u.searchParams.set('user', name); else u.searchParams.delete('user');
    window.history[replace ? 'replaceState' : 'pushState']({}, '', u.toString());
  } catch {}
}
async function copyText(text) {
  try { await navigator.clipboard.writeText(text); return true; }
  catch {
    try {
      const ta = document.createElement('textarea');
      ta.value = text; ta.style.position = 'fixed'; ta.style.opacity = '0';
      document.body.appendChild(ta); ta.select();
      const ok = document.execCommand('copy'); ta.remove(); return ok;
    } catch { return false; }
  }
}
function trapTab(e, container) {
  if (e.key !== 'Tab' || !container) return;
  const f = container.querySelectorAll('a[href],button:not([disabled]),input,select,textarea,[tabindex]:not([tabindex="-1"])');
  if (!f.length) return;
  const first = f[0], last = f[f.length - 1];
  if (e.shiftKey && document.activeElement === first) { e.preventDefault(); last.focus(); }
  else if (!e.shiftKey && document.activeElement === last) { e.preventDefault(); first.focus(); }
}

/* ───────────────────────── API layer ───────────────────────── */
class ApiError extends Error {
  constructor(kind, message, extra = {}) { super(message); this.kind = kind; Object.assign(this, extra); }
}
async function ghFetch(path, { token, accept = 'application/vnd.github+json', signal, onRate, raw = false } = {}) {
  const headers = { Accept: accept };
  if (token) headers.Authorization = `Bearer ${token}`;
  let res;
  try { res = await fetch(path.startsWith('http') ? path : API + path, { headers, signal }); }
  catch (e) {
    if (e.name === 'AbortError') throw e;
    throw new ApiError('network', 'Network error. Check your connection and try again.');
  }
  const limit = res.headers.get('x-ratelimit-limit');
  const remainingH = res.headers.get('x-ratelimit-remaining');
  const resetH = res.headers.get('x-ratelimit-reset');
  if (limit !== null && onRate) onRate({ limit: +limit, remaining: +remainingH, reset: +resetH * 1000 });
  if (res.ok) return raw ? res.text() : res.json();

  let msg = '';
  try { msg = (await res.clone().json()).message || ''; } catch {}
  if (res.status === 404) throw new ApiError('notfound', 'Not found');
  if (res.status === 401) throw new ApiError('badtoken', 'GitHub rejected the access token.');
  if (res.status === 403 || res.status === 429) {
    const retryAfter = res.headers.get('retry-after');
    if (remainingH === '0' || res.status === 429 || /rate limit/i.test(msg)) {
      const reset = resetH ? +resetH * 1000 : retryAfter ? Date.now() + +retryAfter * 1000 : Date.now() + 60000;
      throw new ApiError('ratelimit', 'Rate limit exceeded', { reset });
    }
    throw new ApiError('forbidden', msg || 'Access to this resource is forbidden.');
  }
  throw new ApiError('http', msg || `GitHub API returned ${res.status}`);
}

function sanitizeReadme(html, repo) {
  const doc = new DOMParser().parseFromString(html, 'text/html');
  doc.querySelectorAll('script,style,iframe,object,embed,form,link,meta,base').forEach(n => n.remove());
  doc.querySelectorAll('*').forEach(el => {
    Array.from(el.attributes).forEach(a => { if (/^on/i.test(a.name)) el.removeAttribute(a.name); });
  });
  const branch = repo.default_branch || 'HEAD';
  const rawBase = `https://raw.githubusercontent.com/${repo.full_name}/${branch}/`;
  const blobBase = `https://github.com/${repo.full_name}/blob/${branch}/`;
  doc.querySelectorAll('a.anchor').forEach(a => a.remove());
  doc.querySelectorAll('picture source').forEach(s => s.remove());
  doc.querySelectorAll('img').forEach(img => {
    const src = img.getAttribute('src') || '';
    if (src && !/^(https?:|data:|\/\/)/i.test(src)) {
      try { img.setAttribute('src', new URL(src.replace(/^\.?\//, ''), rawBase).href); } catch {}
    }
    img.setAttribute('loading', 'lazy');
  });
  doc.querySelectorAll('a[href]').forEach(a => {
    const href = (a.getAttribute('href') || '').trim();
    if (/^javascript:/i.test(href)) { a.removeAttribute('href'); return; }
    if (href.startsWith('#')) a.setAttribute('href', `https://github.com/${repo.full_name}${href}`);
    else if (!/^(https?:|mailto:|\/\/)/i.test(href)) {
      try { a.setAttribute('href', new URL(href.replace(/^\.?\//, ''), blobBase).href); } catch {}
    }
    a.setAttribute('target', '_blank'); a.setAttribute('rel', 'noopener noreferrer');
  });
  return doc.body.innerHTML;
}

/* ───────────────────────── analytics ───────────────────────── */
function analyze(repos) {
  const totals = { stars: 0, forks: 0, watchers: 0, issues: 0 };
  const langs = new Map();
  for (const r of repos) {
    totals.stars += r.stargazers_count || 0; totals.forks += r.forks_count || 0;
    totals.watchers += r.watchers_count || 0; totals.issues += r.open_issues_count || 0;
    if (r.language) langs.set(r.language, (langs.get(r.language) || 0) + 1);
  }
  const sorted = [...langs.entries()].sort((a, b) => b[1] - a[1]);
  const segments = sorted.slice(0, 6).map(([name, value]) => ({ name, value, color: langColor(name) }));
  const rest = sorted.slice(6).reduce((s, [, v]) => s + v, 0);
  if (rest) segments.push({ name: 'Other', value: rest, color: '#71717a' });
  const own = repos.filter(r => !r.fork);
  const pool = own.length ? own : repos;
  const byStars = [...pool].sort((a, b) => b.stargazers_count - a.stargazers_count);
  const byForks = [...pool].sort((a, b) => b.forks_count - a.forks_count);
  return {
    totals, segments, langTotal: sorted.reduce((s, [, v]) => s + v, 0),
    topRepos: byStars.slice(0, 8), mostStarred: byStars[0], mostForked: byForks[0],
    languages: sorted.map(([n]) => n),
  };
}
function dayKey(d) { return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}-${String(d.getDate()).padStart(2, '0')}`; }
function buildHeatmap(events) {
  const counts = {};
  events.forEach(e => { const k = dayKey(new Date(e.created_at)); counts[k] = (counts[k] || 0) + 1; });
  const today = new Date(); today.setHours(0, 0, 0, 0);
  const start = new Date(today);
  start.setDate(start.getDate() - (52 * 7 - 1));
  start.setDate(start.getDate() - start.getDay());
  const weeks = []; const cur = new Date(start); let max = 0, total = 0;
  while (cur <= today) {
    const week = [];
    for (let i = 0; i < 7; i++) {
      const future = cur > today; const k = dayKey(cur); const c = future ? 0 : counts[k] || 0;
      if (!future) { max = Math.max(max, c); total += c; }
      week.push({ date: new Date(cur), key: k, count: c, future });
      cur.setDate(cur.getDate() + 1);
    }
    weeks.push(week);
  }
  return { weeks, max, total };
}

/* ───────────────────────── icons ───────────────────────── */
const ICONS = {
  search: <><circle cx="11" cy="11" r="8" /><path d="m21 21-4.3-4.3" /></>,
  x: <><path d="M18 6 6 18" /><path d="m6 6 12 12" /></>,
  star: <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2" />,
  fork: <><circle cx="12" cy="18" r="3" /><circle cx="6" cy="6" r="3" /><circle cx="18" cy="6" r="3" /><path d="M18 9v2c0 .6-.4 1-1 1H7c-.6 0-1-.4-1-1V9" /><path d="M12 12v3" /></>,
  eye: <><path d="M2 12s3-7 10-7 10 7 10 7-3 7-10 7-10-7-10-7Z" /><circle cx="12" cy="12" r="3" /></>,
  issue: <><circle cx="12" cy="12" r="10" /><circle cx="12" cy="12" r="1" /></>,
  pin: <><path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z" /><circle cx="12" cy="10" r="3" /></>,
  building: <><rect width="16" height="20" x="4" y="2" rx="2" /><path d="M9 22v-4h6v4" /><path d="M8 6h.01M16 6h.01M12 6h.01M12 10h.01M12 14h.01M16 10h.01M16 14h.01M8 10h.01M8 14h.01" /></>,
  link: <><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71" /><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71" /></>,
  calendar: <><rect width="18" height="18" x="3" y="4" rx="2" /><path d="M16 2v4M8 2v4M3 10h18" /></>,
  users: <><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2" /><circle cx="9" cy="7" r="4" /><path d="M22 21v-2a4 4 0 0 0-3-3.87" /><path d="M16 3.13a4 4 0 0 1 0 7.75" /></>,
  check: <path d="M20 6 9 17l-5-5" />,
  copy: <><rect width="14" height="14" x="8" y="8" rx="2" /><path d="M4 16c-1.1 0-2-.9-2-2V4c0-1.1.9-2 2-2h10c1.1 0 2 .9 2 2" /></>,
  ext: <><path d="M15 3h6v6" /><path d="M10 14 21 3" /><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6" /></>,
  sun: <><circle cx="12" cy="12" r="4" /><path d="M12 2v2M12 20v2M4.93 4.93l1.41 1.41M17.66 17.66l1.41 1.41M2 12h2M20 12h2M6.34 17.66l-1.41 1.41M19.07 4.93l-1.41 1.41" /></>,
  moon: <path d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9Z" />,
  key: <><circle cx="7.5" cy="15.5" r="5.5" /><path d="m21 2-9.6 9.6" /><path d="m15.5 7.5 3 3L22 7l-3-3" /></>,
  zap: <polygon points="13 2 3 14 12 14 11 22 21 10 12 10 13 2" />,
  commit: <><circle cx="12" cy="12" r="3" /><line x1="3" x2="9" y1="12" y2="12" /><line x1="15" x2="21" y1="12" y2="12" /></>,
  pr: <><circle cx="18" cy="18" r="3" /><circle cx="6" cy="6" r="3" /><path d="M13 6h3a2 2 0 0 1 2 2v7" /><line x1="6" x2="6" y1="9" y2="21" /></>,
  tag: <><path d="M12.586 2.586A2 2 0 0 0 11.172 2H4a2 2 0 0 0-2 2v7.172a2 2 0 0 0 .586 1.414l8.704 8.704a2.426 2.426 0 0 0 3.42 0l6.58-6.58a2.426 2.426 0 0 0 0-3.42z" /><circle cx="7.5" cy="7.5" r=".5" fill="currentColor" /></>,
  plus: <><path d="M5 12h14" /><path d="M12 5v14" /></>,
  branch: <><line x1="6" x2="6" y1="3" y2="15" /><circle cx="18" cy="6" r="3" /><circle cx="6" cy="18" r="3" /><path d="M18 9a9 9 0 0 1-9 9" /></>,
  book: <path d="M4 19.5v-15A2.5 2.5 0 0 1 6.5 2H20v20H6.5a2.5 2.5 0 0 1 0-5H20" />,
  clock: <><circle cx="12" cy="12" r="10" /><polyline points="12 6 12 12 16 14" /></>,
  alert: <><path d="m21.73 18-8-14a2 2 0 0 0-3.48 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3" /><path d="M12 9v4" /><path d="M12 17h.01" /></>,
  trash: <><path d="M3 6h18" /><path d="M19 6v14c0 1-1 2-2 2H7c-1 0-2-1-2-2V6" /><path d="M8 6V4c0-1 1-2 2-2h4c1 0 2 1 2 2v2" /></>,
  history: <><path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8" /><path d="M3 3v5h5" /><path d="M12 7v5l4 2" /></>,
  file: <><path d="M14.5 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7.5L14.5 2z" /><polyline points="14 2 14 8 20 8" /></>,
  twitter: <><path d="M4 4l11.733 16h4.267l-11.733 -16z" /><path d="M4 20l6.768 -6.768m2.46 -2.46l6.772 -6.772" /></>,
  activity: <path d="M22 12h-4l-3 9L9 3l-3 9H2" />,
  refresh: <><path d="M3 12a9 9 0 0 1 9-9 9.75 9.75 0 0 1 6.74 2.74L21 8" /><path d="M21 3v5h-5" /><path d="M21 12a9 9 0 0 1-9 9 9.75 9.75 0 0 1-6.74-2.74L3 16" /><path d="M8 16H3v5" /></>,
  message: <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z" />,
  chart: <><path d="M3 3v18h18" /><path d="M18 17V9" /><path d="M13 17V5" /><path d="M8 17v-3" /></>,
  lock: <><rect width="18" height="11" x="3" y="11" rx="2" /><path d="M7 11V7a5 5 0 0 1 10 0v4" /></>,
};
function Icon({ name, size = 16, className = '', fill = false, strokeWidth = 2 }) {
  return (
    <svg xmlns="http://www.w3.org/2000/svg" width={size} height={size} viewBox="0 0 24 24" fill={fill ? 'currentColor' : 'none'}
      stroke="currentColor" strokeWidth={strokeWidth} strokeLinecap="round" strokeLinejoin="round" className={className} aria-hidden="true">
      {ICONS[name]}
    </svg>
  );
}

/* ───────────────────────── hooks & primitives ───────────────────────── */
function useCountUp(target, duration = 900) {
  const [val, setVal] = useState(0);
  useEffect(() => {
    if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) { setVal(target); return; }
    let raf, start;
    const tick = t => {
      if (!start) start = t;
      const p = Math.min(1, (t - start) / duration);
      setVal(Math.round(target * (1 - Math.pow(1 - p, 3))));
      if (p < 1) raf = requestAnimationFrame(tick);
    };
    raf = requestAnimationFrame(tick);
    return () => cancelAnimationFrame(raf);
  }, [target, duration]);
  return val;
}
function useNow(ms = 1000) {
  const [n, setN] = useState(Date.now());
  useEffect(() => { const id = setInterval(() => setN(Date.now()), ms); return () => clearInterval(id); }, [ms]);
  return n;
}
function useReady() {
  const [ready, setReady] = useState(false);
  useEffect(() => { const id = requestAnimationFrame(() => requestAnimationFrame(() => setReady(true))); return () => cancelAnimationFrame(id); }, []);
  return ready;
}

const card = 'rounded-2xl border border-zinc-200 dark:border-zinc-800 bg-white/80 dark:bg-zinc-900/50 backdrop-blur-sm shadow-sm';
const pillBtn = 'inline-flex items-center gap-1.5 h-7 px-3 rounded-full text-xs font-medium border border-zinc-200 dark:border-zinc-800 bg-white dark:bg-zinc-900 text-zinc-600 dark:text-zinc-300 hover:border-indigo-500/60 hover:text-indigo-500 hover:-translate-y-px active:scale-95 transition';
const btn = 'inline-flex items-center justify-center gap-2 h-9 px-3.5 rounded-lg text-sm font-medium border border-zinc-200 dark:border-zinc-800 bg-white dark:bg-zinc-900 hover:bg-zinc-50 dark:hover:bg-zinc-800 hover:-translate-y-px active:scale-95 transition';
const btnPrimary = 'inline-flex items-center justify-center gap-2 h-9 px-4 rounded-lg text-sm font-medium text-white bg-gradient-to-br from-indigo-500 to-violet-600 shadow-lg shadow-indigo-500/25 hover:brightness-110 hover:-translate-y-px active:scale-95 transition disabled:opacity-60';

const A = ({ href, className = '', children, title }) => (
  <a href={href} title={title} target="_blank" rel="noopener noreferrer" className={`hover:underline ${className}`}>{children}</a>
);
function Reveal({ i = 0, className = '', children }) {
  return <div className={`fade-up ${className}`} style={{ animationDelay: `${i * 70}ms` }}>{children}</div>;
}
const Skel = ({ className = '' }) => <div className={`skeleton rounded-md bg-zinc-200 dark:bg-zinc-800 ${className}`} />;

/* ───────────────────────── header & search ───────────────────────── */
function Header({ inputRef, query, setQuery, onSubmit, loading, rate, hasToken, onToken, theme, onTheme, onHome }) {
  const isMac = /Mac|iPhone|iPad/.test(navigator.platform || '');
  const pct = rate && rate.limit ? rate.remaining / rate.limit : null;
  const tone = pct == null ? 'text-zinc-400' : pct > 0.5 ? 'text-emerald-500' : pct > 0.15 ? 'text-amber-500' : 'text-red-500';
  const bar = pct == null ? 'bg-zinc-400' : pct > 0.5 ? 'bg-emerald-500' : pct > 0.15 ? 'bg-amber-500' : 'bg-red-500';
  const resetTxt = rate ? `Resets at ${new Date(rate.reset).toLocaleTimeString()}` : 'Rate limit not loaded yet';
  return (
    <header className="sticky top-0 z-40 border-b border-zinc-200/70 dark:border-zinc-800/70 bg-white/70 dark:bg-zinc-950/70 backdrop-blur-xl">
      <div className="mx-auto max-w-7xl px-4 sm:px-6 h-16 flex items-center gap-3">
        <button onClick={onHome} aria-label="Go to home" className="flex items-center gap-2 font-semibold tracking-tight shrink-0 rounded-lg">
          <span className="grid place-items-center h-8 w-8 rounded-lg bg-gradient-to-br from-indigo-500 to-cyan-400 text-white shadow-lg shadow-indigo-500/30"><Icon name="branch" size={16} /></span>
          <span className="hidden md:inline">Explorer</span>
        </button>

        <form role="search" onSubmit={e => { e.preventDefault(); onSubmit(query); }} className="flex-1 max-w-xl mx-auto relative">
          <span className="absolute left-3 top-1/2 -translate-y-1/2 text-zinc-400 pointer-events-none">
            {loading ? <span className="spinner" /> : <Icon name="search" size={16} />}
          </span>
          <input ref={inputRef} value={query} onChange={e => setQuery(e.target.value)}
            onKeyDown={e => { if (e.key === 'Escape') e.currentTarget.blur(); }}
            placeholder="Search a GitHub username…" aria-label="Search GitHub username" autoComplete="off" spellCheck={false}
            className="w-full h-10 pl-9 pr-24 rounded-xl text-sm bg-zinc-100 dark:bg-zinc-900 border border-transparent focus:border-indigo-500/60 focus:bg-white dark:focus:bg-zinc-900 focus:ring-4 focus:ring-indigo-500/15 outline-none transition placeholder:text-zinc-400" />
          <div className="absolute right-2 top-1/2 -translate-y-1/2 flex items-center gap-1.5">
            {query && (
              <button type="button" aria-label="Clear search" onClick={() => { setQuery(''); inputRef.current && inputRef.current.focus(); }}
                className="h-6 w-6 grid place-items-center rounded-md text-zinc-400 hover:text-zinc-700 dark:hover:text-zinc-200 hover:bg-zinc-200/70 dark:hover:bg-zinc-800 transition">
                <Icon name="x" size={14} />
              </button>
            )}
            <kbd className="hidden sm:inline-flex items-center h-5 px-1.5 rounded border border-zinc-300 dark:border-zinc-700 text-[10px] font-mono text-zinc-500">{isMac ? '⌘K' : 'Ctrl K'}</kbd>
          </div>
        </form>

        <div className="flex items-center gap-2 shrink-0">
          <button onClick={onToken} title={resetTxt} aria-label="Open rate limit and token settings"
            className="flex items-center gap-2 h-9 px-2.5 sm:px-3 rounded-lg border border-zinc-200 dark:border-zinc-800 bg-white dark:bg-zinc-900 hover:border-indigo-500/60 active:scale-95 transition">
            <Icon name="zap" size={14} className={tone} />
            <span className="text-xs font-mono tabular-nums">{rate ? `${rate.remaining}/${rate.limit}` : '—'}</span>
            <span className="hidden md:block relative h-1 w-10 rounded-full bg-zinc-200 dark:bg-zinc-800 overflow-hidden">
              <span className={`absolute inset-y-0 left-0 rounded-full ${bar} transition-all duration-700`} style={{ width: `${(pct || 0) * 100}%` }} />
            </span>
          </button>
          <button onClick={onToken} aria-label={hasToken ? 'Manage GitHub token (token active)' : 'Add GitHub token'}
            className="relative h-9 w-9 grid place-items-center rounded-lg border border-zinc-200 dark:border-zinc-800 bg-white dark:bg-zinc-900 hover:border-indigo-500/60 active:scale-95 transition">
            <Icon name="key" size={15} />
            {hasToken && <span className="absolute -top-1 -right-1 h-2.5 w-2.5 rounded-full bg-emerald-500 ring-2 ring-white dark:ring-zinc-950" />}
          </button>
          <button onClick={onTheme} aria-label={`Switch to ${theme === 'dark' ? 'light' : 'dark'} theme`}
            className="h-9 w-9 grid place-items-center rounded-lg border border-zinc-200 dark:border-zinc-800 bg-white dark:bg-zinc-900 hover:border-indigo-500/60 active:scale-95 transition">
            <span key={theme} className="check-pop"><Icon name={theme === 'dark' ? 'sun' : 'moon'} size={15} /></span>
          </button>
        </div>
      </div>
    </header>
  );
}

function Discovery({ recent, active, onPick, onRemove, onClear }) {
  return (
    <div className="mb-6 space-y-3">
      <div className="flex flex-wrap items-center gap-2">
        <span className="text-xs font-medium text-zinc-500 mr-1">Quick try</span>
        {QUICK_USERS.map(u => (
          <button key={u} onClick={() => onPick(u)}
            className={`${pillBtn} ${active && active.toLowerCase() === u ? '!border-indigo-500 !text-indigo-500 bg-indigo-500/10' : ''}`}>@{u}</button>
        ))}
      </div>
      {recent.length > 0 && (
        <div className="flex flex-wrap items-center gap-2">
          <span className="text-xs font-medium text-zinc-500 mr-1 inline-flex items-center gap-1"><Icon name="history" size={12} /> Recent</span>
          {recent.map(u => (
            <span key={u} className="inline-flex items-center rounded-full border border-zinc-200 dark:border-zinc-800 bg-white dark:bg-zinc-900 overflow-hidden">
              <button onClick={() => onPick(u)} className="pl-3 pr-1.5 h-7 text-xs font-medium hover:text-indigo-500 transition">{u}</button>
              <button onClick={() => onRemove(u)} aria-label={`Remove ${u} from recent searches`} className="pr-2 pl-1 h-7 text-zinc-400 hover:text-red-500 transition"><Icon name="x" size={12} /></button>
            </span>
          ))}
          <button onClick={onClear} className="ml-1 inline-flex items-center gap-1 text-xs text-zinc-500 hover:text-red-500 transition"><Icon name="trash" size={12} /> Clear all</button>
        </div>
      )}
    </div>
  );
}

/* ───────────────────────── states ───────────────────────── */
function Hero({ onPick }) {
  const features = [
    { icon: 'chart', t: 'Analytics', d: 'Stars, forks, language mix and a contribution heatmap, aggregated live.' },
    { icon: 'book', t: 'Repositories', d: 'Search, filter and sort every repo, then read its README in a drawer.' },
    { icon: 'activity', t: 'Activity', d: 'A timeline of pushes, pull requests, issues, stars and releases.' },
  ];
  return (
    <div className="py-10 sm:py-16 text-center">
      <Reveal>
        <span className="inline-flex items-center gap-2 h-7 px-3 rounded-full text-xs font-medium border border-indigo-500/30 bg-indigo-500/10 text-indigo-500">
          <Icon name="zap" size={12} /> Live data from the GitHub REST API
        </span>
      </Reveal>
      <Reveal i={1}>
        <h1 className="mt-5 text-4xl sm:text-6xl font-bold tracking-tight">
          Explore any <span className="bg-gradient-to-r from-indigo-500 via-violet-500 to-cyan-400 bg-clip-text text-transparent">GitHub profile</span>
        </h1>
      </Reveal>
      <Reveal i={2}>
        <p className="mt-4 text-zinc-500 dark:text-zinc-400 max-w-xl mx-auto">
          Type a username above, press <kbd className="px-1.5 py-0.5 rounded border border-zinc-300 dark:border-zinc-700 text-xs font-mono">/</kbd> to focus search, or pick a developer to start.
        </p>
      </Reveal>
      <div className="mt-10 grid sm:grid-cols-3 gap-4 text-left">
        {features.map((f, i) => (
          <Reveal key={f.t} i={i + 3}>
            <div className={`${card} lift p-5 h-full`}>
              <span className="grid place-items-center h-9 w-9 rounded-xl bg-indigo-500/10 text-indigo-500"><Icon name={f.icon} size={18} /></span>
              <h3 className="mt-3 font-semibold">{f.t}</h3>
              <p className="mt-1 text-sm text-zinc-500 dark:text-zinc-400">{f.d}</p>
            </div>
          </Reveal>
        ))}
      </div>
    </div>
  );
}

function LoadingLayout() {
  return (
    <div className="grid lg:grid-cols-[320px_minmax(0,1fr)] gap-6 items-start" aria-busy="true" aria-label="Loading profile">
      <div className={`${card} p-6 space-y-4`}>
        <Skel className="h-28 w-28 rounded-full mx-auto" />
        <Skel className="h-6 w-2/3 mx-auto" /><Skel className="h-4 w-1/3 mx-auto" />
        <Skel className="h-4 w-full" /><Skel className="h-4 w-5/6" />
        <div className="space-y-2 pt-2"><Skel className="h-4 w-1/2" /><Skel className="h-4 w-2/3" /><Skel className="h-4 w-1/2" /></div>
        <div className="grid grid-cols-2 gap-2 pt-2"><Skel className="h-9" /><Skel className="h-9" /></div>
      </div>
      <div className="space-y-5 min-w-0">
        <Skel className="h-11 w-full rounded-xl" />
        <div className="grid grid-cols-2 xl:grid-cols-4 gap-3">{[0, 1, 2, 3].map(i => <div key={i} className={`${card} p-4 space-y-3`}><Skel className="h-3 w-1/2" /><Skel className="h-8 w-2/3" /><Skel className="h-3 w-3/4" /></div>)}</div>
        <div className="grid xl:grid-cols-2 gap-5">
          <div className={`${card} p-5 flex gap-5 items-center`}><Skel className="h-40 w-40 rounded-full shrink-0" /><div className="flex-1 space-y-3">{[0, 1, 2, 3].map(i => <Skel key={i} className="h-4 w-full" />)}</div></div>
          <div className={`${card} p-5 space-y-4`}>{[0, 1, 2, 3, 4].map(i => <Skel key={i} className="h-5 w-full" />)}</div>
        </div>
        <div className={`${card} p-5`}><Skel className="h-32 w-full" /></div>
      </div>
    </div>
  );
}

function RateBanner({ reset, onToken, onRetry, hasToken }) {
  const now = useNow();
  const left = Math.max(0, reset - now);
  return (
    <div role="alert" className="fade-up rounded-2xl border border-amber-500/30 bg-amber-500/10 p-4 flex flex-col sm:flex-row sm:items-center gap-3">
      <div className="h-10 w-10 grid place-items-center rounded-xl bg-amber-500/15 text-amber-500 shrink-0"><Icon name="alert" size={20} /></div>
      <div className="flex-1 text-sm">
        <p className="font-semibold">GitHub API rate limit reached</p>
        <p className="text-zinc-600 dark:text-zinc-400">
          {left > 0
            ? <>Resets in <span className="font-mono tabular-nums font-semibold">{fmtCountdown(left)}</span> (at {new Date(reset).toLocaleTimeString()}). </>
            : <>The limit has reset. You can retry now. </>}
          {!hasToken && 'Add a personal access token to raise the limit from 60 to 5,000 requests per hour.'}
        </p>
      </div>
      <div className="flex gap-2">
        {left === 0 && onRetry && <button onClick={onRetry} className={btn}><Icon name="refresh" size={14} /> Retry</button>}
        <button onClick={onToken} className={btnPrimary}><Icon name="key" size={14} /> {hasToken ? 'Manage token' : 'Add token'}</button>
      </div>
    </div>
  );
}

function NotFound({ name, onPick }) {
  return (
    <div className="py-14 text-center">
      <Reveal>
        <svg viewBox="0 0 200 160" className="w-48 h-40 mx-auto float" aria-hidden="true">
          <circle cx="100" cy="80" r="62" className="fill-indigo-500/10" />
          <circle cx="92" cy="74" r="32" fill="none" stroke="currentColor" strokeWidth="6" className="text-indigo-500" />
          <path d="M116 98l28 28" stroke="currentColor" strokeWidth="8" strokeLinecap="round" className="text-indigo-500" />
          <text x="92" y="86" textAnchor="middle" fontSize="34" fontWeight="700" className="fill-zinc-400">?</text>
          <circle cx="40" cy="40" r="4" className="fill-cyan-400" /><circle cx="162" cy="52" r="3" className="fill-violet-400" /><circle cx="150" cy="134" r="3" className="fill-indigo-400" />
        </svg>
      </Reveal>
      <Reveal i={1}><h2 className="mt-4 text-2xl font-semibold">No GitHub user named “{name}”</h2></Reveal>
      <Reveal i={2}><p className="mt-2 text-zinc-500 dark:text-zinc-400">Check the spelling, or paste a full profile URL. Here are some profiles to try instead:</p></Reveal>
      <Reveal i={3}>
        <div className="mt-5 flex flex-wrap justify-center gap-2">
          {QUICK_USERS.map(u => <button key={u} onClick={() => onPick(u)} className={pillBtn}>@{u}</button>)}
        </div>
      </Reveal>
    </div>
  );
}

function ErrorState({ error, name, onRetry, onToken, hasToken, onPick }) {
  if (error.kind === 'notfound') return <NotFound name={name} onPick={onPick} />;
  if (error.kind === 'ratelimit') {
    return (
      <div className="space-y-6 py-4">
        <RateBanner reset={error.reset} onToken={onToken} onRetry={onRetry} hasToken={hasToken} />
        <div className="text-center text-sm text-zinc-500 py-10">Nothing was loaded for <span className="font-mono">@{name}</span> because the request quota is used up.</div>
      </div>
    );
  }
  const bad = error.kind === 'badtoken';
  return (
    <div className="py-16 text-center max-w-md mx-auto">
      <Reveal><span className="mx-auto grid place-items-center h-14 w-14 rounded-2xl bg-red-500/10 text-red-500"><Icon name={bad ? 'key' : 'alert'} size={26} /></span></Reveal>
      <Reveal i={1}><h2 className="mt-4 text-xl font-semibold">{bad ? 'Your GitHub token was rejected' : 'Something went wrong'}</h2></Reveal>
      <Reveal i={2}><p className="mt-2 text-sm text-zinc-500 dark:text-zinc-400">{error.message}{bad ? ' It may have expired or been revoked. Remove or replace it.' : ''}</p></Reveal>
      <Reveal i={3}>
        <div className="mt-5 flex justify-center gap-2">
          {bad && <button onClick={onToken} className={btnPrimary}><Icon name="key" size={14} /> Manage token</button>}
          {!bad && <button onClick={onRetry} className={btnPrimary}><Icon name="refresh" size={14} /> Try again</button>}
        </div>
      </Reveal>
    </div>
  );
}

/* ───────────────────────── profile card ───────────────────────── */
function ProfileCard({ user, onCopy, copied, onRefresh }) {
  const created = new Date(user.created_at);
  const years = Math.floor((Date.now() - created) / (365.25 * 864e5));
  const blog = user.blog ? (/^https?:\/\//i.test(user.blog) ? user.blog : `https://${user.blog}`) : '';
  const row = 'flex items-center gap-2.5 text-sm text-zinc-600 dark:text-zinc-400';
  return (
    <div className={`${card} p-6`}>
      <Reveal>
        <div className="relative w-28 h-28 mx-auto">
          <div className="absolute inset-0 rounded-full bg-gradient-to-tr from-indigo-500 to-cyan-400 blur-2xl opacity-40" />
          <img src={user.avatar_url + (user.avatar_url.includes('?') ? '&' : '?') + 's=300'} alt={`${user.login}'s avatar`}
            className="relative h-28 w-28 rounded-full object-cover ring-4 ring-white dark:ring-zinc-900" />
        </div>
      </Reveal>
      <Reveal i={1} className="text-center mt-4">
        <h2 className="text-xl font-semibold tracking-tight">{user.name || user.login}</h2>
        <div className="mt-1 flex items-center justify-center gap-2">
          <span className="inline-flex items-center h-6 px-2.5 rounded-full text-xs font-mono bg-zinc-100 dark:bg-zinc-800 text-zinc-600 dark:text-zinc-300">@{user.login}</span>
          {user.type !== 'User' && <span className="inline-flex items-center h-6 px-2.5 rounded-full text-xs bg-violet-500/10 text-violet-500">{user.type}</span>}
          {user.hireable && <span className="inline-flex items-center h-6 px-2.5 rounded-full text-xs bg-emerald-500/10 text-emerald-500">Hireable</span>}
        </div>
      </Reveal>
      {user.bio && <Reveal i={2}><p className="mt-4 text-sm text-center text-zinc-600 dark:text-zinc-300 leading-relaxed">{user.bio}</p></Reveal>}

      <Reveal i={3}>
        <div className="mt-5 grid grid-cols-2 gap-2">
          <A href={`https://github.com/${user.login}?tab=followers`} className="!no-underline rounded-xl bg-zinc-100 dark:bg-zinc-800/60 p-3 text-center hover:bg-zinc-200/70 dark:hover:bg-zinc-800 transition">
            <div className="text-lg font-semibold tabular-nums">{fmtCount(user.followers)}</div><div className="text-[11px] text-zinc-500 flex items-center justify-center gap-1"><Icon name="users" size={11} /> followers</div>
          </A>
          <A href={`https://github.com/${user.login}?tab=following`} className="!no-underline rounded-xl bg-zinc-100 dark:bg-zinc-800/60 p-3 text-center hover:bg-zinc-200/70 dark:hover:bg-zinc-800 transition">
            <div className="text-lg font-semibold tabular-nums">{fmtCount(user.following)}</div><div className="text-[11px] text-zinc-500">following</div>
          </A>
          <div className="rounded-xl bg-zinc-100 dark:bg-zinc-800/60 p-3 text-center">
            <div className="text-lg font-semibold tabular-nums">{fmtCount(user.public_repos)}</div><div className="text-[11px] text-zinc-500 flex items-center justify-center gap-1"><Icon name="book" size={11} /> repos</div>
          </div>
          <div className="rounded-xl bg-zinc-100 dark:bg-zinc-800/60 p-3 text-center">
            <div className="text-lg font-semibold tabular-nums">{fmtCount(user.public_gists)}</div><div className="text-[11px] text-zinc-500 flex items-center justify-center gap-1"><Icon name="file" size={11} /> gists</div>
          </div>
        </div>
      </Reveal>

      <Reveal i={4}>
        <ul className="mt-5 space-y-2.5">
          {user.company && <li className={row}><Icon name="building" size={15} className="shrink-0" /><span className="truncate">{user.company}</span></li>}
          {user.location && <li className={row}><Icon name="pin" size={15} className="shrink-0" /><span className="truncate">{user.location}</span></li>}
          {blog && <li className={row}><Icon name="link" size={15} className="shrink-0" /><A href={blog} className="truncate text-indigo-500">{blog.replace(/^https?:\/\//i, '')}</A></li>}
          {user.twitter_username && <li className={row}><Icon name="twitter" size={15} className="shrink-0" /><A href={`https://x.com/${user.twitter_username}`} className="text-indigo-500">@{user.twitter_username}</A></li>}
          <li className={row}>
            <Icon name="calendar" size={15} className="shrink-0" />
            <span>Joined {created.toLocaleDateString('en', { month: 'short', year: 'numeric' })}</span>
            <span className="ml-auto inline-flex items-center h-5 px-2 rounded-full text-[10px] font-medium bg-indigo-500/10 text-indigo-500">{years >= 1 ? `${years}y on GitHub` : 'New'}</span>
          </li>
        </ul>
      </Reveal>

      <Reveal i={5}>
        <div className="mt-6 grid grid-cols-2 gap-2">
          <button onClick={onCopy} className={btn} aria-label="Copy profile share link">
            {copied ? <span key="c" className="check-pop text-emerald-500"><Icon name="check" size={15} /></span> : <Icon name="copy" size={15} />}
            {copied ? 'Copied' : 'Copy link'}
          </button>
          <a href={user.html_url} target="_blank" rel="noopener noreferrer" className={btnPrimary}><Icon name="ext" size={14} /> GitHub</a>
        </div>
        <button onClick={onRefresh} className="mt-2 w-full text-xs text-zinc-500 hover:text-indigo-500 inline-flex items-center justify-center gap-1.5 h-8 transition">
          <Icon name="refresh" size={12} /> Refresh data
        </button>
      </Reveal>
    </div>
  );
}

/* ───────────────────────── tabs ───────────────────────── */
function Tabs({ tabs, value, onChange }) {
  const idx = tabs.findIndex(t => t.id === value);
  const onKey = e => {
    if (e.key !== 'ArrowRight' && e.key !== 'ArrowLeft') return;
    e.preventDefault();
    const n = (idx + (e.key === 'ArrowRight' ? 1 : -1) + tabs.length) % tabs.length;
    onChange(tabs[n].id);
    const el = document.getElementById(`tab-${tabs[n].id}`); if (el) el.focus();
  };
  return (
    <div role="tablist" aria-label="Profile sections" onKeyDown={onKey}
      className="relative grid p-1 rounded-xl bg-zinc-100 dark:bg-zinc-900 border border-zinc-200 dark:border-zinc-800"
      style={{ gridTemplateColumns: `repeat(${tabs.length}, 1fr)` }}>
      <span aria-hidden="true" className="absolute top-1 bottom-1 left-1 rounded-lg bg-white dark:bg-zinc-800 shadow-sm ring-1 ring-black/5 dark:ring-white/5"
        style={{ width: `calc((100% - 0.5rem) / ${tabs.length})`, transform: `translateX(${idx * 100}%)`, transition: 'transform .45s cubic-bezier(.34,1.4,.64,1)' }} />
      {tabs.map(t => (
        <button key={t.id} id={`tab-${t.id}`} role="tab" aria-selected={value === t.id} aria-controls={`panel-${t.id}`} tabIndex={value === t.id ? 0 : -1}
          onClick={() => onChange(t.id)}
          className={`relative z-10 flex items-center justify-center gap-2 h-9 text-sm font-medium rounded-lg transition-colors ${value === t.id ? 'text-zinc-900 dark:text-zinc-100' : 'text-zinc-500 hover:text-zinc-800 dark:hover:text-zinc-200'}`}>
          <Icon name={t.icon} size={15} /> <span>{t.label}</span>
          {t.count != null && <span className="hidden sm:inline text-[11px] px-1.5 rounded-full bg-zinc-200/70 dark:bg-zinc-700/60 tabular-nums">{t.count}</span>}
        </button>
      ))}
    </div>
  );
}

/* ───────────────────────── overview widgets ───────────────────────── */
const KPI_COLORS = {
  yellow: { glow: 'bg-yellow-400', chip: 'bg-yellow-500/10 text-yellow-500' },
  sky: { glow: 'bg-sky-400', chip: 'bg-sky-500/10 text-sky-500' },
  violet: { glow: 'bg-violet-400', chip: 'bg-violet-500/10 text-violet-500' },
  rose: { glow: 'bg-rose-400', chip: 'bg-rose-500/10 text-rose-500' },
};
function Kpi({ icon, label, value, hint, color, i }) {
  const n = useCountUp(value);
  const c = KPI_COLORS[color];
  return (
    <Reveal i={i}>
      <div className={`${card} lift p-4 relative overflow-hidden h-full`}>
        <div className={`absolute -top-8 -right-8 h-24 w-24 rounded-full blur-2xl opacity-25 ${c.glow}`} />
        <div className="relative flex items-center justify-between text-xs font-medium text-zinc-500 dark:text-zinc-400">
          <span>{label}</span>
          <span className={`grid place-items-center h-7 w-7 rounded-lg ${c.chip}`}><Icon name={icon} size={14} /></span>
        </div>
        <div className="relative mt-2 text-3xl font-semibold tabular-nums tracking-tight">{n.toLocaleString()}</div>
        <div className="relative mt-1 text-[11px] text-zinc-500">{hint}</div>
      </div>
    </Reveal>
  );
}

function DonutChart({ segments, total }) {
  const ready = useReady();
  const [active, setActive] = useState(null);
  const R = 70, C = 2 * Math.PI * R;
  let offset = 0;
  if (!total) return <p className="text-sm text-zinc-500 py-10 text-center">No language data. This user's repositories have no detected primary language.</p>;
  const act = active != null ? segments[active] : null;
  return (
    <div className="flex flex-col sm:flex-row items-center gap-6">
      <div className="relative shrink-0">
        <svg viewBox="0 0 200 200" className="w-44 h-44 -rotate-90" role="img" aria-label="Language distribution donut chart">
          <circle cx="100" cy="100" r={R} fill="none" stroke="currentColor" strokeWidth="22" className="text-zinc-200 dark:text-zinc-800" />
          {segments.map((s, i) => {
            const len = (s.value / total) * C;
            const dash = Math.max(len - (segments.length > 1 ? 2 : 0), 0);
            const el = (
              <circle key={s.name} cx="100" cy="100" r={R} fill="none" stroke={s.color}
                strokeWidth={active === i ? 27 : 22} strokeDasharray={ready ? `${dash} ${C - dash}` : `0 ${C}`} strokeDashoffset={-offset}
                style={{ transition: `stroke-dasharray 900ms cubic-bezier(.22,1,.36,1) ${i * 90}ms, stroke-width .2s, opacity .2s`, opacity: active == null || active === i ? 1 : 0.3, cursor: 'pointer' }}
                onMouseEnter={() => setActive(i)} onMouseLeave={() => setActive(null)} />
            );
            offset += len; return el;
          })}
        </svg>
        <div className="absolute inset-0 grid place-items-center text-center pointer-events-none">
          <div>
            <div className="text-2xl font-semibold tabular-nums">{act ? `${Math.round((act.value / total) * 100)}%` : total}</div>
            <div className="text-[11px] text-zinc-500 max-w-[80px] truncate">{act ? act.name : 'repos by language'}</div>
          </div>
        </div>
      </div>
      <ul className="flex-1 w-full space-y-1.5">
        {segments.map((s, i) => (
          <li key={s.name}>
            <button onMouseEnter={() => setActive(i)} onMouseLeave={() => setActive(null)} onFocus={() => setActive(i)} onBlur={() => setActive(null)}
              className={`w-full flex items-center gap-2.5 px-2 py-1.5 rounded-lg text-sm transition ${active === i ? 'bg-zinc-100 dark:bg-zinc-800' : ''}`}>
              <span className="h-2.5 w-2.5 rounded-full shrink-0" style={{ background: s.color }} />
              <span className="truncate font-medium">{s.name}</span>
              <span className="ml-auto text-xs text-zinc-500 tabular-nums">{s.value} · {Math.round((s.value / total) * 100)}%</span>
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

function TopRepos({ repos, onOpen }) {
  const ready = useReady();
  const max = Math.max(1, ...repos.map(r => r.stargazers_count));
  if (!repos.length || repos[0].stargazers_count === 0) return <p className="text-sm text-zinc-500 py-10 text-center">No starred repositories yet.</p>;
  return (
    <ul className="space-y-3.5">
      {repos.map((r, i) => (
        <li key={r.id}>
          <button onClick={() => onOpen(r)} className="group w-full text-left" aria-label={`Open ${r.name} README`}>
            <div className="flex items-center justify-between text-sm mb-1.5">
              <span className="truncate font-medium group-hover:text-indigo-500 transition-colors">{r.name}</span>
              <span className="ml-3 shrink-0 tabular-nums text-xs text-zinc-500 inline-flex items-center gap-1"><Icon name="star" size={12} className="text-yellow-500" fill />{r.stargazers_count.toLocaleString()}</span>
            </div>
            <div className="h-2 rounded-full bg-zinc-100 dark:bg-zinc-800 overflow-hidden">
              <div className="h-full rounded-full group-hover:brightness-110"
                style={{ width: ready ? `${Math.max(2, (r.stargazers_count / max) * 100)}%` : '0%', background: langColor(r.language), transition: `width 900ms cubic-bezier(.22,1,.36,1) ${i * 80}ms` }} />
            </div>
          </button>
        </li>
      ))}
    </ul>
  );
}

function Heatmap({ heat, hasEvents, eventsError }) {
  const [hover, setHover] = useState(null);
  const n = heat.weeks.length;
  const level = c => (c === 0 ? 0 : Math.min(4, Math.ceil((c / heat.max) * 4)));
  const labels = heat.weeks.map((w, i) => {
    const m = w[0].date.getMonth();
    const prev = i > 0 ? heat.weeks[i - 1][0].date.getMonth() : -1;
    if (m === prev) return '';
    if (i === 0 && heat.weeks[1] && heat.weeks[1][0].date.getMonth() !== m) return '';
    return w[0].date.toLocaleString('en', { month: 'short' });
  });
  return (
    <div>
      {eventsError ? (
        <p className="text-sm text-zinc-500 py-8 text-center">Activity data could not be loaded ({eventsError.message}).</p>
      ) : !hasEvents ? (
        <p className="text-sm text-zinc-500 py-8 text-center">No public activity in GitHub's recent events window.</p>
      ) : (
        <>
          <div className="overflow-x-auto pb-1">
            <div className="inline-block min-w-max">
              <div className="flex">
                <div className="w-8 shrink-0" />
                <div className="grid mb-1 text-[10px] text-zinc-500" style={{ gridTemplateColumns: `repeat(${n}, 11px)`, columnGap: 3 }}>
                  {labels.map((l, i) => <span key={i} className="whitespace-nowrap leading-3">{l}</span>)}
                </div>
              </div>
              <div className="flex">
                <div className="w-8 shrink-0 grid text-[10px] text-zinc-500" style={{ gridTemplateRows: 'repeat(7, 11px)', rowGap: 3 }}>
                  {['', 'Mon', '', 'Wed', '', 'Fri', ''].map((d, i) => <span key={i} className="leading-[11px]">{d}</span>)}
                </div>
                <div className="grid grid-flow-col" style={{ gridTemplateRows: 'repeat(7, 11px)', gap: 3 }}>
                  {heat.weeks.flatMap((w, wi) => w.map(d => d.future
                    ? <span key={d.key} style={{ width: 11, height: 11 }} />
                    : <span key={d.key} className={`hm hm-${level(d.count)} pop`} style={{ animationDelay: `${wi * 10}ms` }}
                        onMouseEnter={() => setHover(d)} onMouseLeave={() => setHover(null)}
                        title={`${d.count} event${d.count === 1 ? '' : 's'} on ${d.date.toLocaleDateString('en', { month: 'short', day: 'numeric', year: 'numeric' })}`} />
                  ))}
                </div>
              </div>
            </div>
          </div>
          <div className="mt-3 flex flex-wrap items-center justify-between gap-2 text-xs text-zinc-500">
            <span className="tabular-nums min-h-[1rem]">
              {hover ? `${hover.count} event${hover.count === 1 ? '' : 's'} on ${hover.date.toLocaleDateString('en', { weekday: 'short', month: 'short', day: 'numeric', year: 'numeric' })}` : `${heat.total} events · busiest day ${heat.max}`}
            </span>
            <span className="inline-flex items-center gap-1.5">Less {[0, 1, 2, 3, 4].map(l => <span key={l} className={`hm hm-${l}`} />)} More</span>
          </div>
          <p className="mt-2 text-[11px] text-zinc-500">Built from GitHub's public events feed, which covers roughly the last 90 days, so older weeks appear empty. This is not the official contribution graph.</p>
        </>
      )}
    </div>
  );
}

function Spotlight({ title, icon, repo, stat, statLabel, onOpen, color }) {
  if (!repo) return null;
  return (
    <div className={`${card} lift p-5 h-full`}>
      <div className="flex items-center gap-2 text-xs font-medium text-zinc-500"><span className={`grid place-items-center h-6 w-6 rounded-md ${color}`}><Icon name={icon} size={13} /></span>{title}</div>
      <button onClick={() => onOpen(repo)} className="mt-3 text-left group w-full">
        <div className="font-semibold text-lg truncate group-hover:text-indigo-500 transition-colors">{repo.name}</div>
        <p className="mt-1 text-sm text-zinc-500 dark:text-zinc-400 line-clamp-2 min-h-[2.5rem]">{repo.description || 'No description provided.'}</p>
      </button>
      <div className="mt-3 flex items-center justify-between text-sm">
        <span className="inline-flex items-center gap-1.5"><span className="h-2.5 w-2.5 rounded-full" style={{ background: langColor(repo.language) }} />{repo.language || 'n/a'}</span>
        <span className="tabular-nums font-semibold">{stat.toLocaleString()} <span className="text-xs font-normal text-zinc-500">{statLabel}</span></span>
      </div>
    </div>
  );
}

function SectionTitle({ icon, children, right }) {
  return (
    <div className="flex items-center justify-between mb-4">
      <h3 className="flex items-center gap-2 text-sm font-semibold"><Icon name={icon} size={15} className="text-indigo-500" />{children}</h3>
      {right}
    </div>
  );
}

function Overview({ stats, heat, data, onOpenRepo }) {
  const t = stats.totals;
  return (
    <div id="panel-overview" role="tabpanel" aria-labelledby="tab-overview" className="space-y-5">
      <div className="grid grid-cols-2 xl:grid-cols-4 gap-3">
        <Kpi i={0} icon="star" label="Total stars" value={t.stars} hint={`across ${data.repos.length} repos`} color="yellow" />
        <Kpi i={1} icon="fork" label="Total forks" value={t.forks} hint="times repos were forked" color="sky" />
        <Kpi i={2} icon="eye" label="Total watchers" value={t.watchers} hint="GitHub's list API mirrors stars here" color="violet" />
        <Kpi i={3} icon="issue" label="Open issues" value={t.issues} hint="includes open pull requests" color="rose" />
      </div>
      <div className="grid xl:grid-cols-2 gap-5">
        <Reveal i={4}><div className={`${card} p-5 h-full`}><SectionTitle icon="chart">Language distribution</SectionTitle><DonutChart segments={stats.segments} total={stats.langTotal} /></div></Reveal>
        <Reveal i={5}><div className={`${card} p-5 h-full`}><SectionTitle icon="star">Top repositories by stars</SectionTitle><TopRepos repos={stats.topRepos} onOpen={onOpenRepo} /></div></Reveal>
      </div>
      <Reveal i={6}>
        <div className={`${card} p-5`}>
          <SectionTitle icon="activity">Activity heatmap</SectionTitle>
          <Heatmap heat={heat} hasEvents={data.events.length > 0} eventsError={data.eventsError && data.eventsError.kind !== 'ratelimit' ? data.eventsError : null} />
        </div>
      </Reveal>
      <div className="grid md:grid-cols-2 gap-5">
        <Reveal i={7}><Spotlight title="Most starred repo" icon="star" color="bg-yellow-500/10 text-yellow-500" repo={stats.mostStarred} stat={stats.mostStarred ? stats.mostStarred.stargazers_count : 0} statLabel="stars" onOpen={onOpenRepo} /></Reveal>
        <Reveal i={8}><Spotlight title="Most forked repo" icon="fork" color="bg-sky-500/10 text-sky-500" repo={stats.mostForked} stat={stats.mostForked ? stats.mostForked.forks_count : 0} statLabel="forks" onOpen={onOpenRepo} /></Reveal>
      </div>
    </div>
  );
}

/* ───────────────────────── repository browser ───────────────────────── */
const fieldCls = 'h-10 rounded-xl text-sm bg-white dark:bg-zinc-900 border border-zinc-200 dark:border-zinc-800 outline-none focus:border-indigo-500/60 focus:ring-4 focus:ring-indigo-500/15 transition';
function RepoCard({ repo, i, onOpen }) {
  const topics = repo.topics || [];
  const open = () => onOpen(repo);
  return (
    <div role="button" tabIndex={0} onClick={open} onKeyDown={e => { if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); open(); } }}
      aria-label={`Open ${repo.name} README`}
      className={`${card} lift fade-up p-4 cursor-pointer flex flex-col h-full`} style={{ animationDelay: `${Math.min(i, 12) * 40}ms` }}>
      <div className="flex items-start justify-between gap-2">
        <h4 className="font-semibold text-indigo-500 truncate">{repo.name}</h4>
        <div className="flex gap-1 shrink-0">
          <span className="text-[10px] px-2 h-5 inline-flex items-center rounded-full border border-zinc-300 dark:border-zinc-700 text-zinc-500">{repo.private ? 'Private' : 'Public'}</span>
          {repo.fork && <span className="text-[10px] px-2 h-5 inline-flex items-center rounded-full bg-sky-500/10 text-sky-500">Fork</span>}
          {repo.archived && <span className="text-[10px] px-2 h-5 inline-flex items-center rounded-full bg-amber-500/10 text-amber-500">Archived</span>}
        </div>
      </div>
      <p className="mt-2 text-sm text-zinc-500 dark:text-zinc-400 line-clamp-2 min-h-[2.5rem]">{repo.description || 'No description provided.'}</p>
      {topics.length > 0 && (
        <div className="mt-3 flex flex-wrap gap-1.5">
          {topics.slice(0, 4).map(t => <span key={t} className="text-[11px] px-2 h-5 inline-flex items-center rounded-full bg-indigo-500/10 text-indigo-500">{t}</span>)}
          {topics.length > 4 && <span className="text-[11px] px-2 h-5 inline-flex items-center rounded-full bg-zinc-100 dark:bg-zinc-800 text-zinc-500">+{topics.length - 4}</span>}
        </div>
      )}
      <div className="mt-auto pt-4 flex flex-wrap items-center gap-x-4 gap-y-1 text-xs text-zinc-500">
        {repo.language && <span className="inline-flex items-center gap-1.5"><span className="h-2.5 w-2.5 rounded-full" style={{ background: langColor(repo.language) }} />{repo.language}</span>}
        <span className="inline-flex items-center gap-1 tabular-nums"><Icon name="star" size={12} />{fmtCount(repo.stargazers_count)}</span>
        <span className="inline-flex items-center gap-1 tabular-nums"><Icon name="fork" size={12} />{fmtCount(repo.forks_count)}</span>
        <span className="inline-flex items-center gap-1 tabular-nums"><Icon name="issue" size={12} />{fmtCount(repo.open_issues_count)}</span>
        <span className="ml-auto inline-flex items-center gap-1"><Icon name="clock" size={12} />{timeAgo(repo.pushed_at || repo.updated_at)}</span>
      </div>
    </div>
  );
}

function Repos({ repos, languages, onOpen }) {
  const [q, setQ] = useState('');
  const [lang, setLang] = useState('all');
  const [sort, setSort] = useState('stars');
  const [hideForks, setHideForks] = useState(false);
  const [limit, setLimit] = useState(24);
  const filtered = useMemo(() => {
    const needle = q.trim().toLowerCase();
    const list = repos.filter(r =>
      (!hideForks || !r.fork) && (lang === 'all' || r.language === lang) &&
      (!needle || r.name.toLowerCase().includes(needle) || (r.description || '').toLowerCase().includes(needle) || (r.topics || []).some(t => t.toLowerCase().includes(needle))));
    const cmp = {
      stars: (a, b) => b.stargazers_count - a.stargazers_count,
      updated: (a, b) => new Date(b.pushed_at || b.updated_at) - new Date(a.pushed_at || a.updated_at),
      name: (a, b) => a.name.localeCompare(b.name),
    }[sort];
    return [...list].sort(cmp);
  }, [repos, q, lang, sort, hideForks]);
  useEffect(() => setLimit(24), [q, lang, sort, hideForks]);
  return (
    <div id="panel-repos" role="tabpanel" aria-labelledby="tab-repos" className="space-y-4">
      <div className="flex flex-col lg:flex-row gap-3">
        <div className="relative flex-1">
          <Icon name="search" size={15} className="absolute left-3 top-1/2 -translate-y-1/2 text-zinc-400" />
          <input value={q} onChange={e => setQ(e.target.value)} placeholder="Filter by name, description or topic…" aria-label="Filter repositories" className={`${fieldCls} w-full pl-9 pr-9`} />
          {q && <button onClick={() => setQ('')} aria-label="Clear filter" className="absolute right-2 top-1/2 -translate-y-1/2 h-6 w-6 grid place-items-center rounded-md text-zinc-400 hover:text-zinc-700 dark:hover:text-zinc-200"><Icon name="x" size={14} /></button>}
        </div>
        <select value={lang} onChange={e => setLang(e.target.value)} aria-label="Filter by language" className={`${fieldCls} px-3`}>
          <option value="all">All languages</option>
          {languages.map(l => <option key={l} value={l}>{l}</option>)}
        </select>
        <select value={sort} onChange={e => setSort(e.target.value)} aria-label="Sort repositories" className={`${fieldCls} px-3`}>
          <option value="stars">Most stars</option><option value="updated">Recently updated</option><option value="name">Name A–Z</option>
        </select>
        <label className={`${fieldCls} px-3 inline-flex items-center gap-2 cursor-pointer select-none`}>
          <input type="checkbox" checked={hideForks} onChange={e => setHideForks(e.target.checked)} className="accent-indigo-500" /> <span className="text-sm">Hide forks</span>
        </label>
      </div>
      <div className="text-xs text-zinc-500" aria-live="polite">{filtered.length} of {repos.length} repositories</div>
      {filtered.length === 0 ? (
        <div className={`${card} py-14 text-center`}>
          <Icon name="search" size={28} className="mx-auto text-zinc-400" />
          <p className="mt-3 font-medium">No repositories match your filters</p>
          <button onClick={() => { setQ(''); setLang('all'); setHideForks(false); }} className={`${btn} mt-4`}>Reset filters</button>
        </div>
      ) : (
        <>
          <div className="grid md:grid-cols-2 gap-3">
            {filtered.slice(0, limit).map((r, i) => <RepoCard key={r.id} repo={r} i={i} onOpen={onOpen} />)}
          </div>
          {filtered.length > limit && <div className="text-center pt-2"><button onClick={() => setLimit(l => l + 24)} className={btn}>Show more ({filtered.length - limit} left)</button></div>}
        </>
      )}
    </div>
  );
}

/* ───────────────────────── activity timeline ───────────────────────── */
const EVENT_META = {
  PushEvent: { icon: 'commit', cls: 'text-emerald-500 bg-emerald-500/10 ring-emerald-500/30', group: 'Pushes' },
  PullRequestEvent: { icon: 'pr', cls: 'text-violet-500 bg-violet-500/10 ring-violet-500/30', group: 'Pull requests' },
  PullRequestReviewEvent: { icon: 'pr', cls: 'text-violet-500 bg-violet-500/10 ring-violet-500/30', group: 'Pull requests' },
  IssuesEvent: { icon: 'issue', cls: 'text-amber-500 bg-amber-500/10 ring-amber-500/30', group: 'Issues' },
  IssueCommentEvent: { icon: 'message', cls: 'text-amber-500 bg-amber-500/10 ring-amber-500/30', group: 'Issues' },
  WatchEvent: { icon: 'star', cls: 'text-yellow-500 bg-yellow-500/10 ring-yellow-500/30', group: 'Stars' },
  ReleaseEvent: { icon: 'tag', cls: 'text-sky-500 bg-sky-500/10 ring-sky-500/30', group: 'Releases' },
  CreateEvent: { icon: 'plus', cls: 'text-blue-500 bg-blue-500/10 ring-blue-500/30', group: 'Created' },
  ForkEvent: { icon: 'fork', cls: 'text-cyan-500 bg-cyan-500/10 ring-cyan-500/30', group: 'Created' },
  Default: { icon: 'activity', cls: 'text-zinc-500 bg-zinc-500/10 ring-zinc-500/30', group: 'Other' },
};
function EventItem({ e, i }) {
  const meta = EVENT_META[e.type] || EVENT_META.Default;
  const p = e.payload || {};
  const repo = (e.repo && e.repo.name) || 'unknown';
  const repoUrl = `https://github.com/${repo}`;
  const RepoLink = <A href={repoUrl} className="font-medium text-zinc-900 dark:text-zinc-100">{repo}</A>;
  const code = 'px-1.5 py-0.5 rounded bg-zinc-100 dark:bg-zinc-800 font-mono text-xs';
  let title, detail = null;
  switch (e.type) {
    case 'PushEvent': {
      const branch = (p.ref || '').replace('refs/heads/', '');
      const commits = Array.isArray(p.commits) ? p.commits : [];
      const count = p.size != null ? p.size : commits.length;
      title = <>Pushed {count ? `${count} commit${count === 1 ? '' : 's'}` : 'changes'} to <span className={code}>{branch || 'a branch'}</span> in {RepoLink}</>;
      if (commits.length) {
        detail = (
          <ul className="space-y-1">
            {commits.slice(-3).reverse().map(c => (
              <li key={c.sha} className="flex gap-2 text-xs"><A href={`${repoUrl}/commit/${c.sha}`} className="font-mono text-indigo-500 shrink-0">{c.sha.slice(0, 7)}</A><span className="truncate text-zinc-500">{firstLine(c.message)}</span></li>
            ))}
          </ul>
        );
      } else if (p.before && p.head) {
        detail = <A href={`${repoUrl}/compare/${p.before.slice(0, 7)}...${p.head.slice(0, 7)}`} className="text-xs text-indigo-500">View changes ({p.before.slice(0, 7)}…{p.head.slice(0, 7)})</A>;
      }
      break;
    }
    case 'PullRequestEvent': {
      const pr = p.pull_request || {};
      const action = p.action === 'closed' && pr.merged ? 'merged' : p.action;
      title = <>{cap(action)} pull request <A href={pr.html_url || repoUrl} className="text-indigo-500">#{p.number != null ? p.number : pr.number}</A> in {RepoLink}</>;
      if (pr.title) detail = <span className="text-xs text-zinc-500">{pr.title}</span>;
      break;
    }
    case 'PullRequestReviewEvent': {
      const pr = p.pull_request || {};
      title = <>Reviewed pull request <A href={pr.html_url || repoUrl} className="text-indigo-500">#{pr.number}</A> in {RepoLink}</>;
      break;
    }
    case 'IssuesEvent': {
      const is = p.issue || {};
      title = <>{cap(p.action)} issue <A href={is.html_url || repoUrl} className="text-indigo-500">#{is.number}</A> in {RepoLink}</>;
      if (is.title) detail = <span className="text-xs text-zinc-500">{is.title}</span>;
      break;
    }
    case 'IssueCommentEvent': {
      const is = p.issue || {};
      title = <>Commented on <A href={(p.comment && p.comment.html_url) || is.html_url || repoUrl} className="text-indigo-500">#{is.number}</A> in {RepoLink}</>;
      if (is.title) detail = <span className="text-xs text-zinc-500">{is.title}</span>;
      break;
    }
    case 'WatchEvent': title = <>Starred {RepoLink}</>; break;
    case 'ReleaseEvent': {
      const r = p.release || {};
      title = <>{cap(p.action)} release <A href={r.html_url || repoUrl} className="text-indigo-500">{r.name || r.tag_name}</A> in {RepoLink}</>;
      break;
    }
    case 'CreateEvent':
      title = p.ref_type === 'repository' ? <>Created repository {RepoLink}</> : <>Created {p.ref_type} <span className={code}>{p.ref}</span> in {RepoLink}</>;
      break;
    case 'ForkEvent':
      title = <>Forked {RepoLink}{p.forkee ? <> to <A href={p.forkee.html_url} className="text-indigo-500">{p.forkee.full_name}</A></> : null}</>;
      break;
    default: title = <>{e.type.replace('Event', '')} in {RepoLink}</>;
  }
  return (
    <li className="relative pl-12 fade-up" style={{ animationDelay: `${Math.min(i, 14) * 35}ms` }}>
      <span className={`absolute left-0 top-0.5 h-8 w-8 grid place-items-center rounded-full ring-1 ${meta.cls}`}><Icon name={meta.icon} size={15} /></span>
      <div className="text-sm leading-6">{title}</div>
      {detail && <div className="mt-1">{detail}</div>}
      <div className="mt-1 text-[11px] text-zinc-500" title={new Date(e.created_at).toLocaleString()}>{timeAgo(e.created_at)}</div>
    </li>
  );
}
function Activity({ events, error, onToken, hasToken }) {
  const [filter, setFilter] = useState('All');
  const [limit, setLimit] = useState(30);
  const groups = useMemo(() => {
    const m = new Map();
    events.forEach(e => { const g = (EVENT_META[e.type] || EVENT_META.Default).group; m.set(g, (m.get(g) || 0) + 1); });
    return [...m.entries()];
  }, [events]);
  const list = filter === 'All' ? events : events.filter(e => (EVENT_META[e.type] || EVENT_META.Default).group === filter);
  useEffect(() => setLimit(30), [filter]);
  if (error && error.kind === 'ratelimit') return <RateBanner reset={error.reset} onToken={onToken} hasToken={hasToken} />;
  if (error) return <div className={`${card} p-8 text-center text-sm text-zinc-500`}>Activity could not be loaded: {error.message}</div>;
  if (!events.length) return <div className={`${card} p-10 text-center`}><Icon name="activity" size={28} className="mx-auto text-zinc-400" /><p className="mt-3 font-medium">No recent public activity</p><p className="text-sm text-zinc-500 mt-1">GitHub only exposes events from roughly the last 90 days.</p></div>;
  return (
    <div id="panel-activity" role="tabpanel" aria-labelledby="tab-activity" className={`${card} p-5`}>
      <div className="flex flex-wrap gap-2 mb-5">
        {[['All', events.length], ...groups].map(([g, c]) => (
          <button key={g} onClick={() => setFilter(g)}
            className={`${pillBtn} ${filter === g ? '!border-indigo-500 !text-indigo-500 bg-indigo-500/10' : ''}`}>{g} <span className="tabular-nums opacity-70">{c}</span></button>
        ))}
      </div>
      <div className="relative">
        <span aria-hidden="true" className="absolute left-4 top-2 bottom-2 w-px bg-zinc-200 dark:bg-zinc-800" />
        <ul className="relative space-y-5">{list.slice(0, limit).map((e, i) => <EventItem key={e.id} e={e} i={i} />)}</ul>
      </div>
      {list.length > limit && <div className="text-center mt-6"><button onClick={() => setLimit(l => l + 30)} className={btn}>Show more ({list.length - limit} left)</button></div>}
    </div>
  );
}

/* ───────────────────────── drawer & modal ───────────────────────── */
function RepoDrawer({ repo, onClose, loadReadme }) {
  const [shown, setShown] = useState(false);
  const [readme, setReadme] = useState({ status: 'loading' });
  const panelRef = useRef(null);
  const closeRef = useRef(null);
  const openerRef = useRef(document.activeElement);
  const close = useCallback(() => { setShown(false); setTimeout(onClose, 280); }, [onClose]);

  useEffect(() => {
    const id = requestAnimationFrame(() => setShown(true));
    if (closeRef.current) closeRef.current.focus();
    const prev = document.body.style.overflow; document.body.style.overflow = 'hidden';
    const opener = openerRef.current;
    return () => { cancelAnimationFrame(id); document.body.style.overflow = prev; if (opener && opener.focus) opener.focus(); };
  }, []);
  useEffect(() => {
    const h = e => { if (e.key === 'Escape') close(); };
    window.addEventListener('keydown', h); return () => window.removeEventListener('keydown', h);
  }, [close]);
  useEffect(() => {
    const ctrl = new AbortController();
    setReadme({ status: 'loading' });
    loadReadme(repo, ctrl.signal)
      .then(html => setReadme({ status: 'ok', html }))
      .catch(e => { if (e.name === 'AbortError') return; setReadme({ status: e.kind === 'notfound' ? 'none' : 'error', error: e }); });
    return () => ctrl.abort();
  }, [repo.full_name]);

  const chip = 'inline-flex items-center gap-1.5 h-7 px-2.5 rounded-lg text-xs bg-zinc-100 dark:bg-zinc-800/70 text-zinc-600 dark:text-zinc-300';
  return (
    <div className="fixed inset-0 z-50" role="dialog" aria-modal="true" aria-label={`${repo.name} details`}>
      <div onClick={close} className="absolute inset-0 bg-black/50 backdrop-blur-sm transition-opacity duration-300" style={{ opacity: shown ? 1 : 0 }} />
      <aside ref={panelRef} onKeyDown={e => trapTab(e, panelRef.current)}
        className="absolute right-0 top-0 h-full w-full max-w-2xl bg-white dark:bg-zinc-950 border-l border-zinc-200 dark:border-zinc-800 shadow-2xl flex flex-col"
        style={{ transform: shown ? 'translateX(0)' : 'translateX(100%)', transition: 'transform .35s cubic-bezier(.32,.72,0,1)' }}>
        <div className="p-5 border-b border-zinc-200 dark:border-zinc-800 flex items-start gap-3">
          <div className="min-w-0 flex-1">
            <div className="text-xs text-zinc-500 truncate">{repo.owner.login} /</div>
            <h2 className="text-xl font-semibold truncate">{repo.name}</h2>
          </div>
          <a href={repo.html_url} target="_blank" rel="noopener noreferrer" className={btn}><Icon name="ext" size={14} /> <span className="hidden sm:inline">Open</span></a>
          <button ref={closeRef} onClick={close} aria-label="Close drawer" className="h-9 w-9 grid place-items-center rounded-lg border border-zinc-200 dark:border-zinc-800 hover:bg-zinc-100 dark:hover:bg-zinc-800 active:scale-95 transition"><Icon name="x" size={16} /></button>
        </div>
        <div className="flex-1 overflow-y-auto p-5 space-y-5">
          {repo.description && <p className="text-zinc-600 dark:text-zinc-300">{repo.description}</p>}
          <div className="flex flex-wrap gap-2">
            {repo.language && <span className={chip}><span className="h-2.5 w-2.5 rounded-full" style={{ background: langColor(repo.language) }} />{repo.language}</span>}
            <span className={chip}><Icon name="star" size={12} className="text-yellow-500" fill />{repo.stargazers_count.toLocaleString()} stars</span>
            <span className={chip}><Icon name="fork" size={12} />{repo.forks_count.toLocaleString()} forks</span>
            <span className={chip}><Icon name="issue" size={12} />{repo.open_issues_count.toLocaleString()} open issues</span>
            <span className={chip}><Icon name="branch" size={12} />{repo.default_branch}</span>
            {repo.license && <span className={chip}><Icon name="file" size={12} />{repo.license.spdx_id || repo.license.name}</span>}
            <span className={chip}><Icon name="clock" size={12} />Pushed {timeAgo(repo.pushed_at || repo.updated_at)}</span>
          </div>
          {(repo.topics || []).length > 0 && <div className="flex flex-wrap gap-1.5">{repo.topics.map(t => <span key={t} className="text-xs px-2.5 h-6 inline-flex items-center rounded-full bg-indigo-500/10 text-indigo-500">{t}</span>)}</div>}
          {repo.homepage && <A href={/^https?:\/\//i.test(repo.homepage) ? repo.homepage : `https://${repo.homepage}`} className="inline-flex items-center gap-1.5 text-sm text-indigo-500"><Icon name="link" size={14} />{repo.homepage.replace(/^https?:\/\//i, '')}</A>}
          <div className="rounded-xl border border-zinc-200 dark:border-zinc-800 overflow-hidden">
            <div className="px-4 h-10 flex items-center gap-2 text-sm font-medium bg-zinc-50 dark:bg-zinc-900 border-b border-zinc-200 dark:border-zinc-800"><Icon name="book" size={14} />README</div>
            <div className="p-5">
              {readme.status === 'loading' && <div className="space-y-3" aria-busy="true"><Skel className="h-7 w-2/3" /><Skel className="h-4 w-full" /><Skel className="h-4 w-11/12" /><Skel className="h-4 w-4/5" /><Skel className="h-32 w-full" /><Skel className="h-4 w-3/4" /></div>}
              {readme.status === 'ok' && <div className="readme" dangerouslySetInnerHTML={{ __html: readme.html }} />}
              {readme.status === 'none' && <p className="text-sm text-zinc-500 py-8 text-center">This repository has no README.</p>}
              {readme.status === 'error' && (
                <div className="py-8 text-center text-sm">
                  <p className="text-zinc-500">{readme.error && readme.error.kind === 'ratelimit' ? `Rate limit reached. Resets at ${new Date(readme.error.reset).toLocaleTimeString()}.` : (readme.error && readme.error.message) || 'Could not load the README.'}</p>
                </div>
              )}
            </div>
          </div>
        </div>
      </aside>
    </div>
  );
}

function TokenModal({ hasToken, rate, onSave, onRemove, onClose }) {
  const [val, setVal] = useState('');
  const [show, setShow] = useState(false);
  const [busy, setBusy] = useState(false);
  const [err, setErr] = useState('');
  const [shown, setShown] = useState(false);
  const boxRef = useRef(null);
  const inputRef = useRef(null);
  const openerRef = useRef(document.activeElement);
  const close = useCallback(() => { setShown(false); setTimeout(onClose, 200); }, [onClose]);
  useEffect(() => {
    const id = requestAnimationFrame(() => setShown(true));
    if (inputRef.current) inputRef.current.focus();
    const opener = openerRef.current;
    return () => { cancelAnimationFrame(id); if (opener && opener.focus) opener.focus(); };
  }, []);
  useEffect(() => {
    const h = e => { if (e.key === 'Escape') close(); };
    window.addEventListener('keydown', h); return () => window.removeEventListener('keydown', h);
  }, [close]);
  const submit = async e => {
    e.preventDefault();
    const t = val.trim();
    if (!t) { setErr('Paste a token first.'); return; }
    setBusy(true); setErr('');
    const res = await onSave(t);
    setBusy(false);
    if (res !== true) setErr(res);
  };
  return (
    <div className="fixed inset-0 z-50 grid place-items-center p-4" role="dialog" aria-modal="true" aria-label="GitHub token settings">
      <div onClick={close} className="absolute inset-0 bg-black/50 backdrop-blur-sm transition-opacity duration-200" style={{ opacity: shown ? 1 : 0 }} />
      <div ref={boxRef} onKeyDown={e => trapTab(e, boxRef.current)}
        className="relative w-full max-w-md rounded-2xl border border-zinc-200 dark:border-zinc-800 bg-white dark:bg-zinc-950 shadow-2xl p-6"
        style={{ opacity: shown ? 1 : 0, transform: shown ? 'translateY(0) scale(1)' : 'translateY(12px) scale(.97)', transition: 'all .3s cubic-bezier(.34,1.4,.64,1)' }}>
        <div className="flex items-start gap-3">
          <span className="grid place-items-center h-10 w-10 rounded-xl bg-indigo-500/10 text-indigo-500 shrink-0"><Icon name="key" size={20} /></span>
          <div className="flex-1">
            <h2 className="font-semibold text-lg">GitHub access token</h2>
            <p className="text-sm text-zinc-500 dark:text-zinc-400 mt-0.5">Optional. Raises the hourly API limit from 60 to 5,000 requests.</p>
          </div>
          <button onClick={close} aria-label="Close" className="h-8 w-8 grid place-items-center rounded-lg text-zinc-400 hover:bg-zinc-100 dark:hover:bg-zinc-800"><Icon name="x" size={16} /></button>
        </div>
        {rate && (
          <div className="mt-4 rounded-xl bg-zinc-100 dark:bg-zinc-900 p-3 text-sm flex items-center justify-between">
            <span className="text-zinc-500">Requests left</span>
            <span className="font-mono tabular-nums font-semibold">{rate.remaining} / {rate.limit} <span className="text-xs font-normal text-zinc-500">· resets {new Date(rate.reset).toLocaleTimeString()}</span></span>
          </div>
        )}
        {hasToken && <div className="mt-3 flex items-center gap-2 text-sm text-emerald-500"><Icon name="check" size={15} /> A token is saved in this browser.</div>}
        <form onSubmit={submit} className="mt-4 space-y-3">
          <div className="relative">
            <input ref={inputRef} type={show ? 'text' : 'password'} value={val} onChange={e => setVal(e.target.value)} placeholder={hasToken ? 'Paste a new token to replace it' : 'github_pat_… or ghp_…'}
              aria-label="GitHub personal access token" autoComplete="off" spellCheck={false} className={`${fieldCls} w-full px-3 pr-16 font-mono`} />
            <button type="button" onClick={() => setShow(s => !s)} className="absolute right-2 top-1/2 -translate-y-1/2 text-xs text-zinc-500 hover:text-indigo-500 px-2">{show ? 'Hide' : 'Show'}</button>
          </div>
          {err && <p role="alert" className="text-sm text-red-500">{err}</p>}
          <div className="flex gap-2">
            <button type="submit" disabled={busy} className={`${btnPrimary} flex-1`}>{busy ? <span className="spinner" /> : <Icon name="lock" size={14} />} Verify & save</button>
            {hasToken && <button type="button" onClick={onRemove} className={`${btn} text-red-500`}><Icon name="trash" size={14} /> Remove</button>}
          </div>
        </form>
        <p className="mt-4 text-xs text-zinc-500 leading-relaxed">
          A token with <strong>no scopes</strong> is enough, since only public data is read.{' '}
          <A href="https://github.com/settings/tokens/new?description=GitHub%20Profile%20Explorer" className="text-indigo-500">Create one on GitHub</A>.
          It is stored in this browser's localStorage and sent only to api.github.com. localStorage is readable by any script on this page, so use a token you can revoke.
        </p>
      </div>
    </div>
  );
}

/* ───────────────────────── dashboard ───────────────────────── */
function Dashboard({ data, tab, setTab, onOpenRepo, onCopy, copied, onRefresh, onToken, hasToken }) {
  const { user, repos, events } = data;
  const stats = useMemo(() => analyze(repos), [repos]);
  const heat = useMemo(() => buildHeatmap(events), [events]);
  const tabs = [
    { id: 'overview', label: 'Overview', icon: 'chart' },
    { id: 'repos', label: 'Repositories', icon: 'book', count: repos.length },
    { id: 'activity', label: 'Activity', icon: 'activity', count: events.length },
  ];
  return (
    <div key={user.login} className="grid lg:grid-cols-[320px_minmax(0,1fr)] gap-6 items-start">
      <aside className="lg:sticky lg:top-24"><ProfileCard user={user} onCopy={onCopy} copied={copied} onRefresh={onRefresh} /></aside>
      <section className="min-w-0 space-y-5">
        {data.eventsError && data.eventsError.kind === 'ratelimit' && <RateBanner reset={data.eventsError.reset} onToken={onToken} hasToken={hasToken} />}
        {data.truncated && (
          <div className="fade-up rounded-xl border border-zinc-200 dark:border-zinc-800 bg-zinc-100/60 dark:bg-zinc-900/60 px-4 py-2.5 text-xs text-zinc-500">
            Showing the {repos.length} most recently updated of {user.public_repos} public repositories. Add a token to load more.
          </div>
        )}
        <Reveal><Tabs tabs={tabs} value={tab} onChange={setTab} /></Reveal>
        <div key={tab}>
          {tab === 'overview' && <Overview stats={stats} heat={heat} data={data} onOpenRepo={onOpenRepo} />}
          {tab === 'repos' && <Repos repos={repos} languages={stats.languages} onOpen={onOpenRepo} />}
          {tab === 'activity' && <Activity events={events} error={data.eventsError} onToken={onToken} hasToken={hasToken} />}
        </div>
      </section>
    </div>
  );
}

/* ───────────────────────── app ───────────────────────── */
function App() {
  const [theme, setTheme] = useState(() => (document.documentElement.classList.contains('dark') ? 'dark' : 'light'));
  const [token, setToken] = useState(() => LS.get('ghx_token', ''));
  const [rate, setRate] = useState(null);
  const [recent, setRecent] = useState(() => LS.get('ghx_recent', []));
  const [query, setQuery] = useState('');
  const [state, setState] = useState({ status: 'idle' });
  const [tab, setTab] = useState('overview');
  const [drawerRepo, setDrawerRepo] = useState(null);
  const [showToken, setShowToken] = useState(false);
  const [toast, setToast] = useState(null);
  const [copied, setCopied] = useState(false);

  const inputRef = useRef(null);
  const abortRef = useRef(null);
  const cacheRef = useRef(new Map());
  const readmeCache = useRef(new Map());
  const toastTimer = useRef(null);
  const tokenRef = useRef(token);
  const stateRef = useRef(state);
  stateRef.current = state;

  useEffect(() => { document.documentElement.classList.toggle('dark', theme === 'dark'); LS.set('ghx_theme', theme); }, [theme]);
  useEffect(() => { LS.set('ghx_recent', recent); }, [recent]);

  const notify = useCallback(msg => {
    clearTimeout(toastTimer.current);
    setToast({ msg, id: Date.now() });
    toastTimer.current = setTimeout(() => setToast(null), 2400);
  }, []);

  const refreshRate = useCallback(async t => {
    try {
      const d = await ghFetch('/rate_limit', { token: t });
      const c = d && d.resources && d.resources.core;
      if (c) setRate({ limit: c.limit, remaining: c.remaining, reset: c.reset * 1000 });
    } catch {}
  }, []);

  const loadUser = useCallback(async (rawName, { force = false, push = true } = {}) => {
    const name = parseUsername(rawName);
    if (!name) return;
    setQuery(name); if (push) syncUrl(name);
    setTab('overview'); setDrawerRepo(null);
    const cached = cacheRef.current.get(name.toLowerCase());
    if (!force && cached && Date.now() - cached.at < 5 * 60 * 1000) {
      setState({ status: 'ready', data: cached.data, name });
      setRecent(prev => [cached.data.user.login, ...prev.filter(x => x.toLowerCase() !== cached.data.user.login.toLowerCase())].slice(0, 8));
      return;
    }
    if (abortRef.current) abortRef.current.abort();
    const ctrl = new AbortController(); abortRef.current = ctrl;
    setState({ status: 'loading', name });
    const opts = { token: tokenRef.current, signal: ctrl.signal, onRate: setRate };
    try {
      let eventsError = null;
      const [user, repos1, events1] = await Promise.all([
        ghFetch(`/users/${name}`, opts),
        ghFetch(`/users/${name}/repos?per_page=100&sort=updated`, opts),
        ghFetch(`/users/${name}/events/public?per_page=100`, opts).catch(e => { if (e.name === 'AbortError') throw e; eventsError = e; return []; }),
      ]);
      let repos = repos1;
      if (tokenRef.current && user.public_repos > 100) {
        const pages = Math.min(Math.ceil(user.public_repos / 100), 5);
        const rest = await Promise.all(Array.from({ length: pages - 1 }, (_, i) => ghFetch(`/users/${name}/repos?per_page=100&sort=updated&page=${i + 2}`, opts)));
        repos = repos.concat(...rest);
      }
      let events = events1;
      if (tokenRef.current && events1.length === 100) {
        const more = await Promise.all([2, 3].map(pg => ghFetch(`/users/${name}/events/public?per_page=100&page=${pg}`, opts).catch(() => [])));
        events = events.concat(...more);
      }
      const data = { user, repos, events, eventsError, truncated: repos.length < user.public_repos };
      cacheRef.current.set(name.toLowerCase(), { at: Date.now(), data });
      setState({ status: 'ready', data, name });
      setRecent(prev => [user.login, ...prev.filter(x => x.toLowerCase() !== user.login.toLowerCase())].slice(0, 8));
    } catch (e) {
      if (e.name === 'AbortError') return;
      setState({ status: 'error', name, error: e instanceof ApiError ? e : new ApiError('http', e.message || 'Unexpected error') });
    }
  }, []);

  useEffect(() => {
    refreshRate(tokenRef.current);
    const initial = new URLSearchParams(window.location.search).get('user');
    if (initial) loadUser(initial, { push: false });
    const onPop = () => {
      const u = new URLSearchParams(window.location.search).get('user');
      if (u) loadUser(u, { push: false });
      else { if (abortRef.current) abortRef.current.abort(); setState({ status: 'idle' }); setQuery(''); }
    };
    window.addEventListener('popstate', onPop);
    return () => window.removeEventListener('popstate', onPop);
  }, []);

  useEffect(() => {
    const h = e => {
      const tag = ((e.target && e.target.tagName) || '').toLowerCase();
      const typing = tag === 'input' || tag === 'textarea' || tag === 'select' || (e.target && e.target.isContentEditable);
      const focusSearch = () => { e.preventDefault(); if (inputRef.current) { inputRef.current.focus(); inputRef.current.select(); } };
      if ((e.metaKey || e.ctrlKey) && e.key.toLowerCase() === 'k') focusSearch();
      else if (e.key === '/' && !typing && !e.metaKey && !e.ctrlKey && !e.altKey) focusSearch();
    };
    window.addEventListener('keydown', h);
    return () => window.removeEventListener('keydown', h);
  }, []);

  const retryIfNeeded = () => {
    const s = stateRef.current;
    if (s.status === 'error' && s.name) setTimeout(() => loadUser(s.name, { force: true, push: false }), 0);
  };
  const saveToken = async t => {
    try {
      const d = await ghFetch('/rate_limit', { token: t });
      const c = d && d.resources && d.resources.core;
      if (c) setRate({ limit: c.limit, remaining: c.remaining, reset: c.reset * 1000 });
    } catch (e) {
      return e.kind === 'badtoken' ? 'GitHub rejected this token. Check that it was copied in full and has not expired.' : e.message || 'Could not verify the token.';
    }
    tokenRef.current = t; setToken(t); LS.set('ghx_token', t);
    setShowToken(false); notify('Token saved. Higher rate limit active.');
    retryIfNeeded();
    return true;
  };
  const removeToken = () => {
    tokenRef.current = ''; setToken(''); LS.set('ghx_token', '');
    setShowToken(false); notify('Token removed'); refreshRate(''); retryIfNeeded();
  };

  const submit = v => {
    const name = parseUsername(v);
    if (!name) { notify('Enter a valid GitHub username'); return; }
    loadUser(name);
    if (inputRef.current) inputRef.current.blur();
  };
  const goHome = () => {
    if (abortRef.current) abortRef.current.abort();
    setState({ status: 'idle' }); setQuery(''); setDrawerRepo(null); syncUrl('');
  };
  const copyLink = async () => {
    const d = stateRef.current.data; if (!d) return;
    const link = `${window.location.href.split(/[?#]/)[0]}?user=${encodeURIComponent(d.user.login)}`;
    const ok = await copyText(link);
    if (ok) { setCopied(true); setTimeout(() => setCopied(false), 1800); notify('Profile link copied'); }
    else notify('Copy failed. Copy the URL from the address bar instead.');
  };
  const fetchReadme = useCallback(async (repo, signal) => {
    if (readmeCache.current.has(repo.full_name)) return readmeCache.current.get(repo.full_name);
    const html = await ghFetch(`/repos/${repo.full_name}/readme`, { token: tokenRef.current, signal, onRate: setRate, accept: 'application/vnd.github.html+json', raw: true });
    const clean = sanitizeReadme(html, repo);
    readmeCache.current.set(repo.full_name, clean);
    return clean;
  }, []);
  const closeDrawer = useCallback(() => setDrawerRepo(null), []);

  const activeName = state.status === 'ready' ? state.data.user.login : state.name;
  let content;
  if (state.status === 'idle') content = <Hero onPick={loadUser} />;
  else if (state.status === 'loading') content = <LoadingLayout />;
  else if (state.status === 'error') content = <ErrorState error={state.error} name={state.name} hasToken={!!token} onToken={() => setShowToken(true)} onRetry={() => loadUser(state.name, { force: true, push: false })} onPick={loadUser} />;
  else content = <Dashboard data={state.data} tab={tab} setTab={setTab} onOpenRepo={setDrawerRepo} onCopy={copyLink} copied={copied}
    onRefresh={() => loadUser(state.data.user.login, { force: true, push: false })} onToken={() => setShowToken(true)} hasToken={!!token} />;

  return (
    <div className="min-h-screen bg-glow">
      <Header inputRef={inputRef} query={query} setQuery={setQuery} onSubmit={submit} loading={state.status === 'loading'} rate={rate} hasToken={!!token}
        onToken={() => setShowToken(true)} theme={theme} onTheme={() => setTheme(t => (t === 'dark' ? 'light' : 'dark'))} onHome={goHome} />
      <main className="mx-auto max-w-7xl px-4 sm:px-6 py-6">
        <Discovery recent={recent} active={activeName} onPick={loadUser}
          onRemove={u => setRecent(r => r.filter(x => x !== u))} onClear={() => setRecent([])} />
        {content}
      </main>
      <footer className="mx-auto max-w-7xl px-4 sm:px-6 pb-10 text-xs text-zinc-500 text-center">
        Data comes from the GitHub REST API. Requests are made directly from your browser.
      </footer>
      {drawerRepo && <RepoDrawer repo={drawerRepo} onClose={closeDrawer} loadReadme={fetchReadme} />}
      {showToken && <TokenModal hasToken={!!token} rate={rate} onSave={saveToken} onRemove={removeToken} onClose={() => setShowToken(false)} />}
      {toast && (
        <div key={toast.id} role="status" aria-live="polite" className="toast-in fixed bottom-6 left-1/2 z-[60] -translate-x-1/2 inline-flex items-center gap-2 h-10 px-4 rounded-xl bg-zinc-900 dark:bg-zinc-100 text-white dark:text-zinc-900 text-sm shadow-xl">
          <Icon name="check" size={15} className="text-emerald-400 dark:text-emerald-600" /> {toast.msg}
        </div>
      )}
    </div>
  );
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />);
</script>
</body>
</html>
