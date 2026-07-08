<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1">
<title>Suprimentos — Drogaria Globo</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
<style>
:root{
  --red:#b91c1c;--red-l:#fef2f2;--red-b:rgba(185,28,28,.15);
  --blue:#1e40af;--blue-l:#eff6ff;--blue-b:rgba(30,64,175,.15);
  --grn:#166534;--grn-l:#f0fdf4;--grn-b:rgba(22,101,52,.15);
  --amb:#92400e;--amb-l:#fffbeb;--amb-b:rgba(146,64,14,.15);
  --c50:#f8fafc;--c100:#f1f5f9;--c200:#e2e8f0;--c300:#cbd5e1;
  --c400:#94a3b8;--c500:#64748b;--c600:#475569;--c700:#334155;--c900:#0f172a;
  --wh:#fff;--bd:#e2e8f0;--tx:#0f172a;--tx2:#475569;--tx3:#94a3b8;
  --ff:'Segoe UI',system-ui,-apple-system,sans-serif;--fm:'SF Mono','Consolas',monospace;
  --r:6px;--r2:10px;--sh:0 1px 2px rgba(0,0,0,.06),0 1px 3px rgba(0,0,0,.08);
  --nh:52px;--sb:210px;
}
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
html{font-size:14px;}
body{background:var(--c100);color:var(--tx);font-family:var(--ff);overflow-x:clip;-webkit-font-smoothing:antialiased;}

/* ── NAV SUPERIOR — só brand + botões ── */
.nav{position:fixed;top:0;left:0;right:0;z-index:500;height:var(--nh);background:var(--wh);border-bottom:1px solid var(--bd);display:flex;align-items:center;padding:0 12px;gap:8px;box-shadow:var(--sh);}
.brand{display:flex;align-items:center;gap:8px;flex-shrink:0;}
.bmark{width:28px;height:28px;background:var(--red);border-radius:5px;display:flex;align-items:center;justify-content:center;font-weight:800;color:var(--wh);font-size:11px;}
.bname{font-size:12px;font-weight:700;color:var(--c900);line-height:1.15;}
.bsub{font-size:9.5px;color:var(--tx3);}
@media(max-width:599px){.bsub{display:none;}}
.nav-r{display:flex;align-items:center;gap:6px;margin-left:auto;}
.ts{font-size:10px;color:var(--tx3);}
@media(max-width:599px){.ts{display:none;}}
.btn-upd{height:28px;padding:0 10px;background:var(--blue);color:var(--wh);border:none;border-radius:5px;font-size:12px;font-weight:500;cursor:pointer;font-family:var(--ff);}
.btn-upd:disabled{opacity:.4;}
.btn-sb{height:28px;width:28px;background:var(--c100);border:1px solid var(--bd);border-radius:5px;cursor:pointer;font-size:14px;display:none;align-items:center;justify-content:center;}
@media(max-width:767px){.btn-sb{display:flex;}}
/* SIDEBAR */
.sidebar{position:fixed;top:var(--nh);left:0;bottom:0;width:var(--sb);background:var(--wh);border-right:1px solid var(--bd);z-index:490;display:flex;flex-direction:column;overflow:hidden;}
/* Abas dentro da sidebar */
.sb-tabs{display:flex;flex-direction:column;gap:1px;padding:8px 8px 4px;flex-shrink:0;}
.sb-tab{height:28px;padding:0 8px;border:none;border-radius:5px;cursor:pointer;background:transparent;color:var(--tx3);font-family:var(--ff);font-size:12px;font-weight:500;text-align:left;transition:all .12s;display:flex;align-items:center;}
.sb-tab:hover{background:var(--c100);color:var(--tx2);}
.sb-tab.on{background:var(--blue-l);color:var(--blue);font-weight:700;}
.sb-divider{height:1px;background:var(--bd);margin:2px 8px 4px;}
.sb-head{padding:6px 12px;border-bottom:1px solid var(--bd);display:flex;align-items:center;justify-content:space-between;flex-shrink:0;}
.sb-title{font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.08em;color:var(--tx3);}
.sb-clear{font-size:10px;color:var(--blue);cursor:pointer;background:none;border:none;font-family:var(--ff);}
.sb-clear:hover{text-decoration:underline;}
.sb-body{flex:1;overflow-y:auto;padding:8px 10px;display:flex;flex-direction:column;gap:7px;}
.sb-body::-webkit-scrollbar{width:3px;}
.sb-body::-webkit-scrollbar-thumb{background:var(--c200);border-radius:2px;}
.sb-group{display:flex;flex-direction:column;gap:3px;}
.sb-label{font-size:9px;font-weight:700;color:var(--tx3);text-transform:uppercase;letter-spacing:.05em;padding:0 2px;}
.fi{width:100%;height:28px;padding:0 8px;background:var(--wh);border:1px solid var(--bd);border-radius:5px;color:var(--tx2);font-family:var(--ff);font-size:11.5px;outline:none;}
.fi:focus{border-color:var(--blue);}
select.fi{cursor:pointer;}
input.fi{color:var(--tx);}
input.fi::placeholder{color:var(--tx3);}
.sb-cnt{font-size:10px;color:var(--tx3);padding:8px 12px;border-top:1px solid var(--bd);text-align:center;font-family:var(--fm);}
/* sidebar mobile: drawer */
@media(max-width:767px){
  .sidebar{transform:translateX(-100%);transition:transform .2s ease;box-shadow:none;}
  .sidebar.open{transform:translateX(0);box-shadow:4px 0 16px rgba(0,0,0,.12);}
}

/* ── LAYOUT PRINCIPAL ── */
.pg{padding-top:var(--nh);padding-left:var(--sb);min-height:100vh;}
@media(max-width:767px){.pg{padding-left:0;}}
.main{padding:14px;max-width:1500px;margin:0 auto;}
.view{display:none;}.view.on{display:block;animation:fi .14s ease;}
@keyframes fi{from{opacity:0;transform:translateY(3px)}to{opacity:1;transform:none}}
/* SECTION TITLE */
.stit{font-size:10px;font-weight:700;color:var(--tx3);text-transform:uppercase;letter-spacing:.08em;margin-bottom:10px;display:flex;align-items:center;gap:8px;}
.stit::after{content:'';flex:1;height:1px;background:var(--bd);}
/* KPI STRIP */
.krow{display:grid;gap:8px;margin-bottom:12px;}
.k2{grid-template-columns:1fr 1fr;}.k3{grid-template-columns:1fr 1fr;}.k4{grid-template-columns:1fr 1fr;}.k5{grid-template-columns:1fr 1fr;}
@media(min-width:480px){.k3{grid-template-columns:repeat(3,1fr);}.k5{grid-template-columns:repeat(3,1fr);}}
@media(min-width:640px){.k4{grid-template-columns:repeat(4,1fr);}.k5{grid-template-columns:repeat(5,1fr);}}
@media(min-width:1024px){.krow{display:flex;gap:0;background:var(--bd);border:1px solid var(--bd);border-radius:var(--r2);overflow:hidden;box-shadow:var(--sh);}.k2,.k3,.k4,.k5{grid-template-columns:unset;}}
.kpi{background:var(--wh);padding:11px 14px;position:relative;overflow:hidden;border-radius:var(--r);border:1px solid var(--bd);}
@media(min-width:1024px){.kpi{flex:1;border:none;border-radius:0;}}
.kpi::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;}
.kpi.kr::before{background:var(--red);}.kpi.kb::before{background:var(--blue);}.kpi.kg::before{background:var(--grn);}.kpi.ka::before{background:var(--amb);}.kpi.kn::before{background:var(--c300);}
.klb{font-size:9.5px;font-weight:600;color:var(--tx3);text-transform:uppercase;letter-spacing:.05em;margin-bottom:3px;line-height:1.2;}
.kv{font-size:17px;font-weight:700;font-family:var(--fm);line-height:1;color:var(--c900);}
@media(min-width:1024px){.kv{font-size:19px;}}
.ks{font-size:9.5px;color:var(--tx3);margin-top:2px;line-height:1.3;}
.kpi.kr .kv{color:var(--red);}.kpi.kb .kv{color:var(--blue);}.kpi.kg .kv{color:var(--grn);}.kpi.ka .kv{color:var(--amb);}
/* SAVING HERO CARD */
.sav-hero{background:linear-gradient(135deg,var(--grn) 0%,#14532d 100%);border-radius:var(--r2);padding:20px 24px;color:#fff;margin-bottom:12px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:12px;box-shadow:var(--sh);}
.sav-hero-main .lbl{font-size:10.5px;font-weight:600;text-transform:uppercase;letter-spacing:.08em;opacity:.8;margin-bottom:4px;}
.sav-hero-main .val{font-size:32px;font-weight:800;font-family:var(--fm);line-height:1;}
.sav-hero-main .sub{font-size:12px;opacity:.75;margin-top:3px;}
.sav-hero-stats{display:flex;gap:20px;flex-wrap:wrap;}
.sav-stat{text-align:center;}
.sav-stat .sv{font-size:20px;font-weight:700;font-family:var(--fm);line-height:1;}
.sav-stat .sl{font-size:10px;opacity:.75;margin-top:2px;text-transform:uppercase;letter-spacing:.05em;}
/* ALERTS */
.alrows{display:flex;flex-direction:column;gap:5px;margin-bottom:12px;}
@media(min-width:640px){.alrows{flex-direction:row;flex-wrap:wrap;}}
.al{display:flex;align-items:flex-start;gap:6px;padding:6px 10px;font-size:12px;font-weight:500;border-radius:var(--r);border-left:3px solid;line-height:1.45;}
.al.ar{background:var(--red-l);border-color:var(--red);color:var(--red);}
.al.aa{background:var(--amb-l);border-color:var(--amb);color:var(--amb);}
.al.ag{background:var(--grn-l);border-color:var(--grn);color:var(--grn);}
.al.ab{background:var(--blue-l);border-color:var(--blue);color:var(--blue);}
.adot{width:6px;height:6px;border-radius:50%;background:currentColor;flex-shrink:0;margin-top:3px;}
.adot.bl{animation:blink 1.2s infinite;}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.1}}
/* INSIGHT STRIP */
.insights{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:12px;}
.ins{background:var(--wh);border:1px solid var(--bd);border-radius:var(--r);padding:8px 12px;font-size:11.5px;color:var(--tx2);display:flex;align-items:center;gap:6px;line-height:1.4;box-shadow:var(--sh);}
.ins-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0;}
/* GRID */
.grid{display:grid;gap:10px;margin-bottom:10px;}
.g1{grid-template-columns:1fr;}.g2{grid-template-columns:1fr;}.g3{grid-template-columns:1fr;}
@media(min-width:640px){.g2{grid-template-columns:1fr 1fr;}}
@media(min-width:768px){.g3{grid-template-columns:1fr 1fr 1fr;}}
@media(min-width:1024px){.g12{grid-template-columns:1fr 2fr;}.g21{grid-template-columns:2fr 1fr;}.g13{grid-template-columns:1fr 3fr;}}
/* PANEL */
.card{background:var(--wh);border:1px solid var(--bd);border-radius:var(--r2);box-shadow:var(--sh);overflow:hidden;}
.ch{padding:10px 12px 0;display:flex;align-items:center;justify-content:space-between;}
.ct{font-size:10.5px;font-weight:700;color:var(--tx2);text-transform:uppercase;letter-spacing:.04em;}
.cb{padding:10px 12px 12px;}.cb0{padding:0;}
/* CHARTS */
.cv{position:relative;}
.h130{height:130px;}.h160{height:160px;}.h185{height:185px;}.h210{height:210px;}.h250{height:250px;}.h280{height:280px;}
/* TABLES */
.tw{overflow:auto;-webkit-overflow-scrolling:touch;}
.mh200{max-height:200px;}.mh280{max-height:280px;}.mh360{max-height:360px;}.mh440{max-height:440px;}.mh520{max-height:520px;}
table{width:100%;border-collapse:collapse;font-size:11.5px;}
thead{position:sticky;top:0;z-index:4;}
th{background:var(--c50);color:var(--tx3);font-size:9.5px;font-weight:700;text-transform:uppercase;letter-spacing:.05em;padding:6px 10px;text-align:left;border-bottom:1px solid var(--c200);white-space:nowrap;}
td{padding:5px 10px;border-bottom:1px solid var(--c100);color:var(--tx);vertical-align:middle;}
tr:hover td{background:var(--blue-l);}
tr:last-child td{border-bottom:none;}
.mn{font-family:var(--fm);font-size:11px;}.tdr{text-align:right;}.te{max-width:180px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;}
/* TAGS */
.tag{display:inline-flex;align-items:center;padding:1px 5px;border-radius:3px;font-size:10px;font-weight:600;white-space:nowrap;line-height:1.5;}
.tg{background:var(--grn-l);color:var(--grn);}.tb{background:var(--blue-l);color:var(--blue);}
.tr2{background:var(--red-l);color:var(--red);}.ta{background:var(--amb-l);color:var(--amb);}.tn{background:var(--c100);color:var(--tx3);}
.abca{font-size:10px;font-weight:700;padding:1px 4px;border-radius:2px;background:var(--grn-l);color:var(--grn);}
.abcb{font-size:10px;font-weight:700;padding:1px 4px;border-radius:2px;background:var(--amb-l);color:var(--amb);}
.abcc{font-size:10px;font-weight:700;padding:1px 4px;border-radius:2px;background:var(--c100);color:var(--tx3);}
/* AGING */
.aok{color:var(--grn);font-family:var(--fm);font-size:11px;}.awa{color:var(--amb);font-family:var(--fm);font-size:11px;}.acr{color:var(--red);font-family:var(--fm);font-size:11px;}
/* STAT ROW */
.srow{display:flex;align-items:center;padding:4px 0;border-bottom:1px solid var(--c100);}
.srow:last-child{border-bottom:none;}
.slb{font-size:11.5px;color:var(--tx2);flex:1;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;padding-right:6px;}
.sbar{width:50px;height:3px;background:var(--c200);border-radius:2px;margin:0 5px;flex-shrink:0;}
.sfil{height:100%;border-radius:2px;}
.sval{font-family:var(--fm);font-size:11.5px;flex-shrink:0;}
/* MINI BAR */
.mb{display:flex;align-items:center;gap:4px;}.mbt{width:46px;height:3px;background:var(--c200);border-radius:2px;flex-shrink:0;}.mbf{height:100%;border-radius:2px;}
/* PROGRESS BAR (demanda status) */
.pbar-wrap{margin-bottom:8px;}
.pbar-label{display:flex;justify-content:space-between;font-size:11px;color:var(--tx2);margin-bottom:3px;}
.pbar-track{height:6px;background:var(--c200);border-radius:3px;overflow:hidden;}
.pbar-fill{height:100%;border-radius:3px;transition:width .4s ease;}
/* LOCAL FILTERS */
.lf{display:flex;align-items:center;gap:5px;padding:7px 12px;border-bottom:1px solid var(--c100);flex-wrap:wrap;}
.lf-cnt{font-size:10px;color:var(--tx3);white-space:nowrap;margin-left:auto;}
/* FOOTNOTE */
.fn{font-size:9.5px;color:var(--tx3);padding:4px 12px 8px;line-height:1.5;}
/* ABC LEGEND */
.abc-legend{display:flex;gap:12px;padding:6px 12px;border-top:1px solid var(--c100);flex-wrap:wrap;}
.abc-legend-item{display:flex;align-items:center;gap:5px;font-size:11px;color:var(--tx2);}
.abc-dot{width:10px;height:10px;border-radius:2px;}
/* LOAD/ERROR */
.load{display:flex;flex-direction:column;align-items:center;justify-content:center;min-height:55vh;gap:12px;}
.spin{width:26px;height:26px;border:2px solid var(--c200);border-top-color:var(--blue);border-radius:50%;animation:sp .8s linear infinite;}
@keyframes sp{to{transform:rotate(360deg);}}
.ltx{color:var(--tx3);font-size:13px;}
.errw{display:flex;flex-direction:column;align-items:center;justify-content:center;min-height:55vh;gap:10px;padding:20px;text-align:center;}
.errt{font-size:14px;font-weight:700;color:var(--red);}.errm{color:var(--tx3);font-size:12px;max-width:400px;line-height:1.6;}
@media(max-width:479px){.main{padding:8px;}.kv{font-size:15px;}}
</style>
</head>
<body>
<nav class="nav">
  <div class="brand">
    <div class="bmark">DG</div>
    <div><div class="bname">Drogaria Globo</div><div class="bsub">Suprimentos</div></div>
  </div>
  <div class="nav-r">
    <span class="ts" id="bTs"></span>
    <button class="btn-sb" onclick="toggleSb()" title="Menu">☰</button>
    <button class="btn-upd" id="btnR" onclick="init()">↺ Atualizar</button>
  </div>
</nav>

<!-- ── SIDEBAR: abas + filtros ── -->
<aside class="sidebar" id="sidebar">
  <div class="sb-tabs">
    <button class="sb-tab on" onclick="ir('exec',this)">📊 Executivo</button>
    <button class="sb-tab"    onclick="ir('garg',this)">⚠️ Gargalos</button>
    <button class="sb-tab"    onclick="ir('dem',this)">📋 Demanda</button>
    <button class="sb-tab"    onclick="ir('ocs',this)">🛒 OCs</button>
    <button class="sb-tab"    onclick="ir('perf',this)">🏆 Performance</button>
    <button class="sb-tab"    onclick="ir('fin',this)">💰 Financeiro</button>
    <button class="sb-tab"    onclick="ir('sav',this)">💚 Saving</button>
    <button class="sb-tab"    onclick="window.open('../../Painel Chamados MAN/painel_chamados_globo.html','_blank')" style="display:flex;align-items:center;justify-content:space-between;">🔧 Chamados MAN <span style="font-size:9px;opacity:.6">↗</span></button>
    <button class="sb-tab"    onclick="ir('obras',this)">🏗️ Obras</button>
    <button class="sb-tab"    onclick="ir('viagens',this)">✈️ Viagens</button>
  </div>
  <div class="sb-divider"></div>
  <div class="sb-head">
    <span class="sb-title">Filtros</span>
    <button class="sb-clear" onclick="gClear()">Limpar</button>
  </div>
  <div class="sb-body" id="fbar"></div>
  <div class="sb-cnt" id="fcnt"></div>
</aside>
<div class="pg"><div class="main">

