<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Écosystème Innovation — Lyon · Saint-Étienne</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.css"/>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet.markercluster/1.5.3/MarkerCluster.css"/>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet.markercluster/1.5.3/MarkerCluster.Default.css"/>
<style>
:root{
  --bg:#f0f2f5;--bg2:#ffffff;--bg3:#e8eaed;
  --border:rgba(0,0,0,0.1);--text:#1a202c;--muted:#718096;--accent:#1a56db;
  --sh:0 1px 3px rgba(0,0,0,0.08);--shh:0 6px 20px rgba(0,0,0,0.12);
  --sociale:#0d9488;--sociale-bg:rgba(13,148,136,.1);
  --techno:#1a56db;--techno-bg:rgba(26,86,219,.1);
  --eco:#c2410c;--eco-bg:rgba(194,65,12,.1);
  --ouverte:#6d28d9;--ouverte-bg:rgba(109,40,217,.1);
  --mixte:#b45309;--mixte-bg:rgba(180,83,9,.1);
  --univ:#1a56db;--labo:#0d9488;--instit:#c2410c;
  --entreprise:#6d28d9;--sante:#b91c1c;--collectivite:#b45309;
  --satt:#15803d;--pole:#be185d;
}
*{box-sizing:border-box;margin:0;padding:0;}
body{background:var(--bg);color:var(--text);font-family:'DM Sans',sans-serif;display:flex;flex-direction:column;height:100vh;overflow:hidden;}

/* HEADER */
header{background:var(--bg2);border-bottom:1px solid var(--border);padding:12px 28px;display:flex;align-items:center;gap:16px;box-shadow:var(--sh);flex-shrink:0;}
.ht{flex:1;}
.ht h1{font-family:'Syne',sans-serif;font-weight:800;font-size:1.1rem;letter-spacing:-.02em;color:var(--text);}
.ht h1 span{color:var(--accent);}
.ht p{font-size:.64rem;color:var(--muted);letter-spacing:.07em;text-transform:uppercase;margin-top:1px;}
.hst{display:flex;gap:20px;}
.sp{text-align:center;}
.sp .n{font-family:'Syne',sans-serif;font-weight:800;font-size:1.1rem;color:var(--accent);line-height:1;}
.sp .l{font-size:.6rem;color:var(--muted);text-transform:uppercase;letter-spacing:.05em;}

/* TABS */
.tabs{background:var(--bg2);border-bottom:1px solid var(--border);padding:0 28px;display:flex;flex-shrink:0;}
.tbtn{padding:10px 16px;background:none;border:none;border-bottom:2px solid transparent;color:var(--muted);font-family:'DM Sans',sans-serif;font-size:.83rem;cursor:pointer;transition:all .18s;white-space:nowrap;}
.tbtn:hover{color:var(--text);}
.tbtn.active{color:var(--accent);border-bottom-color:var(--accent);font-weight:500;}

