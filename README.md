# Dashboard-Porsche-sales
Dashboard de vendas da porsche desenvolvido com claude afim de estudar automação e tratamento de dados por prompt (Claude + Canvas)

<title>Painel Porsche</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Jost:wght@300;400;500&family=Hanken+Grotesk:wght@300;400;500;600&display=swap">
<style>
/* Layout: palco escuro e silencioso; faixa de filtros fixa no topo, KPIs em linha fina, tabela de cidades ao lado de barras, gráficos de ano-modelo e vitrine de cards. Vermelho Guards é o único acento. */
:root{
  --bg:#0b0b0c; --surface:#131315; --surface2:#1a1a1d; --line:#2a2a2e;
  --fg:#f2f0ec; --muted:#8f8e8a; --accent:#d5001c; --accent-soft:rgba(213,0,28,.16);
  --bar:#4a4a4f; --font-display:'Jost','Futura','Century Gothic',sans-serif;
  --font-body:'Hanken Grotesk','Helvetica Neue',Arial,sans-serif;
  color-scheme:dark;
}
@media (prefers-color-scheme: light){:root:not([data-theme="dark"]){
  --bg:#f5f4f2; --surface:#ffffff; --surface2:#efedea; --line:#dcdad6; --fg:#0e0e10; --muted:#6b6a66;
  --accent:#c8001a; --accent-soft:rgba(200,0,26,.10); --bar:#b9b6b0; color-scheme:light}}
:root[data-theme="light"]{
  --bg:#f5f4f2; --surface:#ffffff; --surface2:#efedea; --line:#dcdad6; --fg:#0e0e10; --muted:#6b6a66;
  --accent:#c8001a; --accent-soft:rgba(200,0,26,.10); --bar:#b9b6b0; color-scheme:light}
*{box-sizing:border-box}
body{background:var(--bg);color:var(--fg);font-family:var(--font-body);font-size:14px;line-height:1.5;padding-inline:clamp(16px,4vw,56px);padding-block:0 56px}
.wrap{max-width:1280px;margin-inline:auto}
header{display:flex;flex-wrap:wrap;align-items:flex-end;justify-content:space-between;gap:16px;padding-block:44px 28px;border-bottom:1px solid var(--line)}
.eyebrow{font-family:var(--font-display);font-size:11px;letter-spacing:.32em;text-transform:uppercase;color:var(--muted);margin:0 0 10px}
h1{font-family:var(--font-display);font-weight:300;font-size:clamp(30px,5vw,52px);letter-spacing:.14em;text-transform:uppercase;margin:0;line-height:1.05;text-wrap:balance}
h1 b{font-weight:500;color:var(--accent)}
.sub{color:var(--muted);max-width:44ch;margin:0;font-size:13px}
h2{font-family:var(--font-display);font-weight:400;font-size:13px;letter-spacing:.24em;text-transform:uppercase;margin:0}
.hint{color:var(--muted);font-size:12.5px;margin:6px 0 0;max-width:62ch}
/* filtros */
.filters{position:sticky;top:env(safe-area-inset-top,0px);z-index:5;background:var(--bg);border-bottom:1px solid var(--line);padding-block:14px;display:flex;flex-wrap:wrap;gap:12px;align-items:flex-end}
.field{display:flex;flex-direction:column;gap:5px;flex:1 1 150px;min-width:0}
.field label{font-family:var(--font-display);font-size:10.5px;letter-spacing:.24em;text-transform:uppercase;color:var(--muted)}
select{appearance:none;-webkit-appearance:none;width:100%;background:var(--surface) url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6'%3E%3Cpath d='M1 1l4 4 4-4' fill='none' stroke='%238f8e8a' stroke-width='1.2'/%3E%3C/svg%3E") no-repeat right 12px center;color:var(--fg);border:1px solid var(--line);border-radius:0;padding:10px 32px 10px 12px;font:inherit;font-size:13.5px;cursor:pointer;text-overflow:ellipsis}
select:hover{border-color:var(--muted)}
select.on{border-color:var(--accent)}
:focus-visible{outline:2px solid var(--accent);outline-offset:2px}
button{font:inherit;cursor:pointer}
.reset{background:none;border:1px solid var(--line);color:var(--fg);padding:10px 18px;font-family:var(--font-display);font-size:11px;letter-spacing:.22em;text-transform:uppercase}
.reset:hover{border-color:var(--accent);color:var(--accent)}
/* kpis */
.kpis{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));border-bottom:1px solid var(--line)}
.kpi{padding:28px 24px 26px 0;min-width:0}
.kpi + .kpi{padding-left:24px;border-left:1px solid var(--line)}
.kpi .l{font-family:var(--font-display);font-size:10.5px;letter-spacing:.26em;text-transform:uppercase;color:var(--muted)}
.kpi .v{font-family:var(--font-display);font-weight:300;font-size:clamp(22px,2.3vw,32px);line-height:1.2;margin-top:8px;font-variant-numeric:tabular-nums;overflow-wrap:anywhere}
.kpi .n{color:var(--muted);font-size:12.5px;margin-top:2px}
@media (max-width:640px){.kpi,.kpi + .kpi{padding:20px 0;border-left:0}.kpi + .kpi{border-top:1px solid var(--line)}}
/* secoes */
section{padding-block:44px 0}
.shead{display:flex;flex-wrap:wrap;justify-content:space-between;gap:12px;align-items:flex-end;margin-bottom:22px}
.grid2{display:grid;grid-template-columns:minmax(0,1.7fr) minmax(0,1fr);gap:40px}
@media (max-width:900px){.grid2{grid-template-columns:minmax(0,1fr)}}
.panel{background:var(--surface);border:1px solid var(--line);padding:22px}
.panel h3{font-family:var(--font-display);font-weight:400;font-size:11px;letter-spacing:.24em;text-transform:uppercase;color:var(--muted);margin:0 0 16px}
.stack{display:flex;flex-direction:column;gap:20px;min-width:0}
/* tabela */
.tscroll{max-height:470px;overflow:auto;border:1px solid var(--line);background:var(--surface)}
table{width:100%;border-collapse:collapse;min-width:520px}
th{position:sticky;top:0;background:var(--surface2);text-align:left;font-family:var(--font-display);font-weight:400;font-size:10.5px;letter-spacing:.22em;text-transform:uppercase;color:var(--muted);padding:12px 16px;z-index:1}
td{padding:12px 16px;border-top:1px solid var(--line);vertical-align:top}
tr.row{cursor:pointer}
tr.row:hover td{background:var(--surface2)}
tr.row.sel td{background:var(--accent-soft)}
td.num,th.num{text-align:right;font-variant-numeric:tabular-nums}
.uf{color:var(--muted);font-size:12px;margin-left:6px}
.tag{display:inline-block;margin:0 6px 4px 0;padding:2px 9px;border:1px solid var(--line);font-size:12px;white-space:nowrap}
.tag.lead{border-color:var(--accent);color:var(--fg)}
/* barras */
.bars{display:flex;flex-direction:column;gap:12px}
.bar{display:grid;grid-template-columns:96px minmax(0,1fr) 28px;gap:12px;align-items:center;font-size:13px}
.bar .t{height:6px;background:var(--surface2);position:relative}
.bar .t i{position:absolute;inset:0 auto 0 0;background:var(--bar)}
.bar.top .t i{background:var(--accent)}
.bar .k{overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.bar .c{text-align:right;font-variant-numeric:tabular-nums;color:var(--muted)}
/* ano modelo */
.years{display:flex;align-items:flex-end;gap:clamp(8px,2vw,22px);height:210px;padding-top:24px;border-bottom:1px solid var(--line)}
.ycol{flex:1;min-width:0;height:100%;display:flex;flex-direction:column;justify-content:flex-end;align-items:center;gap:6px;cursor:pointer;background:none;border:0;color:var(--fg);padding:0}
.ycol .b{width:100%;max-width:64px;background:var(--bar);min-height:2px;transition:height .3s}
.ycol.lead .b{background:var(--accent)}
.ycol.sel .b{outline:1px solid var(--fg);outline-offset:2px}
.ycol .n{font-family:var(--font-display);font-size:15px;font-variant-numeric:tabular-nums}
.ylabels{display:flex;gap:clamp(8px,2vw,22px);margin-top:10px;font-family:var(--font-display);font-size:12px;letter-spacing:.12em;color:var(--muted)}
.ylabels span{flex:1;text-align:center}
svg text{fill:var(--muted);font-family:var(--font-body);font-size:11px}
.mono-note{color:var(--muted);font-size:12px;margin-top:12px}
/* vitrine */
.cards{display:grid;grid-template-columns:repeat(auto-fill,minmax(min(100%,330px),1fr));gap:20px}
.card{background:var(--surface);border:1px solid var(--line);padding:24px 24px 22px;display:flex;flex-direction:column;gap:14px;min-width:0;position:relative;transition:border-color .2s,transform .2s}
.card:hover{border-color:var(--accent);transform:translateY(-2px)}
.card .top{display:flex;justify-content:space-between;align-items:baseline}
.card .fam{font-family:var(--font-display);font-size:10.5px;letter-spacing:.28em;text-transform:uppercase;color:var(--muted)}
.card .rk{font-family:var(--font-display);font-weight:300;font-size:13px;color:var(--accent)}
.card h4{font-family:var(--font-display);font-weight:300;font-size:26px;line-height:1.15;margin:0;letter-spacing:.04em;overflow-wrap:anywhere}
.card dl{margin:0;display:grid;grid-template-columns:auto 1fr;gap:6px 16px;font-size:13px;border-top:1px solid var(--line);padding-top:14px}
.card dt{color:var(--muted)}
.card dd{margin:0;text-align:right;font-variant-numeric:tabular-nums;overflow-wrap:anywhere}
.empty{padding:40px;text-align:center;color:var(--muted);border:1px dashed var(--line)}
footer{margin-top:56px;padding-top:20px;border-top:1px solid var(--line);color:var(--muted);font-size:12px;display:flex;flex-wrap:wrap;gap:6px 24px}
@media (prefers-reduced-motion:reduce){*{transition:none!important}}
</style>

<div class="wrap">
<header>
  <div>
    <p class="eyebrow">Vendas · Base consolidada</p>
    <h1>Painel <b>Porsche</b></h1>
  </div>
  <p class="sub">Modelos, ano-modelo e cidades em uma única leitura. Valores em dólares americanos, como constam na planilha.</p>
</header>

<div class="filters" id="filters">
  <div class="field"><label for="fModel">Modelo</label><select id="fModel"></select></div>
  <div class="field"><label for="fYear">Model year</label><select id="fYear"></select></div>
  <div class="field"><label for="fCity">Cidade</label><select id="fCity"></select></div>
  <div class="field"><label for="fPay">Pagamento</label><select id="fPay"></select></div>
  <div class="field"><label for="fPeriod">Período da venda</label><select id="fPeriod"></select></div>
  <button class="reset" id="reset" type="button">Limpar</button>
</div>

<div class="kpis" id="kpis"></div>

<section>
  <div class="shead">
    <div>
      <h2>Modelos mais vendidos por cidade</h2>
      <p class="hint">Clique em uma cidade para filtrar o painel inteiro. Em empate, todos os modelos aparecem.</p>
    </div>
  </div>
  <div class="grid2">
    <div class="tscroll" id="cityTable"></div>
    <div class="stack">
      <div class="panel"><h3>Famílias de modelo</h3><div class="bars" id="famBars"></div></div>
      <div class="panel"><h3>Formas de pagamento</h3><div class="bars" id="payBars"></div></div>
    </div>
  </div>
</section>

<section>
  <div class="shead">
    <div>
      <h2>Ano-modelo e período</h2>
      <p class="hint" id="yearHint"></p>
    </div>
  </div>
  <div class="grid2">
    <div class="panel">
      <h3>Vendas por ano-modelo</h3>
      <div class="years" id="years"></div>
      <div class="ylabels" id="ylabels"></div>
    </div>
    <div class="panel">
      <h3>Vendas por mês</h3>
      <div id="monthly"></div>
      <p class="mono-note" id="monthNote"></p>
    </div>
  </div>
</section>

<section>
  <div class="shead">
    <div>
      <h2 id="vitrineTitle">Vitrine</h2>
      <p class="hint">Os modelos mais populares segundo os filtros ativos, ordenados por unidades vendidas e, em seguida, por receita.</p>
    </div>
  </div>
  <div class="cards" id="cards"></div>
</section>

<footer id="foot"></footer>
</div>

<script>
const DATA = [{"id":6,"dt":null,"m":"718 Cayman","f":"718","y":2022,"p":79500.0,"km":9800,"pay":"Credit Card","c":"Boston","s":"MA","st":"Delivered"},{"id":7,"dt":"2024-03-14","m":"911 Turbo S","f":"911","y":2024,"p":235000.0,"km":1200,"pay":"Wire Transfer","c":"Seattle","s":"WA","st":"Delivered"},{"id":8,"dt":"2024-04-18","m":"Cayenne Coupe","f":"Cayenne","y":2023,"p":112750.0,"km":6400,"pay":"Financing","c":"Austin","s":"TX","st":"In Transit"},{"id":9,"dt":null,"m":"Macan S","f":"Macan","y":2021,"p":68900.0,"km":28,"pay":"Cash","c":"Denver","s":"CO","st":"Pending"},{"id":10,"dt":"2024-05-22","m":"Taycan 4S","f":"Taycan","y":2024,"p":121000.0,"km":0,"pay":"Bank Transfer","c":"Los Angeles","s":"CA","st":"Delivered"},{"id":11,"dt":"2024-08-06","m":"Panamera 4","f":"Panamera","y":2023,"p":104500.0,"km":14500,"pay":"Credit Card","c":"Miami","s":"FL","st":"Cancelled"},{"id":12,"dt":"2024-07-11","m":"911 Carrera S","f":"911","y":2020,"p":96300.0,"km":41000,"pay":"Lease","c":"New York","s":"NY","st":"Delivered"},{"id":13,"dt":null,"m":"Cayenne E-Hybrid","f":"Cayenne","y":2022,"p":89750.0,"km":11744,"pay":"Wire Transfer","c":"San Diego","s":"CA","st":"Pending Approval"},{"id":14,"dt":"2024-08-19","m":"718 Boxster","f":"718","y":2021,"p":73500.0,"km":22300,"pay":"Debit Card","c":"Chicago","s":"IL","st":"Shipped"},{"id":15,"dt":"2024-09-02","m":"Macan GTS","f":"Macan","y":2024,"p":95000.0,"km":3500,"pay":"Financing","c":"Phoenix","s":"AZ","st":"In Transit"},{"id":16,"dt":"2024-09-17","m":"Taycan Turbo","f":"Taycan","y":2023,"p":153200.5,"km":11,"pay":"ACH Payment","c":"Dallas","s":"TX","st":"Delivered"},{"id":17,"dt":null,"m":"911 GT3","f":"911","y":2024,"p":241000.0,"km":750,"pay":"Wire Transfer","c":"Las Vegas","s":"NV","st":"Pending"},{"id":18,"dt":"2024-05-11","m":"Panamera Turbo S","f":"Panamera","y":2022,"p":132000.0,"km":19250,"pay":"Cash","c":"San Jose","s":"CA","st":"Delivered"},{"id":19,"dt":"2024-12-12","m":"Cayenne Turbo GT","f":"Cayenne","y":2024,"p":188000.0,"km":2100,"pay":"Crypto Payment","c":"Houston","s":"TX","st":"Awaiting Delivery"},{"id":20,"dt":"2024-12-25","m":"911 Carrera Cabriolet","f":"911","y":2023,"p":127800.0,"km":12000,"pay":"Credit Card","c":"Atlanta","s":"GA","st":"Delivered"},{"id":21,"dt":"2025-01-06","m":"Macan","f":"Macan","y":2021,"p":58900.0,"km":33700,"pay":"Bank Transfer","c":"Orlando","s":"FL","st":"Pending"},{"id":22,"dt":null,"m":"718 Spyder RS","f":"718","y":2024,"p":164000.0,"km":900,"pay":"Financing","c":"Portland","s":"OR","st":"In Transit"},{"id":23,"dt":"2025-02-14","m":"Taycan Cross Turismo","f":"Taycan","y":2023,"p":118500.0,"km":7800,"pay":"Wire Transfer","c":"Charlotte","s":"NC","st":"Delivered"},{"id":24,"dt":null,"m":"Cayenne S","f":"Cayenne","y":2022,"p":91300.0,"km":16000,"pay":"Credit Card","c":"Nashville","s":"TN","st":"Pending"},{"id":25,"dt":"2025-03-21","m":"911 Targa 4S","f":"911","y":2024,"p":158750.0,"km":2500,"pay":"Lease","c":"Minneapolis","s":"MN","st":"Delivered"},{"id":26,"dt":"2025-03-28","m":"Panamera","f":"Panamera","y":2020,"p":72000.0,"km":49000,"pay":"Bank Transfer","c":"Philadelphia","s":"PA","st":"Cancelled"},{"id":27,"dt":"2025-04-09","m":"Macan Electric","f":"Macan","y":2025,"p":86500.0,"km":0,"pay":"Wire Transfer","c":"San Antonio","s":"TX","st":"Delivered"},{"id":28,"dt":null,"m":"911 Dakar","f":"911","y":2024,"p":270000.0,"km":1050,"pay":"Cash","c":"Salt Lake City","s":"UT","st":"Awaiting Pickup"},{"id":29,"dt":"2025-05-12","m":"Taycan GTS","f":"Taycan","y":2023,"p":139000.0,"km":6250,"pay":"Financing","c":"Raleigh","s":"NC","st":"In Transit"},{"id":30,"dt":"2025-06-18","m":"Cayenne","f":"Cayenne","y":2021,"p":76800.0,"km":38400,"pay":"Credit Card","c":"Detroit","s":"MI","st":"Delivered"},{"id":31,"dt":null,"m":"718 Cayman GT4 RS","f":"718","y":2024,"p":173600.0,"km":400,"pay":"Wire Transfer","c":"Columbus","s":"OH","st":"Pending"},{"id":32,"dt":"2025-07-07","m":"911 Carrera GTS","f":"911","y":2022,"p":119900.0,"km":13600,"pay":"Cash","c":"Indianapolis","s":"IN","st":"Delivered"},{"id":33,"dt":"2025-07-22","m":"Panamera 4 E-Hybrid","f":"Panamera","y":2023,"p":109250.0,"km":8900,"pay":"Lease","c":"Fort Worth","s":"TX","st":"In Transit"},{"id":34,"dt":"2025-08-14","m":"Macan T","f":"Macan","y":2022,"p":82000.0,"km":21750,"pay":"Wire Transfer","c":"Jacksonville","s":"FL","st":"Delivered"},{"id":35,"dt":"2025-09-01","m":"Taycan Turbo S","f":"Taycan","y":2025,"p":214000.0,"km":0,"pay":"Crypto Payment","c":"San Diego","s":"CA","st":"Pending Review"},{"id":36,"dt":null,"m":"911 Carrera","f":"911","y":2024,"p":124500.0,"km":4800,"pay":"Credit Card","c":"Tampa","s":"FL","st":"Delivered"},{"id":37,"dt":"2025-09-18","m":"Cayenne S","f":"Cayenne","y":2023,"p":98200.0,"km":12200,"pay":"Bank Transfer","c":"Sacramento","s":"CA","st":"Pending"},{"id":38,"dt":"2025-10-04","m":"Macan","f":"Macan","y":2022,"p":67500.0,"km":24100,"pay":"Financing","c":"Cleveland","s":"OH","st":"Delivered"},{"id":39,"dt":"2025-10-12","m":"Taycan","f":"Taycan","y":2025,"p":116900.0,"km":0,"pay":"Wire Transfer","c":"Milwaukee","s":"WI","st":"In Transit"},{"id":40,"dt":"2025-10-29","m":"Panamera 4S","f":"Panamera","y":2024,"p":112000.0,"km":10,"pay":"Cash","c":"Kansas City","s":"MO","st":"Delivered"},{"id":41,"dt":"2025-02-11","m":"718 Boxster","f":"718","y":2021,"p":74000.0,"km":31000,"pay":"Debit Card","c":"Omaha","s":"NE","st":"Cancelled"},{"id":42,"dt":"2025-11-16","m":"911 Turbo","f":"911","y":2024,"p":198300.0,"km":2400,"pay":"Wire Transfer","c":"Albuquerque","s":"NM","st":"Awaiting Delivery"},{"id":43,"dt":null,"m":"Cayenne Coupe","f":"Cayenne","y":2023,"p":103750.0,"km":6773,"pay":"Wire Transfer","c":"Tucson","s":"AZ","st":"Pending Approval"},{"id":44,"dt":null,"m":"Macan GTS","f":"Macan","y":2024,"p":93600.0,"km":5800,"pay":"Financing","c":"Fresno","s":"CA","st":"Shipped"},{"id":45,"dt":"2025-12-07","m":"Taycan 4S","f":"Taycan","y":2025,"p":129000.0,"km":0,"pay":"ACH Payment","c":"Virginia Beach","s":"VA","st":"In Transit"},{"id":46,"dt":"2025-12-22","m":"Panamera Turbo","f":"Panamera","y":2022,"p":136000.0,"km":18400,"pay":"Credit Card","c":"Colorado Springs","s":"CO","st":"Delivered"},{"id":47,"dt":null,"m":"911 GT3 RS","f":"911","y":2024,"p":286500.0,"km":700,"pay":"Wire Transfer","c":"Arlington","s":"TX","st":"Pending"},{"id":48,"dt":"2026-01-08","m":"Cayenne E-Hybrid","f":"Cayenne","y":2023,"p":92800.0,"km":15000,"pay":"Lease","c":"Bakersfield","s":"CA","st":"Delivered"},{"id":49,"dt":"2026-01-15","m":"Macan T","f":"Macan","y":2022,"p":72400.0,"km":19750,"pay":"Cash","c":"Mesa","s":"AZ","st":"Awaiting Pickup"},{"id":50,"dt":"2026-01-28","m":"Taycan Turbo","f":"Taycan","y":2025,"p":158500.0,"km":3200,"pay":"Crypto Payment","c":"Atlanta","s":"GA","st":"Delivered"},{"id":51,"dt":"2026-02-03","m":"718 Cayman","f":"718","y":2021,"p":69900.0,"km":27600,"pay":"Bank Transfer","c":"Long Beach","s":"CA","st":"Pending"},{"id":52,"dt":null,"m":"911 Targa 4","f":"911","y":2024,"p":141250.0,"km":2100,"pay":"Financing","c":"Oakland","s":"CA","st":"In Transit"},{"id":53,"dt":"2026-02-19","m":"Panamera","f":"Panamera","y":2020,"p":71500.0,"km":52000,"pay":"Wire Transfer","c":"Tulsa","s":"OK","st":"Delivered"},{"id":54,"dt":"2026-02-25","m":"Cayenne Turbo","f":"Cayenne","y":2023,"p":146800.0,"km":11300,"pay":"Credit Card","c":"Wichita","s":"KS","st":"Pending"},{"id":55,"dt":"2026-03-01","m":"Macan Electric","f":"Macan","y":2025,"p":89700.0,"km":0,"pay":"Lease","c":"New Orleans","s":"LA","st":"Delivered"},{"id":56,"dt":"2026-03-14","m":"911 Carrera S","f":"911","y":2022,"p":104600.0,"km":15900,"pay":"Bank Transfer","c":"Honolulu","s":"HI","st":"Cancelled"},{"id":57,"dt":null,"m":"Taycan GTS","f":"Taycan","y":2024,"p":142000.0,"km":6600,"pay":"Wire Transfer","c":"Anaheim","s":"CA","st":"Delivered"},{"id":58,"dt":"2026-04-08","m":"Cayenne","f":"Cayenne","y":2021,"p":78400.0,"km":40250,"pay":"Cash","c":"Henderson","s":"NV","st":"Awaiting Review"},{"id":59,"dt":null,"m":"718 Spyder RS","f":"718","y":2025,"p":169000.0,"km":850,"pay":"Financing","c":"Lexington","s":"KY","st":"In Transit"},{"id":60,"dt":"2026-04-21","m":"911 Dakar","f":"911","y":2024,"p":268900.0,"km":1400,"pay":"Credit Card","c":"Riverside","s":"CA","st":"Delivered"},{"id":61,"dt":"2026-04-29","m":"Panamera 4","f":"Panamera","y":2023,"p":101300.0,"km":12700,"pay":"Wire Transfer","c":"Corpus Christi","s":"TX","st":"Pending"},{"id":62,"dt":"2026-05-05","m":"Macan S","f":"Macan","y":2021,"p":66750.0,"km":29800,"pay":"Cash","c":"St. Louis","s":"MO","st":"Delivered"},{"id":63,"dt":"2026-05-14","m":"Taycan Cross Turismo","f":"Taycan","y":2024,"p":127900.0,"km":7500,"pay":"Lease","c":"Pittsburgh","s":"PA","st":"In Transit"},{"id":64,"dt":"2026-05-23","m":"Cayenne Turbo GT","f":"Cayenne","y":2025,"p":200000.0,"km":1950,"pay":"Wire Transfer","c":"Cincinnati","s":"OH","st":"Delivered"},{"id":65,"dt":"2026-06-02","m":"911 Carrera Cabriolet","f":"911","y":2023,"p":132000.0,"km":8800,"pay":"Crypto Payment","c":"Anchorage","s":"AK","st":"Pending Review"},{"id":66,"dt":"2026-06-15","m":"718 Cayman GT4 RS","f":"718","y":2024,"p":176400.0,"km":600,"pay":"Credit Card","c":"Plano","s":"TX","st":"Delivered"},{"id":67,"dt":null,"m":"Panamera 4 E-Hybrid","f":"Panamera","y":2022,"p":108500.0,"km":16200,"pay":"Bank Transfer","c":"Newark","s":"NJ","st":"Cancelled"},{"id":68,"dt":"2026-07-07","m":"Macan","f":"Macan","y":2021,"p":59000.0,"km":36000,"pay":"Financing","c":"Greensboro","s":"NC","st":"Awaiting Delivery"},{"id":69,"dt":"2026-07-20","m":"Taycan Turbo S","f":"Taycan","y":2025,"p":218000.0,"km":0,"pay":"Wire Transfer","c":"Lincoln","s":"NE","st":"Pending"},{"id":70,"dt":null,"m":"Cayenne S","f":"Cayenne","y":2024,"p":99950.0,"km":5300,"pay":"Debit Card","c":"Jersey City","s":"NJ","st":"Delivered"},{"id":71,"dt":"2026-04-08","m":"911 Carrera GTS","f":"911","y":2024,"p":121750.0,"km":5872,"pay":"Credit Card","c":"Chandler","s":"AZ","st":"Delivered"},{"id":72,"dt":"2026-08-18","m":"718 Boxster GTS","f":"718","y":2023,"p":91500.0,"km":13300,"pay":"Lease","c":"Reno","s":"NV","st":"Shipped"},{"id":73,"dt":"2026-08-31","m":"Panamera Turbo S","f":"Panamera","y":2022,"p":134000.0,"km":20100,"pay":"Wire Transfer","c":"Buffalo","s":"NY","st":"In Transit"},{"id":74,"dt":"2026-09-09","m":"Macan GTS","f":"Macan","y":2024,"p":96800.0,"km":0,"pay":"ACH Payment","c":"Durham","s":"NC","st":"Delivered"},{"id":75,"dt":"2026-09-17","m":"Taycan 4S","f":"Taycan","y":2025,"p":131600.0,"km":2900,"pay":"Wire Transfer","c":"Laredo","s":"TX","st":"Pending Approval"},{"id":76,"dt":"2026-09-28","m":"Cayenne E-Hybrid","f":"Cayenne","y":2023,"p":94300.0,"km":13100,"pay":"Cash","c":"Madison","s":"WI","st":"Delivered"},{"id":77,"dt":"2026-10-06","m":"911 Turbo S","f":"911","y":2025,"p":242000.0,"km":1100,"pay":"Crypto Payment","c":"Lubbock","s":"TX","st":"Awaiting Pickup"},{"id":78,"dt":"2026-10-16","m":"718 Cayman S","f":"718","y":2022,"p":82750.0,"km":22500,"pay":"Credit Card","c":"Toledo","s":"OH","st":"Cancelled"},{"id":79,"dt":"2026-10-29","m":"Macan Electric","f":"Macan","y":2026,"p":91300.0,"km":0,"pay":"Wire Transfer","c":"Irvine","s":"CA","st":"Delivered"},{"id":80,"dt":"2026-11-03","m":"Panamera","f":"Panamera","y":2021,"p":79900.0,"km":44800,"pay":"Financing","c":"Garland","s":"TX","st":"Pending"},{"id":81,"dt":null,"m":"Cayenne Coupe","f":"Cayenne","y":2024,"p":111000.0,"km":6700,"pay":"Bank Transfer","c":"Irving","s":"TX","st":"In Transit"},{"id":82,"dt":"2026-12-11","m":"911 Targa 4S","f":"911","y":2023,"p":156500.0,"km":4200,"pay":"Cash","c":"Chesapeake","s":"VA","st":"Delivered"},{"id":83,"dt":"2026-12-24","m":"Taycan","f":"Taycan","y":2025,"p":119900.0,"km":1250,"pay":"Lease","c":"Scottsdale","s":"AZ","st":"Pending"},{"id":84,"dt":null,"m":"Macan T","f":"Macan","y":2022,"p":73200.0,"km":18600,"pay":"Wire Transfer","c":"Norfolk","s":"VA","st":"Delivered"},{"id":85,"dt":"2026-12-28","m":"911 GT3","f":"911","y":2024,"p":224000.0,"km":3000,"pay":"Wire Transfer","c":"Boise","s":"ID","st":"Awaiting Delivery"},{"id":86,"dt":"2027-01-15","m":"911 Carrera","f":"911","y":2024,"p":126900.0,"km":7200,"pay":"Credit Card","c":"Orlando","s":"FL","st":"Delivered"},{"id":87,"dt":"2027-01-29","m":"Cayenne","f":"Cayenne","y":2023,"p":84500.0,"km":21400,"pay":"Bank Transfer","c":"San Jose","s":"CA","st":"Pending"},{"id":88,"dt":"2027-02-11","m":"Macan S","f":"Macan","y":2022,"p":69800.0,"km":26300,"pay":"Financing","c":"Tampa","s":"FL","st":"Delivered"},{"id":89,"dt":null,"m":"Taycan 4S","f":"Taycan","y":2025,"p":132700.0,"km":0,"pay":"Wire Transfer","c":"Denver","s":"CO","st":"In Transit"},{"id":90,"dt":"2027-03-05","m":"Panamera","f":"Panamera","y":2021,"p":81000.0,"km":42,"pay":"Cash","c":"Austin","s":"TX","st":"Delivered"},{"id":91,"dt":"2027-03-18","m":"718 Cayman","f":"718","y":2023,"p":78900.0,"km":17500,"pay":"Debit Card","c":"Seattle","s":"WA","st":"Cancelled"},{"id":92,"dt":"2027-04-02","m":"911 Turbo S","f":"911","y":2026,"p":249300.0,"km":900,"pay":"Wire Transfer","c":"Boston","s":"MA","st":"Awaiting Delivery"},{"id":93,"dt":null,"m":"Cayenne Coupe","f":"Cayenne","y":2024,"p":108750.0,"km":5530,"pay":"Wire Transfer","c":"Phoenix","s":"AZ","st":"Pending Approval"},{"id":94,"dt":null,"m":"Macan Electric","f":"Macan","y":2026,"p":92600.0,"km":0,"pay":"Financing","c":"Chicago","s":"IL","st":"Shipped"},{"id":95,"dt":"2027-05-12","m":"Taycan Turbo","f":"Taycan","y":2025,"p":164000.0,"km":0,"pay":"ACH Payment","c":"Dallas","s":"TX","st":"In Transit"},{"id":96,"dt":"2027-05-27","m":"Panamera 4S","f":"Panamera","y":2024,"p":119000.0,"km":13400,"pay":"Credit Card","c":"San Francisco","s":"CA","st":"Delivered"},{"id":97,"dt":null,"m":"911 GT3","f":"911","y":2026,"p":232500.0,"km":1700,"pay":"Wire Transfer","c":"Las Vegas","s":"NV","st":"Pending"},{"id":98,"dt":"2027-06-18","m":"Cayenne E-Hybrid","f":"Cayenne","y":2023,"p":96800.0,"km":14000,"pay":"Lease","c":"Charlotte","s":"NC","st":"Delivered"},{"id":99,"dt":"2027-07-03","m":"Macan T","f":"Macan","y":2022,"p":74400.0,"km":20750,"pay":"Cash","c":"Mesa","s":"AZ","st":"Awaiting Pickup"},{"id":100,"dt":"2027-07-22","m":"Taycan GTS","f":"Taycan","y":2025,"p":148500.0,"km":5200,"pay":"Crypto Payment","c":"Atlanta","s":"GA","st":"Delivered"},{"id":101,"dt":"2027-08-08","m":"718 Boxster","f":"718","y":2021,"p":71900.0,"km":29600,"pay":"Bank Transfer","c":"Long Beach","s":"CA","st":"Pending"},{"id":102,"dt":null,"m":"911 Targa 4","f":"911","y":2024,"p":143250.0,"km":3100,"pay":"Financing","c":"Oakland","s":"CA","st":"In Transit"},{"id":103,"dt":"2027-09-19","m":"Panamera Turbo","f":"Panamera","y":2020,"p":137500.0,"km":48000,"pay":"Wire Transfer","c":"Tulsa","s":"OK","st":"Delivered"},{"id":104,"dt":"2027-09-25","m":"Cayenne Turbo GT","f":"Cayenne","y":2025,"p":204800.0,"km":3300,"pay":"Credit Card","c":"Wichita","s":"KS","st":"Pending"},{"id":105,"dt":"2027-10-01","m":"911 Dakar","f":"911","y":2024,"p":271700.0,"km":1050,"pay":"Lease","c":"New Orleans","s":"LA","st":"Delivered"}];
const $ = id => document.getElementById(id);
const esc = s => String(s).replace(/[&<>"]/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c]));
const usd = n => new Intl.NumberFormat('pt-BR',{style:'currency',currency:'USD',maximumFractionDigits:0}).format(n);
const usdM = n => 'US$ ' + new Intl.NumberFormat('pt-BR',{minimumFractionDigits:1,maximumFractionDigits:1}).format(n/1e6) + ' mi';
const FAMS = ['911','718','Cayenne','Macan','Panamera','Taycan'];
const S = {model:'',year:'',city:'',pay:'',period:''};

function tally(rows, key){
  const m = new Map();
  rows.forEach(r => { const k = key(r); const o = m.get(k) || {k, n:0, rev:0, rows:[]}; o.n++; o.rev += r.p; o.rows.push(r); m.set(k,o); });
  return [...m.values()].sort((a,b) => b.n - a.n || b.rev - a.rev || String(a.k).localeCompare(String(b.k),'pt'));
}
function match(r, skip){
  if (skip!=='model' && S.model && r.m!==S.model) return false;
  if (skip!=='year' && S.year && r.y!==+S.year) return false;
  if (skip!=='city' && S.city && r.c!==S.city) return false;
  if (skip!=='pay' && S.pay && r.pay!==S.pay) return false;
  if (skip!=='period' && S.period){
    if (S.period==='none') { if (r.dt) return false; }
    else if (!r.dt || r.dt.slice(0,4)!==S.period) return false;
  }
  return true;
}
const rowsFor = skip => DATA.filter(r => match(r, skip));

function fillSelects(){
  const opt = (v,t) => `<option value="${esc(v)}">${esc(t)}</option>`;
  let h = opt('','Todos os modelos');
  FAMS.forEach(f => { h += `<optgroup label="${f}">` + [...new Set(DATA.filter(r=>r.f===f).map(r=>r.m))].sort((a,b)=>a.localeCompare(b,'pt',{numeric:true})).map(m=>opt(m,m)).join('') + '</optgroup>'; });
  $('fModel').innerHTML = h;
  $('fYear').innerHTML = opt('','Todos os anos') + [...new Set(DATA.map(r=>r.y))].sort((a,b)=>b-a).map(y=>opt(y,y)).join('');
  $('fCity').innerHTML = opt('','Todas as cidades') + [...new Set(DATA.map(r=>r.c))].sort((a,b)=>a.localeCompare(b,'pt')).map(c=>opt(c,c)).join('');
  $('fPay').innerHTML = opt('','Todos os métodos') + [...new Set(DATA.map(r=>r.pay))].sort((a,b)=>a.localeCompare(b,'pt')).map(c=>opt(c,c)).join('');
  const yrs = [...new Set(DATA.filter(r=>r.dt).map(r=>r.dt.slice(0,4)))].sort();
  $('fPeriod').innerHTML = opt('','Todo o período') + yrs.map(y=>opt(y,y)).join('') + opt('none','Sem data válida');
}
const sel = {fModel:'model', fYear:'year', fCity:'city', fPay:'pay', fPeriod:'period'};
Object.entries(sel).forEach(([id,k]) => $(id).addEventListener('change', e => { S[k] = e.target.value; render(); }));
$('reset').addEventListener('click', () => { Object.keys(S).forEach(k => S[k]=''); render(); });

function barList(items, max, topN=1){
  return items.map((o,i) => `<div class="bar ${i<topN&&o.n===items[0].n?'top':''}"><span class="k" title="${esc(o.k)}">${esc(o.k)}</span><span class="t"><i style="width:${(o.n/max*100).toFixed(1)}%"></i></span><span class="c">${o.n}</span></div>`).join('');
}

function render(){
  Object.entries(sel).forEach(([id,k]) => { $(id).value = S[k]; $(id).classList.toggle('on', !!S[k]); });
  const rows = rowsFor();

  /* KPIs */
  const n = rows.length, rev = rows.reduce((a,r)=>a+r.p,0);
  const topM = tally(rows, r=>r.m), topY = tally(rows, r=>r.y);
  const tie = (arr) => arr.length>1 && arr[1].n===arr[0].n ? ` · empate com ${arr.filter(o=>o.n===arr[0].n).length-1}` : '';
  $('kpis').innerHTML = n ? [
    ['Vendas', n, 'unidades no recorte'],
    ['Receita', usdM(rev), 'soma dos preços de venda'],
    ['Ticket médio', usd(rev/n), 'por veículo'],
    ['Modelo líder', esc(topM[0].k), topM[0].n + (topM[0].n>1?' unidades':' unidade') + tie(topM)],
    ['Ano-modelo líder', topY[0].k, topY[0].n + ' unidades' + tie(topY)],
  ].map(k=>`<div class="kpi"><div class="l">${k[0]}</div><div class="v">${k[1]}</div><div class="n">${k[2]}</div></div>`).join('') : '';
  if(!n) $('kpis').innerHTML = '<div class="kpi"><div class="l">Vendas</div><div class="v">0</div><div class="n">Nenhuma venda com esses filtros</div></div>';

  /* tabela de cidades */
  const cities = tally(rows, r=>r.c);
  if(!cities.length){ $('cityTable').innerHTML = '<div class="empty">Nenhuma cidade com esses filtros. Use “Limpar” para recomeçar.</div>'; }
  else $('cityTable').innerHTML = `<table><thead><tr><th>Cidade</th><th class="num">Vendas</th><th class="num">Receita</th><th>Modelo mais vendido</th></tr></thead><tbody>` +
    cities.map(c => {
      const ms = tally(c.rows, r=>r.m); const best = ms.filter(o=>o.n===ms[0].n);
      return `<tr class="row ${S.city===c.k?'sel':''}" data-city="${esc(c.k)}" tabindex="0"><td>${esc(c.k)}<span class="uf">${esc(c.rows[0].s)}</span></td><td class="num">${c.n}</td><td class="num">${usd(c.rev)}</td><td>${best.map(o=>`<span class="tag ${best.length===1?'lead':''}">${esc(o.k)}${o.n>1?' · '+o.n:''}</span>`).join('')}</td></tr>`;
    }).join('') + '</tbody></table>';

  /* barras */
  const fam = tally(rows, r=>r.f);
  $('famBars').innerHTML = fam.length ? barList(fam, fam[0].n) : '<div class="hint">Sem dados</div>';
  const pay = tally(rows, r=>r.pay);
  $('payBars').innerHTML = pay.length ? barList(pay, pay[0].n) : '<div class="hint">Sem dados</div>';

  /* ano-modelo (ignora o filtro de ano para manter a comparação) */
  const yr = rowsFor('year'); const ys = [...new Set(DATA.map(r=>r.y))].sort();
  const cnt = Object.fromEntries(ys.map(y=>[y,yr.filter(r=>r.y===y).length])); const mx = Math.max(1,...Object.values(cnt));
  const lead = Math.max(...Object.values(cnt));
  $('years').innerHTML = ys.map(y => `<button type="button" class="ycol ${cnt[y]===lead&&lead>0?'lead':''} ${S.year==y?'sel':''}" data-year="${y}" aria-label="Ano-modelo ${y}: ${cnt[y]} vendas"><span class="n">${cnt[y]}</span><span class="b" style="height:${(cnt[y]/mx*150).toFixed(0)}px"></span></button>`).join('');
  $('ylabels').innerHTML = ys.map(y=>`<span>${y}</span>`).join('');
  const leaders = ys.filter(y=>cnt[y]===lead && lead>0);
  const per = S.period==='' ? 'em todo o período' : S.period==='none' ? 'nas vendas sem data válida' : 'em ' + S.period;
  $('yearHint').textContent = lead ? `Ano-modelo mais vendido ${per}: ${leaders.join(' e ')}, com ${lead} ${lead>1?'unidades':'unidade'}${leaders.length>1?' cada':''}. Clique em uma coluna para filtrar.` : 'Nenhuma venda no recorte.';

  /* mensal */
  const dated = rows.filter(r=>r.dt);
  const none = rows.length - dated.length;
  if(dated.length){
    const mm = dated.map(r=>r.dt.slice(0,7)).sort(); const [a,b] = [mm[0], mm[mm.length-1]];
    const series = []; let [y,m] = a.split('-').map(Number); const [by,bm] = b.split('-').map(Number);
    while(y<by || (y===by && m<=bm)){ const k = y+'-'+String(m).padStart(2,'0'); series.push({k, v:mm.filter(x=>x===k).length}); m++; if(m>12){m=1;y++;} }
    const W=560,H=210,L=26,R=12,T=14,B=26; const vm = Math.max(2,...series.map(s=>s.v));
    const x = i => series.length===1 ? (L+W-R)/2 : L + i*(W-L-R)/(series.length-1);
    const yy = v => H-B - v/vm*(H-B-T);
    const pts = series.map((s,i)=>`${x(i).toFixed(1)},${yy(s.v).toFixed(1)}`);
    const grid = [0,Math.ceil(vm/2),vm].map(g=>`<line x1="${L}" x2="${W-R}" y1="${yy(g)}" y2="${yy(g)}" stroke="var(--line)" stroke-width="1"/><text x="${L-6}" y="${yy(g)+4}" text-anchor="end">${g}</text>`).join('');
    const MES=['jan','fev','mar','abr','mai','jun','jul','ago','set','out','nov','dez'];
    const step = series.length>30 ? 6 : series.length>14 ? 3 : 1;
    const lab = k => MES[+k.slice(5)-1] + '/' + k.slice(2,4);
    const ticks = series.map((s,i)=> i%step===0 ? `<line x1="${x(i)}" x2="${x(i)}" y1="${H-B}" y2="${H-B+4}" stroke="var(--line)"/><text x="${x(i)}" y="${H-8}" text-anchor="${i===0?'start':'middle'}">${lab(s.k)}</text>`:'').join('');
    const last = series.length-1;
    $('monthly').innerHTML = `<svg viewBox="0 0 ${W} ${H}" width="100%" role="img" aria-label="Vendas por mês">${grid}${ticks}
      <polygon points="${x(0)},${yy(0)} ${pts.join(' ')} ${x(last)},${yy(0)}" fill="var(--accent)" fill-opacity=".14"/>
      <polyline points="${pts.join(' ')}" fill="none" stroke="var(--accent)" stroke-width="1.6" stroke-linejoin="round"/>
      <circle cx="${x(last)}" cy="${yy(series[last].v)}" r="3.5" fill="var(--accent)"/></svg>`;
  } else $('monthly').innerHTML = '<div class="empty">Sem vendas com data válida neste recorte.</div>';
  $('monthNote').textContent = none ? `${none} ${none>1?'vendas':'venda'} sem data válida ${dated.length?'não aparecem':'aparecem apenas nos totais'} neste gráfico.` : '';

  /* vitrine */
  $('vitrineTitle').textContent = 'Vitrine · mais procurados ' + (S.city ? 'em ' + S.city : 'em todas as cidades');
  $('cards').innerHTML = topM.length ? topM.slice(0,6).map((o,i) => {
    const yrs = [...new Set(o.rows.map(r=>r.y))].sort(); const cs = [...new Set(o.rows.map(r=>r.c))];
    return `<article class="card"><div class="top"><span class="fam">${esc(o.rows[0].f)}</span><span class="rk">N.º ${i+1}</span></div>
      <h4>${esc(o.k)}</h4>
      <dl><dt>Vendas</dt><dd>${o.n}</dd><dt>Preço médio</dt><dd>${usd(o.rev/o.n)}</dd><dt>Ano-modelo</dt><dd>${yrs[0]}${yrs.length>1?'–'+yrs[yrs.length-1]:''}</dd><dt>Cidades</dt><dd>${esc(cs.slice(0,2).join(', '))}${cs.length>2?' +'+(cs.length-2):''}</dd></dl></article>`;
  }).join('') : '<div class="empty" style="grid-column:1/-1">Nenhum modelo encontrado com esses filtros.</div>';

  $('foot').innerHTML = `<span>${n} de ${DATA.length} vendas no recorte</span><span>Datas inválidas na planilha: ${DATA.filter(r=>!r.dt).length}</span><span>Algumas vendas têm data posterior a hoje; foram mantidas como estão na base.</span>`;
}

$('cityTable').addEventListener('click', e => { const tr = e.target.closest('tr[data-city]'); if(tr){ S.city = S.city===tr.dataset.city ? '' : tr.dataset.city; render(); } });
$('cityTable').addEventListener('keydown', e => { if(e.key==='Enter'||e.key===' '){ const tr = e.target.closest('tr[data-city]'); if(tr){ e.preventDefault(); S.city = S.city===tr.dataset.city ? '' : tr.dataset.city; render(); } } });
$('years').addEventListener('click', e => { const b = e.target.closest('[data-year]'); if(b){ S.year = S.year===b.dataset.year ? '' : b.dataset.year; render(); } });

fillSelects(); render();
</script>