<!-- ══ EXECUTIVO ══ -->
<div class="view on" id="view-exec">
  <div class="stit">Visão Executiva</div>
  <div class="krow k4" id="kE"></div>
  <div class="alrows" id="aE"></div>
  <div class="grid g2" style="margin-bottom:10px">
    <div class="card"><div class="ch"><span class="ct">OCs por Status — Qtd e Valor</span></div><div class="cb"><div class="cv h185"><canvas id="cESt"></canvas></div></div></div>
    <div class="card"><div class="ch"><span class="ct">Distribuição por Comprador</span></div><div class="cb"><div class="cv h185"><canvas id="cECp"></canvas></div></div></div>
  </div>
  <div class="grid g3">
    <div class="card"><div class="ch"><span class="ct">Top 5 Fornecedores</span></div><div class="cb"><div class="cv h160"><canvas id="cEF5"></canvas></div></div></div>
    <div class="card"><div class="ch"><span class="ct">Status Gerencial</span></div><div class="cb" id="eStL" style="padding-top:6px"></div></div>
    <div class="card"><div class="ch"><span class="ct">Saving por Comprador</span></div><div class="cb" id="eSavL" style="padding-top:4px"></div></div>
  </div>
</div>

<!-- ══ GARGALOS ══ -->
<div class="view" id="view-garg">
  <div class="stit">Gargalos Operacionais</div>
  <div class="krow k5" id="kG"></div>
  <div class="alrows" id="aGarg"></div>
  <div class="card" id="cardRiscoSC" style="margin-bottom:10px">
    <div class="ch"><span class="ct">⚠️ Chamados MAN — Serviços Críticos sem Cotação · por Fator de Risco</span></div>
    <div class="cb0"><div class="tw mh280"><table id="tRiscoSC"></table></div></div>
    <div class="fn">Score = Criticidade do Tipo × Aging × Recorrência · Crítico ≥15 · Atenção 8-14 · Comprador Responsável destacado</div>
  </div>
  <div class="grid g2">
    <div class="card">
      <div class="ch"><span class="ct">Pendente de Aprovação — FILIAL-OCs Não Fechado</span></div>
      <div class="fn">Valor Original · Dias corridos desde emissão</div>
      <div class="cb0"><div class="tw mh280"><table id="tNF"></table></div></div>
    </div>
    <div class="card">
      <div class="ch"><span class="ct">Demanda em Processo — por Comprador</span><span style="font-size:9.5px;color:var(--tx3);margin-left:6px">Não Cotada + status intermediários</span></div>
      <div class="cb0"><div class="tw mh280"><table id="tGComp"></table></div></div>
    </div>
  </div>
  <div class="grid g2">
    <div class="card">
      <div class="ch"><span class="ct">Backlog SC por Centro de Custo</span><span style="font-size:9.5px;color:var(--tx3);margin-left:6px">Serv + Prod não cotados</span></div>
      <div class="cb0"><div class="tw mh220"><table id="tGBf"></table></div></div>
    </div>
  <div class="grid g2">
    <div class="card" style="margin-top:0">
      <div class="ch"><span class="ct">SC Serviços Não Cotadas — Aging</span></div>
      <div class="lf">
        <input class="fi" id="sBk" placeholder="Descrição, CC ou comprador..." oninput="rBkTbl()">
        <select class="fi" id="fBkAg" onchange="rBkTbl()">
          <option value="">Todos agings</option><option value="ok">Verde ≤3d</option>
          <option value="warn">Amarelo 4-15d</option><option value="crit">Vermelho +15d</option>
        </select>
        <span class="lf-cnt" id="cntBk"></span>
      </div>
      <div class="cb0"><div class="tw mh360"><table id="tBk"></table></div></div>
    </div>
    <div class="card" style="margin-top:0">
      <div class="ch"><span class="ct">SC Produtos Não Cotados — Aging</span></div>
      <div class="lf">
        <input class="fi" id="sBkP" placeholder="Descrição, CC ou comprador..." oninput="rBkProdTbl()">
        <select class="fi" id="fBkAgP" onchange="rBkProdTbl()">
          <option value="">Todos agings</option><option value="ok">Verde ≤3d</option>
          <option value="warn">Amarelo 4-15d</option><option value="crit">Vermelho +15d</option>
        </select>
        <span class="lf-cnt" id="cntBkP"></span>
      </div>
      <div class="cb0"><div class="tw mh360"><table id="tBkP"></table></div></div>
    </div>
  </div>
</div>
</div>

<!-- ══ DEMANDA SC ══ -->
<div class="view" id="view-dem">
  <div class="stit">Demanda — Solicitações de Compra</div>
  <div class="krow k5" id="kD"></div>
  <div class="grid g3" style="margin-bottom:10px">
    <div class="card">
      <div class="ch"><span class="ct">Status das SCs</span></div>
      <div class="cb" id="dStatusBars" style="padding-top:8px"></div>
    </div>
    <div class="card"><div class="ch"><span class="ct">SC por Regional</span></div><div class="cb"><div class="cv h185"><canvas id="cDreg"></canvas></div></div></div>
  </div>
  <div class="grid g3">
    <div class="card"><div class="ch"><span class="ct">Por Setor Solicitante</span></div><div class="cb"><div class="cv h185"><canvas id="cDsetor"></canvas></div></div></div>
    <div class="card"><div class="ch"><span class="ct">Por Comprador Responsável</span></div><div class="cb"><div class="cv h185"><canvas id="cDcomp2"></canvas></div></div></div>
    <div class="card"><div class="ch"><span class="ct">Top Itens Solicitados</span></div><div class="cb"><div class="cv h185"><canvas id="cDitem"></canvas></div></div></div>
  </div>
  <div class="grid g2">
    <div class="card"><div class="ch"><span class="ct">Por Centro de Custo</span></div><div class="cb"><div class="cv h185"><canvas id="cDcc"></canvas></div></div></div>
    <div class="card"><div class="ch"><span class="ct">Insights de Demanda</span></div><div class="cb" id="dInsights" style="padding-top:4px"></div></div>
  </div>
  <div class="card">
    <div class="ch"><span class="ct">SCs — Detalhe</span></div>
    <div class="lf">
      <input class="fi" id="sDem" placeholder="Nº SC, descrição, CC..." oninput="rDemTbl()">
      <select class="fi" id="fDemTp" onchange="rDemTbl()"><option value="">Produto/Serviço</option><option value="Serviço">Serviço</option><option value="Produto">Produto</option></select>
      <select class="fi" id="fDemSit" onchange="rDemTbl()"><option value="">Status</option><option value="Não Cotada">Não Cotada</option><option value="Gerou O.C.">Gerou O.C.</option></select><select class="fi" id="fDemTipoSC" onchange="rDemTbl()"><option value="">Prod/Serv</option><option value="Serviço">Serviço</option><option value="Produto">Produto</option></select>
      <select class="fi" id="fDemReg" onchange="rDemTbl()"><option value="">Regional</option></select>
      <select class="fi" id="fDemComp" onchange="rDemTbl()"><option value="">Comprador Resp.</option></select>
      <select class="fi" id="fDemRisco" onchange="rDemTbl()"><option value="">Fator de Risco</option><option value="Critico">Crítico</option><option value="Atencao">Atenção</option><option value="Normal">Normal</option></select>
      <span class="lf-cnt" id="cntDem"></span>
    </div>
    <div class="cb0"><div class="tw mh440"><table id="tDem"></table></div></div>
  </div>
</div>

<!-- ══ OCs ══ -->
<div class="view" id="view-ocs">
  <div class="stit">Ordens de Compra — Indicadores e Detalhe</div>
  <div class="krow k4" id="kOC"></div>
  <div class="krow k4" id="kOC2" style="margin-top:-4px"></div>
  <div class="insights" id="insOC"></div>
  <div class="grid g3" style="margin-bottom:10px">
    <div class="card" style="grid-column:span 1"><div class="ch"><span class="ct">Análise por Comprador</span></div><div class="cb0"><div class="tw mh280"><table id="tOCcomp"></table></div></div><div class="fn">Irreg. = OCs com situação Não Fechado (pendentes de aprovação)</div></div>
    <div class="card"><div class="ch"><span class="ct">Valor por Situação</span></div><div class="cb"><div class="cv h210"><canvas id="cOCsit"></canvas></div></div></div>
    <div class="card"><div class="ch"><span class="ct">Top 10 Fornecedores</span></div><div class="cb"><div class="cv h210"><canvas id="cOCforn"></canvas></div></div></div>
  </div>
  <div class="card">
    <div class="ch"><span class="ct">OCs — Detalhe</span></div>
    <div class="lf">
      <input class="fi" id="sOC" placeholder="FILIAL-OC, fornecedor, cód-deriv..." oninput="rOCTbl()">
      <select class="fi" id="fOCSit" onchange="rOCTbl()"><option value="">Status</option><option value="Liquidado">Liquidado</option><option value="Aberto Total">Aberto Total</option><option value="Não Fechado">Não Fechado</option><option value="Cancelado">Cancelado</option></select>
      <select class="fi" id="fOCCp" onchange="rOCTbl()"><option value="">Comprador</option></select>
      <select class="fi" id="fOCReg" onchange="rOCTbl()"><option value="">Regional</option></select>
      <select class="fi" id="fOCCC" onchange="rOCTbl()"><option value="">Centro de Custo</option></select>
      <span class="lf-cnt" id="cntOC"></span>
    </div>
    <div class="cb0"><div class="tw mh520"><table id="tOC"></table></div></div>
  </div>
</div>

<!-- ══ PERFORMANCE ══ -->
<div class="view" id="view-perf">
  <div class="stit">Performance — Compradores</div>
  <div class="krow k4" id="kP"></div>
  <div class="grid g2">
    <div class="card">
      <div class="ch">
        <span class="ct">Ranking de Compradores</span>
      </div>
      <div class="cb0"><div class="tw mh440"><table id="tPcomp"></table></div></div>
      <div class="fn">Volume = valor total OCs · Saving Rate = SAVING ÷ Vlr. Inicial · Irreg. = OCs Não Fechado · % Atend. = SCs atendidas ÷ total SCs · T.Médio = aging médio das SCs já atendidas (verde ≤7d · âmbar ≤15d · vermelho +15d)</div>
    </div>
    <div class="card">
      <div class="ch"><span class="ct">Volume por Comprador — Valor e Qtd OCs</span></div>
      <div class="cb0"><div class="tw mh220"><table id="tPvol"></table></div></div>
    </div>
  </div>
  <div class="grid g2">
    <div class="card"><div class="ch"><span class="ct">Saving por Comprador</span></div><div class="cb"><div class="cv h185"><canvas id="cPsav"></canvas></div></div></div>
    <div class="card"><div class="ch"><span class="ct">Saving Rate % vs Meta 8%</span></div><div class="cb"><div class="cv h185"><canvas id="cPrate"></canvas></div></div></div>
  </div>
</div>

<!-- ══ FINANCEIRO ══ -->
<div class="view" id="view-fin">
  <div class="stit">Controle Financeiro</div>
  <div class="krow k4" id="kFin"></div>
  <div class="grid g2">
    <div class="card"><div class="ch"><span class="ct">Valor por Situação</span></div><div class="cb"><div class="cv h185"><canvas id="cFinB"></canvas></div></div></div>
    <div class="card">
      <div class="ch"><span class="ct">Curva ABC — Concentração de Gasto</span></div>
      <div class="cb0" id="finAbcTbl"></div>
    </div>
  </div>
  <div class="grid g2">
    <div class="card"><div class="ch"><span class="ct">Top 15 Fornecedores — Curva ABC</span></div><div class="cb"><div class="cv h210"><canvas id="cFinForn"></canvas></div></div></div>
    <div class="card">
      <div class="ch"><span class="ct">Pendente de Aprovação — Detalhe</span></div>
      <div class="fn">"Comprometido" = Aberto Total (aprovadas, reservado). "Pendente" = Não Fechado (em aprovação). Ambos: Valor Original.</div>
      <div class="cb0"><div class="tw mh280"><table id="tFinNf"></table></div></div>
    </div>
  </div>
</div>

<!-- ══ SAVING ══ -->
<div class="view" id="view-sav">
  <div class="stit">Saving — Performance de Negociação</div>
  <div id="savHero"></div>
  <div class="grid g2">
    <div class="card"><div class="ch"><span class="ct">Por Setor Solicitante</span></div><div class="cb"><div class="cv h185"><canvas id="cSavS"></canvas></div></div></div>
    <div class="card"><div class="ch"><span class="ct">Modo de Contratação</span></div><div class="cb"><div class="cv h185"><canvas id="cSavM"></canvas></div></div></div>
  </div>
  <div class="card">
    <div class="ch"><span class="ct">Registros de Saving</span></div>
    <div class="lf">
      <input class="fi" id="sSav" placeholder="Descrição ou fornecedor..." oninput="rSavTbl()">
      <select class="fi" id="fSavC" onchange="rSavTbl()"><option value="">Comprador</option></select>
      <select class="fi" id="fSavM" onchange="rSavTbl()"><option value="">Modo</option><option value="CONTRATO">Contrato</option><option value="OC">OC Avulsa</option></select>
      <select class="fi" id="fSavSt" onchange="rSavTbl()"><option value="">Setor</option><option value="Suprimentos">Suprimentos</option><option value="Administrativo">Administrativo</option></select><input class="fi" id="fSavFOC" placeholder="FILIAL-OC..." oninput="rSavTbl()" style="min-width:110px">
      <input class="fi" id="fSavFOC" placeholder="Filial-OC..." oninput="rSavTbl()" style="min-width:100px">
      <span class="lf-cnt" id="cntSav"></span>
    </div>
    <div class="cb0"><div class="tw mh380"><table id="tSav"></table></div></div>
  </div>
</div>

<!-- ══ CHAMADOS MAN ══ -->
<div class="view" id="view-cham">
  <div class="stit">Chamados MAN — Gestão Operacional e Recorrência</div>
  <div class="krow k4" id="kCh"></div>
  <div class="alrows" id="aCh"></div>
  <div class="card" id="cardRiscoCH" style="margin-bottom:10px">
    <div class="ch"><span class="ct">⚠️ Chamados Abertos — Ordenados por Fator de Risco</span></div>
    <div class="cb0"><div class="tw mh280"><table id="tRiscoCH"></table></div></div>
    <div class="fn">Score = Criticidade do Tipo × Aging × Recorrência · Crítico ≥15 · Atenção 8-14</div>
  </div>
  <div class="grid g3">
    <div class="card"><div class="ch"><span class="ct">Por Status</span></div><div class="cb"><div class="cv h160"><canvas id="cChSt"></canvas></div></div></div>
    <div class="card"><div class="ch"><span class="ct">Aging dos Abertos</span></div><div class="cb"><div class="cv h160"><canvas id="cChAg"></canvas></div></div></div>
    <div class="card"><div class="ch"><span class="ct">Por Prioridade</span></div><div class="cb"><div class="cv h160"><canvas id="cChPr"></canvas></div></div></div>
  </div>
  <div class="grid g2">
    <div class="card"><div class="ch"><span class="ct">Abertos por Mês — Comparativo Anual</span></div><div class="cb"><div class="cv h250"><canvas id="cChMes"></canvas></div></div></div>
    <div class="card"><div class="ch"><span class="ct">Top Motivos</span></div><div class="cb"><div class="cv h185"><canvas id="cChMo"></canvas></div></div></div>
  </div>
  <!-- Recorrência incorporada -->
  <div class="stit" style="margin-top:4px">Recorrência e Reincidência</div>
  <div class="krow k2" id="kRec"></div>
  <div class="grid g2">
    <div class="card"><div class="ch"><span class="ct">Por UF (sem N/D)</span></div><div class="cb"><div class="cv h185"><canvas id="cRuf"></canvas></div></div></div>
    <div class="card"><div class="ch"><span class="ct">Top Reincidências — Loja × Motivo</span></div><div class="cb"><div class="cv h185"><canvas id="cRec"></canvas></div></div></div>
  </div>
  <div class="card" style="margin-bottom:10px">
    <div class="ch"><span class="ct">Ranking de Reincidência</span></div>
    <div class="cb0"><div class="tw mh280"><table id="tRec"></table></div></div>
    <div class="fn">≥2 mesma Loja+Motivo = Monitorar · ≥3 = Atenção · ≥5 = Crítico</div>
  </div>
  <!-- Tabela de chamados -->
  <div class="card">
    <div class="ch"><span class="ct">Chamados — Detalhe</span></div>
    <div class="lf">
      <input class="fi" id="sCh" placeholder="Protocolo, local ou motivo..." oninput="rChTbl()">
      <select class="fi" id="fChSt" onchange="rChTbl()"><option value="">Status</option></select>
      <select class="fi" id="fChPr" onchange="rChTbl()"><option value="">Prioridade</option></select>
      <select class="fi" id="fChTp" onchange="rChTbl()"><option value="">Tipo</option><option value="Corretivo">Corretivo</option><option value="Preventivo">Preventivo</option></select>
      <select class="fi" id="fChSC" onchange="rChTbl()"><option value="">Com/Sem SC</option><option value="sim">Com SC</option><option value="nao">Sem SC</option></select>
      <input class="fi" id="fChProto" placeholder="Protocolo..." oninput="rChTbl()" style="min-width:100px">
      <input class="fi" id="fChSCNum" placeholder="Nº SC..." oninput="rChTbl()" style="min-width:80px">
      <select class="fi" id="fChReg" onchange="rChTbl()"><option value="">Regional</option></select>
      <span class="lf-cnt" id="cntCh"></span>
    </div>
    <div class="cb0"><div class="tw mh440"><table id="tCh"></table></div></div>
  </div>
</div>

<!-- ══ OBRAS E REFORMAS ══ -->
<div class="view" id="view-obras">
  <div class="stit">Obras e Reformas</div>
  <div style="display:flex;flex-direction:column;align-items:center;justify-content:center;min-height:50vh;gap:16px;text-align:center;padding:24px;">
    <div style="width:56px;height:56px;background:var(--amb-l);border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:28px;">🏗️</div>
    <div style="font-size:16px;font-weight:700;color:var(--c700)">Em Desenvolvimento</div>
    <div style="font-size:13px;color:var(--tx3);max-width:360px;line-height:1.7">A base de dados de Obras e Reformas está sendo estruturada.<br>Em breve este painel estará disponível.</div>
    <div style="background:var(--amb-l);border:1px solid var(--amb-b);color:var(--amb);font-size:11px;font-weight:600;padding:4px 12px;border-radius:20px;text-transform:uppercase;letter-spacing:.06em">Em breve</div>
  </div>
</div>

