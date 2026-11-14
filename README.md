<!DOCTYPE html>
<html lang="en" class="dark">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>GitHub Profile Explorer</title>
<script src="https://cdn.tailwindcss.com/3.4.17"></script>
<script>tailwind.config={darkMode:'class'}</script>
<script crossorigin src="https://unpkg.com/react@18.3.1/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18.3.1/umd/react-dom.production.min.js"></script>
<script src="https://unpkg.com/@babel/standalone@7.26.4/babel.min.js"></script>
<style>
@keyframes up{from{opacity:0;transform:translateY(14px)}to{opacity:1;transform:none}}
@keyframes sh{to{background-position:-200% 0}}
@keyframes slide{from{transform:translateX(100%)}to{transform:none}}
@keyframes fade{from{opacity:0}to{opacity:1}}
@keyframes pop{0%{transform:scale(.4)}70%{transform:scale(1.25)}100%{transform:scale(1)}}
.up{animation:up .55s cubic-bezier(.2,.8,.2,1) both}
.slide{animation:slide .35s cubic-bezier(.2,.8,.2,1) both}
.fade{animation:fade .25s both}.pop{animation:pop .35s both}
.sk{background:linear-gradient(90deg,rgba(128,128,128,.12) 25%,rgba(128,128,128,.26) 50%,rgba(128,128,128,.12) 75%);background-size:200% 100%;animation:sh 1.4s infinite;border-radius:8px}
.md{font-size:.9rem;line-height:1.65;word-break:break-word}
.md h1,.md h2,.md h3{font-weight:600;margin:1.2em 0 .5em}.md h1,.md h2{font-size:1.25rem;border-bottom:1px solid rgba(128,128,128,.25);padding-bottom:.3em}
.md p,.md ul,.md ol{margin:.6em 0}.md ul{list-style:disc;padding-left:1.4em}.md ol{list-style:decimal;padding-left:1.4em}
.md pre{background:rgba(128,128,128,.15);padding:12px;border-radius:8px;overflow:auto;font-size:.8rem}
.md code{background:rgba(128,128,128,.18);padding:.1em .35em;border-radius:4px;font-size:.85em}.md pre code{background:none;padding:0}
.md a{color:#38bdf8}.md img{max-width:100%}.md .anchor{display:none}.md table{border-collapse:collapse;display:block;overflow:auto}.md td,.md th{border:1px solid rgba(128,128,128,.3);padding:4px 8px}
</style>
</head>
<body class="min-h-screen bg-zinc-50 text-zinc-900 dark:bg-zinc-950 dark:text-zinc-100 antialiased">
<div class="pointer-events-none fixed inset-0 -z-10 bg-[radial-gradient(ellipse_at_top,rgba(56,189,248,.12),transparent_55%)]"></div>
<div id="root"></div>
<script type="text/babel">
const {useState,useEffect,useMemo,useRef,useCallback}=React;
const QUICK=['torvalds','gaearon','yyx990803','sindresorhus','addyosmani','tj'];
const P={search:'M11 19a8 8 0 100-16 8 8 0 000 16zM21 21l-4.3-4.3',x:'M18 6L6 18M6 6l12 12',star:'M12 2l3.1 6.3 6.9 1-5 4.9 1.2 6.8L12 17.8 5.8 21l1.2-6.8-5-4.9 6.9-1z',fork:'M12 15v-3M6 9v2a1 1 0 001 1h10a1 1 0 001-1V9M9 18a3 3 0 106 0 3 3 0 10-6 0M3 6a3 3 0 106 0 3 3 0 10-6 0M15 6a3 3 0 106 0 3 3 0 10-6 0',eye:'M2 12s4-8 10-8 10 8 10 8-4 8-10 8S2 12 2 12zM12 15a3 3 0 100-6 3 3 0 000 6z',alert:'M12 8v4M12 16h.01M12 22a10 10 0 100-20 10 10 0 000 20z',sun:'M12 16a4 4 0 100-8 4 4 0 000 8zM12 2v2M12 20v2M4.9 4.9l1.4 1.4M17.7 17.7l1.4 1.4M2 12h2M20 12h2M4.9 19.1l1.4-1.4M17.7 6.3l1.4-1.4',moon:'M21 12.8A9 9 0 1111.2 3a7 7 0 009.8 9.8z',key:'M21 2l-9.6 9.6M15.5 7.5l3 3L22 7l-3-3M7.5 21a5.5 5.5 0 100-11 5.5 5.5 0 000 11z',link:'M10 13a5 5 0 007.5.5l3-3a5 5 0 00-7-7l-1.7 1.7M14 11a5 5 0 00-7.5-.5l-3 3a5 5 0 007 7l1.7-1.7',ext:'M15 3h6v6M10 14L21 3M18 13v6a2 2 0 01-2 2H5a2 2 0 01-2-2V8a2 2 0 012-2h6',check:'M20 6L9 17l-5-5',pin:'M20 10c0 6-8 12-8 12S4 16 4 10a8 8 0 1116 0zM12 13a3 3 0 100-6 3 3 0 000 6z',bldg:'M3 21h18M5 21V5l7-3 7 3v16M9 9h1M14 9h1M9 13h1M14 13h1',book:'M4 19.5A2.5 2.5 0 016.5 17H20V2H6.5A2.5 2.5 0 004 4.5v15zM20 17v5H6.5a2.5 2.5 0 010-5',cal:'M8 2v4M16 2v4M3 10h18M5 4h14a2 2 0 012 2v14a2 2 0 01-2 2H5a2 2 0 01-2-2V6a2 2 0 012-2z',commit:'M12 15a3 3 0 100-6 3 3 0 000 6zM3 12h6M15 12h6',pr:'M6 9v12M18 15V8a3 3 0 00-3-3h-3M3 6a3 3 0 106 0 3 3 0 10-6 0M15 18a3 3 0 106 0 3 3 0 10-6 0',issue:'M12 22a10 10 0 100-20 10 10 0 000 20zM12 13a1 1 0 100-2 1 1 0 000 2z',tag:'M20.6 13.4l-7.2 7.2a2 2 0 01-2.8 0L2 12V2h10l8.6 8.6a2 2 0 010 2.8zM7 7h.01',plus:'M12 5v14M5 12h14',clock:'M12 22a10 10 0 100-20 10 10 0 000 20zM12 6v6l4 2',globe:'M12 22a10 10 0 100-20 10 10 0 000 20zM2 12h20M12 2a15 15 0 010 20 15 15 0 010-20',at:'M16 12a4 4 0 11-8 0 4 4 0 018 0zm0 0v1.5a2.5 2.5 0 005 0V12a9 9 0 10-3.6 7.2'};
const Icon=({n,c='w-4 h-4'})=><svg viewBox="0 0 24 24" className={c} fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d={P[n]}/></svg>;
const CARD='rounded-xl border border-zinc-200 dark:border-zinc-800 bg-white/70 dark:bg-zinc-900/60 backdrop-blur p-4 transition duration-200 hover:-translate-y-0.5 hover:border-sky-500/40 hover:shadow-lg hover:shadow-sky-500/10';
const PILL='px-2.5 py-1 rounded-full text-xs border border-zinc-300 dark:border-zinc-800 hover:border-sky-500/60 hover:bg-sky-500/10 hover:-translate-y-0.5 active:scale-95 transition';
const D=i=>({animationDelay:i*60+'ms'});
const LC={JavaScript:'#f1e05a',TypeScript:'#3178c6',Python:'#3572A5',Java:'#b07219',Go:'#00ADD8',Rust:'#dea584','C++':'#f34b7d',C:'#888',['C#']:'#178600',Ruby:'#701516',PHP:'#4F5D95',HTML:'#e34c26',CSS:'#563d7c',Shell:'#89e051',Swift:'#F05138',Kotlin:'#A97BFF',Dart:'#00B4AB',Vue:'#41b883'};
const lc=l=>LC[l]||`hsl(${[...l].reduce((a,c)=>a+c.charCodeAt(0)*7,0)%360} 60% 55%)`;
const ago=d=>{const s=(new Date(d)-Date.now())/1e3,f=new Intl.RelativeTimeFormat('en',{numeric:'auto'});for(const[u,n]of[['year',31536e3],['month',2592e3],['day',86400],['hour',3600],['minute',60]])if(Math.abs(s)>=n)return f.format(Math.round(s/n),u);return 'just now'};
const ls={get:(k,d)=>{try{return JSON.parse(localStorage.getItem(k))??d}catch{return d}},set:(k,v)=>{try{localStorage.setItem(k,JSON.stringify(v))}catch{}}};
const useOn=d=>{const[o,s]=useState(false);useEffect(()=>{s(false);const t=setTimeout(()=>s(true),60);return()=>clearTimeout(t)},[d]);return o};
const Num=({v})=>{const[n,setN]=useState(0);useEffect(()=>{let r,t0;const f=t=>{t0??=t;const p=Math.min((t-t0)/900,1);setN(Math.round(v*(1-Math.pow(1-p,3))));if(p<1)r=requestAnimationFrame(f)};r=requestAnimationFrame(f);return()=>cancelAnimationFrame(r)},[v]);return <>{n.toLocaleString()}</>};
const Row=({i,children})=>children?<div className="flex items-center gap-2 text-sm text-zinc-600 dark:text-zinc-400"><Icon n={i}/><span className="truncate">{children}</span></div>:null;

const Donut=({langs})=>{
 const on=useOn(langs),[hov,setHov]=useState(null);
 const tot=langs.reduce((a,[,n])=>a+n,0),top=langs.slice(0,7),rest=tot-top.reduce((a,[,n])=>a+n,0);
 const items=rest>0?[...top,['Other',rest]]:top,R=52,C=2*Math.PI*R;let off=0;
 if(!tot)return <p className="text-sm text-zinc-500">No language data for this user's repositories.</p>;
 const h=items.find(i=>i[0]===hov);
 return <div className="flex items-center gap-5 flex-wrap">
  <div className="relative w-36 h-36">
   <svg viewBox="0 0 140 140" className="w-full h-full -rotate-90"><circle cx="70" cy="70" r={R} fill="none" stroke="rgba(128,128,128,.15)" strokeWidth="14"/>
   {items.map(([l,n])=>{const len=n/tot*C;const el=<circle key={l} cx="70" cy="70" r={R} fill="none" stroke={l==='Other'?'#71717a':lc(l)} strokeWidth={hov===l?18:14} strokeDasharray={`${on?Math.max(len-2,0):0} ${C}`} strokeDashoffset={-off} style={{transition:'stroke-dasharray .9s cubic-bezier(.2,.8,.2,1),stroke-width .2s',cursor:'pointer'}} onMouseEnter={()=>setHov(l)} onMouseLeave={()=>setHov(null)}/>;off+=len;return el})}</svg>
   <div className="absolute inset-0 grid place-items-center text-center pointer-events-none"><div><div className="text-lg font-semibold">{h?Math.round(h[1]/tot*100)+'%':tot}</div><div className="text-[10px] text-zinc-500">{h?h[0]:'repos'}</div></div></div>
  </div>
  <ul className="text-xs space-y-1.5 flex-1 min-w-[120px]">{items.map(([l,n])=><li key={l} onMouseEnter={()=>setHov(l)} onMouseLeave={()=>setHov(null)} className={'flex items-center gap-2 transition '+(hov&&hov!==l?'opacity-40':'')}><span className="w-2.5 h-2.5 rounded-full" style={{background:l==='Other'?'#71717a':lc(l)}}/><span className="flex-1 truncate">{l}</span><span className="tabular-nums text-zinc-500">{(n/tot*100).toFixed(1)}%</span></li>)}</ul>
 </div>};

const Heat=({ev})=>{
 const[tip,setTip]=useState('Hover a day');
 const{cells,max,total}=useMemo(()=>{const m={},key=d=>d.toLocaleDateString('en-CA');ev.forEach(e=>{const k=key(new Date(e.created_at));m[k]=(m[k]||0)+1});const t=new Date();t.setHours(0,0,0,0);const s=new Date(t);s.setDate(s.getDate()-s.getDay()-17*7);const cells=[];for(let i=0;i<126;i++){const d=new Date(s);d.setDate(s.getDate()+i);cells.push({d,n:m[key(d)]||0,f:d>t})}return{cells,max:Math.max(1,...Object.values(m)),total:ev.length}},[ev]);
 const C=['bg-zinc-200 dark:bg-zinc-800','bg-emerald-900','bg-emerald-700','bg-emerald-500','bg-emerald-400'];
 return <><div className="flex justify-between items-center mb-3"><h3 className="text-sm font-medium">Activity heatmap</h3><span className="text-xs text-zinc-500">{tip}</span></div>
 <div className="overflow-x-auto pb-1"><div className="grid grid-rows-[repeat(7,minmax(0,1fr))] grid-flow-col gap-1 w-max">{cells.map((c,i)=>c.f?<div key={i} className="w-3.5 h-3.5"/>:<div key={i} onMouseEnter={()=>setTip(`${c.n} event${c.n===1?'':'s'} on ${c.d.toLocaleDateString('en',{month:'short',day:'numeric',year:'numeric'})}`)} className={'w-3.5 h-3.5 rounded-sm transition hover:scale-125 hover:ring-1 ring-sky-400 '+C[c.n?Math.ceil(4*c.n/max):0]}/>)}</div></div>
 <p className="text-xs text-zinc-500 mt-2">Built from the latest {total} public events (GitHub exposes roughly 90 days).</p></>};

const Profile=({u,onCopy,copied})=>{
 const yrs=Math.floor((Date.now()-new Date(u.created_at))/3.156e10),site=u.blog&&(/^https?:/.test(u.blog)?u.blog:'https://'+u.blog);
 return <aside className={CARD+' up lg:sticky lg:top-4 self-start'}>
  <div className="relative w-28 h-28"><div className="absolute inset-0 rounded-full bg-sky-500/40 blur-2xl"/><img src={u.avatar_url+'&s=240'} alt={u.login} className="relative w-28 h-28 rounded-full ring-2 ring-sky-500/50"/></div>
  <h2 className="mt-4 text-xl font-semibold">{u.name||u.login}</h2>
  <div className="mt-1 flex items-center gap-2 flex-wrap"><span className="px-2 py-0.5 rounded-md text-xs font-mono bg-sky-500/10 text-sky-500">@{u.login}</span><span className="px-2 py-0.5 rounded-md text-xs bg-violet-500/10 text-violet-400">{yrs}y on GitHub</span></div>
  {u.bio&&<p className="mt-3 text-sm">{u.bio}</p>}
  <div className="mt-3 space-y-1.5"><Row i="bldg">{u.company}</Row><Row i="pin">{u.location}</Row>
   {site&&<Row i="globe"><a href={site} target="_blank" rel="noopener" className="hover:text-sky-500">{u.blog}</a></Row>}
   {u.twitter_username&&<Row i="at"><a href={'https://x.com/'+u.twitter_username} target="_blank" rel="noopener" className="hover:text-sky-500">@{u.twitter_username}</a></Row>}
   <Row i="cal">Joined {new Date(u.created_at).toLocaleDateString('en',{month:'short',year:'numeric'})}</Row></div>
  <div className="mt-4 grid grid-cols-2 gap-2">{[['Followers',u.followers],['Following',u.following],['Repos',u.public_repos],['Gists',u.public_gists]].map(([l,v])=><div key={l} className="rounded-lg bg-zinc-100 dark:bg-zinc-800/60 p-2 text-center"><div className="font-semibold tabular-nums">{v.toLocaleString()}</div><div className="text-[11px] text-zinc-500">{l}</div></div>)}</div>
  <div className="mt-4 flex gap-2"><button onClick={onCopy} className="flex-1 flex items-center justify-center gap-1.5 text-sm py-2 rounded-lg bg-sky-500 text-white hover:bg-sky-400 active:scale-95 transition">{copied?<span className="pop"><Icon n="check"/></span>:<Icon n="link"/>}{copied?'Copied':'Copy link'}</button>
   <a href={u.html_url} target="_blank" rel="noopener" className="flex-1 flex items-center justify-center gap-1.5 text-sm py-2 rounded-lg border border-zinc-300 dark:border-zinc-700 hover:bg-zinc-100 dark:hover:bg-zinc-800 active:scale-95 transition"><Icon n="ext"/>GitHub</a></div>
 </aside>};

const Overview=({st,ev,open})=>{
 const top=st.top.slice(0,8),mx=Math.max(1,...top.map(r=>r.stargazers_count)),on=useOn(st);
 const K=[['Stars','star',st.stars,'text-yellow-400'],['Forks','fork',st.forks,'text-sky-400'],['Watchers','eye',st.watch,'text-emerald-400'],['Open issues','issue',st.issues,'text-rose-400']];
 return <div className="space-y-4">
  <div className="grid grid-cols-2 md:grid-cols-4 gap-3">{K.map(([l,i,v,c],j)=><div key={l} style={D(j)} className={CARD+' up'}><div className="flex items-center gap-2 text-xs text-zinc-500"><span className={c}><Icon n={i}/></span>{l}</div><div className="mt-2 text-2xl font-semibold tabular-nums"><Num v={v}/></div></div>)}</div>
  <div className="grid md:grid-cols-2 gap-4">
   <div style={D(4)} className={CARD+' up'}><h3 className="text-sm font-medium mb-3">Languages (by repo)</h3><Donut langs={st.langs}/></div>
   <div style={D(5)} className={CARD+' up'}><h3 className="text-sm font-medium mb-3">Top repositories by stars</h3>{top.length?top.map(r=><button key={r.id} onClick={()=>open(r)} className="block w-full text-left group mb-2.5"><div className="flex justify-between text-xs"><span className="truncate group-hover:text-sky-500">{r.name}</span><span className="tabular-nums text-zinc-500">{r.stargazers_count}</span></div><div className="h-2 mt-1 rounded bg-zinc-200 dark:bg-zinc-800 overflow-hidden"><div className="h-full rounded bg-gradient-to-r from-sky-500 to-violet-500" style={{width:on?Math.max(r.stargazers_count/mx*100,2)+'%':0,transition:'width 1s cubic-bezier(.2,.8,.2,1)'}}/></div></button>):<p className="text-sm text-zinc-500">No public repositories.</p>}</div>
  </div>
  <div style={D(6)} className={CARD+' up'}><Heat ev={ev}/></div>
  <div className="grid md:grid-cols-2 gap-4">{[['Most starred',st.mostStar,'star','text-yellow-400'],['Most forked',st.mostFork,'fork','text-sky-400']].filter(x=>x[1]).map(([t,r,i,c],j)=><button key={t} onClick={()=>open(r)} style={D(7+j)} className={CARD+' up text-left'}><div className="text-xs text-zinc-500 flex items-center gap-1.5"><span className={c}><Icon n={i}/></span>{t}</div><div className="mt-1 font-semibold text-sky-500">{r.name}</div><p className="text-sm text-zinc-500 line-clamp-2 min-h-[2.5rem]">{r.description||'No description'}</p><div className="mt-2 text-xs text-zinc-500 flex gap-3"><span>★ {r.stargazers_count}</span><span>⑂ {r.forks_count}</span></div></button>)}</div>
 </div>};

const Repos=({repos,open})=>{
 const[kw,setKw]=useState(''),[lang,setLang]=useState(''),[sort,setSort]=useState('stars');
 const langs=useMemo(()=>[...new Set(repos.map(r=>r.language).filter(Boolean))].sort(),[repos]);
 const list=useMemo(()=>{const k=kw.toLowerCase();return repos.filter(r=>(!lang||r.language===lang)&&(!k||[r.name,r.description,...(r.topics||[])].some(x=>x&&x.toLowerCase().includes(k)))).sort((a,b)=>sort==='stars'?b.stargazers_count-a.stargazers_count:sort==='name'?a.name.localeCompare(b.name):new Date(b.pushed_at)-new Date(a.pushed_at))},[repos,kw,lang,sort]);
 const F='rounded-lg border border-zinc-300 dark:border-zinc-800 bg-white dark:bg-zinc-900 px-3 py-2 text-sm outline-none focus:border-sky-500 transition';
 return <div><div className="flex gap-2 flex-wrap mb-4">
  <input value={kw} onChange={e=>setKw(e.target.value)} placeholder="Filter by name, description or topic" aria-label="Filter repositories" className={F+' flex-1 min-w-[200px]'}/>
  <select value={lang} onChange={e=>setLang(e.target.value)} aria-label="Language" className={F}><option value="">All languages</option>{langs.map(l=><option key={l}>{l}</option>)}</select>
  <select value={sort} onChange={e=>setSort(e.target.value)} aria-label="Sort" className={F}><option value="stars">Most stars</option><option value="updated">Recently updated</option><option value="name">Name A–Z</option></select></div>
  <p className="text-xs text-zinc-500 mb-3">{list.length} of {repos.length} repositories</p>
  {list.length?<div className="grid md:grid-cols-2 gap-3">{list.map((r,i)=><button key={r.id} onClick={()=>open(r)} style={D(Math.min(i,10)*.6)} className={CARD+' up text-left flex flex-col'}>
   <div className="flex items-center gap-2"><Icon n="book" c="w-4 h-4 text-zinc-500"/><span className="font-medium text-sky-500 truncate">{r.name}</span><span className="ml-auto text-[10px] px-1.5 py-0.5 rounded-full border border-zinc-300 dark:border-zinc-700 text-zinc-500">{r.archived?'Archived':r.fork?'Fork':'Public'}</span></div>
   <p className="mt-2 text-sm text-zinc-600 dark:text-zinc-400 line-clamp-2 min-h-[2.5rem]">{r.description||'No description'}</p>
   {!!r.topics?.length&&<div className="mt-2 flex flex-wrap gap-1">{r.topics.slice(0,4).map(t=><span key={t} className="text-[11px] px-2 py-0.5 rounded-full bg-sky-500/10 text-sky-500">{t}</span>)}</div>}
   <div className="mt-auto pt-3 flex items-center gap-3 text-xs text-zinc-500 flex-wrap">{r.language&&<span className="flex items-center gap-1"><span className="w-2.5 h-2.5 rounded-full" style={{background:lc(r.language)}}/>{r.language}</span>}<span>★ {r.stargazers_count}</span><span>⑂ {r.forks_count}</span><span>● {r.open_issues_count}</span><span className="ml-auto">pushed {ago(r.pushed_at)}</span></div></button>)}</div>
  :<p className="text-center text-zinc-500 py-12">No repositories match your filters.</p>}</div>};

const evInfo=e=>{const p=e.payload,r=e.repo.name,ru='https://github.com/'+r;switch(e.type){
 case'PushEvent':{const b=(p.ref||'').replace('refs/heads/',''),n=p.size??p.commits?.length;return{i:'commit',c:'text-emerald-400',t:<>Pushed {n!=null?`${n} commit${n===1?'':'s'}`:'commits'} to <b>{b}</b> in</>,r,u:p.before&&p.head?`${ru}/compare/${p.before.slice(0,10)}...${p.head.slice(0,10)}`:ru,sub:(p.commits||[]).slice(0,3).map(c=>c.message.split('\n')[0])}}
 case'PullRequestEvent':return{i:'pr',c:'text-violet-400',t:<>{p.action==='closed'&&p.pull_request.merged?'Merged':p.action[0].toUpperCase()+p.action.slice(1)} pull request in</>,r,u:p.pull_request.html_url,sub:[p.pull_request.title]};
 case'IssuesEvent':return{i:'issue',c:'text-amber-400',t:<>{p.action[0].toUpperCase()+p.action.slice(1)} an issue in</>,r,u:p.issue.html_url,sub:[p.issue.title]};
 case'WatchEvent':return{i:'star',c:'text-yellow-400',t:<>Starred</>,r,u:ru};
 case'ReleaseEvent':return{i:'tag',c:'text-sky-400',t:<>Released in</>,r,u:p.release.html_url,sub:[p.release.name||p.release.tag_name]};
 case'CreateEvent':return{i:'plus',c:'text-teal-400',t:<>Created {p.ref_type}{p.ref?<> <b>{p.ref}</b></>:null} in</>,r,u:ru};
 default:return{i:'clock',c:'text-zinc-400',t:<>{e.type.replace(/Event$/,'')} in</>,r,u:ru}}};
const Feed=({ev})=>{const[n,setN]=useState(30);if(!ev.length)return <p className="text-center text-zinc-500 py-12">No recent public activity.</p>;
 return <div className="relative pl-2"><div className="absolute left-[19px] top-2 bottom-2 w-px bg-zinc-200 dark:bg-zinc-800"/>
 {ev.slice(0,n).map((e,i)=>{const x=evInfo(e);return <a key={e.id} href={x.u} target="_blank" rel="noopener" style={D(Math.min(i,10)*.5)} className="up relative flex gap-3 py-2.5 group">
  <span className={'relative z-10 w-8 h-8 shrink-0 rounded-full grid place-items-center bg-white dark:bg-zinc-900 border border-zinc-200 dark:border-zinc-800 group-hover:scale-110 transition '+x.c}><Icon n={x.i}/></span>
  <div className="min-w-0 flex-1"><div className="text-sm">{x.t} <span className="font-mono text-sky-500 group-hover:underline">{x.r}</span></div>
  {x.sub?.map((s,j)=><div key={j} className="text-xs text-zinc-500 truncate">› {s}</div>)}<div className="text-[11px] text-zinc-500 mt-0.5">{ago(e.created_at)}</div></div></a>})}
 {n<ev.length&&<button onClick={()=>setN(n+30)} className={PILL+' mt-2 ml-11'}>Show more</button>}</div>};

const Drawer=({r,close,api})=>{
 const[h,setH]=useState(null),[err,setErr]=useState('');
 useEffect(()=>{setH(null);setErr('');api(`/repos/${r.full_name}/readme`,'application/vnd.github.html+json').then(setH).catch(e=>setErr(e.status===404?'This repository has no README.':e.message))},[r]);
 const html=h&&h.replace(/src="(?!https?:|data:|\/\/)\.?\/?([^"]+)"/g,`src="https://raw.githubusercontent.com/${r.full_name}/${r.default_branch}/$1"`).replace(/<a /g,'<a target="_blank" rel="noopener" ');
 return <div className="fixed inset-0 z-50 flex justify-end" role="dialog" aria-modal="true" aria-label={r.name}>
  <div className="fade absolute inset-0 bg-black/50 backdrop-blur-sm" onClick={close}/>
  <div className="slide relative w-full max-w-2xl h-full overflow-y-auto bg-white dark:bg-zinc-950 border-l border-zinc-200 dark:border-zinc-800 p-6">
   <div className="flex items-start gap-3"><div className="flex-1 min-w-0"><h3 className="text-lg font-semibold text-sky-500 truncate">{r.full_name}</h3><p className="text-sm text-zinc-500 mt-1">{r.description||'No description'}</p></div>
    <a href={r.html_url} target="_blank" rel="noopener" aria-label="Open on GitHub" className="p-2 rounded-lg hover:bg-zinc-100 dark:hover:bg-zinc-800"><Icon n="ext"/></a><button onClick={close} aria-label="Close" className="p-2 rounded-lg hover:bg-zinc-100 dark:hover:bg-zinc-800 active:scale-90 transition"><Icon n="x"/></button></div>
   <div className="mt-3 flex flex-wrap gap-3 text-xs text-zinc-500">{r.language&&<span className="flex items-center gap-1"><span className="w-2.5 h-2.5 rounded-full" style={{background:lc(r.language)}}/>{r.language}</span>}<span>★ {r.stargazers_count}</span><span>⑂ {r.forks_count}</span><span>● {r.open_issues_count} issues</span><span>pushed {ago(r.pushed_at)}</span></div>
   {!!r.topics?.length&&<div className="mt-3 flex flex-wrap gap-1">{r.topics.map(t=><span key={t} className="text-[11px] px-2 py-0.5 rounded-full bg-sky-500/10 text-sky-500">{t}</span>)}</div>}
   <hr className="my-4 border-zinc-200 dark:border-zinc-800"/>
   {err?<p className="text-zinc-500 text-sm">{err}</p>:h===null?<div className="space-y-3">{[90,70,95,60,80,50].map((w,i)=><div key={i} className="sk h-4" style={{width:w+'%'}}/>)}</div>:<div className="md" dangerouslySetInnerHTML={{__html:html}}/>}
  </div></div>};

