[facturen-tracker.html](https://github.com/user-attachments/files/26455699/facturen-tracker.html)# facturen-DPL-tracker
[Up<!DOCTYPE html>
<html lang="nl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Facturen Tracker — De Palletleverancier</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: #f5f4f0; color: #1a1a18; font-size: 14px; line-height: 1.5; min-height: 100vh; }
.container { max-width: 960px; margin: 0 auto; padding: 2rem 1.25rem; }
.topbar { display: flex; justify-content: space-between; align-items: center; margin-bottom: 1.75rem; flex-wrap: wrap; gap: 8px; }
.logo { font-size: 16px; font-weight: 600; }
.logo span { color: #888; font-weight: 400; }
.date { font-size: 12px; color: #888; }
.metrics { display: grid; grid-template-columns: repeat(4,1fr); gap: 10px; margin-bottom: 1.5rem; }
@media(max-width:580px){ .metrics { grid-template-columns: 1fr 1fr; } }
.metric { background: #fff; border: 1px solid #e5e3dd; border-radius: 10px; padding: 1rem; }
.metric-label { font-size: 11px; color: #888; text-transform: uppercase; letter-spacing: .05em; margin-bottom: 4px; }
.metric-value { font-size: 20px; font-weight: 600; }
.metric-value.red { color: #a32d2d; }
.metric-value.orange { color: #854f0b; }
.metric-value.green { color: #3b6d11; }
.apibar { background: #fff; border: 1px solid #e5e3dd; border-radius: 10px; padding: .75rem 1rem; display: flex; align-items: center; gap: 10px; margin-bottom: 1rem; flex-wrap: wrap; }
.apibar label { font-size: 12px; color: #555; white-space: nowrap; }
.apibar input { flex: 1; min-width: 220px; font-size: 12px; font-family: monospace; padding: 5px 8px; border: 1px solid #d5d3cd; border-radius: 6px; background: #f5f4f0; color: #1a1a18; }
.apibar input:focus { outline: none; border-color: #185fa5; }
.apibar .ok { font-size: 11px; color: #3b6d11; }
.dropzone { background: #fff; border: 2px dashed #c5c3bd; border-radius: 10px; padding: 1.75rem; text-align: center; cursor: pointer; position: relative; margin-bottom: 1.25rem; transition: border-color .15s, background .15s; }
.dropzone:hover, .dropzone.over { border-color: #185fa5; background: #f0f6ff; }
.dropzone.busy { border-color: #854f0b; background: #fdf6ec; }
.dropzone input[type=file] { position: absolute; inset: 0; opacity: 0; cursor: pointer; width: 100%; height: 100%; }
.dz-icon { font-size: 26px; margin-bottom: 6px; }
.dz-title { font-size: 14px; font-weight: 500; margin-bottom: 3px; }
.dz-sub { font-size: 12px; color: #888; }
.spinner { width: 22px; height: 22px; border: 2px solid #e5e3dd; border-top-color: #854f0b; border-radius: 50%; animation: spin .7s linear infinite; margin: 0 auto 8px; display: none; }
.dropzone.busy .spinner { display: block; }
.dropzone.busy .dz-icon { display: none; }
@keyframes spin { to { transform: rotate(360deg); } }
.controls { display: flex; gap: 6px; margin-bottom: 1rem; flex-wrap: wrap; align-items: center; }
.fbtn { font-size: 12px; padding: 5px 13px; border-radius: 20px; border: 1px solid #c5c3bd; background: transparent; color: #666; cursor: pointer; }
.fbtn.on { background: #1a1a18; color: #fff; border-color: #1a1a18; }
.fbtn:hover:not(.on) { background: #eee; }
.addbtn { margin-left: auto; font-size: 12px; padding: 5px 14px; border-radius: 20px; border: 1px solid #c5c3bd; background: #fff; color: #1a1a18; cursor: pointer; font-weight: 500; }
.addbtn:hover { background: #f0ede6; }
.card { background: #fff; border: 1px solid #e5e3dd; border-radius: 10px; overflow: hidden; }
table { width: 100%; border-collapse: collapse; }
th { font-size: 10px; font-weight: 600; color: #888; text-align: left; padding: 9px 14px; border-bottom: 1px solid #e5e3dd; text-transform: uppercase; letter-spacing: .06em; white-space: nowrap; }
td { padding: 11px 14px; border-bottom: 1px solid #f0ede6; color: #1a1a18; vertical-align: middle; }
tr:last-child td { border-bottom: none; }
tr:hover td { background: #fafaf8; }
.badge { display: inline-block; font-size: 11px; font-weight: 500; padding: 3px 9px; border-radius: 20px; white-space: nowrap; }
.b-red   { background: #fcebeb; color: #a32d2d; }
.b-orange{ background: #faeeda; color: #854f0b; }
.b-blue  { background: #e6f1fb; color: #185fa5; }
.b-green { background: #eaf3de; color: #3b6d11; }
.b-gray  { background: #f0ede6; color: #666; }
.abtn { font-size: 11px; padding: 3px 8px; border-radius: 5px; border: 1px solid #d5d3cd; background: transparent; color: #666; cursor: pointer; margin-left: 3px; }
.abtn:hover { background: #f0ede6; color: #1a1a18; }
.abtn.del { color: #a32d2d; }
.abtn.del:hover { background: #fcebeb; }
.mono { font-family: monospace; font-size: 12px; }
.muted { color: #aaa; }
.empty { padding: 2.5rem; text-align: center; color: #aaa; font-size: 13px; }
.note { font-size: 11px; color: #854f0b; margin-top: 2px; }
.overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,.4); z-index: 100; align-items: center; justify-content: center; }
.overlay.open { display: flex; }
.modal { background: #fff; border-radius: 12px; border: 1px solid #d5d3cd; padding: 1.5rem; width: 370px; max-width: 95vw; }
.modal h2 { font-size: 15px; font-weight: 600; margin-bottom: 1.25rem; }
.frow { margin-bottom: .9rem; }
.frow2 { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: .9rem; }
.frow label, .frow2 label { font-size: 11px; color: #888; text-transform: uppercase; letter-spacing: .04em; display: block; margin-bottom: 3px; }
.finput { width: 100%; font-size: 13px; padding: 7px 9px; border: 1px solid #d5d3cd; border-radius: 7px; background: #fafaf8; color: #1a1a18; }
.finput:focus { outline: none; border-color: #185fa5; background: #fff; }
.mactions { display: flex; gap: 8px; justify-content: flex-end; margin-top: 1.25rem; }
.mcancelbtn { font-size: 13px; padding: 7px 16px; border-radius: 7px; border: 1px solid #d5d3cd; background: transparent; color: #666; cursor: pointer; }
.msavebtn { font-size: 13px; padding: 7px 16px; border-radius: 7px; border: none; background: #1a1a18; color: #fff; cursor: pointer; font-weight: 500; }
.toast { position: fixed; bottom: 1.5rem; right: 1.5rem; background: #1a1a18; color: #fff; font-size: 13px; padding: 9px 16px; border-radius: 7px; z-index: 999; opacity: 0; transform: translateY(6px); transition: all .2s; pointer-events: none; }
.toast.show { opacity: 1; transform: translateY(0); }
</style>
</head>
<body>
<div class="container">

  <div class="topbar">
    <div class="logo">De Palletleverancier <span>/ Facturen</span></div>
    <div class="date" id="datum"></div>
  </div>

  <div class="metrics">
    <div class="metric"><div class="metric-label">Openstaand</div><div class="metric-value" id="m1">—</div></div>
    <div class="metric"><div class="metric-label">Vervallen</div><div class="metric-value red" id="m2">—</div></div>
    <div class="metric"><div class="metric-label">Deze week</div><div class="metric-value orange" id="m3">—</div></div>
    <div class="metric"><div class="metric-label">Betaald (30d)</div><div class="metric-value green" id="m4">—</div></div>
  </div>

  <div class="apibar">
    <label>🔑 Anthropic API key:</label>
    <input type="password" id="apikey" placeholder="sk-ant-api03-..." oninput="onKey(this.value)">
    <span class="ok" id="keystatus"></span>
  </div>

  <div class="dropzone" id="dz">
    <input type="file" accept=".pdf" multiple onchange="handleFiles(this.files)" id="fileinput">
    <div class="spinner"></div>
    <div class="dz-icon">📄</div>
    <div class="dz-title" id="dztitle">Sleep factuur-PDF's hierheen</div>
    <div class="dz-sub">of klik om te bladeren — bedrag en vervaldatum worden automatisch uitgelezen</div>
  </div>

  <div class="controls">
    <button class="fbtn on" onclick="setFilter('open',this)">Openstaand</button>
    <button class="fbtn" onclick="setFilter('week',this)">Deze week</button>
    <button class="fbtn" onclick="setFilter('vervallen',this)">Vervallen</button>
    <button class="fbtn" onclick="setFilter('betaald',this)">Betaald</button>
    <button class="fbtn" onclick="setFilter('all',this)">Alles</button>
    <button class="addbtn" onclick="openModal()">+ Handmatig toevoegen</button>
  </div>

  <div class="card">
    <table>
      <thead>
        <tr>
          <th>Leverancier</th>
          <th>Factuurnr.</th>
          <th>Bedrag excl.</th>
          <th>Vervaldatum</th>
          <th>Status</th>
          <th></th>
        </tr>
      </thead>
      <tbody id="tbody"></tbody>
    </table>
    <div class="empty" id="empty" style="display:none">Geen facturen in deze weergave</div>
  </div>
</div>

<div class="overlay" id="overlay" onclick="bgClick(event)">
  <div class="modal">
    <h2 id="modaltitel">Factuur toevoegen</h2>
    <input type="hidden" id="eid">
    <div class="frow">
      <label>Leverancier</label>
      <input class="finput" id="flev" placeholder="bv. Veeke Palletreparatie">
    </div>
    <div class="frow2">
      <div><label>Factuurnummer</label><input class="finput" id="fnr" placeholder="F01099"></div>
      <div><label>Bedrag excl. btw (€)</label><input class="finput" id="fbed" type="number" placeholder="0.00" step="0.01" min="0"></div>
    </div>
    <div class="frow2">
      <div><label>Factuurdatum</label><input class="finput" id="fdat" type="date"></div>
      <div><label>Vervaldatum</label><input class="finput" id="fver" type="date"></div>
    </div>
    <div class="frow">
      <label>Notitie</label>
      <input class="finput" id="fnot" placeholder="optioneel">
    </div>
    <div class="mactions">
      <button class="mcancelbtn" onclick="closeModal()">Annuleren</button>
      <button class="msavebtn" onclick="saveFactuur()">Opslaan</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<script>
pdfjsLib.GlobalWorkerOptions.workerSrc='https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';

const SKEY='dpl_facturen_v3', AKEY='dpl_apikey';
let rows=[], activeFilter='open', busy=false;

const today=new Date(); today.setHours(0,0,0,0);
const week7=new Date(today); week7.setDate(week7.getDate()+7);

document.getElementById('datum').textContent=today.toLocaleDateString('nl-NL',{weekday:'long',day:'numeric',month:'long',year:'numeric'});

const init=[
  {id:1,lev:'Veeke Palletreparatie',nr:'F01042',bed:null,dat:'2026-03-27',ver:null,st:'open',not:''},
  {id:2,lev:'Veeke Palletreparatie',nr:'F01076',bed:null,dat:'2026-03-27',ver:null,st:'open',not:''},
  {id:3,lev:'Veeke Palletreparatie',nr:'F01099',bed:null,dat:'2026-03-27',ver:null,st:'open',not:''},
  {id:4,lev:'Veeke Palletreparatie',nr:'F01102',bed:null,dat:'2026-03-27',ver:null,st:'open',not:''},
  {id:5,lev:'Ter Slaa Pallets',nr:'26700422',bed:null,dat:'2026-03-27',ver:null,st:'open',not:'⚠️ Nieuw rekeningnummer per 25 mrt'},
  {id:6,lev:'Kaan / ARKA Digital',nr:'PAL-2026-001',bed:null,dat:'2026-04-01',ver:null,st:'open',not:''},
  {id:7,lev:'Beequip (trailer lease)',nr:'VF699133',bed:null,dat:'2026-03-30',ver:null,st:'incasso',not:'Automatische incasso'},
];

function loadData(){
  try{ const r=localStorage.getItem(SKEY); rows=r?JSON.parse(r):init.map(x=>({...x})); }
  catch(e){ rows=init.map(x=>({...x})); }
}
function saveData(){ try{localStorage.setItem(SKEY,JSON.stringify(rows));}catch(e){} }

function onKey(v){
  try{localStorage.setItem(AKEY,v);}catch(e){}
  document.getElementById('keystatus').textContent=v.startsWith('sk-ant-')?'✓ Opgeslagen':'';
}
function getKey(){ return document.getElementById('apikey').value||(localStorage.getItem(AKEY)||''); }

try{
  const k=localStorage.getItem(AKEY);
  if(k){document.getElementById('apikey').value=k;document.getElementById('keystatus').textContent='✓ Opgeslagen';}
}catch(e){}

function getStatus(f){
  if(f.st==='betaald') return 'betaald';
  if(f.st==='incasso') return 'incasso';
  if(!f.ver) return 'open';
  const v=new Date(f.ver+'T00:00:00');
  if(v<today) return 'vervallen';
  if(v<=week7) return 'week';
  return 'open';
}

function makeBadge(f){
  const s=getStatus(f);
  const m={betaald:['b-green','Betaald'],incasso:['b-blue','Auto-incasso'],vervallen:['b-red','Vervallen'],week:['b-orange','Vervalt deze week'],open:f.ver?['b-blue','Openstaand']:['b-gray','Verval onbekend']};
  const [c,l]=m[s]||['b-gray',s];
  return '<span class="badge '+c+'">'+l+'</span>';
}

function filterOk(f){
  const s=getStatus(f);
  if(activeFilter==='all') return true;
  if(activeFilter==='betaald') return s==='betaald'||s==='incasso';
  if(activeFilter==='vervallen') return s==='vervallen';
  if(activeFilter==='week') return s==='week';
  if(activeFilter==='open') return s!=='betaald';
  return true;
}

function euro(v){ return v!=null?'€\u202f'+Number(v).toLocaleString('nl-NL',{minimumFractionDigits:2,maximumFractionDigits:2}):'<span class="muted">?</span>'; }
function datStr(d){ return d?new Date(d+'T00:00:00').toLocaleDateString('nl-NL',{day:'numeric',month:'short',year:'numeric'}):'<span class="muted">onbekend</span>'; }
function metricFmt(v){ return v>0?'€\u202f'+Math.round(v).toLocaleString('nl-NL'):'—'; }

function render(){
  const list=rows.filter(filterOk);
  let tot=0,ov=0,wk=0,paid=0;
  rows.forEach(f=>{
    const s=getStatus(f); const b=f.bed||0;
    if(s==='betaald'||s==='incasso') paid+=b;
    else{ tot+=b; if(s==='vervallen') ov+=b; if(s==='week') wk+=b; }
  });
  document.getElementById('m1').textContent=metricFmt(tot);
  document.getElementById('m2').textContent=metricFmt(ov);
  document.getElementById('m3').textContent=metricFmt(wk);
  document.getElementById('m4').textContent=metricFmt(paid);

  const tbody=document.getElementById('tbody');
  const empty=document.getElementById('empty');
  if(!list.length){ tbody.innerHTML=''; empty.style.display=''; return; }
  empty.style.display='none';

  tbody.innerHTML=list.map(f=>{
    const s=getStatus(f);
    const isPaid=s==='betaald'||s==='incasso';
    return '<tr>'
      +'<td><strong>'+f.lev+'</strong>'+(f.not?'<div class="note">'+f.not+'</div>':'')+'</td>'
      +'<td><span class="mono">'+f.nr+'</span></td>'
      +'<td>'+euro(f.bed)+'</td>'
      +'<td>'+datStr(f.ver)+'</td>'
      +'<td>'+makeBadge(f)+'</td>'
      +'<td style="text-align:right;white-space:nowrap">'
        +(isPaid?'':'<button class="abtn" onclick="markPaid('+f.id+')">✓ Betaald</button>')
        +'<button class="abtn" onclick="editF('+f.id+')">Bewerk</button>'
        +'<button class="abtn del" onclick="delF('+f.id+')">×</button>'
      +'</td>'
      +'</tr>';
  }).join('');
}

function setFilter(f,btn){
  activeFilter=f;
  document.querySelectorAll('.fbtn').forEach(b=>b.classList.remove('on'));
  btn.classList.add('on');
  render();
}

function markPaid(id){
  const f=rows.find(x=>x.id===id);
  if(f){f.st='betaald';saveData();render();showToast('Factuur gemarkeerd als betaald');}
}
function delF(id){
  if(!confirm('Factuur verwijderen?')) return;
  rows=rows.filter(x=>x.id!==id);
  saveData();render();
}

function openModal(f){
  document.getElementById('modaltitel').textContent=f?'Factuur bewerken':'Factuur toevoegen';
  document.getElementById('eid').value=f?f.id:'';
  document.getElementById('flev').value=f?f.lev:'';
  document.getElementById('fnr').value=f?f.nr:'';
  document.getElementById('fbed').value=(f&&f.bed!=null)?f.bed:'';
  document.getElementById('fdat').value=f?(f.dat||''):new Date().toISOString().split('T')[0];
  document.getElementById('fver').value=f?(f.ver||''):'';
  document.getElementById('fnot').value=f?(f.not||''):'';
  document.getElementById('overlay').classList.add('open');
}
function closeModal(){ document.getElementById('overlay').classList.remove('open'); }
function bgClick(e){ if(e.target===document.getElementById('overlay')) closeModal(); }
function editF(id){ openModal(rows.find(f=>f.id===id)); }

function saveFactuur(){
  const eid=document.getElementById('eid').value;
  const obj={
    lev:document.getElementById('flev').value.trim(),
    nr:document.getElementById('fnr').value.trim(),
    bed:parseFloat(document.getElementById('fbed').value)||null,
    dat:document.getElementById('fdat').value||null,
    ver:document.getElementById('fver').value||null,
    not:document.getElementById('fnot').value.trim(),
    st:'open'
  };
  if(!obj.lev||!obj.nr){alert('Vul leverancier en factuurnummer in.');return;}
  if(eid){
    const i=rows.findIndex(f=>f.id==eid);
    if(i>-1){obj.id=rows[i].id;obj.st=rows[i].st;rows[i]=obj;}
  } else {obj.id=Date.now();rows.push(obj);}
  saveData();closeModal();render();
  showToast(eid?'Factuur bijgewerkt':'Factuur toegevoegd');
}

// Drag & drop
const dz=document.getElementById('dz');
dz.addEventListener('dragover',e=>{e.preventDefault();dz.classList.add('over');});
dz.addEventListener('dragleave',()=>dz.classList.remove('over'));
dz.addEventListener('drop',e=>{e.preventDefault();dz.classList.remove('over');handleFiles(e.dataTransfer.files);});

function handleFiles(files){
  const pdfs=Array.from(files).filter(f=>f.name.toLowerCase().endsWith('.pdf'));
  if(!pdfs.length){showToast('Alleen PDF-bestanden worden ondersteund');return;}
  pdfs.forEach(processPdf);
}

async function extractText(file){
  const buf=await file.arrayBuffer();
  const pdf=await pdfjsLib.getDocument({data:buf}).promise;
  let txt='';
  for(let i=1;i<=Math.min(pdf.numPages,3);i++){
    const pg=await pdf.getPage(i);
    const c=await pg.getTextContent();
    txt+=c.items.map(x=>x.str).join(' ')+'\n';
  }
  return txt;
}

async function processPdf(file){
  if(busy){showToast('Even wachten, vorige PDF wordt nog verwerkt...');return;}
  const key=getKey();
  if(!key||!key.startsWith('sk-ant-')){showToast('Vul eerst een geldige Anthropic API key in');return;}
  busy=true;
  dz.classList.add('busy');
  document.getElementById('dztitle').textContent=file.name+' verwerken...';
  try{
    const txt=await extractText(file);
    const prompt='Je bent een factuur-parser. Extraheer uit deze factuur:\n- leverancier: naam afzender\n- factuurnummer\n- bedrag: totaal EXCLUSIEF btw als getal\n- factuurdatum: YYYY-MM-DD\n- vervaldatum: YYYY-MM-DD\n\nAntwoord UITSLUITEND met JSON:\n{"leverancier":"...","factuurnummer":"...","bedrag":0.00,"factuurdatum":"YYYY-MM-DD","vervaldatum":"YYYY-MM-DD"}\n\nGebruik null als een veld ontbreekt.\n\nFactuur:\n'+txt.slice(0,3000);

    const res=await fetch('https://api.anthropic.com/v1/messages',{
      method:'POST',
      headers:{'Content-Type':'application/json','x-api-key':key,'anthropic-version':'2023-06-01','anthropic-dangerous-direct-browser-access':'true'},
      body:JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:300,messages:[{role:'user',content:prompt}]})
    });
    if(!res.ok){const e=await res.json();throw new Error(e.error?.message||'API fout '+res.status);}
    const json=await res.json();
    const p=JSON.parse((json.content[0]?.text||'{}').replace(/```json|```/g,'').trim());

    const existing=rows.find(f=>(p.factuurnummer&&f.nr===p.factuurnummer)||(p.leverancier&&f.lev===p.leverancier&&f.dat===p.factuurdatum));
    if(existing){
      if(p.bedrag!=null) existing.bed=p.bedrag;
      if(p.vervaldatum) existing.ver=p.vervaldatum;
      if(p.factuurdatum) existing.dat=p.factuurdatum;
      saveData();render();showToast(existing.nr+' bijgewerkt vanuit PDF');
    } else {
      const nw={id:Date.now(),lev:p.leverancier||file.name.replace('.pdf',''),nr:p.factuurnummer||file.name.replace('.pdf',''),bed:p.bedrag||null,dat:p.factuurdatum||null,ver:p.vervaldatum||null,st:'open',not:''};
      rows.push(nw);saveData();render();showToast(nw.nr+' toegevoegd — '+nw.lev);
    }
  } catch(e){showToast('Fout: '+e.message);console.error(e);}
  finally{
    busy=false;dz.classList.remove('busy');
    document.getElementById('dztitle').textContent="Sleep factuur-PDF's hierheen";
    document.getElementById('fileinput').value='';
  }
}

function showToast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg;t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),3000);
}

loadData();
render();
</script>
</body>
</html>
loading facturen-tracker.html…]()