<!-- ══ VIAGENS ══ -->
<div class="view" id="view-viagens">
  <div class="stit">Viagens</div>
  <div style="display:flex;flex-direction:column;align-items:center;justify-content:center;min-height:50vh;gap:16px;text-align:center;padding:24px;">
    <div style="width:56px;height:56px;background:var(--blue-l);border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:28px;">✈️</div>
    <div style="font-size:16px;font-weight:700;color:var(--c700)">Em Desenvolvimento</div>
    <div style="font-size:13px;color:var(--tx3);max-width:360px;line-height:1.7">A base de dados de Viagens está sendo estruturada.<br>Em breve este painel estará disponível.</div>
    <div style="background:var(--blue-l);border:1px solid var(--blue-b);color:var(--blue);font-size:11px;font-weight:600;padding:4px 12px;border-radius:20px;text-transform:uppercase;letter-spacing:.06em">Em breve</div>
  </div>
</div>

</div></div>
<script>
var SCRIPT_URL='COLE_SUA_URL_AQUI';
var D=null,CH={};

function isNaoCotada(sit){
  var s=(sit||'').toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g,'').trim();
  return s==='nao cotada'||s==='nao cotado'||s.includes('nao cotad');
}

// -- MOCK DATA para preview fora do Apps Script --
function getMockData(){
  var comps=['Ana Lima','Carlos Mota','Fernanda Cruz','Pedro Alves','Juliana Reis'];
  var forns=['DISTRIBUIDORA SUL','FARMEX LTDA','MEDPHARMA','GLOBAL DIST.','PHARMA CENTER','SUPRI MAX','DELTA FARMA','NEXO SAUDE','ALMED DIST.','VITAE FARMA'];
  var ocs=[];
  var sits=['Liquidado','Aberto Total','Não Fechado','Cancelado'];
  var regs=['MA','PI','CE','PA','TO'];
  for(var i=0;i<200;i++){
    var comp=comps[i%comps.length];
    var sit=sits[i%4===0?0:i%4===1?1:i%4===2?2:3];
    ocs.push({filialOC:'F'+String(1000+i).slice(1)+'-OC'+i,filial:'F'+String(1000+i%50).slice(1),comprador:comp,setor:i%5===0?'Administrativo':'Suprimentos',situacao:sit,regional:regs[i%regs.length],fornecedorNome:forns[i%forns.length],vlrOrig:Math.round(5000+Math.random()*95000),ano:'2026',mes:String(Math.ceil((i%12)+1)).padStart(2,'0'),cc:'CC'+String(100+i%20),ccNome:'Centro '+String(100+i%20),diasAberto:Math.round(Math.random()*60),emissao:'2026-0'+String(Math.ceil((i%9)+1))+'-01',tipo:i%2===0?'Serviço':'Produto'});
  }
  var scServ=[];
  var scSits=['Não Cotada','Gerou O.C.','Atendida','Em Cotação'];
  for(var j=0;j<80;j++){
    scServ.push({solicitacao:'SC'+String(10000+j),tipoSC:'Serviço',situacao:scSits[j%4],regional:regs[j%regs.length],ccNome:'Centro '+String(100+j%15),compradorResp:comps[j%comps.length],setorSol:'Suprimentos',descricao:'Serviço de manutenção '+j,aging:Math.round(Math.random()*45),data:'2026-04-01',nivelRisco:j%10===0?'Critico':j%5===0?'Atencao':'Normal',risco:j%10===0?18:j%5===0?10:4,ano:'2026',mes:'04',cc:'CC'+String(100+j%15)});
  }
  var scProd=[];
  for(var k=0;k<60;k++){
    scProd.push({solicitacao:'SC'+String(20000+k),tipoSC:'Produto',situacao:scSits[k%4],regional:regs[k%regs.length],ccNome:'Centro '+String(100+k%15),compradorResp:comps[k%comps.length],setorSol:'Suprimentos',descricao:'Produto farmacêutico '+k,aging:Math.round(Math.random()*30),data:'2026-04-15',nivelRisco:k%8===0?'Critico':k%4===0?'Atencao':'Normal',risco:k%8===0?16:k%4===0?9:3,ano:'2026',mes:'04',cc:'CC'+String(100+k%15)});
  }
  var saving=[];
  for(var s=0;s<30;s++){
    var vi=Math.round(20000+Math.random()*80000);
    var sv=Math.round(vi*(0.05+Math.random()*0.15));
    saving.push({filialOC:'F0'+s+'-OC'+s,comprador:comps[s%comps.length],setor:s%5===0?'Administrativo':'Suprimentos',fornecedor:forns[s%forns.length],descricao:'Negociação '+s,vlrInicial:vi,saving:sv,modo:s%2===0?'CONTRATO':'OC',ano:'2026',mes:'04'});
  }
  var chamados=[];
  var priors=['Crítica','Alta','Média','Normal','Baixa'];
  var statsCh=['Encaminhado','Em Análise','Aguardando Orçamento','Encerrado','Em Execução'];
  var motivos=['Ar-condicionado','Elétrica','Hidráulica','Limpeza','TI','Estrutura'];
  for(var c=0;c<50;c++){
    chamados.push({protocolo:'P'+String(10000+c),local:'Loja '+String(c%20+1),uf:regs[c%regs.length],motivo:motivos[c%motivos.length],status:statsCh[c%statsCh.length],prioridade:priors[c%priors.length],aging:Math.round(Math.random()*40),aberto:c%4!==0,sc:c%3===0?'SC'+String(10000+c):'',dtAbertura:'2026-04-'+String(Math.ceil(c%28+1)).padStart(2,'0'),tipo:c%3===0?'Preventivo':'Corretivo',ano:'2026'});
  }
  // Indicadores agregados
  var tvT=ocs.reduce(function(a,o){return a+o.vlrOrig;},0);
  var liqV=ocs.filter(function(o){return o.situacao==='Liquidado';}).reduce(function(a,o){return a+o.vlrOrig;},0);
  var compMap={};ocs.forEach(function(o){var k=o.comprador;if(!compMap[k])compMap[k]={nome:k,setor:o.setor,qtd:0,valor:0};compMap[k].qtd++;compMap[k].valor+=o.vlrOrig;});
  var compArr=Object.values(compMap).sort(function(a,b){return b.valor-a.valor;});
  var fornMap={};ocs.forEach(function(o){var k=o.fornecedorNome;if(!fornMap[k])fornMap[k]={nome:k,qtd:0,valor:0};fornMap[k].qtd++;fornMap[k].valor+=o.vlrOrig;});
  var fornArr=Object.values(fornMap).sort(function(a,b){return b.valor-a.valor;});
  var acum=0;fornArr.forEach(function(f){acum+=f.valor;f.pctAcum=tvT?Math.round(acum/tvT*1000)/10:0;f.abc=f.pctAcum<=80?'A':f.pctAcum<=95?'B':'C';});
  var sitQ={},sitV2={};ocs.forEach(function(o){sitQ[o.situacao]=(sitQ[o.situacao]||0)+1;sitV2[o.situacao]=(sitV2[o.situacao]||0)+o.vlrOrig;});
  var savTotal=saving.reduce(function(a,s){return a+s.saving;},0);
  var savVI=saving.reduce(function(a,s){return a+s.vlrInicial;},0);
  var savRate=savVI?Math.round(savTotal/savVI*1000)/10:0;
  var savByComp={};saving.forEach(function(s){var k=s.comprador;if(!savByComp[k])savByComp[k]={nome:k,saving:0,vlrInicial:0};savByComp[k].saving+=s.saving;savByComp[k].vlrInicial+=s.vlrInicial;});
  var savCompArr=Object.values(savByComp).map(function(s){s.rate=s.vlrInicial?Math.round(s.saving/s.vlrInicial*1000)/10:0;s.nome=s.nome.split(' ')[0];return s;}).sort(function(a,b){return b.saving-a.saving;});
  var savSetor={};saving.forEach(function(s){savSetor[s.setor]=(savSetor[s.setor]||0)+s.saving;});
  var savModo={};saving.forEach(function(s){savModo[s.modo]=(savModo[s.modo]||0)+s.saving;});
  var nfOCs=ocs.filter(function(o){return o.situacao==='Não Fechado';});
  var chamAbertos=chamados.filter(function(c){return c.aberto;});
  var recMap={};chamados.forEach(function(c){var k=c.local+'|'+c.motivo;if(!recMap[k])recMap[k]={loja:c.local,uf:c.uf,motivo:c.motivo,count:0,abertos:0,maxAging:0};recMap[k].count++;if(c.aberto){recMap[k].abertos++;recMap[k].maxAging=Math.max(recMap[k].maxAging,c.aging||0);}});
  var rec=Object.values(recMap).filter(function(r){return r.count>=2;}).sort(function(a,b){return b.count-a.count;});
  var chamSts={},chamPrior={},chamMes={},chamMotivo={};
  chamados.forEach(function(c){chamSts[c.status]=(chamSts[c.status]||0)+1;chamPrior[c.prioridade]=(chamPrior[c.prioridade]||0)+1;chamMes['Abr/26']=(chamMes['Abr/26']||0)+1;chamMotivo[c.motivo]=(chamMotivo[c.motivo]||0)+1;});
  var chamAgBuckets={'0-3d':8,'4-15d':18,'+15d':14,'+30d':6,'+60d':4};
  compArr.forEach(function(c){c.irreg=ocs.filter(function(o){return o.comprador===c.nome&&o.situacao==='Não Fechado';}).length;c.saving=savByComp[c.nome]?savByComp[c.nome].saving:0;c.savingRate=savByComp[c.nome]?savByComp[c.nome].rate:null;});
  return {ok:true,ts:Date.now(),
    ocs:ocs,scServ:scServ,scProd:scProd,saving:saving,chamados:chamados,
    ind:{
      totalValor:tvT,totalSaving:savTotal,totalVI:savVI,savingRate:savRate,
      bkServCount:scServ.filter(function(s){return s.situacao==='Não Cotada';}).length,
      bkProdCount:scProd.filter(function(s){return s.situacao==='Não Cotada';}).length,
      agMaxServ:42,nfCount:nfOCs.length,totalNF:nfOCs.reduce(function(a,o){return a+o.vlrOrig;},0),
      chamSemSC:8,chamTotal:chamados.length,chamAbertos:chamAbertos.length,
      chamCrit:chamados.filter(function(c){return c.prioridade==='Crítica'&&c.aberto;}).length,
      chamAprov:3,chamConc:chamados.filter(function(c){return!c.aberto;}).length,
      chamAgMed:12,chamComSC:chamados.filter(function(c){return c.sc;}).length,
      chamCorr:chamados.filter(function(c){return c.tipo==='Corretivo';}).length,
      chamPrev:chamados.filter(function(c){return c.tipo==='Preventivo';}).length,
      fornAtivos:forns.length,
      compradores:compArr,fornecedores:fornArr,
      porSituacao:sitQ,porSituacaoValor:sitV2,
      savCompArr:savCompArr,savPorSetor:savSetor,savPorModo:savModo,
      naoFechado:nfOCs.slice(0,20).map(function(o){return{filialOC:o.filialOC,tipo:o.tipo,comprador:o.comprador,fornecedorNome:o.fornecedorNome,vlrOrig:o.vlrOrig,diasAberto:o.diasAberto};}),
      bkServRisco:scServ.filter(function(s){return s.situacao==='Não Cotada';}).map(function(s){return Object.assign({},s,{bonusRecorr:0});}),
      bkPorCC:{},bkPorComp:{},
      chamPorStatus:chamSts,chamPorPrior:chamPrior,chamPorMes:chamMes,chamPorMotivo:chamMotivo,
      chamAgBuckets:chamAgBuckets,chamCorr:30,chamPrev:20,
      recorrencia:rec,alertasRiscoCH:chamAbertos.slice(0,10).map(function(c){return Object.assign({},c,{risco:12,nivelRisco:'Atencao'});}),
    }
  };
}
function fN(v){return Math.round(Number(v)||0).toLocaleString('pt-BR');}
function fR(v){return'R$\u00a0'+Math.round(Number(v)||0).toLocaleString('pt-BR');}
function fRd(v){return'R$\u00a0'+(Number(v)||0).toLocaleString('pt-BR',{minimumFractionDigits:2,maximumFractionDigits:2});}
function pct(a,b){return b?((a/b)*100).toFixed(1)+'%':'—';}
function sN(n){var p=(n||'').split(' ').filter(Boolean);return p.length?p[0]+(p.length>1?' '+p[p.length-1][0]+'.':''):n||'—';}
function $(id){return document.getElementById(id);}
function sh(id,h){var e=$(id);if(e)e.innerHTML=h;}
function tagSit(s){var m={'Liquidado':'tg','Aberto Total':'tb','Não Fechado':'ta','Cancelado':'tn','Não Cotada':'tr2','Gerou O.C.':'tg','CONTRATO':'tb','OC':'tn','Contrato':'tb','Serviço':'tb','Produto':'tg','Misto':'ta','Atendida':'tg'};return'<span class="tag '+(m[s]||'tn')+'">'+s+'</span>';}
function agH(d){if(d===null||d===undefined)return'<span class="aok">—</span>';return d<=3?'<span class="aok">'+fN(d)+'d</span>':d<=15?'<span class="awa">'+fN(d)+'d</span>':'<span class="acr">'+fN(d)+'d</span>';}
function prTag(p){var m={'Crítica':'tr2','Urgente':'tr2','Alta':'ta','Média':'tb','Normal':'tb','Baixa':'tg'};return'<span class="tag '+(m[p]||'tn')+'">'+p+'</span>';}
function riscoTag(nivel){
  var m={'Critico':'tr2 Crítico','Atencao':'ta Atenção','Normal':'tb Normal'};
  var parts=(m[nivel]||'tn Normal').split(' ');
  return'<span class="tag '+parts[0]+'">'+parts[1]+'</span>';
}
function kH(c,l,v,s){return'<div class="kpi '+c+'"><div class="klb">'+l+'</div><div class="kv">'+v+'</div><div class="ks">'+(s||'')+'</div></div>';}
Chart.defaults.color='#94a3b8';Chart.defaults.font.family="'Segoe UI',system-ui,sans-serif";Chart.defaults.font.size=11;
var GC='rgba(0,0,0,.05)';
var C={r:'#b91c1c',b:'#1e40af',g:'#166534',a:'#92400e',r2:'rgba(185,28,28,.6)',b2:'rgba(30,64,175,.6)',g2:'rgba(22,101,52,.6)',a2:'rgba(146,64,14,.6)',pal:['#1e40af','#b91c1c','#166534','#92400e','#6d28d9','#0e7490','#c2410c','#475569','#0f766e','#be185d']};
function mk(id,cfg){var el=$(id);if(!el)return null;if(CH[id]){CH[id].destroy();delete CH[id];}try{CH[id]=new Chart(el,cfg);}catch(e){}return CH[id]||null;}
var CUR='exec';
function ir(id,btn){
  // Salvar DOM da aba atual antes de sair
  if(CUR&&_tabCache[CUR]){
    var curView=$('view-'+CUR);
    if(curView)_tabCache[CUR].domHtml=curView.innerHTML;
  }
  CUR=id;
  document.querySelectorAll('.view').forEach(function(v){v.classList.remove('on');});
  document.querySelectorAll('.sb-tab').forEach(function(b){b.classList.remove('on');});
  var v=$('view-'+id);if(v)v.classList.add('on');
  if(btn)btn.classList.add('on');
  showFbar(id);
  if(D)renderTab(id);
}
// -- FILTER BAR POR ABA --
var MESES_NOME=['01 — Janeiro','02 — Fevereiro','03 — Março','04 — Abril','05 — Maio','06 — Junho','07 — Julho','08 — Agosto','09 — Setembro','10 — Outubro','11 — Novembro','12 — Dezembro'];
var FBARS={
  exec:[{t:'sel',id:'gAno',ph:'Ano',src:'anos'},{t:'sel',id:'gMes',ph:'Mês',opts:MESES_NOME},{t:'sel',id:'gComp',ph:'Comprador',src:'comps'},{t:'sel',id:'gSetor',ph:'Setor',opts:['Suprimentos','Administrativo']},{t:'sel',id:'gReg',ph:'Regional',src:'regs'}],
  garg:[{t:'sel',id:'gAno',ph:'Ano',src:'anos'},{t:'sel',id:'gMes',ph:'Mês',opts:MESES_NOME},{t:'sel',id:'gComp',ph:'Comprador Resp.',src:'comps_garg'},{t:'sel',id:'gReg',ph:'Regional',src:'regs'},{t:'inp',id:'gCC',ph:'Centro de Custo'}],
  dem:[{t:'sel',id:'gAno',ph:'Ano',src:'anos'},{t:'sel',id:'gMes',ph:'Mês',opts:MESES_NOME},{t:'sel',id:'gReg',ph:'Regional',src:'regs'},{t:'sel',id:'gComp',ph:'Comprador',src:'comps_garg'},{t:'sel',id:'gRisco',ph:'Fator de Risco',opts:['Critico','Atencao','Normal']}],
  ocs:[{t:'sel',id:'gAno',ph:'Ano',src:'anos'},{t:'sel',id:'gMes',ph:'Mês',opts:MESES_NOME},{t:'sel',id:'gReg',ph:'Regional',src:'regs'},{t:'inp',id:'gCC',ph:'Centro de Custo'},{t:'inp',id:'gTxt',ph:'FILIAL-OC, fornecedor...'}],
  perf:[{t:'sel',id:'gAno',ph:'Ano',src:'anos'},{t:'sel',id:'gMes',ph:'Mês',opts:MESES_NOME}],
  fin:[{t:'sel',id:'gAno',ph:'Ano',src:'anos'},{t:'sel',id:'gMes',ph:'Mês',opts:MESES_NOME},{t:'sel',id:'gReg',ph:'Regional',src:'regs'},{t:'sel',id:'gSit',ph:'Situação',opts:['Liquidado','Aberto Total','Não Fechado','Cancelado']},{t:'sel',id:'gSetor',ph:'Setor',opts:['Suprimentos','Administrativo']}],
  sav:[{t:'sel',id:'gAno',ph:'Ano',src:'anos_sav'},{t:'sel',id:'gMes',ph:'Mês',opts:MESES_NOME},{t:'sel',id:'gComp',ph:'Comprador',src:'comps_sav'},{t:'sel',id:'gSetor',ph:'Setor',opts:['Suprimentos','Administrativo']}],
  obras:[],viagens:[],
  cham:[{t:'sel',id:'gAno',ph:'Ano',src:'anos_ch'},{t:'sel',id:'gUF',ph:'UF',src:'ufs'},{t:'sel',id:'gReg',ph:'Regional',src:'regs_ch'},{t:'inp',id:'gProtocolo',ph:'Nº Protocolo'},{t:'inp',id:'gSC',ph:'Nº SC'}],
};
var META={};
function toggleSb(){var sb=$('sidebar');if(sb)sb.classList.toggle('open');}
function showFbar(view){
  var fb=$('fbar');if(!fb)return;
  var defs=FBARS[view]||[];
  if(!defs.length){fb.innerHTML='<div style="font-size:11px;color:var(--tx3);text-align:center;padding:8px 0">Sem filtros nesta aba</div>';sh('fcnt','');return;}
  var html='';
  defs.forEach(function(d){
    var lbl=d.ph||'';
    html+='<div class="sb-group"><div class="sb-label">'+lbl+'</div>';
    if(d.t==='inp'){
      html+='<input class="fi" id="'+d.id+'" placeholder="'+lbl+'..." oninput="gApply()">';
    } else {
      var arr=d.src?(META[d.src]||[]):(d.opts||[]);
      html+='<select class="fi" id="'+d.id+'" onchange="gApply()"><option value="">Todos</option>'+arr.map(function(v){return'<option value="'+v+'">'+v+'</option>';}).join('')+'</select>';
    }
    html+='</div>';
  });
  fb.innerHTML=html;
  // NÃO chama gApply aqui — evita invalidar cache ao só trocar de aba
}
function gGet(){
  var ids=['gAno','gMes','gComp','gSetor','gReg','gSit','gUF','gTxt','gCC','gRisco','gProtocolo','gSC'];
  var f={};
  ids.forEach(function(id){var e=$(id);if(e)f[id]=(e.value||'').trim();});
  // gMes pode vir como "01 — Janeiro" → extrair só "01"
  if(f.gMes&&f.gMes.length>2)f.gMes=f.gMes.substring(0,2);
  return f;
}
function gApply(){if(D){_invalidarAbas(null);renderTab(CUR);}}
function gClear(){
  ['gAno','gMes','gComp','gSetor','gReg','gSit','gUF','gTxt','gCC','gRisco','gProtocolo','gSC'].forEach(function(id){
    var e=$(id);if(!e)return;e.value='';
  });
  _invalidarAbas(null);
  if(D)renderTab(CUR);
}
function fOCs(inclAdm){
  var f=gGet();
  return(D.ocs||[]).filter(function(o){
    // Por padrão, oculta Administrativo (OCs tab = somente Suprimentos)
    if(!inclAdm&&o.setor==='Administrativo'&&!f.gSetor)return false;
    return(!f.gAno||o.ano===f.gAno)&&(!f.gMes||o.mes===f.gMes)&&
      (!f.gSetor||o.setor===f.gSetor)&&(!f.gReg||o.regional===f.gReg)&&
      (!f.gSit||o.situacao===f.gSit)&&
      (!f.gCC||((o.ccNome||'').toLowerCase().includes(f.gCC.toLowerCase())||(o.cc||'').toLowerCase().includes(f.gCC.toLowerCase())))&&
      (!f.gTxt||(o.filialOC+' '+o.fornecedorNome+' '+o.cc+' '+o.ccNome).toLowerCase().includes(f.gTxt.toLowerCase()));
  });
}
function fSav(){var f=gGet();return(D.saving||[]).filter(function(s){return(!f.gAno||s.ano===f.gAno)&&(!f.gMes||s.mes===f.gMes)&&(!f.gSetor||s.setor===f.gSetor)&&(!f.gComp||s.comprador.toLowerCase().startsWith(f.gComp.toLowerCase().split(' ')[0]));});}
function fCham(){
  var f=gGet();
  return(D.chamados||[]).filter(function(c){
    return(!f.gAno||c.ano===f.gAno)&&
      (!f.gUF||c.uf===f.gUF)&&
      (!f.gReg||c.uf===f.gReg)&&
      (!f.gProtocolo||!f.gProtocolo||c.protocolo.includes(f.gProtocolo))&&
      (!f.gSC||c.sc.includes(f.gSC));
  });
}
// Client-side risco computation (mirrors backend calcRiscoArr)
var TIPO_PESO_JS=[
  {kw:['refriger','ar-cond','hvac','split'],peso:5},
  {kw:['eletric','quadro','disjuntor','tomada','iluminac'],peso:5},
  {kw:['hidraul','encanam','vazament','esgoto'],peso:4},
  {kw:['elevador','plataform','acessib'],peso:4},
  {kw:['seguranc','alarme','cftv','camera','incendio'],peso:4},
  {kw:['estrutur','civil','telhado','parede','piso','pintura'],peso:3},
  {kw:['ti ','sistema','rede','computad','servidor','internet'],peso:3},
  {kw:['limpeza','dedetiz','sanitari','higieniz'],peso:2},
];
function jsPesoTipo(t){var tl=(t||'').toLowerCase();for(var i=0;i<TIPO_PESO_JS.length;i++){var kws=TIPO_PESO_JS[i].kw;for(var j=0;j<kws.length;j++){if(tl.indexOf(kws[j])!==-1)return TIPO_PESO_JS[i].peso;}}return 1;}
function jsMultAging(d){if(!d||d<=3)return 1;if(d<=7)return 1.5;if(d<=15)return 2;if(d<=30)return 3;return 4;}
function jsNivelRisco(score){if(score>=15)return'Critico';if(score>=8)return'Atencao';return'Normal';}
function enrichRisco(s){var score=Math.round(jsPesoTipo((s.descricao||'')+(s.servico||''))*jsMultAging(s.aging||0));s.risco=score;s.nivelRisco=jsNivelRisco(score);return s;}