const TokenModal=({tok,save,close})=>{const[v,setV]=useState(tok);
 return <div className="fixed inset-0 z-50 grid place-items-center p-4" role="dialog" aria-modal="true"><div className="fade absolute inset-0 bg-black/50 backdrop-blur-sm" onClick={close}/>
  <div className="up relative w-full max-w-md rounded-2xl bg-white dark:bg-zinc-900 border border-zinc-200 dark:border-zinc-800 p-5">
   <h3 className="font-semibold flex items-center gap-2"><Icon n="key"/>GitHub token (optional)</h3>
   <p className="text-sm text-zinc-500 mt-2">Raises the API limit from 60 to 5,000 requests/hour. No scopes are needed for public data. The token is saved in this browser's localStorage and sent only to api.github.com.</p>
   <input type="password" autoFocus value={v} onChange={e=>setV(e.target.value)} placeholder="ghp_… or github_pat_…" className="mt-3 w-full rounded-lg border border-zinc-300 dark:border-zinc-700 bg-transparent px-3 py-2 text-sm font-mono outline-none focus:border-sky-500"/>
   <div className="mt-4 flex gap-2 justify-end">{tok&&<button onClick={()=>save('')} className="px-3 py-2 text-sm rounded-lg text-rose-500 hover:bg-rose-500/10">Remove</button>}<button onClick={close} className="px-3 py-2 text-sm rounded-lg hover:bg-zinc-100 dark:hover:bg-zinc-800">Cancel</button><button onClick={()=>save(v.trim())} className="px-4 py-2 text-sm rounded-lg bg-sky-500 text-white hover:bg-sky-400 active:scale-95 transition">Save</button></div>
  </div></div>};