/* LEGEND BAR */
.legbar{background:var(--bg2);border-bottom:1px solid var(--border);padding:7px 28px;display:flex;flex-wrap:wrap;gap:4px;align-items:center;flex-shrink:0;}
.ll{font-size:.67rem;color:var(--muted);text-transform:uppercase;letter-spacing:.07em;margin-right:2px;}
.li{display:inline-flex;align-items:center;gap:4px;font-size:.71rem;color:var(--muted);cursor:pointer;padding:2px 8px;border-radius:20px;border:1px solid transparent;transition:all .15s;}
.li:hover{background:var(--bg3);color:var(--text);}
.li.active{border-color:var(--ic,#aaa);color:var(--text);background:var(--bg3);}
.ld{width:7px;height:7px;border-radius:50%;flex-shrink:0;}

/* CONTROLS */
.ctrlbar{background:var(--bg2);border-bottom:1px solid var(--border);padding:8px 28px;display:flex;flex-wrap:wrap;gap:7px;align-items:center;flex-shrink:0;}
.sw{position:relative;flex:1;min-width:140px;max-width:240px;}
.sw svg{position:absolute;left:9px;top:50%;transform:translateY(-50%);opacity:.3;pointer-events:none;}
.sw input{width:100%;padding:6px 9px 6px 28px;background:var(--bg3);border:1px solid var(--border);border-radius:8px;color:var(--text);font-family:'DM Sans',sans-serif;font-size:.8rem;outline:none;transition:border .2s;}
.sw input:focus{border-color:var(--accent);}
.sw input::placeholder{color:var(--muted);}
select{padding:6px 24px 6px 9px;background:var(--bg3);border:1px solid var(--border);border-radius:8px;color:var(--text);font-family:'DM Sans',sans-serif;font-size:.8rem;outline:none;cursor:pointer;appearance:none;background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='11' height='11' viewBox='0 0 12 12'%3E%3Cpath fill='%23718096' d='M6 8L1 3h10z'/%3E%3C/svg%3E");background-repeat:no-repeat;background-position:right 7px center;background-color:var(--bg3);}
.rc{margin-left:auto;font-size:.73rem;color:var(--muted);white-space:nowrap;}
.rc span{color:var(--accent);font-weight:600;}
.rbtn{padding:6px 10px;background:none;border:1px solid var(--border);border-radius:8px;color:var(--muted);font-family:'DM Sans',sans-serif;font-size:.75rem;cursor:pointer;transition:all .18s;}
.rbtn:hover{border-color:var(--accent);color:var(--accent);}

/* CONTENT */
.content{flex:1;overflow-y:auto;padding:18px 28px;min-height:0;}
.tp{display:none;}
.tp.active{display:block;}
#panel-carte.active{display:flex;flex-direction:column;padding:0;overflow:hidden;height:100%;}

/* CARDS */
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(270px,1fr));gap:10px;}
@keyframes fi{from{opacity:0;transform:translateY(4px);}to{opacity:1;transform:translateY(0);}}
.card{background:var(--bg2);border:1px solid var(--border);border-radius:11px;padding:14px 14px 14px 17px;cursor:pointer;transition:all .18s;position:relative;overflow:hidden;box-shadow:var(--sh);animation:fi .3s ease both;}
.card::before{content:'';position:absolute;top:0;left:0;width:3px;height:100%;background:var(--ca,#ccc);}
.card:hover{box-shadow:var(--shh);transform:translateY(-1px);}
.card.hidden{display:none;}
.ch{display:flex;justify-content:space-between;align-items:flex-start;gap:7px;margin-bottom:7px;}
.cn{font-family:'Syne',sans-serif;font-weight:700;font-size:.88rem;line-height:1.3;color:var(--text);}
.badge{display:inline-flex;align-items:center;padding:2px 7px;border-radius:20px;font-size:.63rem;font-weight:600;white-space:nowrap;flex-shrink:0;}
.cm{display:flex;flex-direction:column;gap:3px;}
.mr{display:flex;align-items:flex-start;gap:5px;font-size:.73rem;color:var(--muted);}
.mr strong{color:var(--text);opacity:.55;font-weight:500;min-width:46px;flex-shrink:0;}
.mr .v{color:var(--text);}
.tw{margin-top:9px;}
.tl{display:flex;justify-content:space-between;font-size:.66rem;color:var(--muted);margin-bottom:3px;}
.tl span:last-child{color:var(--accent);font-weight:600;}
.tb{height:4px;background:var(--bg3);border-radius:4px;overflow:hidden;}
.tf{height:100%;border-radius:4px;}
.es{display:none;flex-direction:column;align-items:center;justify-content:center;padding:50px 20px;color:var(--muted);gap:8px;font-size:.88rem;}
.es.v{display:flex;}

/* BADGE COLORS */
.bs{background:var(--sociale-bg);color:var(--sociale);}
.bt{background:var(--techno-bg);color:var(--techno);}
.be{background:var(--eco-bg);color:var(--eco);}
.bo{background:var(--ouverte-bg);color:var(--ouverte);}
.bm{background:var(--mixte-bg);color:var(--mixte);}
.bu{background:rgba(26,86,219,.1);color:var(--univ);}
.bl{background:rgba(13,148,136,.1);color:var(--labo);}
.bi{background:rgba(194,65,12,.1);color:var(--instit);}
.bep{background:rgba(109,40,217,.1);color:var(--entreprise);}
.bsa{background:rgba(185,28,28,.1);color:var(--sante);}
.bc{background:rgba(180,83,9,.1);color:var(--collectivite);}
.btr{background:rgba(21,128,61,.1);color:var(--satt);}

/* MAP CONTAINER */
.mcw{position:relative;flex:1;display:flex;min-height:0;}
#map{flex:1;width:100%;min-height:0;z-index:0;}

/* MAP CONTROLS OVERLAY */
.mcp{position:absolute;top:12px;left:12px;z-index:800;display:flex;flex-direction:column;gap:7px;max-width:320px;pointer-events:none;}
.mcp>*{pointer-events:auto;}
.msw{position:relative;}
.msw svg{position:absolute;left:9px;top:50%;transform:translateY(-50%);opacity:.4;pointer-events:none;}
.minput{background:var(--bg2);border:1px solid var(--border);border-radius:9px;padding:7px 9px 7px 29px;font-family:'DM Sans',sans-serif;font-size:.8rem;color:var(--text);outline:none;width:220px;box-shadow:0 2px 8px rgba(0,0,0,.12);}
.minput::placeholder{color:var(--muted);}
.mpills{display:flex;flex-wrap:wrap;gap:4px;}
.pill{background:var(--bg2);border:1px solid var(--border);border-radius:20px;padding:3px 9px;font-size:.69rem;color:var(--muted);cursor:pointer;transition:all .15s;white-space:nowrap;display:inline-flex;align-items:center;gap:3px;box-shadow:0 1px 4px rgba(0,0,0,.1);}
.pill:hover{color:var(--text);}
.pill.active{border-color:var(--pc,var(--accent));color:var(--pc,var(--accent));font-weight:600;background:white;box-shadow:0 2px 8px rgba(0,0,0,.12);}
.mlegbox{background:var(--bg2);border:1px solid var(--border);border-radius:9px;padding:9px 11px;display:flex;flex-direction:column;gap:4px;box-shadow:0 2px 8px rgba(0,0,0,.1);}
.mlegtitle{font-family:'Syne',sans-serif;font-weight:700;font-size:.65rem;color:var(--muted);text-transform:uppercase;letter-spacing:.07em;margin-bottom:1px;}
.mlegrow{display:flex;align-items:center;gap:6px;font-size:.69rem;color:var(--muted);}

/* SIDEBAR */
.sb-panel{position:absolute;top:0;right:0;width:290px;height:100%;background:var(--bg2);border-left:1px solid var(--border);z-index:900;overflow-y:auto;transform:translateX(100%);transition:transform .25s ease;display:flex;flex-direction:column;box-shadow:-4px 0 20px rgba(0,0,0,.1);}
.sb-panel.open{transform:translateX(0);}
.sbhdr{padding:13px 14px;border-bottom:1px solid var(--border);display:flex;justify-content:space-between;align-items:flex-start;gap:7px;position:sticky;top:0;background:var(--bg2);z-index:1;}
.sbtitle{font-family:'Syne',sans-serif;font-weight:700;font-size:.9rem;line-height:1.3;color:var(--text);}
.sbclose{background:var(--bg3);border:none;color:var(--muted);border-radius:6px;width:24px;height:24px;cursor:pointer;font-size:.95rem;display:flex;align-items:center;justify-content:center;flex-shrink:0;}
.sbclose:hover{color:var(--text);}
.sbbody{padding:12px;display:flex;flex-direction:column;gap:6px;}
.sf{background:var(--bg3);border-radius:7px;padding:8px 10px;}
.fl{font-size:.6rem;color:var(--muted);text-transform:uppercase;letter-spacing:.08em;margin-bottom:2px;}
.fv{font-size:.8rem;color:var(--text);line-height:1.4;}
.fv a{color:var(--accent);text-decoration:none;}
.fv a:hover{text-decoration:underline;}
.contact-row{display:flex;align-items:flex-start;gap:6px;font-size:.77rem;color:var(--muted);margin-top:2px;}
.contact-row svg{flex-shrink:0;margin-top:1px;color:var(--accent);}
.contact-row a{color:var(--accent);text-decoration:none;}
.contact-row a:hover{text-decoration:underline;}

/* POPUP */
.leaflet-popup-content-wrapper{background:var(--bg2)!important;color:var(--text)!important;border:1px solid rgba(0,0,0,.12)!important;border-radius:10px!important;box-shadow:0 4px 20px rgba(0,0,0,.15)!important;padding:0!important;}
.leaflet-popup-content{margin:0!important;padding:12px 13px!important;font-family:'DM Sans',sans-serif;font-size:.78rem;min-width:175px;max-width:240px;}
.leaflet-popup-tip-container{margin-top:-1px;}
.leaflet-popup-tip{background:var(--bg2)!important;}
.pname{font-family:'Syne',sans-serif;font-weight:700;font-size:.87rem;display:block;margin-bottom:1px;color:var(--text);}
.ptype{color:var(--muted);font-size:.66rem;display:block;margin-bottom:6px;}
.pcontact{display:flex;flex-direction:column;gap:2px;margin-bottom:7px;}
.prow{display:flex;align-items:center;gap:5px;font-size:.7rem;color:var(--muted);line-height:1.4;}
.prow a{color:var(--accent);text-decoration:none;}
.pbtn{background:var(--accent);color:white;border:none;border-radius:6px;padding:5px 10px;font-size:.72rem;cursor:pointer;width:100%;font-family:'DM Sans',sans-serif;font-weight:500;transition:background .15s;}
.pbtn:hover{background:#1447b3;}

/* LEAFLET */
.leaflet-control-zoom a{background:var(--bg2)!important;color:var(--text)!important;border-color:rgba(0,0,0,.12)!important;box-shadow:0 1px 5px rgba(0,0,0,.15)!important;}
.leaflet-control-zoom a:hover{background:var(--bg3)!important;}
.leaflet-control-attribution{font-size:.6rem!important;background:rgba(255,255,255,.8)!important;}
.marker-cluster-small,.marker-cluster-medium,.marker-cluster-large{background:rgba(26,86,219,.18)!important;}
.marker-cluster-small div,.marker-cluster-medium div,.marker-cluster-large div{background:rgba(26,86,219,.65)!important;color:white!important;font-weight:700;font-family:'Syne',sans-serif;}

/* MODAL */
.overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.4);backdrop-filter:blur(3px);z-index:2000;align-items:center;justify-content:center;padding:20px;}
.overlay.open{display:flex;}
.modal{background:var(--bg2);border:1px solid var(--border);border-radius:14px;padding:22px;max-width:480px;width:100%;position:relative;animation:mIn .18s ease;box-shadow:0 20px 60px rgba(0,0,0,.18);}
@keyframes mIn{from{opacity:0;transform:scale(.95);}to{opacity:1;transform:scale(1);}}
.mcl{position:absolute;top:12px;right:12px;background:var(--bg3);border:none;border-radius:7px;color:var(--muted);width:28px;height:28px;cursor:pointer;font-size:1rem;display:flex;align-items:center;justify-content:center;}
.mcl:hover{color:var(--text);}
.mt{font-family:'Syne',sans-serif;font-weight:800;font-size:1.08rem;margin-bottom:3px;padding-right:34px;line-height:1.3;color:var(--text);}
.ms{font-size:.74rem;color:var(--muted);margin-bottom:11px;}
.mb{display:flex;flex-wrap:wrap;gap:4px;margin-bottom:11px;}
.mg{display:grid;gap:5px;}
.mf{background:var(--bg3);border-radius:7px;padding:8px 10px;}
.mf .fl{font-size:.6rem;color:var(--muted);text-transform:uppercase;letter-spacing:.08em;margin-bottom:2px;}
.mf .fv{font-size:.82rem;color:var(--text);}
.mf .fv a{color:var(--accent);text-decoration:none;}
.mf .fv a:hover{text-decoration:underline;}
.mtrwrap{margin-top:12px;}
.mtrh{display:flex;justify-content:space-between;font-size:.72rem;color:var(--muted);margin-bottom:4px;}
.mtrh strong{color:var(--accent);font-size:.86rem;}
.mtrbar{height:6px;background:var(--bg3);border-radius:6px;overflow:hidden;}
.mtrfill{height:100%;border-radius:6px;}
.trscale{display:flex;justify-content:space-between;margin-top:2px;}
.trscale span{font-size:.57rem;color:var(--muted);}

::-webkit-scrollbar{width:4px;height:4px;}
::-webkit-scrollbar-track{background:transparent;}
::-webkit-scrollbar-thumb{background:#d1d5db;border-radius:4px;}

@media(max-width:640px){
  header,.tabs,.legbar,.ctrlbar,.content{padding-left:14px;padding-right:14px;}
  .hst{display:none;}
  .grid{grid-template-columns:1fr;}
  .sb-panel{width:100%;height:55%;top:auto;bottom:0;transform:translateY(100%);border-left:none;border-top:1px solid var(--border);}
  .sb-panel.open{transform:translateY(0);}
  .mcp{max-width:calc(100% - 24px);}
}
</style>
</head>
<body>

<header>
  <div class="ht">
    <h1>Écosystème Innovation <span>Lyon · Saint-Étienne</span></h1>
    <p>Cartographie interactive des acteurs et dispositifs</p>
  </div>
  <div class="hst">
    <div class="sp"><div class="n">20</div><div class="l">Dispositifs</div></div>
    <div class="sp"><div class="n">120+</div><div class="l">Acteurs</div></div>
    <div class="sp"><div class="n">2</div><div class="l">Métropoles</div></div>
  </div>
</header>

<!-- TABS — carte en premier -->
<div class="tabs">
  <button class="tbtn active" onclick="switchTab('carte',this)">🗺 Carte interactive</button>
  <button class="tbtn" onclick="switchTab('dispositifs',this)">🚀 Dispositifs</button>
  <button class="tbtn" onclick="switchTab('acteurs',this)">🏛 Acteurs</button>
</div>

<!-- LEGENDS -->
<div id="leg-carte" class="legbar">
  <span class="ll">Filtrer :</span>
  <div class="li active" style="--ic:var(--accent)" onclick="legMap('all',this)"><div class="ld" style="background:#aaa"></div>Tous</div>
  <div class="li" style="--ic:var(--mixte)" onclick="legMap('dispositif',this)"><div class="ld" style="background:var(--mixte)"></div>Dispositifs</div>
  <div class="li" style="--ic:var(--univ)" onclick="legMap('université',this)"><div class="ld" style="background:var(--univ)"></div>Universités</div>
  <div class="li" style="--ic:var(--labo)" onclick="legMap('laboratoire',this)"><div class="ld" style="background:var(--labo)"></div>Laboratoires</div>
  <div class="li" style="--ic:var(--sante)" onclick="legMap('santé',this)"><div class="ld" style="background:var(--sante)"></div>Santé</div>
  <div class="li" style="--ic:var(--instit)" onclick="legMap('institution',this)"><div class="ld" style="background:var(--instit)"></div>Institutions</div>
  <div class="li" style="--ic:var(--entreprise)" onclick="legMap('entreprise',this)"><div class="ld" style="background:var(--entreprise)"></div>Entreprises</div>
  <div class="li" style="--ic:var(--collectivite)" onclick="legMap('collectivité',this)"><div class="ld" style="background:var(--collectivite)"></div>Collectivités</div>
  <div class="li" style="--ic:var(--satt)" onclick="legMap('transfert',this)"><div class="ld" style="background:var(--satt)"></div>Transfert/Pôle</div>
</div>
<div id="leg-disp" class="legbar" style="display:none">
  <span class="ll">Type :</span>
  <div class="li active" style="--ic:var(--accent)" onclick="legD('all',this)"><div class="ld" style="background:#aaa"></div>Tous</div>
  <div class="li" style="--ic:var(--sociale)" onclick="legD('sociale',this)"><div class="ld" style="background:var(--sociale)"></div>Sociale</div>
  <div class="li" style="--ic:var(--techno)" onclick="legD('techno',this)"><div class="ld" style="background:var(--techno)"></div>Technologique</div>
  <div class="li" style="--ic:var(--eco)" onclick="legD('eco',this)"><div class="ld" style="background:var(--eco)"></div>Économique</div>
  <div class="li" style="--ic:var(--ouverte)" onclick="legD('ouverte',this)"><div class="ld" style="background:var(--ouverte)"></div>Ouverte</div>
</div>
<div id="leg-act" class="legbar" style="display:none">
  <span class="ll">Type :</span>
  <div class="li active" style="--ic:var(--accent)" onclick="legA('all',this)"><div class="ld" style="background:#aaa"></div>Tous</div>
  <div class="li" style="--ic:var(--univ)" onclick="legA('université',this)"><div class="ld" style="background:var(--univ)"></div>Universités</div>
  <div class="li" style="--ic:var(--labo)" onclick="legA('laboratoire',this)"><div class="ld" style="background:var(--labo)"></div>Laboratoires</div>
  <div class="li" style="--ic:var(--instit)" onclick="legA('institution',this)"><div class="ld" style="background:var(--instit)"></div>Institutions</div>
  <div class="li" style="--ic:var(--entreprise)" onclick="legA('entreprise',this)"><div class="ld" style="background:var(--entreprise)"></div>Entreprises</div>
  <div class="li" style="--ic:var(--sante)" onclick="legA('santé',this)"><div class="ld" style="background:var(--sante)"></div>Santé</div>
  <div class="li" style="--ic:var(--collectivite)" onclick="legA('collectivité',this)"><div class="ld" style="background:var(--collectivite)"></div>Collectivités</div>
  <div class="li" style="--ic:var(--satt)" onclick="legA('transfert',this)"><div class="ld" style="background:var(--satt)"></div>Transfert</div>
</div>
<div id="ctrl-disp" class="ctrlbar" style="display:none">
  <div class="sw"><svg width="13" height="13" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35"/></svg><input id="sd" placeholder="Rechercher…" oninput="applyD()"></div>
  <select id="fport" onchange="applyD()"><option value="">Tous porteurs</option><option>ComUE</option><option>DGE</option><option>INCa</option><option>HCL</option><option>Pulsalys</option><option>IEP de Lyon</option><option>Université Jean-Monnet</option><option>Saint-Etienne métropole</option><option>Métropole de Saint-Etienne</option><option>IAE Lyon</option><option>Laboratoire ICAR</option><option>INSERM-CNRS-Lyon 1</option></select>
  <select id="fmat" onchange="applyD()"><option value="">Toute maturité</option><option value="low">Faible (1–3)</option><option value="mid">Moyenne (4–6)</option><option value="high">Haute (7–9)</option></select>
  <button class="rbtn" onclick="resetD()">✕ Reset</button>
  <div class="rc">Affichage : <span id="cntd">20</span></div>
</div>
<div id="ctrl-act" class="ctrlbar" style="display:none">
  <div class="sw"><svg width="13" height="13" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35"/></svg><input id="sa" placeholder="Rechercher…" oninput="applyA()"></div>
  <select id="ftype" onchange="applyA()"><option value="">Tous types</option><option>Université</option><option>Laboratoire</option><option>Institution</option><option>Collectivité</option><option>CHU / Hôpital</option><option>SATT / Transfert</option><option>Entreprise</option><option>Pôle</option></select>
  <button class="rbtn" onclick="resetA()">✕ Reset</button>
  <div class="rc">Affichage : <span id="cnta">0</span></div>
</div>

<!-- MAIN CONTENT -->
<div class="content" id="mc">

  <!-- === CARTE (premier onglet) === -->
  <div class="tp active" id="panel-carte">
    <div class="mcw">
      <div id="map"></div>
      <!-- overlay controls -->
      <div class="mcp">
        <div class="msw">
          <svg width="13" height="13" fill="none" viewBox="0 0 24 24" stroke="#718096" stroke-width="2"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35"/></svg>
          <input class="minput" id="mq" placeholder="Rechercher sur la carte…" oninput="filterMarkers()">
        </div>
        <div class="mpills">
          <div class="pill active" style="--pc:var(--accent)" onclick="pillClick('all',this)">● Tous</div>
          <div class="pill" style="--pc:var(--mixte)" onclick="pillClick('dispositif',this)">🚀 Dispositifs</div>
          <div class="pill" style="--pc:var(--univ)" onclick="pillClick('université',this)">🎓 Universités</div>
          <div class="pill" style="--pc:var(--labo)" onclick="pillClick('laboratoire',this)">🔬 Labos</div>
          <div class="pill" style="--pc:var(--sante)" onclick="pillClick('santé',this)">🏥 Santé</div>
          <div class="pill" style="--pc:var(--instit)" onclick="pillClick('institution',this)">🏛 Institutions</div>
          <div class="pill" style="--pc:var(--entreprise)" onclick="pillClick('entreprise',this)">💡 Entreprises</div>
          <div class="pill" style="--pc:var(--collectivite)" onclick="pillClick('collectivité',this)">🏙 Collectivités</div>
          <div class="pill" style="--pc:var(--satt)" onclick="pillClick('transfert',this)">🔗 Transfert</div>
        </div>
        <div class="mlegbox">
          <div class="mlegtitle">Légende</div>
          <div class="mlegrow"><div style="width:10px;height:10px;border-radius:2px;background:var(--mixte);flex-shrink:0"></div>Dispositif d'innovation</div>
          <div class="mlegrow"><div class="ld" style="background:var(--univ)"></div>Université / Grande école</div>
          <div class="mlegrow"><div class="ld" style="background:var(--labo)"></div>Laboratoire de recherche</div>
          <div class="mlegrow"><div class="ld" style="background:var(--sante)"></div>Établissement de santé</div>
          <div class="mlegrow"><div class="ld" style="background:var(--instit)"></div>Institution publique</div>
          <div class="mlegrow"><div class="ld" style="background:var(--entreprise)"></div>Entreprise</div>
          <div class="mlegrow"><div class="ld" style="background:var(--collectivite)"></div>Collectivité</div>
          <div class="mlegrow"><div class="ld" style="background:var(--satt)"></div>Transfert / Pôle</div>
        </div>
      </div>
      <!-- sliding sidebar -->
      <div class="sb-panel" id="sbpanel">
        <div class="sbhdr">
          <div class="sbtitle" id="sbtitle">—</div>
          <button class="sbclose" onclick="closeSb()">×</button>
        </div>
        <div class="sbbody" id="sbbody"></div>
      </div>
    </div>
  </div>

  <!-- === DISPOSITIFS === -->
  <div class="tp" id="panel-dispositifs">
    <div class="grid" id="grid-d"></div>
    <div class="es" id="empty-d">Aucun résultat</div>
  </div>

  <!-- === ACTEURS === -->
  <div class="tp" id="panel-acteurs">
    <div class="grid" id="grid-a"></div>
    <div class="es" id="empty-a">Aucun résultat</div>
  </div>
</div>

<!-- MODAL -->
<div class="overlay" id="overlay" onclick="closeModal(event)">
  <div class="modal">
    <button class="mcl" onclick="closeModal()">×</button>
    <div id="modalbody"></div>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet.markercluster/1.5.3/leaflet.markercluster.min.js"></script>
<script>
/* ════════════════════════════════════
   CONTACTS (adresse, tél, web, email)
   Sources : sites officiels, annuaires publics
   ════════════════════════════════════ */
const CONTACTS = {
  "Université Claude Bernard Lyon 1":    {adresse:"43 Bd du 11 Novembre 1918, 69622 Villeurbanne",tel:"04 72 44 80 00",web:"www.univ-lyon1.fr"},
  "Université Lumière Lyon 2":           {adresse:"86 Rue Pasteur, 69365 Lyon Cedex 07",tel:"04 78 69 70 00",web:"www.univ-lyon2.fr"},
  "Sciences Po Lyon":                    {adresse:"14 Avenue Berthelot, 69007 Lyon",tel:"04 37 28 38 00",web:"www.sciencespo-lyon.fr",email:"contact@sciencespo-lyon.fr"},
  "Université Jean Monnet":              {adresse:"10 Rue Tréfilerie, 42023 Saint-Étienne Cedex 2",tel:"04 77 42 17 00",web:"www.univ-st-etienne.fr"},
  "COMUE - Université de Lyon":          {adresse:"92 Rue Pasteur, 69007 Lyon",tel:"04 37 37 27 00",web:"www.universite-lyon.fr"},
  "Hospices Civils de Lyon":             {adresse:"3 Quai des Célestins, 69229 Lyon Cedex 02",tel:"04 72 40 72 40",web:"www.chu-lyon.fr"},
  "Centre Léon Bérard":                  {adresse:"28 Rue Laennec, 69373 Lyon Cedex 08",tel:"04 78 78 26 26",web:"www.centreleonberard.fr"},
  "Pulsalys":                            {adresse:"47 Bd du 11 Novembre 1918, 69625 Villeurbanne",tel:"04 26 23 56 60",web:"www.pulsalys.fr",email:"contact@pulsalys.fr"},
  "CNRS - Délégation Rhône Auvergne":    {adresse:"2 Av. Albert Einstein, 69626 Villeurbanne",tel:"04 72 43 13 50",web:"www.rhone-auvergne.cnrs.fr"},
  "Pôle AXELERA":                        {adresse:"Tour Part-Dieu, 129 Rue Servient, 69003 Lyon",tel:"04 28 29 82 50",web:"www.axelera.org",email:"contact@axelera.org"},
  "Minalogic":                           {adresse:"5 Place Robert Schuman, 38025 Grenoble",tel:"04 76 15 60 35",web:"www.minalogic.com",email:"info@minalogic.com"},
  "Ville de Saint-Étienne":              {adresse:"Pl. de l'Hôtel de Ville, 42000 Saint-Étienne",tel:"04 77 48 71 23",web:"www.saint-etienne.fr"},
  "CETIM (Nantes)":                      {adresse:"74 Route de la Jonelière, 44326 Nantes",tel:"02 40 37 35 00",web:"www.cetim.fr"},
  "DRARI":                               {adresse:"92 Rue Pasteur, 69007 Lyon",tel:"04 72 80 62 00",web:"www.enseignementsup-recherche.gouv.fr"},
  "CCIN2P3":                             {adresse:"43 Bd du 11 Novembre 1918, 69622 Villeurbanne",tel:"04 72 43 15 54",web:"cc.in2p3.fr"},
  "Kairos Discovery":                    {adresse:"Lyon, France",web:"www.kairosdiscovery.com"},
  "HEC Montréal":                        {adresse:"3000 Ch. de la Côte-Sainte-Catherine, Montréal",tel:"+1 514 340-6000",web:"www.hec.ca"},
  "CIC Lyon":                            {adresse:"103 Grande Rue de la Croix-Rousse, 69004 Lyon",tel:"04 72 07 19 40",web:"www.chu-lyon.fr"},
  "CIC Saint-Étienne":                   {adresse:"Hôpital Nord, 42055 Saint-Étienne Cedex 2",tel:"04 77 82 83 84"},
};

/* ════════════════════════════════════
   DISPOSITIFS D'INNOVATION
   ════════════════════════════════════ */
const DISP = [
  {nom:"Boutique des sciences",portage:"ComUE",type:"Innovation sociale",cible:"Société civile",maturite:3,coords:[45.757,4.832]},
  {nom:"Dispositif innovation sociale et publique",portage:"ComUE",type:"Innovation sociale",cible:"Enseignants-chercheurs",maturite:2,coords:[45.7575,4.8318]},
  {nom:"CELSE",portage:"ComUE / Univ. Jean Monnet",type:"Innovation économique",cible:"Etudiants",maturite:9,coords:[45.4403,4.3872]},
  {nom:"Public Factory",portage:"IEP de Lyon",type:"Innovation sociale",cible:"Etudiants et prestataires",maturite:7,coords:[45.7565,4.8322]},
  {nom:"Accélération Deeptech",portage:"Pulsalys",type:"Innovation technologique",cible:"Entrepreneurs",maturite:7,coords:[45.7796,4.8742]},
  {nom:"Direction de l'innovation HCL",portage:"HCL",type:"Innovation sociale / technologique",cible:"Hospitaliers",maturite:7,coords:[45.7622,4.8545]},
  {nom:"CIMES",portage:"DGE",type:"Innovation ouverte",cible:"Entrepreneurs, universitaires",maturite:9,coords:[45.7640,4.8358]},
  {nom:"Fondation Jean Monnet",portage:"Université Jean-Monnet",type:"Innovation sociale",cible:"Enseignants-chercheurs",maturite:7,coords:[45.4398,4.3878]},
  {nom:"Minalogic",portage:"DGE",type:"Innovation ouverte",cible:"Entrepreneurs, universitaires",maturite:9,coords:[45.1890,5.7244]},
  {nom:"Cluster Noveka medtech",portage:"Saint-Etienne métropole",type:"Innovation technologique",cible:"Professionnels de santé",maturite:9,coords:[45.4342,4.3915]},
  {nom:"CETIM",portage:"DGE",type:"Innovation technologique",cible:"Société civile",maturite:9,coords:[47.2184,-1.5536]},
  {nom:"Centre Léon Bérard",portage:"INCa",type:"Innovation technologique",cible:"Professionnels de santé",maturite:9,coords:[45.7440,4.8330]},
  {nom:"Fonds investissement innovation",portage:"Métropole de Saint-Etienne",type:"Innovation économique",cible:"Entrepreneurs, chercheurs",maturite:7,coords:[45.4345,4.3908]},
  {nom:"Projet COPING",portage:"IAE Lyon",type:"Innovation sociale",cible:"Hospitaliers",maturite:7,coords:[45.7682,4.8530]},
  {nom:"Projet MARTI",portage:"Labo ICAR / ALSAN",type:"Innovation sociale",cible:"HCL",maturite:9,coords:[45.7300,4.8270]},
  {nom:"Odimedi",portage:"Laboratoire ICAR",type:"Innovation sociale",cible:"Travailleurs sociaux, interprètes",maturite:9,coords:[45.7297,4.8264]},
  {nom:"CRCL",portage:"INSERM-CNRS-Lyon 1",type:"Innovation technologique",cible:"Chercheurs",maturite:7,coords:[45.7815,4.8668]},
  {nom:"Cancéropôle",portage:"INCa",type:"Innovation technologique",cible:"Hospitaliers",maturite:5,coords:[45.7438,4.8325]},
  {nom:"Kairos Discovery",portage:"—",type:"Innovation technologique",cible:"Entrepreneurs / chercheurs",maturite:6,coords:[45.7510,4.8392]},
  {nom:"Axxelera",portage:"DGE",type:"Innovation ouverte",cible:"Entrepreneurs, universitaires",maturite:9,coords:[45.7492,4.8296]},
];

/* ════════════════════════════════════
   ACTEURS — avec coords géolocalisées
   ════════════════════════════════════ */
function jit(b,i,r=.0022){const a=(i*2.399)%(Math.PI*2),d=(((i*7+3)%10)/10)*r;return[b[0]+Math.sin(a)*d,b[1]+Math.cos(a)*d];}
const C={
  l1:[45.7824,4.8657],   // Campus La Doua — Lyon 1 / INSA
  l2:[45.7453,4.8347],   // Campus Porte des Alpes — Lyon 2
  sp:[45.7564,4.8318],   // Sciences Po Lyon — Av. Berthelot
  jm:[45.4397,4.3874],   // Université Jean Monnet — SE
  ens:[45.7298,4.8272],  // ENS Lyon — site Gerland
  ctr:[45.7936,4.9218],  // Centrale Lyon — Écully
  mns:[45.4234,4.4062],  // Mines Saint-Étienne
  hcl:[45.7614,4.8558],  // HCL siège Quai des Célestins
  clb:[45.7438,4.8330],  // Centre Léon Bérard
  cnrs:[45.7843,4.8694], // CNRS Délégation Villeurbanne
  doua:[45.7797,4.8724], // Site La Doua (commun INSA/Lyon1)
  bron:[45.7344,4.9162], // Bron — SBRI, ISC
  obsl:[45.7001,4.8340], // Observatoire Saint-Genis-Laval
  axl:[45.7492,4.8298],  // Axelera
  comue:[45.7566,4.8315],// COMUE — Rue Pasteur
  inria:[45.7886,4.8779],// Inria Lyon
  ifstar:[45.7662,4.9044]// IFSTTAR (Bron)
};
function L1(i){return jit(C.l1,i,.003);}
function L2(i){return jit(C.l2,i,.002);}
function EN(i){return jit(C.ens,i,.002);}
function JM(i){return jit(C.jm,i,.003);}
function CT(i){return jit(C.ctr,i,.002);}
function MN(i){return jit(C.mns,i,.002);}
function DU(i){return jit(C.doua,i,.0025);}

const AR = [
  // Institutions & gouvernance
  {nom:"DRARI",type:"Institution publique",desc:"Direction Régionale des Affaires de Recherche et de l'Innovation",tutelle:"MESRI",coords:C.comue},
  {nom:"Ville de Saint-Étienne",type:"Collectivité territoriale",desc:"Collectivité territoriale — Hôtel de Ville",tutelle:"",coords:[45.4347,4.3906]},
  {nom:"COMUE - Université de Lyon",type:"Structure de coordination académique",desc:"Communauté d'universités et établissements — site Lyon Saint-Étienne",tutelle:"",coords:C.comue},
  // Universités
  {nom:"Université Claude Bernard Lyon 1",type:"Université",desc:"Sciences, santé, technologie — 45 000 étudiants",tutelle:"MESRI",coords:C.doua},
  {nom:"Université Lumière Lyon 2",type:"Université",desc:"Sciences humaines et sociales",tutelle:"MESRI",coords:C.l2},
  {nom:"Sciences Po Lyon",type:"Grande école / IEP",desc:"Institut d'études politiques de Lyon, fondé en 1948",tutelle:"MESRI",coords:C.sp},
  {nom:"Université Jean Monnet",type:"Université",desc:"Université pluridisciplinaire — 13 000 étudiants",tutelle:"MESRI",coords:C.jm},
  {nom:"HEC Montréal",type:"Université internationale partenaire",desc:"Partenaire international — Montréal, Canada",tutelle:"",coords:[45.5048,-73.6149]},
  // Organismes de recherche
  {nom:"CNRS - Délégation Rhône Auvergne",type:"Organisme national de recherche",desc:"Délégation régionale du Centre National de la Recherche Scientifique",tutelle:"",coords:C.cnrs},
  {nom:"PEPR (coord. Lyon 1)",type:"Programme national de recherche",desc:"Programme et Équipements Prioritaires de Recherche",tutelle:"Lyon 1",coords:L1(20)},
  // Transfert & valorisation
  {nom:"Pulsalys",type:"SATT / Transfert de technologie",desc:"SATT Lyon Saint-Étienne — deeptech, maturation, startups",tutelle:"",coords:C.doua},
  // Santé
  {nom:"Hospices Civils de Lyon",type:"CHU / Hôpital public",desc:"13 hôpitaux publics — soin, formation, recherche",tutelle:"",coords:C.hcl},
  {nom:"Centre Léon Bérard",type:"Centre de lutte contre le cancer",desc:"Centre de lutte contre le cancer de Lyon",tutelle:"INCa",coords:C.clb},
  {nom:"CIC Lyon",type:"CHU / Hôpital public",desc:"Centre d'Investigation Clinique de Lyon",tutelle:"HCL / INSERM",coords:jit(C.hcl,1,.003)},
  {nom:"CIC Saint-Étienne",type:"CHU / Hôpital public",desc:"Centre d'Investigation Clinique de Saint-Étienne",tutelle:"CHU SE",coords:JM(7)},
  // Pôles & compétitivité
  {nom:"Pôle AXELERA",type:"Pôle de compétitivité",desc:"Pôle chimie / environnement Rhône-Alpes Auvergne",tutelle:"DGE",coords:C.axl},
  {nom:"Minalogic",type:"Pôle de compétitivité",desc:"Pôle mondial micro-nano technologies et logiciels embarqués",tutelle:"DGE",coords:[45.1890,5.7244]},
  // Entreprise
  {nom:"Kairos Discovery",type:"Entreprise biotech",desc:"Entreprise de biotechnologie — innovations thérapeutiques",tutelle:"",coords:[45.7512,4.8392]},
  // Centre technique
  {nom:"CETIM (Nantes)",type:"Centre technique industriel",desc:"Centre Technique des Industries Mécaniques",tutelle:"DGE",coords:[47.2184,-1.5536]},
  // Calcul
  {nom:"CCIN2P3",type:"Institution publique",desc:"Centre de Calcul de l'IN2P3 — physique nucléaire et particules",tutelle:"CNRS",coords:DU(7)},
  // Labos ENS Lyon (site Gerland)
  {nom:"ELICO",type:"Laboratoire universitaire",desc:"Sciences de l'Information et de la Communication",tutelle:"ENS Lyon / Lyon 2",coords:EN(1)},
  {nom:"ASLAN",type:"Laboratoire universitaire",desc:"Atelier Sciences du Langage",tutelle:"ENS Lyon / CNRS",coords:EN(2)},
  {nom:"ICAR - ENS Lyon",type:"Laboratoire universitaire",desc:"Interactions, corpus, apprentissages, représentations",tutelle:"ENS Lyon / CNRS",coords:EN(3)},
  {nom:"CIRI",type:"Laboratoire de recherche",desc:"Centre International de Recherche en Infectiologie",tutelle:"ENS Lyon / INSERM",coords:EN(4)},
  {nom:"IGFL",type:"Laboratoire de recherche",desc:"Institut de Génomique Fonctionnelle de Lyon",tutelle:"ENS Lyon / CNRS",coords:EN(5)},
  {nom:"LIP",type:"Laboratoire de recherche",desc:"Laboratoire de l'Informatique du Parallélisme",tutelle:"ENS Lyon / CNRS",coords:EN(6)},
  {nom:"TRIANGLE",type:"Laboratoire de recherche",desc:"Action, discours, pensée politique et économique",tutelle:"ENS Lyon / CNRS",coords:EN(7)},
  {nom:"IAO",type:"Laboratoire de recherche",desc:"Institut d'Asie Orientale",tutelle:"ENS Lyon",coords:EN(8)},
  {nom:"UMPA",type:"Laboratoire de recherche",desc:"Unité de Mathématiques Pures et Appliquées",tutelle:"ENS Lyon / CNRS",coords:EN(9)},
  {nom:"RDP",type:"Laboratoire de recherche",desc:"Reproduction et Développement des Plantes",tutelle:"ENS Lyon / INRAE",coords:EN(10)},
  {nom:"LPH ENS",type:"Laboratoire de recherche",desc:"Laboratoire de Physique ENS Lyon",tutelle:"ENS Lyon / CNRS",coords:EN(11)},
  {nom:"LBMCHOCS",type:"Laboratoire de recherche",desc:"Biologie et modélisation de la cellule",tutelle:"ENS Lyon",coords:EN(12)},
  {nom:"LCH",type:"Laboratoire de recherche",desc:"Laboratoire de Chimie ENS Lyon",tutelle:"ENS Lyon / CNRS",coords:EN(13)},
  // Labos Campus La Doua / Lyon 1
  {nom:"CRCL",type:"Laboratoire de recherche",desc:"Centre de Recherche en Cancérologie de Lyon",tutelle:"INSERM / CNRS / Lyon 1",coords:L1(1)},
  {nom:"CRNL",type:"Laboratoire de recherche",desc:"Centre de Recherche en Neurosciences de Lyon",tutelle:"Lyon 1 / INSERM",coords:DU(1)},
  {nom:"CarMeN",type:"Laboratoire de recherche",desc:"Cardiovasculaire, Métabolisme, Diabétologie & Nutrition",tutelle:"Lyon 1 / INSERM",coords:L1(3)},
  {nom:"CRAL",type:"Laboratoire de recherche",desc:"Centre de Recherche en Astrophysique de Lyon",tutelle:"Lyon 1 / ENS / CNRS",coords:C.obsl},
  {nom:"IBCP",type:"Laboratoire de recherche",desc:"Institut de Biologie et Chimie des Protéines",tutelle:"Lyon 1 / CNRS",coords:L1(4)},
  {nom:"ICBMS",type:"Laboratoire de recherche",desc:"Institut de Chimie et Biochimie Moléculaire et Supramoléculaire",tutelle:"Lyon 1 / CNRS",coords:L1(5)},
  {nom:"ICJ",type:"Laboratoire de recherche",desc:"Institut Camille Jordan — mathématiques",tutelle:"Lyon 1 / CNRS",coords:L1(6)},
  {nom:"ILM",type:"Laboratoire de recherche",desc:"Institut Lumière Matière",tutelle:"Lyon 1 / CNRS",coords:DU(2)},
  {nom:"INMG",type:"Laboratoire de recherche",desc:"Institut Neuromyogène",tutelle:"Lyon 1 / INSERM",coords:L1(8)},
  {nom:"IP2i",type:"Laboratoire de recherche",desc:"Institut des Deux Infinis — physique des particules",tutelle:"Lyon 1 / CNRS",coords:DU(3)},
  {nom:"IRCELYON",type:"Laboratoire de recherche",desc:"Institut de Recherche sur la Catalyse et l'Environnement de Lyon",tutelle:"CNRS / Lyon 1",coords:DU(4)},
  {nom:"ISA",type:"Laboratoire de recherche",desc:"Institut des Sciences Analytiques",tutelle:"Lyon 1 / CNRS",coords:L1(11)},
  {nom:"LBBE",type:"Laboratoire de recherche",desc:"Laboratoire Biométrie et Biologie Évolutive",tutelle:"Lyon 1 / CNRS",coords:L1(12)},
  {nom:"LBMC",type:"Laboratoire de recherche",desc:"Laboratoire de Biomécanique et Mécanique des Chocs",tutelle:"Lyon 1",coords:L1(13)},
  {nom:"LBTI",type:"Laboratoire de recherche",desc:"Biologie Tissulaire et Ingénierie Thérapeutique",tutelle:"Lyon 1 / CNRS",coords:L1(14)},
  {nom:"LEHNA",type:"Laboratoire de recherche",desc:"Laboratoire d'Écologie des Hydrosystèmes Naturels et Anthropisés",tutelle:"Lyon 1 / CNRS",coords:L1(15)},
  {nom:"LGL TPE",type:"Laboratoire de recherche",desc:"Géologie de Lyon : Terre, Planètes et Environnement",tutelle:"Lyon 1 / ENS",coords:L1(16)},
  {nom:"LMI",type:"Laboratoire de recherche",desc:"Laboratoire des Multimatériaux et Interfaces",tutelle:"Lyon 1 / CNRS",coords:L1(17)},
  {nom:"LAGEPP",type:"Laboratoire de recherche",desc:"Automatique et Génie des Procédés",tutelle:"Lyon 1 / CNRS",coords:L1(18)},
  {nom:"LABTAU",type:"Laboratoire de recherche",desc:"Applications des ultrasons à la thérapie",tutelle:"Lyon 1 / INSERM",coords:L1(19)},
  {nom:"LEM",type:"Laboratoire de recherche",desc:"Laboratoire d'Écologie Microbienne",tutelle:"Lyon 1 / CNRS",coords:L1(21)},
  {nom:"INL",type:"Laboratoire de recherche",desc:"Institut des Nanotechnologies de Lyon",tutelle:"Lyon 1 / Centrale / CNRS",coords:DU(5)},
  {nom:"HEMOSTASE",type:"Laboratoire de recherche",desc:"Hémostase, inflammation et sepsis",tutelle:"Lyon 1",coords:L1(24)},
  {nom:"HESPER - RESHAPE",type:"Laboratoire de recherche",desc:"Health Services and Performance Research",tutelle:"Lyon 1",coords:L1(25)},
  {nom:"IMMUNO",type:"Laboratoire de recherche",desc:"Immunogénomique et inflammation",tutelle:"Lyon 1",coords:L1(26)},
  {nom:"LYOS",type:"Laboratoire de recherche",desc:"Physiopathologie des maladies osseuses",tutelle:"Lyon 1 / INSERM",coords:L1(27)},
  {nom:"MAP",type:"Laboratoire de recherche",desc:"Microbiologie, adaptation et pathogénie",tutelle:"Lyon 1 / INSERM",coords:L1(28)},
  {nom:"MMSB",type:"Laboratoire de recherche",desc:"Microbiologie moléculaire et biochimie structurale",tutelle:"Lyon 1 / CNRS",coords:L1(29)},
  {nom:"NUDICE",type:"Laboratoire de recherche",desc:"Nutrition et Cerveau",tutelle:"Lyon 1 / INSERM",coords:L1(30)},
  {nom:"P2S",type:"Laboratoire de recherche",desc:"Parcours Santé Systémique",tutelle:"Lyon 1",coords:L1(31)},
  {nom:"PI3",type:"Laboratoire de recherche",desc:"Physiopathologie de l'immunodépression",tutelle:"Lyon 1",coords:L1(32)},
  {nom:"SBRI",type:"Laboratoire de recherche",desc:"Stem Cell and Brain Research Institute",tutelle:"INSERM / Lyon 1",coords:C.bron},
  {nom:"CNC / ISC",type:"Laboratoire de recherche",desc:"Centre de Neurosciences Cognitives — Institut des Sciences Cognitives Marc Jeannerot",tutelle:"CNRS / Lyon 1",coords:C.bron},
  {nom:"B2MC",type:"Laboratoire de recherche",desc:"Molécules Bioactives et Chimie Médicinale",tutelle:"Lyon 1",coords:L1(34)},
  {nom:"BIODYMIA",type:"Laboratoire de recherche",desc:"Bioingénierie et Dynamique Microbienne aux Interfaces des Aliments",tutelle:"Lyon 1",coords:L1(35)},
  {nom:"IVPC",type:"Laboratoire de recherche",desc:"Infection virale et pathologie comparée",tutelle:"Lyon 1",coords:L1(36)},
  {nom:"L2C2",type:"Laboratoire de recherche",desc:"Laboratoire langage, cerveau, cognition",tutelle:"CNRS / Lyon 1",coords:DU(6)},
  {nom:"SAF",type:"Laboratoire de recherche",desc:"Sciences Actuarielles et Financières",tutelle:"Lyon 1",coords:L1(39)},
  {nom:"OBSLYON",type:"Laboratoire de recherche",desc:"Observatoire de Lyon",tutelle:"Lyon 1 / ENS / CNRS",coords:C.obsl},
  {nom:"CTO",type:"Laboratoire de recherche",desc:"Ciblage Thérapeutique en Oncologie",tutelle:"Lyon 1",coords:jit(C.clb,2,.002)},
  {nom:"LMA",type:"Laboratoire de recherche",desc:"Laboratoire des Matériaux Avancés",tutelle:"CNRS",coords:DU(8)},
  {nom:"LIRIS",type:"Laboratoire de recherche",desc:"Laboratoire d'InfoRmatique en Image et Systèmes d'information",tutelle:"Lyon 1 / INSA / CNRS",coords:DU(9)},
  {nom:"UMRESTTE",type:"Laboratoire de recherche",desc:"Épidémiologie et Surveillance Transport Travail Environnement",tutelle:"Lyon 1 / INSA",coords:L1(43)},
  // Labos Lyon 2 / SHS
  {nom:"EVS",type:"Laboratoire de recherche",desc:"Environnement, Ville, Société",tutelle:"Lyon 2 / CNRS",coords:L2(1)},
  {nom:"LAET",type:"Laboratoire de recherche",desc:"Laboratoire Aménagement, Économie, Transports",tutelle:"Lyon 2",coords:L2(2)},
  {nom:"LARHRA",type:"Laboratoire de recherche",desc:"Laboratoire de Recherche Historique Rhône-Alpes",tutelle:"Lyon 2",coords:L2(3)},
  {nom:"Magellan",type:"Laboratoire de recherche",desc:"Centre de Recherche Magellan — sciences de gestion",tutelle:"Lyon 3",coords:[45.7668,4.8536]},
  {nom:"COACTIS",type:"Laboratoire de recherche",desc:"Conception de l'Action en Situation",tutelle:"Lyon 2 / Jean Monnet",coords:L2(4)},
  {nom:"CRPPC",type:"Laboratoire de recherche",desc:"Recherches en psychopathologie et psychologie clinique",tutelle:"Lyon 2",coords:L2(5)},
  {nom:"DDL",type:"Laboratoire de recherche",desc:"Dynamique du Langage",tutelle:"CNRS / Lyon 2",coords:L2(7)},
  {nom:"DIPHE",type:"Laboratoire de recherche",desc:"Développement Individu Processus Handicap Education",tutelle:"Lyon 2",coords:L2(8)},
  {nom:"EMC",type:"Laboratoire de recherche",desc:"Étude des Mécanismes Cognitifs",tutelle:"Lyon 2",coords:L2(10)},
  {nom:"ERIC",type:"Laboratoire de recherche",desc:"Recherche en Ingénierie des Connaissances",tutelle:"Lyon 2",coords:L2(11)},
  {nom:"GREPS",type:"Laboratoire de recherche",desc:"Groupe de Recherche en Psychologie Sociale",tutelle:"Lyon 2",coords:L2(12)},
  {nom:"HISOMA",type:"Laboratoire de recherche",desc:"Histoire et Sources des Mondes Antiques",tutelle:"Lyon 2 / Lyon 3",coords:L2(13)},
  {nom:"ICAR",type:"Laboratoire de recherche",desc:"Interactions, corpus, apprentissages, représentations",tutelle:"Lyon 2 / CNRS",coords:L2(14)},
  {nom:"CIHAM",type:"Laboratoire de recherche",desc:"Histoire des mondes chrétiens et musulmans médiévaux",tutelle:"Lyon 2 / CNRS",coords:L2(17)},
  {nom:"CMW",type:"Laboratoire de recherche",desc:"Centre Max Weber — sociologie",tutelle:"Lyon 2 / ENS / CNRS",coords:L2(18)},
  {nom:"GRAPHOS",type:"Laboratoire de recherche",desc:"Recherche Appliquée Pluridisciplinaire sur l'Hôpital",tutelle:"Lyon 2",coords:L2(19)},
  {nom:"IXXI",type:"Laboratoire de recherche",desc:"Institut Rhône-Alpin des systèmes complexes",tutelle:"CNRS / UdL",coords:[45.7642,4.8353]},
  {nom:"GATE",type:"Laboratoire de recherche",desc:"Groupe d'Analyse et de Théorie Économique Lyon Saint-Étienne",tutelle:"Lyon 2 / Jean Monnet",coords:jit([45.7455,4.835],2)},
  // Labos Centrale Lyon (Écully)
  {nom:"Ampère - Centrale Lyon",type:"Laboratoire de recherche",desc:"Électrotechnique, électromagnétisme, génie électrique",tutelle:"Centrale Lyon / CNRS",coords:CT(1)},
  {nom:"LMFA",type:"Laboratoire de recherche",desc:"Mécanique des Fluides et d'Acoustique",tutelle:"Centrale Lyon / CNRS",coords:CT(3)},
  {nom:"LTDS",type:"Laboratoire de recherche",desc:"Tribologie et Dynamique des Systèmes",tutelle:"Centrale Lyon / CNRS",coords:CT(4)},
  {nom:"LGPC",type:"Laboratoire de recherche",desc:"Génie des Procédés Catalytiques",tutelle:"Centrale Lyon / CNRS",coords:CT(5)},
  // Labos INSA Lyon (La Doua)
  {nom:"LAMCOS",type:"Laboratoire de recherche",desc:"Mécanique des Contacts et des Structures",tutelle:"INSA Lyon / CNRS",coords:DU(10)},
  {nom:"MATEIS",type:"Laboratoire de recherche",desc:"Matériaux : Ingénierie et Science",tutelle:"INSA Lyon / CNRS",coords:DU(11)},
  {nom:"CETHIL",type:"Laboratoire de recherche",desc:"Centre Thermique de Lyon",tutelle:"INSA / Centrale",coords:DU(12)},
  {nom:"LVA",type:"Laboratoire de recherche",desc:"Laboratoire Vibrations Acoustique",tutelle:"INSA Lyon",coords:DU(13)},
  {nom:"LGEF",type:"Laboratoire de recherche",desc:"Génie Électrique et Ferroélectricité",tutelle:"Lyon 1 / INSA",coords:DU(14)},
  {nom:"CREATIS",type:"Laboratoire de recherche",desc:"Acquisition et Traitement de l'Image pour la Santé",tutelle:"Lyon 1 / INSA / CNRS",coords:DU(15)},
  {nom:"CITI",type:"Laboratoire de recherche",desc:"Innovation en Télécommunication et Intégration de Services",tutelle:"INSA Lyon",coords:DU(16)},
  {nom:"DEEP",type:"Laboratoire de recherche",desc:"Déchets – Eaux – Environnement – Pollutions",tutelle:"INSA Lyon",coords:DU(17)},
  {nom:"GEOMAS",type:"Laboratoire de recherche",desc:"Géomécanique Matériaux Structure",tutelle:"INSA Lyon",coords:DU(19)},
  {nom:"IMP",type:"Laboratoire de recherche",desc:"Ingénierie des Matériaux Polymères",tutelle:"CNRS / Lyon 1 / INSA",coords:DU(20)},
  {nom:"BF2I",type:"Laboratoire de recherche",desc:"Biologie Fonctionnelle, Insectes et Interactions",tutelle:"INSA Lyon",coords:DU(21)},
  // Labos Mines Saint-Étienne
  {nom:"LGF",type:"Laboratoire de recherche",desc:"Laboratoire Georges Friedel — science des matériaux",tutelle:"Mines Saint-Étienne / CNRS",coords:MN(1)},
  {nom:"SMS",type:"Laboratoire de recherche",desc:"Sciences des Matériaux et des Structures",tutelle:"Mines Saint-Étienne",coords:MN(2)},
  {nom:"SPIN",type:"Laboratoire de recherche",desc:"Sciences des Processus Industriels et Naturels",tutelle:"Mines Saint-Étienne",coords:MN(3)},
  {nom:"IHF",type:"Laboratoire de recherche",desc:"Institut Henri Fayol — ingénierie et gestion",tutelle:"Mines Saint-Étienne",coords:MN(4)},
  {nom:"SAS",type:"Laboratoire de recherche",desc:"Sécurité des architectures et des systèmes",tutelle:"Mines Saint-Étienne",coords:MN(5)},
  // Labos Jean Monnet / Saint-Étienne
  {nom:"CELSE",type:"Laboratoire universitaire",desc:"Centre d'Expérimentation et de Liaison des Sciences de l'Éducation",tutelle:"Université Jean Monnet",coords:JM(1)},
  {nom:"SAINBIOSE",type:"Laboratoire de recherche",desc:"Santé Ingénierie Biologie Saint-Étienne",tutelle:"Jean Monnet / CHU SE",coords:JM(2)},
  {nom:"LaHC",type:"Laboratoire de recherche",desc:"Laboratoire Hubert Curien — informatique, optique",tutelle:"Jean Monnet / CNRS",coords:JM(3)},
  {nom:"LASPI",type:"Laboratoire de recherche",desc:"Analyse des Signaux et Processus Industriels",tutelle:"Jean Monnet",coords:JM(4)},
  {nom:"CERCRID",type:"Laboratoire de recherche",desc:"Recherches Critiques sur le Droit",tutelle:"Jean Monnet",coords:JM(5)},
  {nom:"GIMAP",type:"Laboratoire de recherche",desc:"Immunité des Muqueuses et Agents Pathogènes",tutelle:"Jean Monnet / INSERM",coords:JM(6)},
  {nom:"ECLLA",type:"Laboratoire de recherche",desc:"Études du contemporain en Littératures, Langues, Arts",tutelle:"Jean Monnet",coords:JM(8)},
  {nom:"LGCB",type:"Laboratoire de recherche",desc:"Génie Civil et Bâtiment",tutelle:"Jean Monnet",coords:JM(11)},
  {nom:"LIBM",type:"Laboratoire de recherche",desc:"Biologie de la Motricité",tutelle:"Jean Monnet / Lyon 1",coords:JM(12)},
  {nom:"GIMAP",type:"Laboratoire de recherche",desc:"Groupe sur l'Immunité des Muqueuses et Agents Pathogènes",tutelle:"Jean Monnet",coords:JM(13)},
  {nom:"SNA EPIS",type:"Laboratoire de recherche",desc:"Système Nerveux Autonome — épidémiologie, physiologie, santé",tutelle:"Jean Monnet",coords:JM(14)},
  {nom:"LIMOS",type:"Laboratoire de recherche",desc:"Informatique, Modélisation et Optimisation des Systèmes",tutelle:"Jean Monnet / CNRS",coords:JM(15)},
  {nom:"GATE",type:"Laboratoire de recherche",desc:"Groupe d'Analyse et de Théorie Économique",tutelle:"Lyon 2 / Jean Monnet",coords:jit(C.jm,9,.002)},
  // Transport / IFSTTAR
  {nom:"LESCOT",type:"Laboratoire de recherche",desc:"Ergonomie et sciences cognitives pour les transports",tutelle:"Univ. Gustave Eiffel",coords:C.ifstar},
  {nom:"LTE",type:"Laboratoire de recherche",desc:"Laboratoire Transport Environnement",tutelle:"Univ. Gustave Eiffel",coords:jit(C.ifstar,1,.001)},
  {nom:"UMRAE",type:"Laboratoire de recherche",desc:"Acoustique Environnementale",tutelle:"Univ. Gustave Eiffel",coords:jit(C.ifstar,2,.001)},
  {nom:"LICIT",type:"Laboratoire de recherche",desc:"Ingénierie Circulation Transports",tutelle:"",coords:jit(C.ifstar,3,.001)},
  {nom:"RIVERLY",type:"Laboratoire de recherche",desc:"Hydro-écologie fluviale",tutelle:"",coords:[45.768,4.908]},
];
const seen=new Set();
const ACTEURS=AR.filter(a=>{if(seen.has(a.nom))return false;seen.add(a.nom);return true;});

/* ════════════ HELPERS ════════════ */
const COL={sociale:'#0d9488',techno:'#1a56db',eco:'#c2410c',ouverte:'#6d28d9',mixte:'#b45309',
  université:'#1a56db',laboratoire:'#0d9488',institution:'#c2410c',entreprise:'#6d28d9',
  santé:'#b91c1c',collectivité:'#b45309',transfert:'#15803d',dispositif:'#b45309'};
function tkD(t){t=t.toLowerCase();if(t.includes('sociale'))return'sociale';if(t.includes('techno'))return'techno';if(t.includes('éco')||t.includes('eco'))return'eco';if(t.includes('ouverte'))return'ouverte';return'mixte';}
function tkA(t){t=(t||'').toLowerCase();if(t.includes('universit')&&!t.includes('inter'))return'université';if(t.includes('laborat')||t.includes('programme')||t.includes('coord'))return'laboratoire';if(t.includes('instit')||t.includes('organisme')||t.includes('calcul'))return'institution';if(t.includes('collectiv'))return'collectivité';if(t.includes('chu')||t.includes('hôpit')||t.includes('investig')||t.includes('cancer'))return'santé';if(t.includes('satt')||t.includes('transfert')||t.includes('pôle')||t.includes('pole'))return'transfert';if(t.includes('entreprise'))return'entreprise';if(t.includes('grande')||t.includes('iep'))return'institution';return'laboratoire';}
function bcD(k){return{sociale:'bs',techno:'bt',eco:'be',ouverte:'bo',mixte:'bm'}[k]||'';}
function bcA(k){return{université:'bu',laboratoire:'bl',institution:'bi',entreprise:'bep',santé:'bsa',collectivité:'bc',transfert:'btr'}[k]||'bl';}
function stD(t){if(t.includes('sociale'))return'Sociale';if(t.includes('techno'))return'Techno';if(t.includes('éco'))return'Éco';if(t.includes('ouverte'))return'Ouverte';return'Mixte';}
function stA(t){if(!t)return'—';if(t.includes('Univ'))return'Université';if(t.includes('Labo'))return'Labo';if(t.includes('CHU')||t.includes('Hôpit'))return'CHU';if(t.includes('cancer'))return'CLCC';if(t.includes('SATT'))return'SATT';if(t.includes('Entreprise'))return'Entreprise';if(t.includes('Pôle'))return'Pôle';if(t.includes('Grande')||t.includes('IEP'))return'Gde École';if(t.includes('Institution'))return'Institution';if(t.includes('Collectiv'))return'Collectivité';if(t.includes('Organisme'))return'Org. nat.';if(t.includes('Structure'))return'COMUE';return t.split(' ').slice(0,2).join(' ');}
function trlC(v){if(v<=3)return{f:'#c2410c',t:'#b45309'};if(v<=6)return{f:'#b45309',t:'#0d9488'};return{f:'#0d9488',t:'#1a56db'};}
function trlL(v){if(v<=3)return'Concept / Recherche fondamentale';if(v<=5)return'Développement / Validation';if(v<=7)return'Démonstration / Pré-déploiement';return'Maturité opérationnelle';}
function ct(nom){return CONTACTS[nom]||null;}

// SVG icons for contact rows
const iAddr=`<svg width="11" height="11" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path d="M21 10c0 7-9 13-9 13S3 17 3 10a9 9 0 0118 0z"/><circle cx="12" cy="10" r="3"/></svg>`;
const iTel=`<svg width="11" height="11" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path d="M22 16.92v3a2 2 0 01-2.18 2 19.79 19.79 0 01-8.63-3.07A19.5 19.5 0 013.15 12 19.79 19.79 0 01.1 3.4a2 2 0 012-2.18h3a2 2 0 012 1.72c.127.96.361 1.903.7 2.81a2 2 0 01-.45 2.11L6.09 8.91a16 16 0 006 6l1.27-1.27a2 2 0 012.11-.45c.907.339 1.85.573 2.81.7A2 2 0 0122 16.92z"/></svg>`;
const iWeb=`<svg width="11" height="11" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="M2 12h20M12 2a15.3 15.3 0 014 10 15.3 15.3 0 01-4 10 15.3 15.3 0 01-4-10 15.3 15.3 0 014-10z"/></svg>`;
const iMail=`<svg width="11" height="11" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>`;

function contactRowsPopup(c){
  if(!c)return'';
  let h='<div class="pcontact">';
  if(c.adresse)h+=`<div class="prow">${iAddr}${c.adresse}</div>`;
  if(c.tel)h+=`<div class="prow">${iTel}${c.tel}</div>`;
  if(c.email)h+=`<div class="prow">${iMail}<a href="mailto:${c.email}">${c.email}</a></div>`;
  if(c.web)h+=`<div class="prow">${iWeb}<a href="https://${c.web}" target="_blank">${c.web}</a></div>`;
  return h+'</div>';
}
function contactRowsSb(c){
  if(!c)return'';
  let h='<div class="sf"><div class="fl">Contact</div>';
  if(c.adresse)h+=`<div class="contact-row">${iAddr}${c.adresse}</div>`;
  if(c.tel)h+=`<div class="contact-row">${iTel}${c.tel}</div>`;
  if(c.email)h+=`<div class="contact-row">${iMail}<a href="mailto:${c.email}">${c.email}</a></div>`;
  if(c.web)h+=`<div class="contact-row">${iWeb}<a href="https://${c.web}" target="_blank">${c.web}</a></div>`;
  return h+'</div>';
}

/* ════════════ RENDER CARDS ════════════ */
let legDval='all',legAval='all';
function renderDisp(){
  document.getElementById('grid-d').innerHTML=DISP.map((d,i)=>{
    const k=tkD(d.type),col=COL[k],c=trlC(d.maturite);
    return `<div class="card" style="--ca:${col};animation-delay:${i*16}ms" onclick="openModalD(${i})" data-type="${k}" data-p="${d.portage.toLowerCase()}" data-m="${d.maturite}">
      <div class="ch"><div class="cn">${d.nom}</div><span class="badge ${bcD(k)}">${stD(d.type)}</span></div>
      <div class="cm">
        <div class="mr"><strong>Porteur</strong><span class="v">${d.portage}</span></div>
        <div class="mr"><strong>Cible</strong><span class="v">${d.cible}</span></div>
      </div>
      <div class="tw"><div class="tl"><span>TRL/SRL</span><span>${d.maturite}/9</span></div>
        <div class="tb"><div class="tf" style="width:${d.maturite/9*100}%;background:linear-gradient(90deg,${c.f},${c.t})"></div></div></div>
    </div>`;
  }).join('');
}
function renderActeurs(){
  document.getElementById('grid-a').innerHTML=ACTEURS.map((a,i)=>{
    const k=tkA(a.type),col=COL[k];
    return `<div class="card" style="--ca:${col};animation-delay:${i*4}ms" onclick="openModalA(${i})" data-type="${k}" data-tn="${(a.type||'').toLowerCase()}">
      <div class="ch"><div class="cn">${a.nom}</div><span class="badge ${bcA(k)}">${stA(a.type)}</span></div>
      <div class="cm">
        <div class="mr"><strong>Type</strong><span class="v">${a.type}</span></div>
        ${a.desc&&a.desc!==a.nom?`<div class="mr"><strong>Desc.</strong><span class="v">${a.desc}</span></div>`:''}
        ${a.tutelle?`<div class="mr"><strong>Tutelle</strong><span class="v">${a.tutelle}</span></div>`:''}
      </div>
    </div>`;
  }).join('');
  document.getElementById('cnta').textContent=ACTEURS.length;
}

/* ════════════ FILTERS ════════════ */
function applyD(){
  const s=document.getElementById('sd').value.toLowerCase(),p=document.getElementById('fport').value.toLowerCase(),m=document.getElementById('fmat').value;
  let v=0;
  document.querySelectorAll('#grid-d .card').forEach(c=>{
    const n=c.querySelector('.cn').textContent.toLowerCase(),ct=c.dataset.type,cp=c.dataset.p,cm=parseInt(c.dataset.m);
    let ok=true;
    if(s&&!n.includes(s)&&!cp.includes(s))ok=false;
    if(p&&!cp.includes(p))ok=false;
    if(m==='low'&&cm>3)ok=false;if(m==='mid'&&(cm<4||cm>6))ok=false;if(m==='high'&&cm<7)ok=false;
    if(legDval!=='all'&&ct!==legDval)ok=false;
    c.classList.toggle('hidden',!ok);if(ok)v++;
  });
  document.getElementById('cntd').textContent=v;
  document.getElementById('empty-d').classList.toggle('v',v===0);
}
function applyA(){
  const s=document.getElementById('sa').value.toLowerCase(),t=document.getElementById('ftype').value.toLowerCase();
  let v=0;
  document.querySelectorAll('#grid-a .card').forEach(c=>{
    const n=c.querySelector('.cn').textContent.toLowerCase(),ct=c.dataset.type,ctn=c.dataset.tn;
    let ok=true;
    if(s&&!n.includes(s))ok=false;
    if(t&&!ctn.includes(t))ok=false;
    if(legAval!=='all'&&ct!==legAval)ok=false;
    c.classList.toggle('hidden',!ok);if(ok)v++;
  });
  document.getElementById('cnta').textContent=v;
  document.getElementById('empty-a').classList.toggle('v',v===0);
}
function resetD(){legDval='all';document.getElementById('sd').value='';document.getElementById('fport').value='';document.getElementById('fmat').value='';document.querySelectorAll('#leg-disp .li').forEach(e=>e.classList.remove('active'));document.querySelector('#leg-disp .li').classList.add('active');applyD();}
function resetA(){legAval='all';document.getElementById('sa').value='';document.getElementById('ftype').value='';document.querySelectorAll('#leg-act .li').forEach(e=>e.classList.remove('active'));document.querySelector('#leg-act .li').classList.add('active');applyA();}
function legD(val,el){legDval=val;document.querySelectorAll('#leg-disp .li').forEach(e=>e.classList.remove('active'));el.classList.add('active');applyD();}
function legA(val,el){legAval=val;document.querySelectorAll('#leg-act .li').forEach(e=>e.classList.remove('active'));el.classList.add('active');applyA();}
function legMap(val,el){
  document.querySelectorAll('#leg-carte .li').forEach(e=>e.classList.remove('active'));el.classList.add('active');
  mapFilter=val;filterMarkers();
  document.querySelectorAll('.pill').forEach(p=>{
    const text=p.textContent;
    const match=(val==='all'&&text.includes('Tous'))||(val!=='all'&&text.toLowerCase().includes(val.substring(0,5)));
    p.classList.toggle('active',match);
  });
}

/* ════════════ TABS ════════════ */
function switchTab(tab,btn){
  document.querySelectorAll('.tbtn').forEach(b=>b.classList.remove('active'));btn.classList.add('active');
  document.querySelectorAll('.tp').forEach(p=>p.classList.remove('active'));document.getElementById('panel-'+tab).classList.add('active');
  ['leg-carte','leg-disp','leg-act'].forEach(id=>document.getElementById(id).style.display='none');
  ['ctrl-disp','ctrl-act'].forEach(id=>document.getElementById(id).style.display='none');
  const mc=document.getElementById('mc');
  mc.style.overflowY='auto';mc.style.padding='18px 28px';
  if(tab==='carte'){mc.style.padding='0';mc.style.overflowY='hidden';document.getElementById('leg-carte').style.display='flex';setTimeout(()=>map&&map.invalidateSize(),80);}
  else if(tab==='dispositifs'){document.getElementById('leg-disp').style.display='flex';document.getElementById('ctrl-disp').style.display='flex';}
  else{document.getElementById('leg-act').style.display='flex';document.getElementById('ctrl-act').style.display='flex';}
}

/* ════════════ MAP ════════════ */
let map,allMarkers=[],clusterGroup,mapFilter='all';
function makeIcon(color,shape){
  const s=shape==='rect'?26:22;
  const svg=shape==='rect'
    ?`<svg xmlns='http://www.w3.org/2000/svg' width='${s}' height='${s}' viewBox='0 0 26 26'><rect x='2' y='2' width='22' height='22' rx='5' fill='${color}' stroke='white' stroke-width='2'/><rect x='8' y='8' width='10' height='10' rx='2' fill='white' opacity='.35'/></svg>`
    :`<svg xmlns='http://www.w3.org/2000/svg' width='${s}' height='${s}' viewBox='0 0 22 22'><circle cx='11' cy='11' r='8' fill='${color}' stroke='white' stroke-width='2'/><circle cx='11' cy='11' r='3' fill='white' opacity='.35'/></svg>`;
  return L.divIcon({html:svg,className:'',iconSize:[s,s],iconAnchor:[s/2,s/2],popupAnchor:[0,-s/2+2]});
}
function initMap(){
  map=L.map('map',{zoomControl:true}).setView([45.62,4.76],10);
  // Classic OSM tile — standard rendering
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{
    attribution:'© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
    maxZoom:19
  }).addTo(map);
  clusterGroup=L.markerClusterGroup({
    showCoverageOnHover:false,maxClusterRadius:50,
    iconCreateFunction(cluster){
      const n=cluster.getChildCount(),s=n>50?42:n>20?34:28;
      return L.divIcon({className:'',html:`<div style="background:rgba(26,86,219,.75);border:2px solid white;border-radius:50%;width:${s}px;height:${s}px;display:flex;align-items:center;justify-content:center;font-family:'DM Sans',sans-serif;font-weight:700;font-size:.73rem;color:white;box-shadow:0 2px 8px rgba(0,0,0,.25)">${n}</div>`,iconSize:[s,s]});
    }
  });
  DISP.forEach((d,i)=>{
    const k=tkD(d.type),c=ct(d.nom)||ct(d.portage);
    const m=L.marker(d.coords,{icon:makeIcon(COL[k],'rect')});
    m.bindPopup(`<span class="pname">${d.nom}</span><span class="ptype">🚀 Dispositif · ${d.type}</span>${contactRowsPopup(c)}<button class="pbtn" onclick="openModalD(${i})">Voir le détail →</button>`,{maxWidth:250});
    m._ft='dispositif';m._nom=d.nom.toLowerCase();allMarkers.push(m);clusterGroup.addLayer(m);
  });
  ACTEURS.forEach((a,i)=>{
    if(!a.coords)return;
    const k=tkA(a.type),c=ct(a.nom);
    const m=L.marker(a.coords,{icon:makeIcon(COL[k],'circle')});
    m.bindPopup(`<span class="pname">${a.nom}</span><span class="ptype">${a.type}${a.tutelle?' · '+a.tutelle:''}</span>${contactRowsPopup(c)}<button class="pbtn" onclick="openSb(${i})">Voir la fiche →</button>`,{maxWidth:250});
    m._ft=k;m._nom=a.nom.toLowerCase();allMarkers.push(m);clusterGroup.addLayer(m);
  });
  map.addLayer(clusterGroup);
}
function pillClick(val,el){
  document.querySelectorAll('.pill').forEach(p=>p.classList.remove('active'));el.classList.add('active');
  mapFilter=val;filterMarkers();
  document.querySelectorAll('#leg-carte .li').forEach(e=>{
    const txt=e.textContent.trim();
    const match=(val==='all'&&txt==='Tous')||(val!=='all'&&txt.toLowerCase().includes(val.substring(0,5)));
    e.classList.toggle('active',match);
  });
}
function filterMarkers(){
  const q=(document.getElementById('mq').value||'').toLowerCase();
  clusterGroup.clearLayers();
  allMarkers.forEach(m=>{if((mapFilter==='all'||m._ft===mapFilter)&&(!q||m._nom.includes(q)))clusterGroup.addLayer(m);});
}
function openSb(i){
  const a=ACTEURS[i],k=tkA(a.type),c=ct(a.nom);
  document.getElementById('sbtitle').textContent=a.nom;
  let b=`<span class="badge ${bcA(k)}" style="align-self:flex-start;margin-bottom:4px">${a.type}</span>`;
  if(a.desc&&a.desc!==a.nom)b+=`<div class="sf"><div class="fl">Description</div><div class="fv">${a.desc}</div></div>`;
  if(a.tutelle)b+=`<div class="sf"><div class="fl">Tutelle principale</div><div class="fv">${a.tutelle}</div></div>`;
  b+=contactRowsSb(c);
  if(a.coords)b+=`<div class="sf"><div class="fl">Coordonnées GPS</div><div class="fv" style="font-family:monospace;font-size:.73rem">${a.coords[0].toFixed(5)}, ${a.coords[1].toFixed(5)}</div></div>`;
  document.getElementById('sbbody').innerHTML=b;
  document.getElementById('sbpanel').classList.add('open');
}
function closeSb(){document.getElementById('sbpanel').classList.remove('open');}
function openModalD(i){
  const d=DISP[i],k=tkD(d.type),tc=trlC(d.maturite),c=ct(d.nom)||ct(d.portage);
  document.getElementById('modalbody').innerHTML=`
    <div class="mt">${d.nom}</div>
    <div class="ms">Dispositif d'innovation</div>
    <div class="mb"><span class="badge ${bcD(k)}">${d.type}</span></div>
    <div class="mg">
      <div class="mf"><div class="fl">Porteur / Structure</div><div class="fv">${d.portage}</div></div>
      <div class="mf"><div class="fl">Public cible</div><div class="fv">${d.cible}</div></div>
      ${c&&c.adresse?`<div class="mf"><div class="fl">Adresse</div><div class="fv">${c.adresse}</div></div>`:''}
      ${c&&c.tel?`<div class="mf"><div class="fl">Téléphone</div><div class="fv">${c.tel}</div></div>`:''}
      ${c&&c.email?`<div class="mf"><div class="fl">Email</div><div class="fv"><a href="mailto:${c.email}">${c.email}</a></div></div>`:''}
      ${c&&c.web?`<div class="mf"><div class="fl">Site web</div><div class="fv"><a href="https://${c.web}" target="_blank">${c.web}</a></div></div>`:''}
    </div>
    <div class="mtrwrap">
      <div class="mtrh"><span>Maturité TRL/SRL — <em>${trlL(d.maturite)}</em></span><strong>${d.maturite}/9</strong></div>
      <div class="mtrbar"><div class="mtrfill" style="width:${d.maturite/9*100}%;background:linear-gradient(90deg,${tc.f},${tc.t})"></div></div>
      <div class="trscale"><span>1 Idéation</span><span>5 Prototype</span><span>9 Déploiement</span></div>
    </div>`;
  document.getElementById('overlay').classList.add('open');
}
function openModalA(i){
  const a=ACTEURS[i],k=tkA(a.type),c=ct(a.nom);
  document.getElementById('modalbody').innerHTML=`
    <div class="mt">${a.nom}</div>
    <div class="ms">${a.desc||''}</div>
    <div class="mb"><span class="badge ${bcA(k)}">${a.type}</span></div>
    <div class="mg">
      <div class="mf"><div class="fl">Type d'acteur</div><div class="fv">${a.type}</div></div>
      ${a.tutelle?`<div class="mf"><div class="fl">Tutelle principale</div><div class="fv">${a.tutelle}</div></div>`:''}
      ${c&&c.adresse?`<div class="mf"><div class="fl">Adresse</div><div class="fv">${c.adresse}</div></div>`:''}
      ${c&&c.tel?`<div class="mf"><div class="fl">Téléphone</div><div class="fv">${c.tel}</div></div>`:''}
      ${c&&c.email?`<div class="mf"><div class="fl">Email</div><div class="fv"><a href="mailto:${c.email}">${c.email}</a></div></div>`:''}
      ${c&&c.web?`<div class="mf"><div class="fl">Site web</div><div class="fv"><a href="https://${c.web}" target="_blank">${c.web}</a></div></div>`:''}
      ${a.desc&&a.desc!==a.nom?`<div class="mf"><div class="fl">Description</div><div class="fv">${a.desc}</div></div>`:''}
    </div>`;
  document.getElementById('overlay').classList.add('open');
}
function closeModal(e){if(!e||e.target===document.getElementById('overlay'))document.getElementById('overlay').classList.remove('open');}
document.addEventListener('keydown',e=>{if(e.key==='Escape'){closeModal();closeSb();}});

renderDisp();
renderActeurs();
initMap();
</script>
</body>
</html>