function fDem(){
  var f=gGet();
  var allSC=[].concat(D.scServ||[],D.scProd||[]);
  return allSC.filter(function(s){
    return(!f.gAno||s.ano===f.gAno)&&(!f.gMes||s.mes===f.gMes)&&
      (!f.gReg||s.regional===f.gReg)&&
      (!f.gComp||!s.compradorResp||s.compradorResp.toLowerCase().startsWith(f.gComp.toLowerCase().split(' ')[0]))&&
      (!f.gRisco||s.nivelRisco===f.gRisco);
  });
}
// -- INIT --
function limparEtentar(){
  if(isGAS()){
    google.script.run
      .withSuccessHandler(function(){setTimeout(init,500);})
      .withFailureHandler(function(){init();})
      .limparCache();
  } else { init(); }
}
function isGAS(){
  try{return typeof google!=='undefined'&&typeof google.script!=='undefined'&&typeof google.script.run!=='undefined';}
  catch(e){return false;}
}
function init(){
  var btn=$('btnR');if(btn){btn.disabled=true;btn.textContent='...';}
  var ve=$('view-exec');
  if(ve)ve.innerHTML='<div class="load"><div class="spin"></div><div class="ltx">Carregando dados...</div></div>';
  document.querySelectorAll('.view').forEach(function(v){v.classList.remove('on');});
  if(ve)ve.classList.add('on');
  function onOk(data){
    if(!data||!data.ok){
      var msg=data&&data.error?data.error:'Resposta inválida do servidor';
      onErr(msg);return;
    }
    D=data;
    console.log('[DIAG] ocs:',D.ocs&&D.ocs.length,'scServ:',D.scServ&&D.scServ.length,'scProd:',D.scProd&&D.scProd.length,'fromCache:',D.fromCache||false);
    restoreExec();buildMeta();showFbar(CUR);renderTab(CUR);
    sh('bTs',D.gerado||(D.ts?new Date(D.ts).toLocaleString('pt-BR'):''));
    if(btn){btn.disabled=false;btn.textContent='↺';}
  }
  function onErr(msg){
    var diag='GAS: '+isGAS()+' | '+new Date().toLocaleTimeString('pt-BR');
    if(ve)ve.innerHTML='<div class="errw">'+
      '<div class="errt">Falha ao carregar dados</div>'+
      '<div class="errm">'+msg+'</div>'+
      '<div style="font-size:10px;color:#94a3b8;margin-top:8px">'+diag+'</div>'+
      '<div style="margin-top:12px;display:flex;gap:8px">'+
        '<button onclick="init()" style="padding:6px 14px;background:var(--blue);color:#fff;border:none;border-radius:5px;cursor:pointer;font-size:12px">Tentar novamente</button>'+
        '<button onclick="limparEtentar()" style="padding:6px 14px;background:var(--amb);color:#fff;border:none;border-radius:5px;cursor:pointer;font-size:12px">Limpar cache e tentar</button>'+
      '</div>'+
    '</div>';
    if(btn){btn.disabled=false;btn.textContent='↺';}
  }  // Dentro do Apps Script: usa google.script.run (sem CORS, sem fetch)
  if(isGAS()){
    google.script.run
      .withSuccessHandler(function(json){
        try{onOk(JSON.parse(json));}
        catch(ex){onErr('Erro ao interpretar dados: '+ex.message);}
      })
      .withFailureHandler(function(ex){onErr(ex.message||'Erro no servidor');})
      .getDadosJSON();
    return;
  }
  // Fora do Apps Script: usa mock data para preview
  if(!SCRIPT_URL||SCRIPT_URL==='COLE_SUA_URL_AQUI'){
    setTimeout(function(){onOk(getMockData());},300);
    return;
  }
  fetch(SCRIPT_URL+'?action=dados&t='+Date.now())
    .then(function(r){if(!r.ok)throw new Error('HTTP '+r.status);return r.json();})
    .then(onOk)
    .catch(function(e){onErr('Erro de rede: '+e.message);});
}
function buildMeta(){
  var ocs=D.ocs||[];var ch=D.chamados||[];var sv=D.saving||[];
  var allSC=[].concat(D.scServ||[],D.scProd||[]);
  META.anos=Array.from(new Set(ocs.map(function(o){return o.ano;}).filter(Boolean))).sort().reverse();
  META.meses=Array.from(new Set(ocs.map(function(o){return o.mes;}).filter(Boolean))).sort().reverse();
  META.comps=Array.from(new Set(ocs.map(function(o){return o.comprador;}).filter(Boolean))).sort().map(sN);
  META.regs=Array.from(new Set(ocs.map(function(o){return o.regional;}).filter(Boolean).filter(function(v){return v&&v!=='N/D';}))).sort();
  META.ufs=Array.from(new Set(ch.map(function(c){return c.uf;}).filter(Boolean).filter(function(v){return v&&v!=='N/D';}))).sort();
  META.regs_ch=Array.from(new Set(ch.map(function(c){return c.uf;}).filter(Boolean).filter(function(v){return v&&v!=='N/D';}))).sort();
  META.anos_ch=Array.from(new Set(ch.map(function(c){return c.ano;}).filter(Boolean))).sort().reverse();
  META.anos_sav=Array.from(new Set(sv.map(function(s){return s.ano;}).filter(Boolean))).sort().reverse();
  META.meses_sav=Array.from(new Set(sv.map(function(s){return s.mes;}).filter(Boolean))).sort().reverse();
  META.comps_sav=Array.from(new Set(sv.map(function(s){return s.comprador;}).filter(Boolean))).sort();
  META.comps_garg=Array.from(new Set(allSC.map(function(s){return s.compradorResp;}).filter(Boolean))).sort();
  META.ccs=Array.from(new Set(allSC.map(function(s){return s.ccNome||s.cc;}).filter(Boolean).filter(function(v){return v&&v!=='N/D';}))).sort();
  // OC local filters
  var allComps=Array.from(new Set(ocs.map(function(o){return o.comprador;}).filter(Boolean))).sort();
  var fOCCp=$('fOCCp');if(fOCCp)fOCCp.innerHTML='<option value="">Comprador</option>'+allComps.map(function(c){return'<option value="'+c+'">'+sN(c)+'</option>';}).join('');
  var allRegs=Array.from(new Set(ocs.map(function(o){return o.regional;}).filter(Boolean).filter(function(v){return v&&v!=='N/D';}))).sort();
  var fOCReg=$('fOCReg');if(fOCReg)fOCReg.innerHTML='<option value="">Regional</option>'+allRegs.map(function(r){return'<option value="'+r+'">'+r+'</option>';}).join('');
  // Saving local
  var scs=Array.from(new Set(sv.map(function(s){return s.comprador;}).filter(Boolean))).sort();
  var fSC=$('fSavC');if(fSC)fSC.innerHTML='<option value="">Comprador</option>'+scs.map(function(c){return'<option value="'+c+'">'+c+'</option>';}).join('');
  // Chamados local
  var chSts=Array.from(new Set(ch.map(function(c){return c.status;}).filter(Boolean))).sort();
  var chPrs=Array.from(new Set(ch.map(function(c){return c.prioridade;}).filter(Boolean))).sort();
  var fCSt=$('fChSt');if(fCSt)fCSt.innerHTML='<option value="">Status</option>'+chSts.map(function(s){return'<option value="'+s+'">'+s+'</option>';}).join('');
  var fCPr=$('fChPr');if(fCPr)fCPr.innerHTML='<option value="">Prioridade</option>'+chPrs.map(function(p){return'<option value="'+p+'">'+p+'</option>';}).join('');
  // Demanda regional
  var demRegs=Array.from(new Set(allSC.map(function(s){return s.regional;}).filter(function(v){return v&&v!=='N/D';}))).sort();
  var fDR=$('fDemReg');if(fDR)fDR.innerHTML='<option value="">Regional</option>'+demRegs.map(function(r){return'<option value="'+r+'">'+r+'</option>';}).join('');
}

function restoreExec(){
  sh('view-exec',
    '<div class="stit">Visão Executiva</div>'+
    '<div class="krow k4" id="kE"></div><div class="alrows" id="aE"></div>'+
    '<div class="grid g2" style="margin-bottom:10px">'+
      '<div class="card"><div class="ch"><span class="ct">OCs por Status — Aberto / Aprovação / Cancelado</span></div><div class="cb"><div class="cv h185"><canvas id="cESt"></canvas></div></div></div>'+
      '<div class="card"><div class="ch"><span class="ct">Distribuição por Comprador</span></div><div class="cb"><div class="cv h185"><canvas id="cECp"></canvas></div></div></div>'+
    '</div>'+
    '<div class="grid g2" style="margin-bottom:10px">'+
      '<div class="card"><div class="ch"><span class="ct">Demanda Pendente por Comprador</span><span style="font-size:9.5px;color:var(--tx3);margin-left:6px">SC Não Cotada / Em Cotação</span></div><div class="cb"><div class="cv h185"><canvas id="cEDemComp"></canvas></div></div></div>'+
      '<div class="card"><div class="ch"><span class="ct">Status Gerencial</span></div><div class="cb" id="eStL" style="padding-top:6px"></div></div>'+
    '</div>'+
    '<div class="grid g3">'+
      '<div class="card"><div class="ch"><span class="ct">Top 10 Fornecedores</span></div><div class="cb"><div class="cv h250"><canvas id="cEF5"></canvas></div></div></div>'+
      '<div class="card"><div class="ch"><span class="ct">Saving por Comprador</span></div><div class="cb" id="eSavL" style="padding-top:4px"></div></div>'+
      '<div class="card"><div class="ch"><span class="ct">Distribuição por Comprador — Valor OC</span></div><div class="cb"><div class="cv h160"><canvas id="cECp2"></canvas></div></div></div>'+
    '</div>'
  );
}
// Renderiza apenas a aba ativa — navegação instantânea
// -- CACHE DE RENDER POR ABA --
// _tabCache[id] = {fkey: string, charts: [...]} 
// O HTML fica no próprio DOM — ao sair da aba guardamos o innerHTML, ao voltar restauramos
var _tabCache = {};    // {id: {fkey, domHtml, charts}}
var _tabDirty = {};    // {id: true} quando precisa re-render

function _fkey(){
  // Chave que representa o estado atual dos filtros globais
  var f=gGet();
  return [f.gAno,f.gMes,f.gComp,f.gSetor,f.gReg,f.gSit,f.gCC,f.gTxt,f.gRisco].join('|');
}

function _invalidarAbas(ids){
  // Marca abas como dirty — ids=null invalida todas
  var todas=['exec','garg','dem','ocs','perf','fin','sav','cham'];
  var alvo=ids||todas;
  alvo.forEach(function(id){_tabDirty[id]=true;});
}

function renderAll(){if(!D)return;_invalidarAbas(null);renderTab(CUR);}