const Countdown=({to})=>{const[t,setT]=useState(Date.now());useEffect(()=>{const i=setInterval(()=>setT(Date.now()),1000);return()=>clearInterval(i)},[]);const s=Math.max(0,Math.floor(to-t/1000));return <b className="tabular-nums">{Math.floor(s/60)}:{String(s%60).padStart(2,'0')}</b>};
const Skel=()=><div className="grid lg:grid-cols-[320px_1fr] gap-6"><div className={CARD+' space-y-3'}><div className="sk w-28 h-28 rounded-full"/><div className="sk h-5 w-2/3"/><div className="sk h-4 w-1/3"/><div className="sk h-16"/><div className="sk h-4"/><div className="sk h-4 w-5/6"/></div><div className="space-y-4"><div className="sk h-10 w-full sm:w-96"/><div className="grid grid-cols-2 md:grid-cols-4 gap-3">{[0,1,2,3].map(i=><div key={i} className="sk h-24"/>)}</div><div className="grid md:grid-cols-2 gap-4"><div className="sk h-56"/><div className="sk h-56"/></div><div className="sk h-40"/></div></div>;

function App(){
 const[theme,setTheme]=useState(ls.get('gh_theme','dark')),[tok,setTok]=useState(ls.get('gh_token','')),[showTok,setShowTok]=useState(false);
 const[input,setInput]=useState(''),[q,setQ]=useState(''),[status,setStatus]=useState('idle'),[err,setErr]=useState('');
 const[data,setData]=useState(null),[rl,setRl]=useState(null),[recents,setRecents]=useState(ls.get('gh_recent',[]));
 const[tab,setTab]=useState(0),[repo,setRepo]=useState(null),[copied,setCopied]=useState(false),[focus,setFocus]=useState(false);
 const rid=useRef(0),inp=useRef(),tk=useRef(tok);tk.current=tok;
 const api=useCallback(async(path,accept)=>{
  const r=await fetch('https://api.github.com'+path,{headers:{Accept:accept||'application/vnd.github+json',...(tk.current?{Authorization:'Bearer '+tk.current}:{})}});
  const h=r.headers.get('x-ratelimit-remaining');
  if(h!=null)setRl({rem:+h,lim:+r.headers.get('x-ratelimit-limit'),reset:+r.headers.get('x-ratelimit-reset')});
  if(!r.ok){const b=await r.json().catch(()=>({}));const e=new Error(b.message||r.statusText);e.status=r.status;e.rate=(r.status===403||r.status===429)&&(h==='0'||/rate limit/i.test(b.message||''));throw e}
  return accept?r.text():r.json()},[]);
 const addRecent=n=>setRecents(r=>{const x=[n,...r.filter(y=>y.toLowerCase()!==n.toLowerCase())].slice(0,8);ls.set('gh_recent',x);return x});
 const load=useCallback(async name=>{
  const id=++rid.current;setStatus('loading');setTab(0);
  try{const[u,repos,e1,e2]=await Promise.all([api(`/users/${name}`),api(`/users/${name}/repos?per_page=100&sort=updated`),api(`/users/${name}/events/public?per_page=100`),api(`/users/${name}/events/public?per_page=100&page=2`).catch(()=>[])]);
   if(id!==rid.current)return;setData({u,repos,ev:[...e1,...e2]});setStatus('ok');addRecent(u.login)}
  catch(e){if(id!==rid.current)return;setStatus(e.status===404?'404':e.rate?'rate':'err');setErr(e.message)}},[api]);
 useEffect(()=>{const go=()=>{const u=new URLSearchParams(location.search).get('user');if(u){setInput(u);setQ(u);load(u)}else{rid.current++;setQ('');setStatus('idle')}};go();addEventListener('popstate',go);api('/rate_limit').catch(()=>{});return()=>removeEventListener('popstate',go)},[]);
 useEffect(()=>{document.documentElement.classList.toggle('dark',theme==='dark');ls.set('gh_theme',theme)},[theme]);
 useEffect(()=>{const k=e=>{const t=document.activeElement.tagName;if((e.key==='/'&&!/INPUT|TEXTAREA|SELECT/.test(t))||((e.metaKey||e.ctrlKey)&&e.key.toLowerCase()==='k')){e.preventDefault();inp.current?.focus();inp.current?.select()}if(e.key==='Escape'){setRepo(null);setShowTok(false)}};addEventListener('keydown',k);return()=>removeEventListener('keydown',k)},[]);
 const search=name=>{name=name.trim().replace(/^@/,'');if(!name)return;try{history.pushState(null,'','?user='+encodeURIComponent(name))}catch{}setInput(name);setQ(name);load(name)};
 const saveTok=t=>{setTok(t);tk.current=t;ls.set('gh_token',t);setShowTok(false);api('/rate_limit').catch(()=>{});if(q&&status!=='ok')load(q)};
 const copy=()=>{navigator.clipboard?.writeText(location.href.split('?')[0]+'?user='+data.u.login);setCopied(true);setTimeout(()=>setCopied(false),1800)};
 const st=useMemo(()=>{if(!data)return null;const R=data.repos,sum=k=>R.reduce((a,r)=>a+(r[k]||0),0),L={};R.forEach(r=>r.language&&(L[r.language]=(L[r.language]||0)+1));
  return{stars:sum('stargazers_count'),forks:sum('forks_count'),watch:sum('watchers_count'),issues:sum('open_issues_count'),langs:Object.entries(L).sort((a,b)=>b[1]-a[1]),top:[...R].sort((a,b)=>b.stargazers_count-a.stargazers_count),mostStar:[...R].sort((a,b)=>b.stargazers_count-a.stargazers_count)[0],mostFork:[...R].sort((a,b)=>b.forks_count-a.forks_count)[0]}},[data]);
 const tabs=['Overview',`Repositories${data?' ('+data.repos.length+')':''}`,'Activity'];
 return <div className="max-w-7xl mx-auto px-4 py-5">
  <header className="flex items-center gap-3 flex-wrap">
   <div className="flex items-center gap-2 font-semibold"><span className="w-8 h-8 rounded-lg grid place-items-center bg-gradient-to-br from-sky-500 to-violet-500 text-white"><Icon n="commit"/></span>GitHub Explorer</div>
   <div className="ml-auto flex items-center gap-2">
    <button onClick={()=>setShowTok(true)} title="API rate limit — click to add a token" className="flex items-center gap-2 px-3 py-1.5 rounded-full text-xs border border-zinc-300 dark:border-zinc-800 hover:border-sky-500/60 active:scale-95 transition"><span className={'w-2 h-2 rounded-full '+(!rl?'bg-zinc-500':rl.rem<=5?'bg-rose-500':rl.rem<rl.lim*.25?'bg-amber-400':'bg-emerald-400')}/><span className="tabular-nums">{rl?`${rl.rem}/${rl.lim}`:'API'}</span><Icon n="key" c="w-3.5 h-3.5"/>{tok&&<span className="text-emerald-400">token</span>}</button>
    <button onClick={()=>setTheme(theme==='dark'?'light':'dark')} aria-label="Toggle theme" className="p-2 rounded-full border border-zinc-300 dark:border-zinc-800 hover:border-sky-500/60 active:scale-90 transition"><Icon n={theme==='dark'?'sun':'moon'}/></button></div>
  </header>
  <form onSubmit={e=>{e.preventDefault();search(input)}} className="mt-5 max-w-2xl mx-auto">
   <div className={'relative flex items-center rounded-xl border bg-white dark:bg-zinc-900 transition '+(focus?'border-sky-500 ring-4 ring-sky-500/15':'border-zinc-300 dark:border-zinc-800')}>
    <span className="pl-3 text-zinc-500"><Icon n="search"/></span>
    <input ref={inp} value={input} onChange={e=>setInput(e.target.value)} onFocus={()=>setFocus(true)} onBlur={()=>setFocus(false)} aria-label="GitHub username" placeholder="Search a GitHub username…" className="flex-1 bg-transparent px-3 py-3 outline-none text-sm" autoComplete="off" spellCheck="false"/>
    {status==='loading'?<span className="mr-3 w-4 h-4 rounded-full border-2 border-sky-500 border-t-transparent animate-spin"/>:input?<button type="button" onClick={()=>{setInput('');inp.current.focus()}} aria-label="Clear" className="mr-2 p-1 rounded hover:bg-zinc-100 dark:hover:bg-zinc-800"><Icon n="x"/></button>:<kbd className="mr-3 hidden sm:block text-[11px] px-1.5 py-0.5 rounded border border-zinc-300 dark:border-zinc-700 text-zinc-500">/</kbd>}
   </div></form>
  <div className="mt-3 flex flex-wrap justify-center gap-2">{QUICK.map(n=><button key={n} onClick={()=>search(n)} className={PILL}>@{n}</button>)}</div>
  {!!recents.length&&<div className="mt-2 flex flex-wrap justify-center items-center gap-2 text-xs text-zinc-500"><Icon n="clock" c="w-3.5 h-3.5"/>Recent:{recents.map(n=><span key={n} className={PILL+' flex items-center gap-1 !py-0.5'}><button onClick={()=>search(n)}>{n}</button><button aria-label={'Remove '+n} onClick={()=>setRecents(r=>{const x=r.filter(y=>y!==n);ls.set('gh_recent',x);return x})} className="hover:text-rose-500"><Icon n="x" c="w-3 h-3"/></button></span>)}<button onClick={()=>{setRecents([]);ls.set('gh_recent',[])}} className="underline hover:text-rose-500">clear all</button></div>}
  <main className="mt-8">
   {status==='idle'&&<div className="up text-center py-16"><h1 className="text-4xl sm:text-5xl font-bold tracking-tight bg-gradient-to-r from-sky-400 to-violet-500 bg-clip-text text-transparent">Explore any GitHub profile</h1><p className="mt-4 text-zinc-500 max-w-lg mx-auto">Stats, languages, repositories, README previews and a live activity heatmap — straight from the public GitHub API. Press <kbd className="px-1.5 border rounded border-zinc-400">/</kbd> to search.</p></div>}
   {status==='loading'&&<Skel/>}
   {status==='404'&&<div className="up text-center py-14"><svg viewBox="0 0 120 120" className="w-32 h-32 mx-auto text-zinc-400"><circle cx="52" cy="52" r="30" fill="none" stroke="currentColor" strokeWidth="6"/><path d="M75 75l28 28" stroke="currentColor" strokeWidth="8" strokeLinecap="round"/><path d="M44 44c0-6 16-6 16 2 0 6-8 6-8 12M52 66v.5" fill="none" stroke="#38bdf8" strokeWidth="5" strokeLinecap="round"/></svg><h2 className="mt-4 text-xl font-semibold">No user named @{q}</h2><p className="text-zinc-500 mt-1">Check the spelling, or try one of these:</p><div className="mt-4 flex flex-wrap justify-center gap-2">{QUICK.map(n=><button key={n} onClick={()=>search(n)} className={PILL}>@{n}</button>)}</div></div>}
   {status==='rate'&&<div className="up rounded-xl border border-amber-500/40 bg-amber-500/10 p-5 flex items-center gap-4 flex-wrap"><Icon n="alert" c="w-6 h-6 text-amber-400"/><div className="flex-1 min-w-[220px]"><div className="font-semibold">GitHub API rate limit reached</div><div className="text-sm text-zinc-500">Resets in {rl?<Countdown to={rl.reset}/>:'about an hour'}. Add a personal access token to raise the limit to 5,000/hour.</div></div><button onClick={()=>setShowTok(true)} className="px-4 py-2 rounded-lg bg-amber-500 text-black text-sm font-medium hover:bg-amber-400 active:scale-95 transition">Add token</button><button onClick={()=>load(q)} className="px-4 py-2 rounded-lg border border-zinc-400/40 text-sm hover:bg-zinc-500/10">Retry</button></div>}
   {status==='err'&&<div className="up rounded-xl border border-rose-500/40 bg-rose-500/10 p-5 flex items-center gap-4"><Icon n="alert" c="w-6 h-6 text-rose-400"/><div className="flex-1"><div className="font-semibold">Something went wrong</div><div className="text-sm text-zinc-500">{err}</div></div><button onClick={()=>load(q)} className="px-4 py-2 rounded-lg border border-zinc-400/40 text-sm hover:bg-zinc-500/10">Retry</button></div>}
   {status==='ok'&&data&&<div key={q} className="grid lg:grid-cols-[320px_1fr] gap-6">
    <Profile u={data.u} onCopy={copy} copied={copied}/>
    <section className="min-w-0">
     <div role="tablist" className="relative grid grid-cols-3 p-1 rounded-xl bg-zinc-200/70 dark:bg-zinc-900 w-full sm:w-[28rem] mb-5">
      <span className="absolute top-1 bottom-1 left-1 w-[calc((100%-0.5rem)/3)] rounded-lg bg-white dark:bg-zinc-800 shadow transition-transform duration-300 ease-[cubic-bezier(.34,1.56,.64,1)]" style={{transform:`translateX(${tab*100}%)`}}/>
      {tabs.map((t,i)=><button key={t} role="tab" aria-selected={tab===i} onClick={()=>setTab(i)} className={'relative z-10 py-1.5 text-sm truncate transition '+(tab===i?'font-medium':'text-zinc-500')}>{t}</button>)}</div>
     <div key={tab} className="fade">
      {tab===0&&<Overview st={st} ev={data.ev} open={setRepo}/>}
      {tab===1&&<Repos repos={data.repos} open={setRepo}/>}
      {tab===2&&<Feed ev={data.ev}/>}</div>
    </section></div>}
  </main>
  {repo&&<Drawer r={repo} close={()=>setRepo(null)} api={api}/>}
  {showTok&&<TokenModal tok={tok} save={saveTok} close={()=>setShowTok(false)}/>}
 </div>}
ReactDOM.createRoot(document.getElementById('root')).render(<App/>);
</script>
</body>
</html>
