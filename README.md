<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Broers & Burgers</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=DM+Sans:wght@300;400;500;600&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
:root{--black:#1A1A1A;--gold:#C9922A;--gold-l:#E8B84B;--cream:#FFF8EE;--cream-d:#D4C4A8;--grey:#6B6B6B;}
*{margin:0;padding:0;box-sizing:border-box;}
body{background:#0f0f0f;color:#FFF8EE;font-family:'DM Sans',sans-serif;min-height:100vh;}
.app{display:grid;grid-template-columns:380px 1fr;min-height:100vh;}
@media(max-width:700px){.app{grid-template-columns:1fr;}}
.sidebar{background:#1a1a1a;border-right:1px solid #2a2a2a;padding:24px 20px;overflow-y:auto;display:flex;flex-direction:column;gap:18px;}
.brand{display:flex;align-items:center;gap:12px;padding-bottom:18px;border-bottom:1px solid #2a2a2a;}
.brand-icon{width:40px;height:40px;background:#C9922A;border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:20px;}
.brand-name{font-family:'Playfair Display',serif;font-size:16px;font-weight:700;line-height:1.2;}
.brand-name span{color:#C9922A;}
.slabel{font-size:9px;font-weight:600;letter-spacing:2px;text-transform:uppercase;color:#6B6B6B;margin-bottom:8px;}
input,select{width:100%;background:#111;border:1px solid #2a2a2a;border-radius:8px;color:#FFF8EE;font-family:'DM Sans',sans-serif;font-size:13px;padding:9px 11px;outline:none;transition:border-color 0.2s;}
input:focus,select:focus{border-color:#C9922A;}
label{display:block;font-size:11px;color:#6B6B6B;margin-bottom:4px;}
.field{margin-bottom:12px;}
.row2{display:grid;grid-template-columns:1fr auto;gap:8px;}
.btn-gold{padding:9px 16px;background:#C9922A;border:none;border-radius:8px;color:#1A1A1A;font-family:'DM Sans',sans-serif;font-size:13px;font-weight:600;cursor:pointer;white-space:nowrap;}
.btn-gold:hover{background:#E8B84B;}
.status{padding:9px 12px;border-radius:8px;font-size:12px;display:none;align-items:center;gap:8px;}
.status.loading{display:flex;background:rgba(201,146,42,0.1);border:1px solid rgba(201,146,42,0.3);color:#C9922A;}
.status.error{display:flex;background:rgba(170,50,50,0.1);border:1px solid rgba(170,50,50,0.3);color:#e07070;}
.status.success{display:flex;background:rgba(42,122,42,0.1);border:1px solid rgba(42,122,42,0.3);color:#70c070;}
.rlist{display:flex;flex-direction:column;gap:5px;max-height:260px;overflow-y:auto;}
.ritem{padding:9px 12px;background:#111;border:1px solid #2a2a2a;border-radius:8px;cursor:pointer;transition:all 0.2s;display:flex;align-items:center;justify-content:space-between;}
.ritem:hover,.ritem.active{border-color:#C9922A;background:rgba(201,146,42,0.08);}
.rname{font-size:13px;font-weight:500;}
.rmeta{font-size:11px;color:#6B6B6B;margin-top:2px;}
.rscore{font-family:'DM Mono',monospace;font-size:14px;font-weight:700;color:#C9922A;}
.tabs{display:flex;background:#111;border-radius:10px;padding:4px;gap:4px;}
.tab{flex:1;padding:7px;border:none;border-radius:7px;background:transparent;color:#6B6B6B;font-family:'DM Sans',sans-serif;font-size:12px;font-weight:500;cursor:pointer;transition:all 0.2s;}
.tab.active{background:#C9922A;color:#1A1A1A;}
.fmts{display:flex;gap:6px;}
.fmt{padding:5px 12px;border-radius:6px;border:1px solid #2a2a2a;background:transparent;color:#6B6B6B;font-size:11px;cursor:pointer;font-family:'DM Sans',sans-serif;transition:all 0.2s;}
.fmt.active{border-color:#C9922A;color:#C9922A;}
.genbtn{width:100%;padding:13px;background:#C9922A;border:none;border-radius:10px;color:#1A1A1A;font-family:'DM Sans',sans-serif;font-size:14px;font-weight:600;cursor:pointer;transition:all 0.2s;}
.genbtn:hover{background:#E8B84B;}
.genbtn:disabled{opacity:0.4;cursor:not-allowed;}
.vchips{display:flex;gap:5px;flex-wrap:wrap;margin-top:5px;}
.vchip{padding:4px 9px;border-radius:20px;border:1px solid #2a2a2a;background:transparent;color:#6B6B6B;font-size:11px;cursor:pointer;font-family:'DM Sans',sans-serif;transition:all 0.2s;}
.vchip:hover,.vchip.active{border-color:#C9922A;color:#C9922A;background:rgba(201,146,42,0.1);}
.preview{background:#0d0d0d;display:flex;flex-direction:column;align-items:center;padding:32px 24px;gap:24px;overflow-y:auto;}
.ptop{width:100%;display:flex;align-items:center;justify-content:space-between;}
.plbl{font-size:10px;letter-spacing:2px;text-transform:uppercase;color:#333;}
.canvases{display:flex;gap:24px;flex-wrap:wrap;justify-content:center;align-items:flex-start;}
.cwrap{display:flex;flex-direction:column;align-items:center;gap:8px;}
.clbl{font-size:10px;letter-spacing:1.5px;text-transform:uppercase;color:#333;}
canvas{border-radius:12px;box-shadow:0 20px 60px rgba(0,0,0,0.7);}
.dlbtn{padding:6px 16px;border-radius:7px;border:1px solid #2a2a2a;background:transparent;color:#FFF8EE;font-size:12px;cursor:pointer;font-family:'DM Sans',sans-serif;transition:all 0.2s;}
.dlbtn:hover{border-color:#C9922A;color:#C9922A;}
.placeholder{display:flex;flex-direction:column;align-items:center;justify-content:center;gap:12px;height:400px;color:#333;text-align:center;}
.phi{font-size:48px;}
.pht{font-size:13px;color:#333;line-height:1.6;}
.tier-row{display:grid;grid-template-columns:36px 1fr;gap:8px;align-items:center;margin-bottom:8px;}
.tbadge{width:36px;height:36px;border-radius:7px;display:flex;align-items:center;justify-content:center;font-family:'Playfair Display',serif;font-size:16px;font-weight:900;color:#fff;}
</style>
</head>
<body>
<div class="app">
<div class="sidebar">
  <div class="brand">
    <div class="brand-icon">🍔</div>
    <div class="brand-name">Broers &<br><span>Burgers</span></div>
  </div>
  <div>
    <div class="slabel">Google Sheet</div>
    <div class="row2">
      <input type="text" id="sid" value="1HM7v6Vr0E7gAqUTyL826jfJxihknbJnM" placeholder="Sheet ID"/>
      <button class="btn-gold" onclick="loadSheet()">Load</button>
    </div>
  </div>
  <div class="status" id="status"></div>
  <div id="psec" style="display:none">
    <div class="slabel">Select Restaurant</div>
    <div class="rlist" id="rlist"></div>
  </div>
  <div id="tsec" style="display:none">
    <div class="slabel">Graphic Type</div>
    <div class="tabs">
      <button class="tab active" onclick="switchType('scorecard',this)">Scorecard</button>
      <button class="tab" onclick="switchType('leaderboard',this)">Leaderboard</button>
      <button class="tab" onclick="switchType('tier',this)">Tier List</button>
    </div>
  </div>
  <div id="sc-opts" style="display:none">
    <div class="field">
      <label>Verdict</label>
      <input type="text" id="verdict" placeholder="e.g. Solid patty, chips let it down" maxlength="65"/>
      <div class="vchips">
        <button class="vchip" onclick="setV(this,'A hidden gem 💎')">Hidden gem</button>
        <button class="vchip" onclick="setV(this,'Top tier, must visit! 🔥')">Top tier</button>
        <button class="vchip" onclick="setV(this,'Overrated 👎')">Overrated</button>
        <button class="vchip" onclick="setV(this,'We\'ll be back 👊')">We\'ll be back</button>
      </div>
    </div>
  </div>
  <div id="lb-opts" style="display:none">
    <div class="field">
      <label>Caption (optional)</label>
      <input type="text" id="lb-cap" placeholder="e.g. After 5 visits"/>
    </div>
  </div>
  <div id="tier-opts" style="display:none">
    <div class="slabel">Assign Tiers</div>
    <div id="tier-rows"></div>
  </div>
  <div id="fsec" style="display:none">
    <div class="slabel">Format</div>
    <div class="fmts">
      <button class="fmt active" onclick="setFmt('square',this)">Square</button>
      <button class="fmt" onclick="setFmt('story',this)">Story</button>
      <button class="fmt" onclick="setFmt('both',this)">Both</button>
    </div>
  </div>
  <button class="genbtn" id="genbtn" onclick="generate()" disabled>🍔 Generate</button>
</div>
<div class="preview" id="preview">
  <div class="ptop"><span class="plbl">Preview</span></div>
  <div class="placeholder" id="ph"><div class="phi">🍔</div><div class="pht">Load your sheet and<br>select a restaurant</div></div>
  <div class="canvases" id="canvases" style="display:none"></div>
</div>
</div>
<script>
const MEMBERS=['Sbu','Lebza M','KG','Francis','Thabiso','Jabu','Ofentse','Sosa','Vaughan','Lebza N','Vusie'];
const GOLD='#C9922A',BLACK='#1A1A1A',CREAM='#FFF8EE',CREAMD='#D4C4A8',GREY='#888';
let restaurants=[],selectedIdx=-1,currentType='scorecard',currentFmt='square',tierA={};

async function loadSheet(){
  let id=document.getElementById('sid').value.trim();
  const m=id.match(/\/d\/([a-zA-Z0-9_-]+)/);
  if(m)id=m[1];
  setStatus('loading','⏳ Loading...');
  try{
    const url=`https://docs.google.com/spreadsheets/d/${id}/gviz/tq?tqx=out:json&sheet=Scoreboard`;
    const r=await fetch(url);
    const t=await r.text();
    const json=JSON.parse(t.substring(t.indexOf('{'),t.lastIndexOf('}')+1));
    const rows=json.table.rows;
    restaurants=[];
    rows.forEach((row,i)=>{
      if(i<6)return;
      const c=row.c;
      if(!c||!c[1]||!c[1].v)return;
      const name=c[1].v;
      if(!name||name==='—')return;
      const scores=[];
      for(let s=2;s<=12;s++){
        const v=c[s]&&c[s].v!==null?parseFloat(c[s].v):null;
        scores.push({member:MEMBERS[s-2],score:isNaN(v)?null:v});
      }
      const valid=scores.filter(s=>s.score!==null);
      const avg=valid.length?valid.reduce((a,b)=>a+b.score,0)/valid.length:null;
      const price=c[13]&&c[13].v?parseFloat(c[13].v):null;
      const dateStr=c[17]?(c[17].f||c[17].v||''):'';
      restaurants.push({name,scores,avg,price,dateStr});
    });
    if(!restaurants.length){setStatus('error','❌ No data found. Check sheet is public.');return;}
    setStatus('success',`✓ Loaded ${restaurants.length} restaurant${restaurants.length>1?'s':''}!`);
    renderList();buildTierRows();
    document.getElementById('psec').style.display='block';
    document.getElementById('tsec').style.display='block';
    document.getElementById('fsec').style.display='block';
  }catch(e){setStatus('error','❌ Could not load. Make sure sheet is public.');console.error(e);}
}

function setStatus(type,msg){const el=document.getElementById('status');el.className='status '+type;el.innerHTML=msg;}

function renderList(){
  const list=document.getElementById('rlist');list.innerHTML='';
  restaurants.forEach((r,i)=>{
    const d=document.createElement('div');
    d.className='ritem'+(i===selectedIdx?' active':'');
    d.onclick=()=>selectR(i);
    d.innerHTML=`<div><div class="rname">${r.name}</div><div class="rmeta">${r.dateStr||'No date'}${r.price?' · R'+r.price:''}</div></div><div class="rscore">${r.avg!==null?r.avg.toFixed(2):'—'}</div>`;
    list.appendChild(d);
  });
}

function selectR(i){selectedIdx=i;renderList();document.getElementById('sc-opts').style.display=currentType==='scorecard'?'block':'none';document.getElementById('genbtn').disabled=false;}

function switchType(t,btn){
  currentType=t;
  document.querySelectorAll('.tab').forEach(b=>b.classList.remove('active'));btn.classList.add('active');
  document.getElementById('sc-opts').style.display=t==='scorecard'?'block':'none';
  document.getElementById('lb-opts').style.display=t==='leaderboard'?'block':'none';
  document.getElementById('tier-opts').style.display=t==='tier'?'block':'none';
  if(t!=='scorecard')document.getElementById('genbtn').disabled=false;
}

function setFmt(f,btn){currentFmt=f;document.querySelectorAll('.fmt').forEach(b=>b.classList.remove('active'));btn.classList.add('active');}
function setV(btn,t){document.getElementById('verdict').value=t;document.querySelectorAll('.vchip').forEach(b=>b.classList.remove('active'));btn.classList.add('active');}

function buildTierRows(){
  const c=document.getElementById('tier-rows');c.innerHTML='';
  const tc={S:'#C9922A',A:'#2A7A2A',B:'#2A5A9A',C:'#7A7A2A',D:'#7A2A2A'};
  restaurants.forEach(r=>{
    if(!tierA[r.name])tierA[r.name]='B';
    const row=document.createElement('div');row.className='tier-row';
    row.innerHTML=`<div class="tbadge" id="tb_${r.name.replace(/\W/g,'_')}" style="background:${tc[tierA[r.name]]}">${tierA[r.name]}</div>
    <div><div style="font-size:12px;margin-bottom:3px">${r.name}</div>
    <select onchange="setTier('${r.name}',this.value)" style="padding:4px 8px;font-size:11px">
    ${['S','A','B','C','D'].map(t=>`<option ${tierA[r.name]===t?'selected':''}>${t}</option>`).join('')}
    </select></div>`;
    c.appendChild(row);
  });
}

function setTier(name,t){
  tierA[name]=t;
  const tc={S:'#C9922A',A:'#2A7A2A',B:'#2A5A9A',C:'#7A7A2A',D:'#7A2A2A'};
  const b=document.getElementById('tb_'+name.replace(/\W/g,'_'));
  if(b){b.style.background=tc[t];b.textContent=t;}
}

function generate(){
  document.getElementById('ph').style.display='none';
  const con=document.getElementById('canvases');con.style.display='flex';con.innerHTML='';
  const fmts=currentFmt==='both'?['square','story']:[currentFmt];
  fmts.forEach(fmt=>{
    const wrap=document.createElement('div');wrap.className='cwrap';
    const lbl=document.createElement('div');lbl.className='clbl';lbl.textContent=fmt==='square'?'Square 1080×1080':'Story 1080×1920';
    const canvas=document.createElement('canvas');
    const W=1080,H=fmt==='square'?1080:1920;
    canvas.width=W;canvas.height=H;
    const sc=fmt==='square'?0.38:0.23;
    canvas.style.width=(W*sc)+'px';canvas.style.height=(H*sc)+'px';
    if(currentType==='scorecard')drawSC(canvas,fmt);
    else if(currentType==='leaderboard')drawLB(canvas,fmt);
    else drawTL(canvas,fmt);
    const dl=document.createElement('button');dl.className='dlbtn';dl.textContent='↓ Download';
    dl.onclick=()=>{const a=document.createElement('a');a.download=`BB_${currentType}_${fmt}.png`;a.href=canvas.toDataURL('image/png');a.click();};
    wrap.appendChild(lbl);wrap.appendChild(canvas);wrap.appendChild(dl);con.appendChild(wrap);
  });
}

function drawBg(ctx,W,H){ctx.fillStyle='#FAF3E8';ctx.fillRect(0,0,W,H);}
function rr(ctx,x,y,w,h,r){ctx.beginPath();ctx.moveTo(x+r,y);ctx.lineTo(x+w-r,y);ctx.quadraticCurveTo(x+w,y,x+w,y+r);ctx.lineTo(x+w,y+h-r);ctx.quadraticCurveTo(x+w,y+h,x+w-r,y+h);ctx.lineTo(x+r,y+h);ctx.quadraticCurveTo(x,y+h,x,y+h-r);ctx.lineTo(x,y+r);ctx.quadraticCurveTo(x,y,x+r,y);ctx.closePath();}
function wt(ctx,text,x,y,mw,lh){const words=text.split(' ');let line='',cy=y;for(let w of words){const t=line+(line?' ':'')+w;if(ctx.measureText(t).width>mw&&line){ctx.fillText(line,x,cy);line=w;cy+=lh*0.58;}else line=t;}ctx.fillText(line,x,cy);return cy;}
function topBar(ctx,W,H,is){ctx.fillStyle=BLACK;ctx.fillRect(0,0,W,is?H*0.052:H*0.065);ctx.font=`600 ${W*0.021}px 'DM Sans',sans-serif`;ctx.fillStyle=GOLD;ctx.textAlign='left';ctx.fillText('BROERS & BURGERS',W*0.07,is?H*0.036:H*0.044);ctx.font=`400 ${W*0.013}px 'DM Sans',sans-serif`;ctx.fillStyle='#555';ctx.textAlign='right';ctx.fillText('#BroersAndBurgers',W-W*0.07,is?H*0.036:H*0.044);}

function drawSC(canvas,fmt){
  const ctx=canvas.getContext('2d'),W=canvas.width,H=canvas.height,is=fmt==='story';
  const r=restaurants[selectedIdx];if(!r)return;
  drawBg(ctx,W,H);topBar(ctx,W,H,is);
  const PAD=W*0.072,verdict=document.getElementById('verdict').value||'';
  const tY=is?H*0.072:H*0.085;
  ctx.fillStyle=GOLD;rr(ctx,PAD,tY,W*0.30,H*0.030,4);ctx.fill();
  ctx.font=`600 ${W*0.015}px 'DM Sans',sans-serif`;ctx.fillStyle=BLACK;ctx.textAlign='left';
  ctx.fillText('VISIT SCORECARD',PAD+W*0.012,tY+H*0.021);
  ctx.font=`900 ${is?W*0.075:W*0.067}px 'Playfair Display',serif`;ctx.fillStyle=BLACK;
  const nY=tY+H*0.072;const enY=wt(ctx,r.name.toUpperCase(),PAD,nY,W-PAD*2,is?H*0.088:H*0.077);
  const mY=enY+H*0.032;ctx.font=`400 ${W*0.019}px 'DM Sans',sans-serif`;ctx.fillStyle=GREY;ctx.textAlign='left';
  let meta='';if(r.dateStr)meta+='📅 '+r.dateStr+'   ';if(r.price)meta+='💰 R'+r.price;
  if(meta)ctx.fillText(meta,PAD,mY);
  const dY=mY+H*0.028;ctx.strokeStyle=CREAMD;ctx.lineWidth=1.5;ctx.beginPath();ctx.moveTo(PAD,dY);ctx.lineTo(W-PAD,dY);ctx.stroke();
  const bW=W*0.27,bH=H*(is?0.09:0.10),bX=W-PAD-bW,bY=dY+H*0.022;
  ctx.fillStyle=BLACK;rr(ctx,bX,bY,bW,bH,14);ctx.fill();
  ctx.font=`900 ${W*0.056}px 'Playfair Display',serif`;ctx.fillStyle=GOLD;ctx.textAlign='center';
  ctx.fillText(r.avg!==null?r.avg.toFixed(2):'—',bX+bW/2,bY+bH*0.64);
  ctx.font=`500 ${W*0.014}px 'DM Sans',sans-serif`;ctx.fillStyle=CREAMD;ctx.fillText('GROUP AVG / 10',bX+bW/2,bY+bH*0.87);
  const tY2=bY+H*0.012,lW=W-PAD*2-bW-W*0.035,fn=is?5:4,rH1=bH/fn;
  r.scores.slice(0,fn).forEach((s,i)=>{
    const ry=tY2+i*rH1;
    ctx.fillStyle=i%2===0?'rgba(0,0,0,0.05)':'transparent';rr(ctx,PAD,ry,lW,rH1-2,4);ctx.fill();
    ctx.font=`500 ${W*0.017}px 'DM Sans',sans-serif`;ctx.fillStyle=BLACK;ctx.textAlign='left';ctx.fillText(s.member,PAD+W*0.012,ry+rH1*0.65);
    ctx.font=`700 ${W*0.019}px 'DM Mono',monospace`;ctx.fillStyle=s.score!==null?(s.score>=7?'#2A7A2A':s.score>=5?GOLD:'#AA3333'):GREY;ctx.textAlign='right';ctx.fillText(s.score!==null?s.score.toFixed(1):'—',PAD+lW-W*0.01,ry+rH1*0.65);
  });
  const rem=r.scores.slice(fn),gY=bY+bH+H*0.022,gCW=(W-PAD*2-W*0.025)/2,gRH=H*(is?0.044:0.048);
  rem.forEach((s,i)=>{
    const col=i%2,row=Math.floor(i/2),rx=PAD+col*(gCW+W*0.025),ry=gY+row*gRH;
    ctx.fillStyle='rgba(0,0,0,0.05)';rr(ctx,rx,ry,gCW,gRH-3,4);ctx.fill();
    ctx.font=`500 ${W*0.017}px 'DM Sans',sans-serif`;ctx.fillStyle=BLACK;ctx.textAlign='left';ctx.fillText(s.member,rx+W*0.012,ry+gRH*0.65);
    ctx.font=`700 ${W*0.019}px 'DM Mono',monospace`;ctx.fillStyle=s.score!==null?(s.score>=7?'#2A7A2A':s.score>=5?GOLD:'#AA3333'):GREY;ctx.textAlign='right';ctx.fillText(s.score!==null?s.score.toFixed(1):'—',rx+gCW-W*0.012,ry+gRH*0.65);
  });
  const vY=H-(is?H*0.095:H*0.105);
  ctx.fillStyle=GOLD;ctx.fillRect(0,vY,W,H*0.002);ctx.fillStyle=BLACK;ctx.fillRect(0,vY+H*0.002,W,H-vY);
  ctx.font=`400 ${W*0.016}px 'DM Sans',sans-serif`;ctx.fillStyle=CREAMD;ctx.textAlign='left';ctx.fillText('VERDICT',PAD,vY+H*0.038);
  if(verdict){ctx.font=`italic ${W*0.021}px 'Playfair Display',serif`;ctx.fillStyle=CREAM;ctx.fillText('"'+verdict+'"',PAD+ctx.measureText('VERDICT  ').width,vY+H*0.038);}
  if(r.price&&r.avg){const spr=(r.avg/r.price*100).toFixed(2);ctx.font=`500 ${W*0.015}px 'DM Mono',monospace`;ctx.fillStyle=GOLD;ctx.textAlign='right';ctx.fillText('Score/R100: '+spr,W-PAD,vY+H*0.038);}
}

function drawLB(canvas,fmt){
  const ctx=canvas.getContext('2d'),W=canvas.width,H=canvas.height,is=fmt==='story';
  drawBg(ctx,W,H);topBar(ctx,W,H,is);
  const PAD=W*0.072,cap=document.getElementById('lb-cap').value||'';
  const sorted=[...restaurants].filter(r=>r.avg!==null).sort((a,b)=>b.avg-a.avg);
  const medals=['🥇','🥈','🥉'];
  const tY=is?H*0.095:H*0.11;
  ctx.font=`900 ${W*0.068}px 'Playfair Display',serif`;ctx.fillStyle=BLACK;ctx.textAlign='left';ctx.fillText('THE',PAD,tY);
  ctx.fillStyle=GOLD;ctx.fillText('RANKINGS',PAD,tY+W*0.074);
  if(cap){ctx.font=`400 ${W*0.019}px 'DM Sans',sans-serif`;ctx.fillStyle=GREY;ctx.fillText(cap,PAD,tY+W*0.074+H*0.038);}
  const dY=tY+W*0.074+H*(cap?0.062:0.042);
  ctx.strokeStyle=CREAMD;ctx.lineWidth=1.5;ctx.beginPath();ctx.moveTo(PAD,dY);ctx.lineTo(W-PAD,dY);ctx.stroke();
  const rH=is?H*0.067:H*0.071,sY=dY+H*0.018;
  sorted.forEach((r,i)=>{
    const ry=sY+i*(rH+H*0.007);if(ry+rH>H*0.91)return;
    ctx.fillStyle=i===0?'rgba(201,146,42,0.1)':i%2===0?'rgba(0,0,0,0.04)':'transparent';rr(ctx,PAD,ry,W-PAD*2,rH,8);ctx.fill();
    if(i===0){ctx.fillStyle=GOLD;ctx.fillRect(PAD,ry,4,rH);}
    ctx.font=`${W*0.028}px serif`;ctx.textAlign='left';ctx.fillText(i<3?medals[i]:`#${i+1}`,PAD+W*0.014,ry+rH*0.66);
    ctx.font=`${i===0?'700':'500'} ${W*0.023}px 'DM Sans',sans-serif`;ctx.fillStyle=BLACK;ctx.textAlign='left';ctx.fillText(r.name,PAD+W*0.088,ry+rH*0.66);
    const bW=W*0.20,bH2=rH*0.17,bX=W-PAD-bW-W*0.09,bY2=ry+rH*0.5-bH2/2;
    ctx.fillStyle=CREAMD;rr(ctx,bX,bY2,bW,bH2,3);ctx.fill();ctx.fillStyle=i===0?GOLD:BLACK;rr(ctx,bX,bY2,bW*(r.avg/10),bH2,3);ctx.fill();
    ctx.font=`700 ${W*0.021}px 'DM Mono',monospace`;ctx.fillStyle=i===0?GOLD:BLACK;ctx.textAlign='right';ctx.fillText(r.avg.toFixed(2),W-PAD-W*0.01,ry+rH*0.66);
  });
}

function drawTL(canvas,fmt){
  const ctx=canvas.getContext('2d'),W=canvas.width,H=canvas.height,is=fmt==='story';
  drawBg(ctx,W,H);topBar(ctx,W,H,is);
  const PAD=W*0.072,tY=is?H*0.095:H*0.11;
  ctx.font=`900 ${W*0.072}px 'Playfair Display',serif`;ctx.fillStyle=BLACK;ctx.textAlign='left';ctx.fillText('TIER',PAD,tY);
  ctx.fillStyle=GOLD;ctx.fillText('LIST',PAD+ctx.measureText('TIER ').width,tY);
  const tc={S:'#C9922A',A:'#2A7A2A',B:'#2A5A9A',C:'#7A7A2A',D:'#7A2A2A'};
  const tiers=['S','A','B','C','D'];
  const grouped={S:[],A:[],B:[],C:[],D:[]};
  restaurants.forEach(r=>{const t=tierA[r.name]||'B';grouped[t].push(r.name);});
  const sY=tY+H*0.068,rH=is?H*0.1:H*0.105;
  tiers.forEach((t,i)=>{
    const ry=sY+i*(rH+H*0.01);if(ry+rH>H*0.93)return;
    ctx.fillStyle=tc[t];rr(ctx,PAD,ry,W*0.1,rH,8);ctx.fill();
    ctx.font=`900 ${W*0.052}px 'Playfair Display',serif`;ctx.fillStyle='#fff';ctx.textAlign='center';ctx.fillText(t,PAD+W*0.05,ry+rH*0.67);
    ctx.fillStyle='rgba(0,0,0,0.05)';rr(ctx,PAD+W*0.118,ry,W-PAD*2-W*0.118,rH,8);ctx.fill();
    ctx.font=`500 ${W*0.021}px 'DM Sans',sans-serif`;ctx.fillStyle=BLACK;ctx.textAlign='left';
    ctx.fillText(grouped[t].length?grouped[t].join('  ·  '):'—',PAD+W*0.133,ry+rH*0.62);
  });
}

window.onload=()=>loadSheet();
</script>
</body>
</html>