function renderTab(id){
  var map={exec:rExec,garg:rGarg,dem:rDem,ocs:rOCs,perf:rPerf,fin:rFin,sav:rSav,cham:rCham};
  if(!map[id])return;

  var fk=_fkey();
  var cached=_tabCache[id];

  // Cache hit: mesmo estado de filtros e não marcada como dirty
  if(cached&&cached.fkey===fk&&!_tabDirty[id]){
    var view=document.getElementById('view-'+id);
    if(view&&cached.domHtml){
      view.innerHTML=cached.domHtml;
      // Recriar charts destruídos ao trocar innerHTML
      if(cached.charts&&cached.charts.length){
        cached.charts.forEach(function(cfg){
          var el=document.getElementById(cfg.id);
          if(el)mk(cfg.id,cfg.config);
        });
      }
    }
    return;
  }

  // Cache miss: renderiza normalmente
  // Captura os charts criados durante o render
  var _chartsThisRender=[];
  var _mkOrig=mk;
  mk=function(id,cfg){
    _chartsThisRender.push({id:id,config:cfg});
    _mkOrig(id,cfg);
  };

  map[id]();

  mk=_mkOrig;

  // Salva resultado no cache
  var view=document.getElementById('view-'+id);
  _tabCache[id]={
    fkey:fk,
    domHtml:view?view.innerHTML:'',
    charts:_chartsThisRender,
  };
  _tabDirty[id]=false;
}
// -- EXECUTIVO --
function rExec(){
  var ind=D.ind;var ocs=fOCs();
  var tv=ocs.reduce(function(s,o){return s+o.vlrOrig;},0);
  var tl=ocs.filter(function(o){return o.situacao==='Liquidado';}).reduce(function(s,o){return s+o.vlrOrig;},0);
  var ta=ocs.filter(function(o){return o.situacao==='Aberto Total';}).reduce(function(s,o){return s+o.vlrOrig;},0);
  var nNF=ocs.filter(function(o){return o.situacao==='Não Fechado';}).length;
  var vNF=ocs.filter(function(o){return o.situacao==='Não Fechado';}).reduce(function(s,o){return s+o.vlrOrig;},0);
  var txL=tv?Math.round(tl/tv*1000)/10:0;
  // Saving filtrado pelo mesmo gAno/gMes dos filtros globais
  var f=gGet();
  var savFilt=(D.saving||[]).filter(function(s){
    return(!f.gAno||s.ano===f.gAno)&&(!f.gMes||s.mes===f.gMes)&&(!f.gReg||s.regional===f.gReg);
  });
  var savTotal=savFilt.reduce(function(a,s){return a+s.saving;},0);
  var savVI=savFilt.reduce(function(a,s){return a+s.vlrInicial;},0);
  var savRate=savVI?Math.round(savTotal/savVI*1000)/10:0;

  sh('kE',kH('kb','OCs',fN(ocs.length),Array.from(new Set(ocs.map(function(o){return o.filial;}))).length+' filiais')+kH('kb','Valor Total',fR(tv),'')+kH('kg','Liquidado',fR(tl),txL+'%')+kH('ka','Comprometido',fR(ta),'Aberto Total')+kH('kr','Backlog SC',fN((ind.bkServCount||0)+(ind.bkProdCount||0)),(ind.bkServCount||0)+' serv · '+(ind.bkProdCount||0)+' prod')+kH('ka','Pend. Aprovação',fN(nNF),fR(vNF))+kH('kg','Saving',fR(savTotal),savRate+'%')+kH('kr','Cham. Críticos',fN(ind.chamCrit),''));
  sh('fcnt',fN(ocs.length)+' OCs · '+fR(tv));
  var alts=[];
  var tc=ind.compradores&&ind.compradores[0];
  if(tc&&tv&&tc.valor/tv>0.5)alts.push({c:'ar',b:true,t:'⚡ '+sN(tc.nome)+': '+pct(tc.valor,tv)+' do valor total — concentração crítica'});
  if((ind.bkServCount||0)+(ind.bkProdCount||0)>20)alts.push({c:'ar',b:true,t:fN((ind.bkServCount||0)+(ind.bkProdCount||0))+' SCs sem cotação ('+fN(ind.bkServCount||0)+' serviços · '+fN(ind.bkProdCount||0)+' produtos)'});
  if(nNF>0)alts.push({c:'aa',b:false,t:fN(nNF)+' OCs pendentes de aprovação — '+fR(vNF)});
  if(ind.chamCrit>0)alts.push({c:'ar',b:true,t:fN(ind.chamCrit)+' chamados críticos/urgentes abertos'});
  var savPeriodo=f.gAno?' ('+f.gAno+')':f.gMes?' (mes '+f.gMes+')':'';
  if(savRate>=8)alts.push({c:'ag',b:false,t:'Saving rate '+savRate+'% -- acima do benchmark 8%'+savPeriodo});
  else if(savVI>0)alts.push({c:'aa',b:false,t:'Saving rate '+savRate+'% -- abaixo do benchmark 8%'+savPeriodo});
  sh('aE',alts.map(function(a){return'<div class="al '+a.c+'"><div class="adot'+(a.b?' bl':'')+'"></div>'+a.t+'</div>';}).join(''));
  var SC={'Aberto Total':C.b,'Não Fechado':C.a,'Cancelado':'#94a3b8'};
  var sits=Object.keys(SC);
  mk('cESt',{type:'bar',data:{labels:sits,datasets:[{label:'Qtd',data:sits.map(function(k){return ind.porSituacao[k]||0;}),backgroundColor:sits.map(function(k){return SC[k]||'#94a3b8';}),borderRadius:3,yAxisID:'y'},{label:'Valor',data:sits.map(function(k){return ind.porSituacaoValor[k]||0;}),backgroundColor:sits.map(function(k){return(SC[k]||'#94a3b8')+'44';}),borderColor:sits.map(function(k){return SC[k]||'#94a3b8';}),borderWidth:1,borderRadius:3,yAxisID:'y2'}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'top',labels:{font:{size:10}}}},scales:{y:{grid:{color:GC},title:{display:true,text:'Qtd'}},y2:{position:'right',grid:{display:false},ticks:{callback:function(v){return fR(v);}}}}}});
  var cpMap={};ocs.forEach(function(o){var k=sN(o.comprador||'N/D');if(!cpMap[k])cpMap[k]={qtd:0,valor:0};cpMap[k].qtd++;cpMap[k].valor+=o.vlrOrig;});
  var cpArr=Object.keys(cpMap).map(function(k){return{k:k,qtd:cpMap[k].qtd,valor:cpMap[k].valor};}).sort(function(a,b){return b.qtd-a.qtd;}).slice(0,8);
  mk('cECp',{type:'bar',data:{labels:cpArr.map(function(x){return x.k;}),datasets:[{label:'Qtd OCs',data:cpArr.map(function(x){return x.qtd;}),backgroundColor:C.b2,borderColor:C.b,borderWidth:1,borderRadius:3},{label:'Valor',data:cpArr.map(function(x){return x.valor;}),backgroundColor:C.pal.map(function(_,i){return i===0?C.g2:'transparent';}),borderWidth:0,yAxisID:'y2',hidden:true}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:{callbacks:{label:function(ctx){return ctx.datasetIndex===0?fN(ctx.raw)+' OCs':fR(ctx.raw);}}}},scales:{x:{grid:{color:GC}},y:{grid:{display:false},ticks:{font:{size:10}}},y2:{display:false}}}});
  var cE=(ind.compradores||[]).slice(0,6);
  mk('cECp2',{type:'doughnut',data:{labels:cE.map(function(c){return sN(c.nome);}),datasets:[{data:cE.map(function(c){return c.valor;}),backgroundColor:C.pal,borderWidth:1,borderColor:'#fff',hoverOffset:4}]},options:{responsive:true,maintainAspectRatio:false,cutout:'52%',plugins:{legend:{position:'right',labels:{font:{size:10},padding:6}}}}});
  // Demanda pendente por comprador (SC Não Cotada)
  var allSCExec=[].concat(D.scServ||[],D.scProd||[]);
  var pendMap={};
  allSCExec.filter(function(s){return s.situacao==='Não Cotada'||s.situacao==='Em Cotação'||s.situacao==='Em processo de cotação';}).forEach(function(s){var k=sN(s.compradorResp||'Sem comprador');pendMap[k]=(pendMap[k]||0)+1;});
  var pendArr=Object.keys(pendMap).map(function(k){return{k:k,v:pendMap[k]};}).sort(function(a,b){return b.v-a.v;}).slice(0,10);
  mk('cEDemComp',{type:'bar',data:{labels:pendArr.map(function(x){return x.k;}),datasets:[{label:'SCs pendentes',data:pendArr.map(function(x){return x.v;}),backgroundColor:C.r2,borderColor:C.r,borderWidth:1,borderRadius:3}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:{callbacks:{label:function(ctx){return fN(ctx.raw)+' SCs não cotadas';}}}},scales:{x:{grid:{color:GC}},y:{grid:{display:false},ticks:{font:{size:10}}}}}});
  var f5=(ind.fornecedores||[]).slice(0,10);
  mk('cEF5',{type:'bar',data:{labels:f5.map(function(f){var n=f.nome||'';return n.length>22?n.slice(0,20)+'..':n;}),datasets:[{data:f5.map(function(f){return f.valor;}),backgroundColor:C.b2,borderColor:C.b,borderWidth:1,borderRadius:3}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:GC},ticks:{callback:function(v){return fR(v);}}},y:{grid:{display:false}}}}});
  var tot=ocs.length||1;
  sh('eStL',sits.map(function(k){var v=ind.porSituacao[k]||0;return'<div class="srow"><span class="slb">'+tagSit(k)+'</span><div class="sbar"><div class="sfil" style="width:'+Math.round(v/tot*100)+'%;background:'+(SC[k]||'#94a3b8')+'"></div></div><span class="sval">'+fN(v)+'</span></div>';}).join(''));
  var sl=ind.savCompArr||[];var mx=sl.length?sl[0].saving:1;
  sh('eSavL',sl.map(function(s){return'<div class="srow"><span class="slb">'+s.nome+'</span><div class="sbar" style="width:46px"><div class="sfil" style="width:'+Math.round(s.saving/mx*100)+'%;background:'+C.g+'"></div></div><span class="sval" style="color:'+C.g+'">'+fR(s.saving)+'</span><span style="color:var(--tx3);font-size:9.5px;margin-left:3px">'+s.rate+'%</span></div>';}).join(''));
}
// -- GARGALOS --
function fGarg(){
  var f=gGet();
  var scServMap={};
  (D.scServ||[]).forEach(function(s){if(s.solicitacao)scServMap[s.solicitacao]=s;});
  return(D.ind.bkServRisco||[]).map(function(s){
    var orig=scServMap[s.solicitacao]||{};
    // Chamados MAN (SC Serv) → comprador responsável é sempre Marina
    return Object.assign({},s,{
      compradorResp:'Marina',
      ccNome:s.ccNome||orig.ccNome||'—',
      regional:s.regional||orig.regional||'—',
      descricao:s.descricao||orig.descricao||'—',
      data:s.data||orig.data||'—'
    });
  }).filter(function(s){
    return(!f.gReg||s.regional===f.gReg)&&
      (!f.gMes||s.mes===f.gMes)&&
      (!f.gCC||(s.ccNome||'').toLowerCase().includes((f.gCC||'').toLowerCase()));
  });
}
function rGarg(){
  var ind=D.ind;
  var ocs=D.ocs||[];
  var ch=D.chamados||[];

  // -- Card 1: Backlog SC SERV — Nº Solicitação distintos, situação Não Cotada
  var bkServSols=new Set((D.scServ||[]).filter(function(s){return isNaoCotada(s.situacao);}).map(function(s){return s.solicitacao;}).filter(Boolean));
  var bkServCount=bkServSols.size;

  // -- Card 2: Backlog SC PROD — Nº Solicitação distintos, situação Não Cotada
  var bkProdSols=new Set((D.scProd||[]).filter(function(s){return isNaoCotada(s.situacao);}).map(function(s){return s.solicitacao;}).filter(Boolean));
  var bkProdCount=bkProdSols.size;

  // -- Card 3: Chamados MAN em Suprimentos — status encaminhado/em análise/aguardando orçamento
  var CH_SUPR=['ENCAMINHADO PARA O SUPRIMENTOS','EM ANÁLISE – SUPRIMENTOS','EM ANALISE - SUPRIMENTOS',
               'EM ANÁLISE - SUPRIMENTOS','AGUARDANDO ORÇAMENTOS – SUPRIMENTOS',
               'AGUARDANDO ORÇAMENTOS - SUPRIMENTOS'];
  var chamSupr=ch.filter(function(c){
    var st=(c.status||'').toUpperCase().trim();
    return CH_SUPR.some(function(k){return st===k||st.includes(k.split('–')[0].trim())||st.includes(k.split('-')[0].trim());});
  }).length;

  // -- Card 4: Pend. Aprovação — FILIAL-OC distintas situação Não Fechado (serv + prod)
  var pendFiliaisOC=new Set(ocs.filter(function(o){return o.situacao==='Não Fechado';}).map(function(o){return o.filialOC;}).filter(Boolean));
  var pendCount=pendFiliaisOC.size;
  var pendValor=ocs.filter(function(o){return o.situacao==='Não Fechado';}).reduce(function(s,o){return s+o.vlrOrig;},0);

  // -- Card 5: Aguardando NF — FILIAL-OC distintas situação Aberto Total
  var aguFiliaisOC=new Set(ocs.filter(function(o){return o.situacao==='Aberto Total';}).map(function(o){return o.filialOC;}).filter(Boolean));
  var aguCount=aguFiliaisOC.size;
  var aguValor=ocs.filter(function(o){return o.situacao==='Aberto Total';}).reduce(function(s,o){return s+o.vlrOrig;},0);

  sh('kG',
    kH('kr','Backlog SC Serv.',fN(bkServCount),'SCs distintas não cotadas')+
    kH('kr','Backlog SC Prod.',fN(bkProdCount),'SCs distintas não cotadas')+
    kH('ka','Chamados Suprimentos',fN(chamSupr),'encam. / análise / orçamento')+
    kH('ka','Pend. Aprovação',fN(pendCount),'FILIAL-OCs · '+fR(pendValor))+
    kH('kb','Aguardando NF',fN(aguCount),'FILIAL-OCs · '+fR(aguValor))
  );

  // -- Alertas — comprador com maior backlog (usa mesmas SCs distintas dos cards) --
  var bkCompMap={};
  bkServSols.forEach(function(sol){
    // recupera compradorResp da SC correspondente
    var sc=(D.scServ||[]).find(function(s){return s.solicitacao===sol&&s.situacao==='Não Cotada';});
    if(sc){var k=sc.compradorResp||'Sem comprador';bkCompMap[k]=(bkCompMap[k]||0)+1;}
  });
  bkProdSols.forEach(function(sol){
    var sc=(D.scProd||[]).find(function(s){return s.solicitacao===sol&&s.situacao==='Não Cotada';});
    if(sc){var k=sc.compradorResp||'Sem comprador';bkCompMap[k]=(bkCompMap[k]||0)+1;}
  });
  var topCompGarg=Object.keys(bkCompMap).filter(function(k){return k!=='Sem comprador';}).sort(function(a,b){return bkCompMap[b]-bkCompMap[a];})[0];
  var alts=[];
  if(topCompGarg)alts.push({c:'ar',t:'⚡ Maior backlog: '+sN(topCompGarg)+' — '+fN(bkCompMap[topCompGarg])+' SCs não cotadas (serv + prod)'});
  if(ind.agMaxServ>15)alts.push({c:'ar',t:'SC mais antiga há '+fN(ind.agMaxServ)+'d sem cotação — risco de desabastecimento'});
  if(pendCount>0)alts.push({c:'aa',t:fN(pendCount)+' FILIAL-OCs aguardando aprovação — '+fR(pendValor)});
  if(chamSupr>5)alts.push({c:'aa',t:fN(chamSupr)+' chamados em aberto no Suprimentos'});
  sh('aGarg',alts.map(function(a){return'<div class="al '+a.c+'"><div class="adot"></div>'+a.t+'</div>';}).join(''));

  // -- Tabela Risco SC — badge MAN + comprador destacado --
  var riscoSC=fGarg();
  if(riscoSC.length){
    sh('tRiscoSC','<thead><tr><th>Score</th><th>Risco</th><th>Nº SC</th><th>Regional</th><th>CC</th><th>Comprador Resp.</th><th>Descrição / Chamado MAN</th><th>Aging</th></tr></thead><tbody>'+
      riscoSC.slice(0,20).map(function(s){return'<tr>'+
        '<td class="mn" style="font-weight:700;color:'+(s.nivelRisco==='Critico'?C.r:s.nivelRisco==='Atencao'?C.a:C.b)+'">'+s.risco+'</td>'+
        '<td>'+riscoTag(s.nivelRisco)+'</td>'+
        '<td class="mn" style="color:var(--blue)">'+s.solicitacao+'</td>'+
        '<td>'+s.regional+'</td>'+
        '<td class="te">'+s.ccNome+'</td>'+
        '<td style="font-weight:700;color:var(--blue)">'+sN(s.compradorResp||'—')+'</td>'+
        '<td class="te" title="'+s.descricao+'"><span style="font-size:9px;background:var(--amb-l);color:var(--amb);padding:1px 4px;border-radius:2px;margin-right:4px;font-weight:700">MAN</span>'+s.descricao+'</td>'+
        '<td>'+agH(s.aging)+'</td>'+
      '</tr>';}).join('')+'</tbody>');
  }else{sh('tRiscoSC','<tbody><tr><td colspan="9" style="text-align:center;color:var(--tx3);padding:12px">Nenhuma SC crítica com os filtros aplicados</td></tr></tbody>');}

  // -- Tabela backlog por CC — serv + prod separados --
  var bkCCmap={};
  (D.scServ||[]).filter(function(s){return isNaoCotada(s.situacao);}).forEach(function(s){var k=s.ccNome||s.cc||'N/D';if(k==='N/D')return;if(!bkCCmap[k])bkCCmap[k]={serv:0,prod:0};bkCCmap[k].serv++;});
  (D.scProd||[]).filter(function(s){return isNaoCotada(s.situacao);}).forEach(function(s){var k=s.ccNome||s.cc||'N/D';if(k==='N/D')return;if(!bkCCmap[k])bkCCmap[k]={serv:0,prod:0};bkCCmap[k].prod++;});
  var ccArr2=Object.keys(bkCCmap).map(function(k){return{n:k,serv:bkCCmap[k].serv,prod:bkCCmap[k].prod,tot:bkCCmap[k].serv+bkCCmap[k].prod};}).sort(function(a,b){return b.tot-a.tot;}).slice(0,20);
  sh('tGBf','<thead><tr><th>Centro de Custo</th><th class="tdr">Serviços</th><th class="tdr">Produtos</th><th class="tdr">Total</th></tr></thead><tbody>'+
    ccArr2.map(function(x){return'<tr>'+
      '<td class="te">'+x.n+'</td>'+
      '<td class="mn tdr" style="color:'+C.b+'">'+fN(x.serv)+'</td>'+
      '<td class="mn tdr" style="color:'+C.g+'">'+fN(x.prod)+'</td>'+
      '<td class="mn tdr" style="font-weight:700;color:'+C.r+'">'+fN(x.tot)+'</td>'+
    '</tr>';}).join('')+'</tbody>');

  // -- Tabela Volume por Comprador — SC em processo de cotação (deduplicado por Nº SC) --
  var STATUS_PROC=['não cotada','em cotação','em processo de cotação','em análise','aguardando cotação','cotação em andamento','nao cotada','nao cotado'];
  function emProc(sit){var s=(sit||'').toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g,'').trim();return STATUS_PROC.some(function(p){return s.includes(p);});}
  var compDemMap={};
  // Deduplicar por solicitacao dentro de cada tipo
  var solServVistas=new Set();
  (D.scServ||[]).forEach(function(s){
    if(!emProc(s.situacao)||!s.solicitacao||solServVistas.has(s.solicitacao))return;
    solServVistas.add(s.solicitacao);
    var k=s.compradorResp||'Sem comprador';
    if(!compDemMap[k])compDemMap[k]={serv:0,prod:0};compDemMap[k].serv++;
  });
  var solProdVistas=new Set();
  (D.scProd||[]).forEach(function(s){
    if(!emProc(s.situacao)||!s.solicitacao||solProdVistas.has(s.solicitacao))return;
    solProdVistas.add(s.solicitacao);
    var k=s.compradorResp||'Sem comprador';
    if(!compDemMap[k])compDemMap[k]={serv:0,prod:0};compDemMap[k].prod++;
  });
  var compDemArr=Object.keys(compDemMap).map(function(k){return{n:k,serv:compDemMap[k].serv,prod:compDemMap[k].prod,tot:compDemMap[k].serv+compDemMap[k].prod};}).sort(function(a,b){return b.tot-a.tot;});
  sh('tGComp','<thead><tr><th>Comprador Resp.</th><th class="tdr">SC Serv.</th><th class="tdr">SC Prod.</th><th class="tdr">Total</th></tr></thead><tbody>'+
    compDemArr.map(function(x){return'<tr>'+
      '<td style="font-weight:600">'+sN(x.n)+'</td>'+
      '<td class="mn tdr" style="color:'+C.b+'">'+fN(x.serv)+'</td>'+
      '<td class="mn tdr" style="color:'+C.g+'">'+fN(x.prod)+'</td>'+
      '<td class="mn tdr" style="font-weight:700;color:'+C.r+'">'+fN(x.tot)+'</td>'+
    '</tr>';}).join('')+'</tbody>');

  // OCs em aprovação
  var nf=ind.naoFechado||[];
  sh('tNF','<thead><tr><th>FILIAL-OC</th><th>Tipo</th><th>Comprador</th><th>Fornecedor</th><th class="tdr">Vlr. Original</th><th>Dias</th></tr></thead><tbody>'+
    nf.map(function(o){return'<tr><td class="mn" style="color:var(--amb)">'+o.filialOC+'</td><td>'+tagSit(o.tipo||'—')+'</td><td>'+sN(o.comprador)+'</td><td class="te">'+o.fornecedorNome+'</td><td class="mn tdr">'+fR(o.vlrOrig)+'</td><td>'+agH(o.diasAberto)+'</td></tr>';}).join('')+'</tbody>');
  // Tabelas de aging serv + prod
  rBkTbl();rBkProdTbl();
}
function _bkCols(s){
  return '<td class="mn" style="color:var(--blue)">'+s.solicitacao+'</td>'+
    '<td class="mn">'+s.data+'</td>'+
    '<td>'+s.regional+'</td>'+
    '<td class="te">'+s.ccNome+'</td>'+
    '<td style="font-weight:600;color:var(--blue)">'+sN(s.compradorResp||'—')+'</td>'+
    '<td class="te" title="'+s.descricao+'">'+s.descricao+'</td>'+
    '<td>'+agH(s.aging)+'</td>';
}
var _bkHead='<thead><tr><th>Nº SC</th><th>Data</th><th>Regional</th><th>Centro de Custo</th><th>Comprador Resp.</th><th>Descrição</th><th>Aging</th></tr></thead>';
function _bkFilter(rows,q,ag){
  return rows.filter(function(s){
    var a=s.aging||0;
    return(!ag||(ag==='ok'&&a<=3)||(ag==='warn'&&a>3&&a<=15)||(ag==='crit'&&a>15))&&
      (!q||(s.descricao||'').toLowerCase().includes(q)||(s.ccNome||'').toLowerCase().includes(q)||(s.compradorResp||'').toLowerCase().includes(q)||(s.solicitacao||'').toLowerCase().includes(q));
  }).sort(function(a,b){return(b.aging||0)-(a.aging||0);});
}
function rBkTbl(){
  var bk=(D.scServ||[]).filter(function(s){return isNaoCotada(s.situacao);});
  var q=(($('sBk')||{}).value||'').toLowerCase();
  var ag=(($('fBkAg')||{}).value||'');
  var rows=_bkFilter(bk,q,ag);
  sh('cntBk',fN(rows.length)+' de '+fN(bk.length)+' SC serviços');
  sh('tBk',_bkHead+'<tbody>'+rows.map(function(s){return'<tr>'+_bkCols(s)+'</tr>';}).join('')+'</tbody>');
}
function rBkProdTbl(){
  var bk=(D.scProd||[]).filter(function(s){return isNaoCotada(s.situacao);});
  var q=(($('sBkP')||{}).value||'').toLowerCase();
  var ag=(($('fBkAgP')||{}).value||'');
  var rows=_bkFilter(bk,q,ag);
  sh('cntBkP',fN(rows.length)+' de '+fN(bk.length)+' SC produtos');
  sh('tBkP',_bkHead+'<tbody>'+rows.map(function(s){return'<tr>'+_bkCols(s)+'</tr>';}).join('')+'</tbody>');
}
// -- DEMANDA SC --
function fDemFiltered(){
  var all=fDem();
  var fComp=(($('fDemComp')||{}).value||'');
  var fRisco=(($('fDemRisco')||{}).value||'');
  if(fComp)all=all.filter(function(s){return(s.compradorResp||'')=== fComp;});
  if(fRisco)all=all.filter(function(s){return(s.nivelRisco||'')=== fRisco;});
  return all;
}
function rDem(){
  var all=fDem();
  var serv=all.filter(function(s){return s.tipoSC==='Serviço';});
  var prod=all.filter(function(s){return s.tipoSC==='Produto';});
  var nc=all.filter(function(s){return s.situacao==='Não Cotada';}).length;
  var gc=all.filter(function(s){return s.situacao==='Gerou O.C.'||s.situacao==='Atendida';}).length;
  var tx=all.length?Math.round(gc/all.length*1000)/10:0;
  sh('kD',
    kH('kb','Total SC',fN(all.length),fN(serv.length)+' serv · '+fN(prod.length)+' prod')+
    kH('kg','Atendidas',fN(gc),tx+'% de conversão')+
    kH('kr','Não Cotadas',fN(nc),pct(nc,all.length))+
    kH('ka','Backlog Crítico',fN(all.filter(function(s){return s.nivelRisco==='Critico';}).length),'fator de risco')+
    kH('kn','Compradores',fN(Array.from(new Set(all.map(function(s){return s.compradorResp;}).filter(Boolean))).length),'responsáveis')
  );
  sh('fcnt',fN(all.length)+' SCs');

  // Status bars
  var statuses=['Não Cotada','Gerou O.C.','Atendida','Cancelada'];
  var statC={'Não Cotada':C.r,'Gerou O.C.':C.g,'Atendida':C.g,'Cancelada':'#94a3b8'};
  var html='';
  statuses.forEach(function(st){
    var n=all.filter(function(s){return s.situacao===st;}).length;
    if(n>0){var pctV=all.length?Math.round(n/all.length*100):0;
      html+='<div class="pbar-wrap"><div class="pbar-label"><span>'+tagSit(st)+'</span><span class="mn">'+fN(n)+' ('+pctV+'%)</span></div><div class="pbar-track"><div class="pbar-fill" style="width:'+pctV+'%;background:'+(statC[st]||'#94a3b8')+'"></div></div></div>';}
  });
  sh('dStatusBars',html);

  // SC por Regional
  var regMap={};
  all.forEach(function(s){if(s.regional&&s.regional!=='N/D'){if(!regMap[s.regional])regMap[s.regional]={serv:0,prod:0};regMap[s.regional][s.tipoSC==='Serviço'?'serv':'prod']++;}});
  var regs=Object.keys(regMap);
  mk('cDreg',{type:'bar',data:{labels:regs,datasets:[{label:'Serviços',data:regs.map(function(r){return regMap[r].serv;}),backgroundColor:C.b2,borderColor:C.b,borderWidth:1,borderRadius:3},{label:'Produtos',data:regs.map(function(r){return regMap[r].prod;}),backgroundColor:C.g2,borderColor:C.g,borderWidth:1,borderRadius:3}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'top',labels:{font:{size:10}}}},scales:{y:{grid:{color:GC}},x:{grid:{display:false}}}}});

  // Por setor solicitante
  var setorMap={};
  all.forEach(function(s){var k=s.setorSol||s.ccNome||'N/D';if(k&&k!=='N/D')setorMap[k]=(setorMap[k]||0)+1;});
  var sa=Object.keys(setorMap).map(function(k){return{k:k,v:setorMap[k]};}).sort(function(a,b){return b.v-a.v;}).slice(0,8);
  mk('cDsetor',{type:'bar',data:{labels:sa.map(function(x){return x.k.length>20?x.k.slice(0,18)+'..':x.k;}),datasets:[{data:sa.map(function(x){return x.v;}),backgroundColor:C.a2,borderColor:C.a,borderWidth:1,borderRadius:3}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:GC}},y:{grid:{display:false},ticks:{font:{size:9}}}}}});

  // Por comprador responsável
  var compMap={};
  all.forEach(function(s){var k=s.compradorResp||'Sem comprador';compMap[k]=(compMap[k]||0)+1;});
  var ca=Object.keys(compMap).map(function(k){return{k:k,v:compMap[k]};}).sort(function(a,b){return b.v-a.v;}).slice(0,8);
  mk('cDcomp2',{type:'bar',data:{labels:ca.map(function(x){return sN(x.k);}),datasets:[{label:'SCs',data:ca.map(function(x){return x.v;}),backgroundColor:C.b2,borderColor:C.b,borderWidth:1,borderRadius:3}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{y:{grid:{color:GC}},x:{grid:{display:false}}}}});

  // Por centro de custo
  var ccMap={};
  all.forEach(function(s){var k=s.ccNome||s.cc||'N/D';if(k&&k!=='N/D')ccMap[k]=(ccMap[k]||0)+1;});
  var cca=Object.keys(ccMap).map(function(k){return{k:k,v:ccMap[k]};}).sort(function(a,b){return b.v-a.v;}).slice(0,8);
  mk('cDcc',{type:'bar',data:{labels:cca.map(function(x){return x.k.length>20?x.k.slice(0,18)+'..':x.k;}),datasets:[{data:cca.map(function(x){return x.v;}),backgroundColor:C.g2,borderColor:C.g,borderWidth:1,borderRadius:3}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:GC}},y:{grid:{display:false},ticks:{font:{size:9}}}}}});

  // Top itens solicitados
  var itemMap={};
  all.forEach(function(s){var k=(s.descricao||'').trim();if(k&&k!=='N/D')itemMap[k]=(itemMap[k]||0)+1;});
  var ia=Object.keys(itemMap).map(function(k){return{k:k,v:itemMap[k]};}).sort(function(a,b){return b.v-a.v;}).slice(0,8);
  mk('cDitem',{type:'bar',data:{labels:ia.map(function(x){return x.k.length>22?x.k.slice(0,20)+'..':x.k;}),datasets:[{data:ia.map(function(x){return x.v;}),backgroundColor:C.a2,borderColor:C.a,borderWidth:1,borderRadius:3}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:GC}},y:{grid:{display:false},ticks:{font:{size:9}}}}}});

  // Insights de demanda
  var crit=all.filter(function(s){return s.nivelRisco==='Critico';});
  var insHtml='';
  if(crit.length>0)insHtml+='<div class="ins"><div class="ins-dot" style="background:'+C.r+'"></div>'+fN(crit.length)+' SCs com fator de risco Crítico sem cotação</div>';
  if(nc>0){var ccM={};all.filter(function(s){return s.situacao==='Não Cotada';}).forEach(function(s){var k=s.ccNome||'N/D';ccM[k]=(ccM[k]||0)+1;});var topCCk=Object.keys(ccM).sort(function(a,b){return ccM[b]-ccM[a];})[0];if(topCCk)insHtml+='<div class="ins"><div class="ins-dot" style="background:'+C.a+'"></div>CC com maior backlog: '+topCCk+' ('+ccM[topCCk]+' SCs não cotadas)</div>';}
  sh('dInsights',insHtml);

  rDemTbl();
}
function rDemTbl(){
  var all=fDem();
  var q=(($('sDem')||{}).value||'').toLowerCase();
  var tp=(($('fDemTipoSC')||{}).value||'');
  var sit=(($('fDemSit')||{}).value||'');
  var reg=(($('fDemReg')||{}).value||'');
  var rows=all.filter(function(s){
    return(!q||(s.solicitacao||'').includes(q)||(s.descricao||'').toLowerCase().includes(q)||(s.ccNome||'').toLowerCase().includes(q)||sN(s.compradorResp||'').toLowerCase().includes(q))&&
      (!tp||s.tipoSC===tp)&&(!sit||s.situacao===sit)&&(!reg||s.regional===reg);
  }).sort(function(a,b){return(b.risco||0)-(a.risco||0);});
  sh('cntDem',fN(rows.length)+' SCs');
  sh('tDem','<thead><tr><th>Nº SC</th><th>Tipo</th><th>Regional</th><th>CC</th><th>Comprador Resp.</th><th>Setor Sol.</th><th>Descrição</th><th>Risco</th><th>Status</th><th>Data</th><th>Aging</th></tr></thead><tbody>'+
    rows.map(function(s){return'<tr>'+
      '<td class="mn" style="color:var(--blue)">'+s.solicitacao+'</td>'+
      '<td>'+tagSit(s.tipoSC)+'</td>'+
      '<td>'+s.regional+'</td>'+
      '<td class="te">'+s.ccNome+'</td>'+
      '<td style="font-weight:600">'+sN(s.compradorResp||'—')+'</td>'+
      '<td class="te">'+(s.setorSol||'—')+'</td>'+
      '<td class="te" title="'+s.descricao+'">'+s.descricao+'</td>'+
      '<td>'+riscoTag(s.nivelRisco||'Normal')+'</td>'+
      '<td>'+tagSit(s.situacao)+'</td>'+
      '<td class="mn">'+s.data+'</td>'+
      '<td>'+agH(s.aging)+'</td>'+
    '</tr>';}).join('')+'</tbody>');
}
// -- OCs --
function rOCs(){
  var ind=D.ind;var ocs=fOCs();
  var tv=ocs.reduce(function(s,o){return s+o.vlrOrig;},0);
  var tl=ocs.filter(function(o){return o.situacao==='Liquidado';}).reduce(function(s,o){return s+o.vlrOrig;},0);
  var ta=ocs.filter(function(o){return o.situacao==='Aberto Total';}).reduce(function(s,o){return s+o.vlrOrig;},0);
  var nNF=ocs.filter(function(o){return o.situacao==='Não Fechado';});
  var txL=tv?Math.round(tl/tv*1000)/10:0;
  // Aguardando NF: OCs Aberto Total sem data de fechamento ou com flag específico
  // Mapeamento via situação — ajustar com STATUS DE-PARA quando disponível
  var aguNF=ocs.filter(function(o){return(o.situacao||'').toLowerCase().includes('aguardando')&&(o.situacao||'').toLowerCase().includes('nf');});
  var emAprov=ocs.filter(function(o){return o.situacao==='Não Fechado';});
  var adiant=ocs.filter(function(o){return(o.situacao||'').toLowerCase().includes('adiantamento');});
  // KPI linha 1
  sh('kOC',
    kH('kb','Total OCs',fN(ocs.length),fN(Array.from(new Set(ocs.map(function(o){return o.filial;}))).length)+' filiais')+
    kH('kb','Valor Total',fR(tv),'')+
    kH('kg','Liquidado',fR(tl),txL+'%')+
    kH('ka','Comprometido',fR(ta),'Aberto Total')
  );
  // KPI linha 2
  sh('kOC2',
    kH('ka','Pend. Aprovação',fN(emAprov.length),fR(emAprov.reduce(function(s,o){return s+o.vlrOrig;},0)))+
    kH('kr','Aguardando NF',aguNF.length?fN(aguNF.length):'—',aguNF.length?fR(aguNF.reduce(function(s,o){return s+o.vlrOrig;},0)):'')+
    kH('kn','Adiantamentos',adiant.length?fN(adiant.length):'—',adiant.length?fR(adiant.reduce(function(s,o){return s+o.vlrOrig;},0)):'se existir na base')+
    kH('kn','Fornecedores',fN(ind.fornAtivos),'ativos')
  );
  // Insights
  var ins=[];
  var top=ind.compradores&&ind.compradores[0];
  if(top&&tv&&top.valor/tv>0.3)ins.push({c:C.b,t:'Maior volume: '+sN(top.nome)+' — '+fR(top.valor)+' ('+pct(top.valor,tv)+')'});
  var topForn=ind.fornecedores&&ind.fornecedores[0];
  if(topForn)ins.push({c:C.b,t:'Fornecedor #1: '+topForn.nome+' — '+fR(topForn.valor)+' (Classe '+topForn.abc+')'});
  if(nNF.length>0)ins.push({c:C.r,t:fN(nNF.length)+' OCs pendentes de aprovação · '+fR(nNF.reduce(function(s,o){return s+o.vlrOrig;},0))+' em risco'});
  var aSit=ind.fornecedores&&ind.fornecedores.filter(function(f){return f.abc==='A';}).length;
  if(aSit)ins.push({c:C.g,t:fN(aSit)+' fornecedores Classe A respondem por 80% do gasto total'});
  sh('insOC',ins.map(function(i){return'<div class="ins"><div class="ins-dot" style="background:'+i.c+'"></div>'+i.t+'</div>';}).join(''));
  // Análise por comprador — com Qtde OC separada
  var cm={};ocs.forEach(function(o){var k=o.comprador||'N/D';if(!cm[k])cm[k]={nome:k,setor:o.setor,qtd:0,valor:0,liq:0,nf:0};cm[k].qtd++;cm[k].valor+=o.vlrOrig;if(o.situacao==='Liquidado')cm[k].liq+=o.vlrOrig;if(o.situacao==='Não Fechado')cm[k].nf++;});
  var compList=Object.values(cm).sort(function(a,b){return b.valor-a.valor;});
  sh('tOCcomp','<thead><tr><th>Comprador</th><th>Setor</th><th class="tdr">Qtde OC</th><th class="tdr">R$ Total OC</th><th>Liquidadas</th><th>Irreg.</th></tr></thead><tbody>'+
    compList.map(function(c){
      var txLc=c.valor?Math.round(c.liq/c.valor*1000)/10:0;
      return'<tr>'+
        '<td style="font-weight:600">'+sN(c.nome)+'</td>'+
        '<td><span class="tag '+(c.setor==='Suprimentos'?'tb':'tn')+'">'+c.setor+'</span></td>'+
        '<td class="mn tdr" style="font-weight:700">'+fN(c.qtd)+'</td>'+
        '<td class="mn tdr">'+fR(c.valor)+'</td>'+
        '<td class="mn" style="color:'+(txLc>=85?C.g:txLc>=60?C.a:C.r)+'">'+fN(c.liq)+' ('+txLc+'%)</td>'+
        '<td class="mn" style="color:'+(c.nf>0?C.a:'var(--tx3)')+'">'+fN(c.nf)+'</td>'+
      '</tr>';
    }).join('')+'</tbody>');
  // Valor por situação
  var SC={'Liquidado':C.g,'Aberto Total':C.b,'Não Fechado':C.a,'Cancelado':'#94a3b8'};var sits=Object.keys(SC);var sitV={};ocs.forEach(function(o){sitV[o.situacao]=(sitV[o.situacao]||0)+o.vlrOrig;});
  mk('cOCsit',{type:'bar',data:{labels:sits,datasets:[{data:sits.map(function(k){return sitV[k]||0;}),backgroundColor:sits.map(function(k){return SC[k]+'aa';}),borderColor:sits.map(function(k){return SC[k];}),borderWidth:1,borderRadius:4}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{y:{grid:{color:GC},ticks:{callback:function(v){return fR(v);}}},x:{grid:{display:false}}}}});
  var f10=(ind.fornecedores||[]).slice(0,10);
  mk('cOCforn',{type:'bar',data:{labels:f10.map(function(f){var n=f.nome||'';return n.length>22?n.slice(0,20)+'..':n;}),datasets:[{data:f10.map(function(f){return f.valor;}),backgroundColor:f10.map(function(f){return f.abc==='A'?C.g2:f.abc==='B'?C.a2:'rgba(148,163,184,.5)';}),borderRadius:3}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:GC},ticks:{callback:function(v){return fR(v);}}},y:{grid:{display:false},ticks:{font:{size:9}}}}}});
  rOCTbl();
}
function rOCTbl(){
  if(!D)return;
  var q=(($('sOC')||{}).value||'').toLowerCase();
  var sit=(($('fOCSit')||{}).value||'');
  var reg=(($('fOCReg')||{}).value||'');
  var tp=(($('fOCTp')||{}).value||'');
  var f=gGet();
  var rows=fOCs().filter(function(o){
    return(!q||(o.filialOC+' '+o.fornecedorNome+' '+o.cc+' '+o.ccNome).toLowerCase().includes(q))&&
      (!sit||o.situacao===sit)&&(!reg||o.regional===reg)&&(!tp||o.tipo===tp);
  });
  var tv=rows.reduce(function(s,o){return s+o.vlrOrig;},0);
  sh('cntOC',fN(rows.length)+' OCs · '+fR(tv));
  sh('tOC','<thead><tr><th>FILIAL-OC</th><th>Regional</th><th>CC</th><th>Situação</th><th>Tipo</th><th>Comprador</th><th>Fornecedor</th><th class="tdr">Vlr. Original</th><th>Emissão</th><th>Dias</th></tr></thead><tbody>'+
    rows.map(function(o){return'<tr>'+
      '<td class="mn" style="color:var(--blue);font-weight:600">'+o.filialOC+'</td>'+
      '<td>'+o.regional+'</td>'+
      '<td class="te">'+o.ccNome+'</td>'+
      '<td>'+tagSit(o.situacao)+'</td>'+
      '<td>'+tagSit(o.tipo||'—')+'</td>'+
      '<td>'+sN(o.comprador)+'</td>'+
      '<td class="te" title="'+o.fornecedorNome+'">'+o.fornecedorNome+'</td>'+
      '<td class="mn tdr">'+fR(o.vlrOrig)+'</td>'+
      '<td class="mn">'+o.emissao+'</td>'+
      '<td>'+agH(o.diasAberto)+'</td>'+
    '</tr>';}).join('')+'</tbody>');
}
// -- PERFORMANCE --
function rPerf(){
  rPComp();
  var ind=D.ind;var sl=ind.savCompArr||[];
  var ts=ind.totalSaving;var tvi=ind.totalVI;var sr=tvi?Math.round(ts/tvi*1000)/10:0;
  var topRate=sl.filter(function(s){return s.rate>=8;}).length;
  sh('kP',kH('kb','Compradores',fN(ind.compradores.length),'')+kH('kg','Saving Total',fR(ts),sr+'% rate')+kH(sr>=8?'kg':'ka','Rate Médio',sr+'%','meta: >8%')+kH('kb','Acima da Meta',fN(topRate),topRate+' comprad. ≥8%'));

  // Volume por comprador — tabela
  var tv3=ind.totalValor||1;
  sh('tPvol','<thead><tr><th>Comprador</th><th>Setor</th><th class="tdr">OCs</th><th class="tdr">Valor</th><th>%</th></tr></thead><tbody>'+
    (ind.compradores||[]).map(function(comp){return'<tr>'+
      '<td style="font-weight:600">'+sN(comp.nome)+'</td>'+
      '<td><span class="tag '+(comp.setor==='Suprimentos'?'tb':'tn')+'">'+comp.setor+'</span></td>'+
      '<td class="mn tdr">'+fN(comp.qtd)+'</td>'+
      '<td class="mn tdr">'+fR(comp.valor)+'</td>'+
      '<td><div class="mb"><div class="mbt"><div class="mbf" style="width:'+Math.min(Math.round(comp.valor/tv3*100),100)+'%;background:'+C.b+'"></div></div>'+pct(comp.valor,tv3)+'</div></td>'+
    '</tr>';}).join('')+'</tbody>');

  mk('cPsav',{type:'bar',data:{labels:sl.map(function(s){return s.nome;}),datasets:[{label:'Saving',data:sl.map(function(s){return s.saving;}),backgroundColor:C.g2,borderColor:C.g,borderWidth:1,borderRadius:3}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{y:{grid:{color:GC},ticks:{callback:function(v){return fR(v);}}},x:{grid:{display:false}}}}});
  mk('cPrate',{type:'bar',data:{labels:sl.map(function(s){return s.nome;}),datasets:[{label:'Rate %',data:sl.map(function(s){return s.rate;}),backgroundColor:sl.map(function(s){return s.rate>=20?C.g2:s.rate>=8?C.a2:C.r2;}),borderRadius:3},{label:'Meta 8%',data:sl.map(function(){return 8;}),borderColor:C.b,borderDash:[4,4],type:'line',pointRadius:0,borderWidth:1.5}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'top',labels:{font:{size:10}}}},scales:{y:{grid:{color:GC},ticks:{callback:function(v){return v+'%';}}},x:{grid:{display:false}}}}});
}
function rPComp(){
  var ind=D.ind;var comps=ind.compradores||[];var tv=ind.totalValor||1;
  var rows=comps.slice().sort(function(a,b){return(b.qtd||0)-(a.qtd||0);});

  // Calcular % atendimento e tempo médio por comprador a partir das SCs
  var allSC=[].concat(D.scServ||[],D.scProd||[]);
  var scByComp={};
  allSC.forEach(function(s){
    var k=s.compradorResp||'';if(!k)return;
    if(!scByComp[k])scByComp[k]={total:0,atend:0,agingAtend:[]};
    scByComp[k].total++;
    var atendida=s.situacao==='Gerou O.C.'||s.situacao==='Atendida';
    if(atendida){scByComp[k].atend++;if(s.aging!=null)scByComp[k].agingAtend.push(s.aging||0);}
  });

  sh('tPcomp','<thead><tr>'+
    '<th>Comprador</th><th>Setor</th><th>OCs</th><th class="tdr">Volume</th><th>%</th>'+
    '<th>Irreg.</th><th class="tdr">Saving</th><th>Rate</th>'+
    '<th title="SCs Atendidas ÷ Total SCs do comprador">% Atend.</th>'+
    '<th title="Aging médio das SCs já atendidas (Gerou O.C. / Atendida)">T.Médio</th>'+
    '</tr></thead><tbody>'+
    rows.map(function(c){
      // Encontrar chave de comprador nas SCs (nome completo ou parcial)
      var scKey=Object.keys(scByComp).find(function(k){return k.toLowerCase().startsWith((c.nome||'').split(' ')[0].toLowerCase());});
      var sc=scKey?scByComp[scKey]:{total:0,atend:0,agingAtend:[]};
      var pctAtend=sc.total?Math.round(sc.atend/sc.total*1000)/10:null;
      var tmedio=sc.agingAtend.length?Math.round(sc.agingAtend.reduce(function(a,b){return a+b;},0)/sc.agingAtend.length):null;
      var pctColor=pctAtend===null?'var(--tx3)':pctAtend>=85?C.g:pctAtend>=60?C.a:C.r;
      var tmedColor=tmedio===null?'var(--tx3)':tmedio<=7?C.g:tmedio<=15?C.a:C.r;
      return'<tr>'+
        '<td style="font-weight:600">'+sN(c.nome)+'</td>'+
        '<td><span class="tag '+(c.setor==='Suprimentos'?'tb':'tn')+'">'+c.setor+'</span></td>'+
        '<td class="mn">'+fN(c.qtd)+'</td>'+
        '<td class="mn tdr">'+fR(c.valor)+'</td>'+
        '<td><div class="mb"><div class="mbt"><div class="mbf" style="width:'+Math.min(Math.round(c.valor/tv*100),100)+'%;background:'+C.b+'"></div></div>'+pct(c.valor,tv)+'</div></td>'+
        '<td class="mn" style="color:'+(c.irreg>0?C.a:'var(--tx3)')+'">'+fN(c.irreg)+'</td>'+
        '<td class="mn tdr" style="color:'+C.g+'">'+(c.saving?fR(c.saving):'—')+'</td>'+
        '<td class="mn" style="color:'+(c.savingRate&&c.savingRate>=8?C.g:C.a)+'">'+(c.savingRate!==null?c.savingRate+'%':'—')+'</td>'+
        '<td class="mn" style="color:'+pctColor+';font-weight:600">'+(pctAtend!==null?pctAtend+'%<br><span style="font-size:9px;font-weight:400;color:var(--tx3)">'+fN(sc.atend)+'/'+fN(sc.total)+'</span>':'—')+'</td>'+
        '<td class="mn" style="color:'+tmedColor+';font-weight:600">'+(tmedio!==null?tmedio+'d':'—')+'</td>'+
      '</tr>';
    }).join('')+'</tbody>');
}
function rPForn(){
  var forns=D.ind.fornecedores||[];var af=(($('fPFabc')||{}).value||'');var rows=af?forns.filter(function(f){return f.abc===af;}):forns;
  sh('tPforn','<thead><tr><th>#</th><th>Fornecedor</th><th>OCs</th><th class="tdr">Valor</th><th>Acum.</th><th>ABC</th></tr></thead><tbody>'+rows.slice(0,30).map(function(f,i){return'<tr><td class="mn" style="color:var(--tx3)">'+(i+1)+'</td><td class="te">'+f.nome+'</td><td class="mn">'+fN(f.qtd)+'</td><td class="mn tdr">'+fR(f.valor)+'</td><td class="mn">'+f.pctAcum+'%</td><td><span class="abc'+f.abc.toLowerCase()+'">'+f.abc+'</span></td></tr>';}).join('')+'</tbody>');
}
// -- FINANCEIRO --
function rFin(){
  var ind=D.ind;var ocs=fOCs();
  var tv=ocs.reduce(function(s,o){return s+o.vlrOrig;},0);
  var tl=ocs.filter(function(o){return o.situacao==='Liquidado';}).reduce(function(s,o){return s+o.vlrOrig;},0);
  var ta=ocs.filter(function(o){return o.situacao==='Aberto Total';}).reduce(function(s,o){return s+o.vlrOrig;},0);
  var tnf=ocs.filter(function(o){return o.situacao==='Não Fechado';}).reduce(function(s,o){return s+o.vlrOrig;},0);
  var txL=tv?Math.round(tl/tv*1000)/10:0;
  sh('kFin',kH('kg','Liquidado',fR(tl),txL+'%')+kH('kb','Comprometido',fR(ta),fN(ocs.filter(function(o){return o.situacao==='Aberto Total';}).length)+' OCs')+kH('ka','Pend. Aprovação',fR(tnf),fN(ocs.filter(function(o){return o.situacao==='Não Fechado';}).length)+' OCs')+kH('kg','Taxa Liquidação',txL+'%','meta >85%'));
  sh('fcnt',fN(ocs.length)+' OCs · '+fR(tv));
  var SC={'Liquidado':C.g,'Aberto Total':C.b,'Não Fechado':C.a,'Cancelado':'#94a3b8'};var sits=['Liquidado','Aberto Total','Não Fechado','Cancelado'];var sitV={};ocs.forEach(function(o){sitV[o.situacao]=(sitV[o.situacao]||0)+o.vlrOrig;});
  mk('cFinB',{type:'bar',data:{labels:sits,datasets:[{data:sits.map(function(k){return sitV[k]||0;}),backgroundColor:sits.map(function(k){return SC[k]+'aa';}),borderColor:sits.map(function(k){return SC[k];}),borderWidth:1,borderRadius:4}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{y:{grid:{color:GC},ticks:{callback:function(v){return fR(v);}}},x:{grid:{display:false}}}}});
  // Curva ABC — tabela compacta com minibarras
  var forns=ind.fornecedores||[];
  var grpA=forns.filter(function(f){return f.abc==='A';});
  var grpB=forns.filter(function(f){return f.abc==='B';});
  var grpC=forns.filter(function(f){return f.abc==='C';});
  var vA=grpA.reduce(function(s,f){return s+f.valor;},0);
  var vB=grpB.reduce(function(s,f){return s+f.valor;},0);
  var vC=grpC.reduce(function(s,f){return s+f.valor;},0);
  var vTot=(vA+vB+vC)||1;
  var abcRows=[
    {cls:'A',cor:'var(--grn)',cor2:'var(--grn-l)',label:'Classe A',desc:'Concentração crítica — 80% do gasto',grp:grpA,val:vA,corte:'0–80%'},
    {cls:'B',cor:'var(--amb)',cor2:'var(--amb-l)',label:'Classe B',desc:'Relevantes — entre 80% e 95%',grp:grpB,val:vB,corte:'80–95%'},
    {cls:'C',cor:'var(--c400)',cor2:'var(--c100)',label:'Classe C',desc:'Cauda longa — acima de 95%',grp:grpC,val:vC,corte:'95–100%'}
  ];
  var abcHtml='<div style="padding:12px 14px;display:flex;flex-direction:column;gap:14px;">';
  abcRows.forEach(function(r){
    var pctVal=Math.round(r.val/vTot*1000)/10;
    var barW=Math.round(r.val/vTot*100);
    var topEx=r.grp.slice(0,3).map(function(f){return f.nome.split(' ')[0];}).join(', ');
    abcHtml+=
      '<div style="background:'+r.cor2+';border:1px solid '+r.cor+';border-radius:6px;padding:10px 12px;">'+
        '<div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:6px;">'+
          '<div style="display:flex;align-items:center;gap:8px;">'+
            '<span style="background:'+r.cor+';color:#fff;font-size:10px;font-weight:800;padding:1px 6px;border-radius:3px;">'+r.cls+'</span>'+
            '<span style="font-size:11px;font-weight:700;color:var(--tx);">'+r.label+'</span>'+
            '<span style="font-size:10px;color:var(--tx3);">'+r.corte+'</span>'+
          '</div>'+
          '<div style="text-align:right;">'+
            '<div style="font-size:13px;font-weight:800;font-family:var(--fm);color:var(--tx);">'+pctVal+'%</div>'+
            '<div style="font-size:9.5px;color:var(--tx3);">'+fR(r.val)+'</div>'+
          '</div>'+
        '</div>'+
        '<div style="height:6px;background:rgba(0,0,0,.08);border-radius:3px;overflow:hidden;margin-bottom:7px;">'+
          '<div style="width:'+barW+'%;height:100%;background:'+r.cor+';border-radius:3px;"></div>'+
        '</div>'+
        '<div style="display:flex;justify-content:space-between;font-size:10px;color:var(--tx3);">'+
          '<span>'+fN(r.grp.length)+' fornecedor'+(r.grp.length!==1?'es':'')+(topEx?' — ex: '+topEx:'')+'</span>'+
        '</div>'+
      '</div>';
  });
  abcHtml+='</div>';
  sh('finAbcTbl',abcHtml);
  var f15=(ind.fornecedores||[]).slice(0,15);
  mk('cFinForn',{type:'bar',data:{labels:f15.map(function(f){var n=f.nome||'';return n.length>24?n.slice(0,22)+'..':n;}),datasets:[{data:f15.map(function(f){return f.valor;}),backgroundColor:f15.map(function(f){return f.abc==='A'?C.g2:f.abc==='B'?C.a2:'rgba(148,163,184,.45)';}),borderRadius:3}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:GC},ticks:{callback:function(v){return fR(v);}}},y:{grid:{display:false},ticks:{font:{size:9}}}}}});
  var nf=ind.naoFechado||[];
  sh('tFinNf','<thead><tr><th>FILIAL-OC</th><th>Tipo</th><th>Comprador</th><th>Fornecedor</th><th class="tdr">Vlr. Original</th><th>Dias</th></tr></thead><tbody>'+nf.map(function(o){return'<tr><td class="mn" style="color:var(--amb)">'+o.filialOC+'</td><td>'+tagSit(o.tipo||'—')+'</td><td>'+sN(o.comprador)+'</td><td class="te">'+o.fornecedorNome+'</td><td class="mn tdr">'+fR(o.vlrOrig)+'</td><td>'+agH(o.diasAberto)+'</td></tr>';}).join('')+'</tbody>');
}
// -- SAVING --
function rSav(){
  var ind=D.ind;var sv=fSav();
  var ts=sv.reduce(function(s,x){return s+x.saving;},0);var tvi=sv.reduce(function(s,x){return s+x.vlrInicial;},0);var sr=tvi?Math.round(ts/tvi*1000)/10:0;
  var sl=ind.savCompArr||[];
  // Hero card
  sh('savHero','<div class="sav-hero"><div class="sav-hero-main"><div class="lbl">💰 Economia Gerada</div><div class="val">'+fR(ts)+'</div><div class="sub">'+fN(sv.length)+' negociações · Rate '+sr+'% '+( sr>=8?'✓ acima do benchmark 8%':'⚠ abaixo do benchmark 8%')+'</div></div><div class="sav-hero-stats"><div class="sav-stat"><div class="sv">'+fR(tvi)+'</div><div class="sl">Valor Inicial</div></div><div class="sav-stat"><div class="sv">'+sr+'%</div><div class="sl">Rate Médio</div></div><div class="sav-stat"><div class="sv">'+fN(sl.filter(function(s){return s.rate>=8;}).length)+'</div><div class="sl">Comprad. ≥8%</div></div></div></div>');
  sh('kSav','<!-- hero card used -->');sh('fcnt',fN(sv.length)+' registros · '+fR(ts)+' saving');
  var st=ind.savPorSetor||{};var sa=Object.keys(st).map(function(k){return{k:k,v:st[k]};}).sort(function(a,b){return b.v-a.v;}).slice(0,8);
  mk('cSavS',{type:'bar',data:{labels:sa.map(function(x){return x.k;}),datasets:[{data:sa.map(function(x){return x.v;}),backgroundColor:C.b2,borderColor:C.b,borderWidth:1,borderRadius:3}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:GC},ticks:{callback:function(v){return fR(v);}}},y:{grid:{display:false}}}}});
  var md=ind.savPorModo||{};var mA=Object.keys(md);
  mk('cSavM',{type:'doughnut',data:{labels:mA.map(function(k){return k+' — '+fR(md[k]);}),datasets:[{data:mA.map(function(k){return md[k];}),backgroundColor:[C.g2,C.b2,C.a2],borderWidth:1,borderColor:'#fff',hoverOffset:4}]},options:{responsive:true,maintainAspectRatio:false,cutout:'50%',plugins:{legend:{position:'bottom',labels:{font:{size:10},padding:7}}}}});
  // Rate % substituído por tabela simples
  sh('cSavR',''); // removido conforme solicitado
  rSavTbl();
}
function rSavTbl(){
  var sv=fSav();
  var q=(($('sSav')||{}).value||'').toLowerCase();
  var cp=(($('fSavC')||{}).value||'');
  var md=(($('fSavM')||{}).value||'');
  var st=(($('fSavSt')||{}).value||'');
  var foc=(($('fSavFOC')||{}).value||'').toLowerCase();
  var rows=sv.filter(function(s){
    return(!q||(s.descricao||'').toLowerCase().includes(q)||(s.fornecedor||'').toLowerCase().includes(q))&&
      (!cp||s.comprador===cp)&&(!md||s.modo===md)&&(!st||s.setor===st)&&
      (!foc||s.filialOC.toLowerCase().includes(foc));
  });
  sh('cntSav',fN(rows.length)+' registros');
  sh('tSav','<thead><tr><th>FILIAL-OC</th><th>Comprador</th><th>Setor</th><th>Fornecedor</th><th>Descrição</th><th class="tdr">Vlr. Inicial</th><th class="tdr">Saving</th><th>Rate</th><th>Modo</th></tr></thead><tbody>'+
    rows.map(function(s){var r=s.vlrInicial?Math.round(s.saving/s.vlrInicial*1000)/10:0;return'<tr>'+
      '<td class="mn">'+s.filialOC+'</td>'+
      '<td>'+s.comprador+'</td>'+
      '<td><span class="tag '+(s.setor==='Suprimentos'?'tb':'tn')+'">'+s.setor+'</span></td>'+
      '<td class="te">'+s.fornecedor+'</td>'+
      '<td class="te" title="'+s.descricao+'">'+s.descricao+'</td>'+
      '<td class="mn tdr">'+fRd(s.vlrInicial)+'</td>'+
      '<td class="mn tdr" style="color:'+C.g+';font-weight:600">'+fRd(s.saving)+'</td>'+
      '<td class="mn" style="color:'+(r>=8?C.g:C.a)+'">'+r+'%</td>'+
      '<td>'+tagSit(s.modo||'—')+'</td>'+
    '</tr>';}).join('')+'</tbody>');
}
// -- CHAMADOS MAN (+ Recorrência incorporada) --
function rCham(){
  var ind=D.ind;var ch=fCham();var ca=ch.filter(function(c){return c.aberto;});
  sh('kCh',kH('kb','Total',fN(ind.chamTotal),'')+kH('kr','Abertos',fN(ind.chamAbertos),'aguardando')+kH('kb','Em Aprovação',fN(ind.chamAprov),'')+kH('kg','Concluídos',fN(ind.chamConc),'')+kH('ka','Aging Médio',fN(ind.chamAgMed)+'d','')+kH('kr','Críticos',fN(ind.chamCrit),'')+kH('kb','Com SC',fN(ind.chamComSC),pct(ind.chamComSC,ind.chamTotal))+kH('kb','Preventivo',fN(ind.chamPrev),pct(ind.chamPrev,ind.chamTotal)));
  // Render tabela de risco Chamados
  var riscoCH = D.ind.alertasRiscoCH || [];
  if (riscoCH.length) {
    sh('tRiscoCH',
      '<thead><tr><th>Score</th><th>Risco</th><th>Protocolo</th><th>Local</th><th>Motivo</th><th>Aging</th><th>Status</th></tr></thead><tbody>' +
      riscoCH.slice(0,15).map(function(c){
        return '<tr>' +
          '<td class="mn" style="font-weight:700;color:'+(c.nivelRisco==='Critico'?C.r:c.nivelRisco==='Atencao'?C.a:C.b)+'">' + c.risco + '</td>' +
          '<td>' + riscoTag(c.nivelRisco) + '</td>' +
          '<td class="mn" style="color:var(--blue)">' + c.protocolo + '</td>' +
          '<td class="te">' + c.local + '</td>' +
          '<td class="te" title="' + c.motivo + '">' + c.motivo + '</td>' +
          '<td>' + agH(c.aging) + '</td>' +
          '<td><span class="tag tn" style="font-size:9px">' + c.status + '</span></td>' +
        '</tr>';
      }).join('') + '</tbody>'
    );
  } else {
    sh('tRiscoCH','<tbody><tr><td colspan="7" style="text-align:center;color:var(--tx3);padding:12px;font-size:11px">Nenhum chamado em nível de risco elevado</td></tr></tbody>');
  }

  var alts=[];
  if(ind.chamCrit>0)alts.push({c:'ar',b:true,t:fN(ind.chamCrit)+' chamados críticos/urgentes abertos'});
  if(ind.chamSemSC>10)alts.push({c:'aa',b:false,t:fN(ind.chamSemSC)+' chamados abertos sem SC vinculada'});
  if(ind.chamAgMed>7)alts.push({c:'aa',b:false,t:'Aging médio '+fN(ind.chamAgMed)+'d — acima do ideal'});
  sh('aCh',alts.map(function(a){return'<div class="al '+a.c+'"><div class="adot'+(a.b?' bl':'')+'"></div>'+a.t+'</div>';}).join(''));
  if(!D.chamados||!D.chamados.length){['cChSt','cChAg','cChPr','cChMes','cChMo','cRuf','cRec'].forEach(function(id){var el=$(id);if(el&&el.parentNode)el.parentNode.innerHTML='<div style="padding:14px;text-align:center;color:var(--tx3);font-size:11px">Sem dados · verifique aba CHAMADOS MAN</div>';});sh('tCh','<tbody><tr><td colspan="9" style="text-align:center;color:var(--tx3);padding:16px">Nenhum chamado carregado</td></tr></tbody>');sh('cntCh','0');sh('tRec','');return;}
  var stC=function(s){var u=s.toUpperCase();if(u.includes('ENCAMINHADO')||u.includes('AGUARDANDO ORÇAMENTO')||u.includes('EM ANÁLISE'))return C.r;if(u.includes('APROVAÇÃO'))return C.a;if(u.includes('EXECUÇÃO'))return C.b;if(u.includes('ENCERRADO')||u.includes('CONCLUÍDO'))return C.g;return '#94a3b8';};
  var ss=ind.chamPorStatus||{};var ssK=Object.keys(ss);
  mk('cChSt',{type:'doughnut',data:{labels:ssK,datasets:[{data:ssK.map(function(k){return ss[k];}),backgroundColor:ssK.map(function(k){return stC(k)+'bb';}),borderWidth:1,borderColor:'#fff',hoverOffset:4}]},options:{responsive:true,maintainAspectRatio:false,cutout:'50%',plugins:{legend:{position:'right',labels:{font:{size:9}}}}}});
  var ab=ind.chamAgBuckets||{};var abK=Object.keys(ab);
  mk('cChAg',{type:'bar',data:{labels:abK,datasets:[{data:abK.map(function(k){return ab[k];}),backgroundColor:[C.g2,C.g2,C.a2,C.r2,C.r2],borderRadius:3}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{y:{grid:{color:GC}},x:{grid:{display:false}}}}});
  var pp=ind.chamPorPrior||{};var ppK=Object.keys(pp);
  var ppC={'Crítica':C.r,'Urgente':C.r,'Alta':C.a,'Média':C.b,'Normal':C.b,'Baixa':C.g};
  var ppOrder=['Crítica','Urgente','Alta','Média','Normal','Baixa'];
  var ppKS=ppOrder.filter(function(k){return pp[k];}).concat(ppK.filter(function(k){return ppOrder.indexOf(k)===-1&&pp[k];}));
  mk('cChPr',{type:'doughnut',data:{labels:ppKS.map(function(k){return k+' ('+fN(pp[k]||0)+')'}),datasets:[{data:ppKS.map(function(k){return pp[k]||0;}),backgroundColor:ppKS.map(function(k){return ppC[k]||'#94a3b8';}),borderWidth:2,borderColor:'#fff',hoverOffset:6}]},options:{responsive:true,maintainAspectRatio:false,cutout:'50%',plugins:{legend:{position:'right',labels:{font:{size:9},padding:6}}}}});
  // Abertos por Mês — barras agrupadas por ano + linha de tendência
  var cpm=ind.chamPorMes||{};
  var MNAMES=['Janeiro','Fevereiro','Março','Abril','Maio','Junho','Julho','Agosto','Setembro','Outubro','Novembro','Dezembro'];
  var anosSet={};Object.keys(cpm).forEach(function(k){var a=k.substring(0,4);if(a)anosSet[a]=1;});
  var anos=Object.keys(anosSet).sort();
  // Paleta: azul escuro + dourado (igual à foto)
  var palBg=['#1e3a6e','#c8971a','#166534','#7c3aed'];
  var palBd=['#1e3a6e','#c8971a','#166534','#7c3aed'];
  var datasets=anos.map(function(ano,i){
    var vals=MNAMES.map(function(_,mi){var key=ano+'-'+String(mi+1).padStart(2,'0');return cpm[key]||null;});
    return{label:ano,data:vals,backgroundColor:palBg[i%palBg.length],borderColor:palBd[i%palBd.length],borderWidth:0,borderRadius:3,type:'bar'};
  });
  // Linha de tendência: média móvel de 3 meses sobre o ano mais recente
  if(anos.length>0){
    var lastAno=anos[anos.length-1];
    var raw=MNAMES.map(function(_,mi){var key=lastAno+'-'+String(mi+1).padStart(2,'0');return cpm[key]||0;});
    var trend=raw.map(function(v,i){
      var s=0,n=0;
      for(var d=-1;d<=1;d++){if(raw[i+d]!==undefined){s+=raw[i+d];n++;}}
      return n?Math.round(s/n):null;
    });
    datasets.push({label:'Tendência '+lastAno,data:trend,type:'line',borderColor:'#0f172a',backgroundColor:'transparent',borderWidth:2,pointRadius:3,pointBackgroundColor:'#0f172a',tension:0.3,order:0});
  }
  mk('cChMes',{type:'bar',data:{labels:MNAMES,datasets:datasets},options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},plugins:{legend:{position:'top',labels:{font:{size:10},padding:10,usePointStyle:true}},tooltip:{callbacks:{label:function(ctx){return ctx.dataset.label+': '+fN(ctx.raw);}}}} ,scales:{y:{grid:{color:GC},ticks:{font:{size:10}}},x:{grid:{display:false},ticks:{font:{size:9},maxRotation:0}}}}});
  var pm=ind.chamPorMotivo||{};var pma=Object.keys(pm).map(function(k){return{k:k,v:pm[k]};}).sort(function(a,b){return b.v-a.v;}).slice(0,8);
  mk('cChMo',{type:'bar',data:{labels:pma.map(function(x){return x.k.length>26?x.k.slice(0,24)+'..':x.k;}),datasets:[{data:pma.map(function(x){return x.v;}),backgroundColor:C.a2,borderColor:C.a,borderWidth:1,borderRadius:3}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:GC}},y:{grid:{display:false},ticks:{font:{size:9}}}}}});
  // Recorrência
  var rec=ind.recorrencia||[];var grave=rec.filter(function(r){return r.count>=3;}).length;
  sh('kRec',kH('kr','Reincidências',fN(rec.length),'Loja+Motivo 2+ ocorr.')+kH('kr','Grave (3+)',fN(grave),'')+kH('ka','Corretivo',fN(ind.chamCorr),pct(ind.chamCorr,ind.chamTotal))+kH('kg','Preventivo',fN(ind.chamPrev),pct(ind.chamPrev,ind.chamTotal)));
  var pu={};ch.forEach(function(c){if(c.uf&&c.uf!=='N/D')pu[c.uf]=(pu[c.uf]||0)+1;});
  var pua=Object.keys(pu).map(function(k){return{k:k,v:pu[k]};}).sort(function(a,b){return b.v-a.v;});
  mk('cRuf',{type:'doughnut',data:{labels:pua.map(function(x){return x.k+'  '+fN(x.v);}),datasets:[{data:pua.map(function(x){return x.v;}),backgroundColor:C.pal,borderWidth:1,borderColor:'#fff',hoverOffset:4}]},options:{responsive:true,maintainAspectRatio:false,cutout:'50%',plugins:{legend:{position:'right',labels:{font:{size:10}}}}}});
  var top15=rec.slice(0,15);
  mk('cRec',{type:'bar',data:{labels:top15.map(function(r){return(r.loja||'').slice(0,10)+'|'+(r.motivo||'').slice(0,10);}),datasets:[{label:'Concluídos',data:top15.map(function(r){return r.count-r.abertos;}),backgroundColor:C.g2,stack:'s',borderRadius:0},{label:'Abertos',data:top15.map(function(r){return r.abertos;}),backgroundColor:C.r2,stack:'s',borderRadius:3}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'top',labels:{font:{size:10}}}},scales:{y:{grid:{color:GC},stacked:true},x:{grid:{display:false},stacked:true,ticks:{font:{size:8},maxRotation:35}}}}});
  // Enrich recorrencia with maxAging of open chamados
  var recEnriched=(rec||[]).map(function(r){
    var abChams=(D.chamados||[]).filter(function(c){return c.aberto&&c.local===r.loja&&c.motivo===r.motivo;});
    var maxAg=abChams.length?Math.max.apply(null,abChams.map(function(c){return c.aging||0;})):null;
    return Object.assign({},r,{maxAgingAbertos:maxAg});
  });
  sh('tRec','<thead><tr><th>#</th><th>Loja</th><th>UF</th><th>Motivo</th><th>Total</th><th>Abertos</th><th>Aging Máx.</th><th>Nível</th></tr></thead><tbody>'+
    rec.map(function(r,i){var nv=r.count>=5?['tr2','Crítico']:r.count>=3?['ta','Atenção']:['tb','Monitorar'];
      return'<tr>'+
        '<td class="mn" style="color:var(--tx3)">'+(i+1)+'</td>'+
        '<td style="font-weight:500" class="te">'+r.loja+'</td>'+
        '<td class="mn">'+r.uf+'</td>'+
        '<td class="te">'+r.motivo+'</td>'+
        '<td class="mn" style="font-weight:700;color:'+(r.count>=5?C.r:r.count>=3?C.a:C.b)+'">'+fN(r.count)+'×</td>'+
        '<td class="mn" style="color:'+(r.abertos>0?C.a:'var(--tx3)')+'">'+fN(r.abertos)+'</td>'+
        '<td>'+(r.abertos>0?agH(r.maxAging||0):'<span class="aok">—</span>')+'</td>'+
        '<td><span class="tag '+nv[0]+'">'+nv[1]+'</span></td>'+
      '</tr>';}).join('')+'</tbody>');
  rChTbl();
}
function rChTbl(){
  var ch=D.chamados||[];var q=(($('sCh')||{}).value||'').toLowerCase();var fSt=(($('fChSt')||{}).value||'');var fPr=(($('fChPr')||{}).value||'');var fTp=(($('fChTp')||{}).value||'');var fSC=(($('fChSC')||{}).value||'');var f=gGet();var fProt=f.gProtocolo||'';var fSCg=f.gSC||'';
  var fProto=(($('fChProto')||{}).value||'').toLowerCase();
  var fSCNum=(($('fChSCNum')||{}).value||'').toLowerCase();
  var fReg=(($('fChReg')||{}).value||'');
  var rows=ch.filter(function(c){return(!f.gAno||c.ano===f.gAno)&&(!f.gUF||c.uf===f.gUF)&&
    (!q||(c.protocolo||'').includes(q)||(c.local||'').toLowerCase().includes(q)||(c.motivo||'').toLowerCase().includes(q))&&
    (!fSt||c.status===fSt)&&(!fPr||c.prioridade===fPr)&&(!fTp||c.tipo===fTp)&&
    (!fSC||(fSC==='sim'&&c.sc&&c.sc!=='')||(fSC==='nao'&&(!c.sc||c.sc==='')))&&
    (!fProto||(c.protocolo||'').toLowerCase().includes(fProto))&&
    (!fSCNum||(c.sc||'').toLowerCase().includes(fSCNum))&&
    (!fReg||c.uf===fReg);
  }).sort(function(a,b){var po={'Crítica':0,'Urgente':0,'Alta':1,'Média':2,'Normal':2,'Baixa':3};var pa=po[a.prioridade]!==undefined?po[a.prioridade]:2,pb=po[b.prioridade]!==undefined?po[b.prioridade]:2;if(pa!==pb)return pa-pb;return(b.aging||0)-(a.aging||0);});
  sh('cntCh',fN(rows.length)+' chamados');
  sh('tCh','<thead><tr><th>Protocolo</th><th>Abertura</th><th>Local</th><th>UF</th><th>Motivo</th><th>Status</th><th>Prioridade</th><th>Aging</th><th>SC</th></tr></thead><tbody>'+
    rows.map(function(c){
      var prioStyle={'Crítica':'background:#b91c1c;color:#fff','Urgente':'background:#b91c1c;color:#fff','Alta':'background:#92400e;color:#fff','Média':'background:#1e40af;color:#fff','Normal':'background:#f1f5f9;color:#64748b','Baixa':'background:#f0fdf4;color:#166534'};
      return'<tr>'+
        '<td class="mn" style="color:var(--blue);font-weight:600">'+c.protocolo+'</td>'+
        '<td class="mn">'+(c.dtAbertura||'—')+'</td>'+
        '<td class="te">'+c.local+'</td>'+
        '<td class="mn">'+c.uf+'</td>'+
        '<td class="te" title="'+c.motivo+'">'+c.motivo+'</td>'+
        '<td><span style="font-size:9px;padding:1px 4px;border-radius:2px;background:var(--c100);color:var(--tx3)">'+c.status+'</span></td>'+
        '<td><span style="display:inline-block;padding:2px 6px;border-radius:3px;font-size:10px;font-weight:700;'+(prioStyle[c.prioridade]||'background:#f1f5f9;color:#64748b')+'">'+c.prioridade+'</span></td>'+
        '<td>'+agH(c.aging)+'</td>'+
        '<td class="mn" style="color:'+(c.sc?C.b:'var(--tx3)')+'">'+( c.sc||'—')+'</td>'+
      '</tr>';}).join('')+'</tbody>');
}
window.addEventListener('load',init);
</script>
</body>
</html>
