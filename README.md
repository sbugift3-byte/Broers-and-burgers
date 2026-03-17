<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Broers & Burgers</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
<style>
*{margin:0;padding:0;box-sizing:border-box}
body{background:#111;color:#FFF8EE;font-family:'DM Sans',sans-serif;padding:20px;min-height:100vh}
h1{font-family:'Playfair Display',serif;color:#C9922A;font-size:28px;margin-bottom:4px}
.sub{color:#666;font-size:13px;margin-bottom:24px}
.card{background:#1a1a1a;border:1px solid #2a2a2a;border-radius:12px;padding:20px;margin-bottom:16px}
.card h2{font-size:14px;font-weight:600;margin-bottom:12px;color:#C9922A;letter-spacing:1px;text-transform:uppercase}
input,select{width:100%;background:#111;border:1px solid #2a2a2a;border-radius:8px;color:#FFF8EE;font-family:'DM Sans',sans-serif;font-size:14px;padding:10px 12px;outline:none;margin-bottom:8px}
input:focus,select:focus{border-color:#C9922A}
.btn{width:100%;padding:12px;background:#C9922A;border:none;border-radius:8px;color:#1A1A1A;font-family:'DM Sans',sans-serif;font-size:14px;font-weight:600;cursor:pointer;margin-bottom:8px}
.btn:hover{background:#E8B84B}
.btn-o{background:transparent;border:1px solid #2a2a2a;color:#FFF8EE}
.btn-o:hover{border-color:#C9922A;color:#C9922A}
.btn:disabled{opacity:0.4;cursor:not-allowed}
.status{padding:10px;border-radius:8px;font-size:13px;margin-bottom:12px;display:none}
.status.ok{display:block;background:rgba(42,122,42,0.15);border:1px solid rgba(42,122,42,0.3);color:#70c070}
.status.err{display:block;background:rgba(170,50,50,0.15);border:1px solid rgba(170,50,50,0.3);color:#e07070}
.status.load{display:block;background:rgba(201,146,42,0.15);border:1px solid rgba(201,146,42,0.3);color:#C9922A}
.rlist{display:flex;flex-direction:column;gap:6px;max-height:220px;overflow-y:auto}
.ritem{padding:10px 14px;background:#111;border:1px solid #2a2a2a;border-radius:8px;cursor:pointer;display:flex;justify-content:space-between;align-items:center;transition:all 0.2s}
.ritem:hover,.ritem.sel{border-color:#C9922A;background:rgba(201,146,42,0.08)}
.rn{font-size:13px;font-weight:500}
.rm{font-size:11px;color:#666;margin-top:2px}
.rs{font-size:14px;font-weight:700;color:#C9922A;font-family:monospace}
.tabs{display:flex;gap:4px;background:#111;padding:4px;border-radius:10px;margin-bottom:12px}
.tab{flex:1;padding:8px;border:none;border-radius:7px;background:transparent;color:#666;font-family:'DM Sans',sans-serif;font-size:12px;cursor:pointer;transition:all 0.2s}
.tab.on{background:#C9922A;color:#1A1A1A;font-weight:600}
.fmts{display:flex;gap:6px;margin-bottom:12px}
.fmt{padding:6px 14px;border-radius:6px;border:1px solid #2a2a2a;background:transparent;color:#666;font-size:12px;cursor:pointer;font-family:'DM Sans',sans-serif}
.fmt.on{border-color:#C9922A;color:#C9922A}
.vchips{display:flex;gap:5px;flex-wrap:wrap;margin-top:6px}
.vc{padding:4px 10px;border-radius:20px;border:1px solid #2a2a2a;background:transparent;color:#666;font-size:11px;cursor:pointer;font-family:'DM Sans',sans-serif}
.vc.on,.vc:hover{border-color:#C9922A;color:#C9922A}
canvas{border-radius:10px;box-shadow:0 8px 30px rgba(0,0,0,0.5);max-width:100%;display:block;margin:0 auto}
.cw{display:flex;flex-direction:column;gap:8px;align-items:center}
.cl{font-size:10px;letter-spacing:1.5px;text-transform:uppercase;color:#444;text-align:center}
.dlb{padding:7px 20px;border-radius:7px;border:1px solid #2a2a2a;background:transparent;color:#FFF8EE;font-size:12px;cursor:pointer;font-family:'DM Sans',sans-serif}
.dlb:hover{border-color:#C9922A;color:#C9922A}
.tr{display:grid;grid-template-columns:36px 1fr;gap:8px;align-items:center;margin-bottom:8px}
.tb{width:36px;height:36px;border-radius:7px;display:flex;align-items:center;justify-content:center;font-family:'Playfair Display',serif;font-size:16px;font-weight:900;color:#fff}
label{font-size:11px;color:#666;display:block;margin-bottom:4px}
</style>
</head>
<body>
<h1>🍔 Broers & Burgers</h1>
<p class="sub">Social Media Graphic Generator</p>

<div class="card">
  <h2>1. Connect Sheet</h2>
  <input type="text" id="sid" value="1HM7v6Vr0E7gAqUTyL826jfJxihknbJnM" placeholder="Google Sheet ID"/>
  <button class="btn" onclick="loadSheet()">Load Data</button>
  <div class="status" id="st"></div>
</div>

<div class="card" id="c2" style="display:none">
  <h2>2. Select Restaurant</h2>
  <div class="rlist" id="rl"></div>
</div>

<div class="card" id="c3" style="display:none">
  <h2>3. Graphic Type</h2>
  <div class="tabs">
    <button class="tab on" onclick="sType('sc',this)">Scorecard</button>
    <button class="tab" onclick="sType('lb',this)">Leaderboard</button>
    <button class="tab" onclick="sType('tl',this)">Tier List</button>
  </div>
  <div id="sc-o">
    <label>Verdict</label>
    <input type="text" id="verd" placeholder="e.g. Top patty in Jozi!"/>
    <div class="vchips">
      <button class="vc" onclick="sv(this,'A hidden gem 💎')">Hidden gem</button>
      <button class="vc" onclick="sv(this,'Top tier! 🔥')">Top tier</button>
      <button class="vc" onclick="sv(this,'Overrated 👎')">Overrated</button>
      <button class="vc" onclick="sv(this,'We\'ll be back 👊')">We\'ll be back</button>
    </div>
  </div>
  <div id="lb-o" style="display:none">
    <label>Caption</label>
    <input type="text" id="lbc" placeholder="e.g. After 5 visits"/>
  </div>
  <div id="tl-o" style="display:none">
    <div id="trows"></div>
  </div>
</div>

<div class="card" id="c4" style="display:none">
  <h2>4. Format</h2>
  <div class="fmts">
    <button class="fmt on" onclick="sFmt('sq',this)">Square</button>
    <button class="fmt" onclick="sFmt('st',this)">Story</button>
    <button class="fmt" onclick="sFmt('bo',this)">Both</button>
  </div>
  <button class="btn" id="gb" onclick="gen()">🍔 Generate Graphic</button>
</div>

<div id="out"></div>

<script>
const MB=['Sbu','Lebza M','KG','Francis','Thabiso','Jabu','Ofentse','Sosa','Vaughan','Lebza N','Vusie'];
const G='#C9922A',BK='#1A1A1A',CR='#FFF8EE',CD='#D4C4A8',GR='#888';
let RS=[],SI=-1,CT='sc',CF='sq',TA={};

async function loadSheet(){
  let id=document.getElementById('sid').value.trim();
  const m=id.match(/\/d\/([a-zA-Z0-9_-]+)/);if(m)id=m[1];
  ss('load','⏳ Loading...');
  try{
    const res=await fetch(`https://docs.google.com/spreadsheets/d/${id}/gviz/tq?tqx=out:json&sheet=Scoreboard`);
    const txt=await res.text();
    const j=JSON.parse(txt.substring(txt.indexOf('{'),txt.lastIndexOf('}')+1));
    RS=[];
    j.table.rows.forEach((row,i)=>{
      if(i<6)return;
      const c=row.c;if(!c||!c[1]||!c[1].v)return;
      const name=String(c[1].v).trim();if(!name||name==='—')return;
      const sc=[];
      for(let s=2;s<=12;s++){const v=c[s]&&c[s].v!=null?parseFloat(c[s].v):null;sc.push({m:MB[s-2],v:isNaN(v)?null:v});}
      const vl=sc.filter(s=>s.v!=null);
      const avg=vl.length?vl.reduce((a,b)=>a+b.v,0)/vl.length:null;
      const price=c[13]&&c[13].v?parseFloat(c[13].v):null;
      const ds=c[17]?(c[17].f||String(c[17].v)||''):'';
      RS.push({name,sc,avg,price,ds});
    });
    if(!RS.length){ss('err','❌ No data. Is sheet public?');return;}
    ss('ok','✓ Loaded '+RS.length+' restaurant'+(RS.length>1?'s!':'!'));
    renderRL();buildTR();
    ['c2','c3','c4'].forEach(id=>document.getElementById(id).style.display='block');
  }catch(e){ss('err','❌ Could not load. Set sheet to Anyone with link.');console.error(e);}
}

function ss(t,m){const e=document.getElementById('st');e.className='status '+t;e.textContent=m;}
function renderRL(){
  const l=document.getElementById('rl');l.innerHTML='';
  RS.forEach((r,i)=>{
    const d=document.createElement('div');d.className='ritem'+(i===SI?' sel':'');d.onclick=()=>selR(i);
    d.innerHTML=`<div><div class="rn">${r.name}</div><div class="rm">${r.ds||'No date'}${r.price?' · R'+r.price:''}</div></div><div class="rs">${r.avg!=null?r.avg.toFixed(2):'—'}</div>`;
    l.appendChild(d);
  });
}
function selR(i){SI=i;renderRL();}
function sType(t,btn){CT=t;document.querySelectorAll('.tab').forEach(b=>b.classList.remove('on'));btn.classList.add('on');['sc-o','lb-o','tl-o'].forEach(id=>document.getElementById(id).style.display='none');document.getElementById(t+'-o').style.display='block';}
function sFmt(f,btn){CF=f;document.querySelectorAll('.fmt').forEach(b=>b.classList.remove('on'));btn.classList.add('on');}
function sv(btn,t){document.getElementById('verd').value=t;document.querySelectorAll('.vc').forEach(b=>b.classList.remove('on'));btn.classList.add('on');}
function buildTR(){
  const c=document.getElementById('trows');c.innerHTML='';
  const tc={S:'#C9922A',A:'#2A7A2A',B:'#2A5A9A',C:'#7A7A2A',D:'#7A2A2A'};
  RS.forEach(r=>{
    if(!TA[r.name])TA[r.name]='B';
    const row=document.createElement('div');row.className='tr';
    const sid='tb_'+r.name.replace(/\W/g,'_');
    row.innerHTML=`<div class="tb" id="${sid}" style="background:${tc[TA[r.name]]}">${TA[r.name]}</div><div><div style="font-size:12px;margin-bottom:3px">${r.name}</div><select onchange="setT('${r.name}',this.value)" style="padding:4px 8px;font-size:11px">${['S','A','B','C','D'].map(t=>`<option ${TA[r.name]===t?'selected':''}>${t}</option>`).join('')}</select></div>`;
    c.appendChild(row);
  });
}
function setT(n,t){TA[n]=t;const tc={S:'#C9922A',A:'#2A7A2A',B:'#2A5A9A',C:'#7A7A2A',D:'#7A2A2A'};const b=document.getElementById('tb_'+n.replace(/\W/g,'_'));if(b){b.style.background=tc[t];b.textContent=t;}}

function gen(){
  if(CT==='sc'&&SI<0){alert('Select a restaurant first!');return;}
  const out=document.getElementById('out');out.innerHTML='';
  const fmts=CF==='bo'?['sq','st']:[CF];
  fmts.forEach(f=>{
    const wrap=document.createElement('div');wrap.className='card cw';
    const lbl=document.createElement('div');lbl.className='cl';lbl.textContent=f==='sq'?'Square 1080×1080':'Story 1080×1920';
    const cv=document.createElement('canvas');
    const W=1080,H=f==='sq'?1080:1920;cv.width=W;cv.height=H;
    const sc=f==='sq'?0.35:0.21;cv.style.width=(W*sc)+'px';cv.style.height=(H*sc)+'px';
    if(CT==='sc')dSC(cv,f);else if(CT==='lb')dLB(cv,f);else dTL(cv,f);
    const dl=document.createElement('button');dl.className='dlb';dl.textContent='↓ Download';
    dl.onclick=()=>{const a=document.createElement('a');a.download='BB_'+CT+'_'+f+'.png';a.href=cv.toDataURL();a.click();};
    wrap.appendChild(lbl);wrap.appendChild(cv);wrap.appendChild(dl);out.appendChild(wrap);
  });
  out.scrollIntoView({behavior:'smooth'});
}

function bg(ctx,W,H){ctx.fillStyle='#FAF3E8';ctx.fillRect(0,0,W,H);}
function rr(ctx,x,y,w,h,r){ctx.beginPath();ctx.moveTo(x+r,y);ctx.lineTo(x+w-r,y);ctx.quadraticCurveTo(x+w,y,x+w,y+r);ctx.lineTo(x+w,y+h-r);ctx.quadraticCurveTo(x+w,y+h,x+w-r,y+h);ctx.lineTo(x+r,y+h);ctx.quadraticCurveTo(x,y+h,x,y+h-r);ctx.lineTo(x,y+r);ctx.quadraticCurveTo(x,y,x+r,y);ctx.closePath();}
function wt(ctx,t,x,y,mw,lh){const ws=t.split(' ');let l='',cy=y;for(let w of ws){const tt=l+(l?' ':'')+w;if(ctx.measureText(tt).width>mw&&l){ctx.fillText(l,x,cy);l=w;cy+=lh*0.6;}else l=tt;}ctx.fillText(l,x,cy);return cy;}
function tb(ctx,W,H,is){ctx.fillStyle=BK;ctx.fillRect(0,0,W,is?H*.052:H*.065);ctx.font=`600 ${W*.021}px 'DM Sans',sans-serif`;ctx.fillStyle=G;ctx.textAlign='left';ctx.fillText('BROERS & BURGERS',W*.07,is?H*.036:H*.044);ctx.font=`400 ${W*.013}px 'DM Sans',sans-serif`;ctx.fillStyle='#555';ctx.textAlign='right';ctx.fillText('#BroersAndBurgers',W-W*.07,is?H*.036:H*.044);}

function dSC(cv,f){
  const ctx=cv.getContext('2d'),W=cv.width,H=cv.height,is=f==='st';
  const r=RS[SI];bg(ctx,W,H);tb(ctx,W,H,is);
  const P=W*.072,v=document.getElementById('verd').value||'';
  const tY=is?H*.072:H*.085;
  ctx.fillStyle=G;rr(ctx,P,tY,W*.30,H*.030,4);ctx.fill();
  ctx.font=`600 ${W*.015}px 'DM Sans',sans-serif`;ctx.fillStyle=BK;ctx.textAlign='left';ctx.fillText('VISIT SCORECARD',P+W*.012,tY+H*.021);
  ctx.font=`900 ${is?W*.075:W*.065}px 'Playfair Display',serif`;ctx.fillStyle=BK;
  const nY=tY+H*.072;const eY=wt(ctx,r.name.toUpperCase(),P,nY,W-P*2,is?H*.088:H*.077);
  const mY=eY+H*.03;ctx.font=`400 ${W*.019}px 'DM Sans',sans-serif`;ctx.fillStyle=GR;ctx.textAlign='left';
  let mt='';if(r.ds)mt+='📅 '+r.ds+'   ';if(r.price)mt+='💰 R'+r.price;if(mt)ctx.fillText(mt,P,mY);
  const dY=mY+H*.026;ctx.strokeStyle=CD;ctx.lineWidth=1.5;ctx.beginPath();ctx.moveTo(P,dY);ctx.lineTo(W-P,dY);ctx.stroke();
  const bW=W*.27,bH=H*(is?.09:.10),bX=W-P-bW,bY=dY+H*.02;
  ctx.fillStyle=BK;rr(ctx,bX,bY,bW,bH,14);ctx.fill();
  ctx.font=`900 ${W*.056}px 'Playfair Display',serif`;ctx.fillStyle=G;ctx.textAlign='center';ctx.fillText(r.avg!=null?r.avg.toFixed(2):'—',bX+bW/2,bY+bH*.64);
  ctx.font=`500 ${W*.014}px 'DM Sans',sans-serif`;ctx.fillStyle=CD;ctx.fillText('GROUP AVG / 10',bX+bW/2,bY+bH*.87);
  const t2=bY+H*.012,lW=W-P*2-bW-W*.035,fn=is?5:4,r1=bH/fn;
  r.sc.slice(0,fn).forEach((s,i)=>{
    const ry=t2+i*r1;ctx.fillStyle=i%2===0?'rgba(0,0,0,0.05)':'transparent';rr(ctx,P,ry,lW,r1-2,4);ctx.fill();
    ctx.font=`500 ${W*.017}px 'DM Sans',sans-serif`;ctx.fillStyle=BK;ctx.textAlign='left';ctx.fillText(s.m,P+W*.012,ry+r1*.65);
    ctx.font=`700 ${W*.019}px monospace`;ctx.fillStyle=s.v!=null?(s.v>=7?'#2A7A2A':s.v>=5?G:'#AA3333'):GR;ctx.textAlign='right';ctx.fillText(s.v!=null?s.v.toFixed(1):'—',P+lW-W*.01,ry+r1*.65);
  });
  const rm=r.sc.slice(fn),gY=bY+bH+H*.02,gC=(W-P*2-W*.025)/2,gR=H*(is?.044:.048);
  rm.forEach((s,i)=>{
    const col=i%2,row=Math.floor(i/2),rx=P+col*(gC+W*.025),ry=gY+row*gR;
    ctx.fillStyle='rgba(0,0,0,0.05)';rr(ctx,rx,ry,gC,gR-3,4);ctx.fill();
    ctx.font=`500 ${W*.017}px 'DM Sans',sans-serif`;ctx.fillStyle=BK;ctx.textAlign='left';ctx.fillText(s.m,rx+W*.012,ry+gR*.65);
    ctx.font=`700 ${W*.019}px monospace`;ctx.fillStyle=s.v!=null?(s.v>=7?'#2A7A2A':s.v>=5?G:'#AA3333'):GR;ctx.textAlign='right';ctx.fillText(s.v!=null?s.v.toFixed(1):'—',rx+gC-W*.012,ry+gR*.65);
  });
  const vY=H-(is?H*.095:H*.105);
  ctx.fillStyle=G;ctx.fillRect(0,vY,W,H*.002);ctx.fillStyle=BK;ctx.fillRect(0,vY+H*.002,W,H-vY);
  ctx.font=`400 ${W*.016}px 'DM Sans',sans-serif`;ctx.fillStyle=CD;ctx.textAlign='left';ctx.fillText('VERDICT',P,vY+H*.038);
  if(v){ctx.font=`italic ${W*.021}px 'Playfair Display',serif`;ctx.fillStyle=CR;ctx.fillText('"'+v+'"',P+ctx.measureText('VERDICT  ').width,vY+H*.038);}
  if(r.price&&r.avg){ctx.font=`500 ${W*.015}px monospace`;ctx.fillStyle=G;ctx.textAlign='right';ctx.fillText('Score/R100: '+(r.avg/r.price*100).toFixed(2),W-P,vY+H*.038);}
}

function dLB(cv,f){
  const ctx=cv.getContext('2d'),W=cv.width,H=cv.height,is=f==='st';
  bg(ctx,W,H);tb(ctx,W,H,is);
  const P=W*.072,cap=document.getElementById('lbc').value||'';
  const sorted=[...RS].filter(r=>r.avg!=null).sort((a,b)=>b.avg-a.avg);
  const md=['🥇','🥈','🥉'];
  const tY=is?H*.095:H*.11;
  ctx.font=`900 ${W*.068}px 'Playfair Display',serif`;ctx.fillStyle=BK;ctx.textAlign='left';ctx.fillText('THE',P,tY);
  ctx.fillStyle=G;ctx.fillText('RANKINGS',P,tY+W*.074);
  if(cap){ctx.font=`400 ${W*.019}px 'DM Sans',sans-serif`;ctx.fillStyle=GR;ctx.fillText(cap,P,tY+W*.074+H*.038);}
  const dY=tY+W*.074+H*(cap?.062:.042);
  ctx.strokeStyle=CD;ctx.lineWidth=1.5;ctx.beginPath();ctx.moveTo(P,dY);ctx.lineTo(W-P,dY);ctx.stroke();
  const rH=is?H*.067:H*.071,sY=dY+H*.018;
  sorted.forEach((r,i)=>{
    const ry=sY+i*(rH+H*.007);if(ry+rH>H*.91)return;
    ctx.fillStyle=i===0?'rgba(201,146,42,0.1)':i%2===0?'rgba(0,0,0,0.04)':'transparent';rr(ctx,P,ry,W-P*2,rH,8);ctx.fill();
    if(i===0){ctx.fillStyle=G;ctx.fillRect(P,ry,4,rH);}
    ctx.font=`${W*.028}px serif`;ctx.textAlign='left';ctx.fillText(i<3?md[i]:'#'+(i+1),P+W*.014,ry+rH*.66);
    ctx.font=`${i===0?'700':'500'} ${W*.023}px 'DM Sans',sans-serif`;ctx.fillStyle=BK;ctx.textAlign='left';ctx.fillText(r.name,P+W*.088,ry+rH*.66);
    const bW=W*.20,bH=rH*.17,bX=W-P-bW-W*.09,bY2=ry+rH*.5-bH/2;
    ctx.fillStyle=CD;rr(ctx,bX,bY2,bW,bH,3);ctx.fill();ctx.fillStyle=i===0?G:BK;rr(ctx,bX,bY2,bW*(r.avg/10),bH,3);ctx.fill();
    ctx.font=`700 ${W*.021}px monospace`;ctx.fillStyle=i===0?G:BK;ctx.textAlign='right';ctx.fillText(r.avg.toFixed(2),W-P-W*.01,ry+rH*.66);
  });
}

function dTL(cv,f){
  const ctx=cv.getContext('2d'),W=cv.width,H=cv.height,is=f==='st';
  bg(ctx,W,H);tb(ctx,W,H,is);
  const P=W*.072,tY=is?H*.095:H*.11;
  ctx.font=`900 ${W*.072}px 'Playfair Display',serif`;ctx.fillStyle=BK;ctx.textAlign='left';ctx.fillText('TIER',P,tY);
  ctx.fillStyle=G;ctx.fillText('LIST',P+ctx.measureText('TIER ').width,tY);
  const tc={S:'#C9922A',A:'#2A7A2A',B:'#2A5A9A',C:'#7A7A2A',D:'#7A2A2A'};
  const gr={S:[],A:[],B:[],C:[],D:[]};
  RS.forEach(r=>{const t=TA[r.name]||'B';gr[t].push(r.name);});
  const sY=tY+H*.068,rH=is?H*.1:H*.105;
  ['S','A','B','C','D'].forEach((t,i)=>{
    const ry=sY+i*(rH+H*.01);if(ry+rH>H*.93)return;
    ctx.fillStyle=tc[t];rr(ctx,P,ry,W*.1,rH,8);ctx.fill();
    ctx.font=`900 ${W*.052}px 'Playfair Display',serif`;ctx.fillStyle='#fff';ctx.textAlign='center';ctx.fillText(t,P+W*.05,ry+rH*.67);
    ctx.fillStyle='rgba(0,0,0,0.05)';rr(ctx,P+W*.118,ry,W-P*2-W*.118,rH,8);ctx.fill();
    ctx.font=`500 ${W*.021}px 'DM Sans',sans-serif`;ctx.fillStyle=BK;ctx.textAlign='left';ctx.fillText(gr[t].length?gr[t].join('  ·  '):'—',P+W*.133,ry+rH*.62);
  });
}

loadSheet();
</script>
</body>
</html>
