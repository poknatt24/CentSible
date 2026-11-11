# CentSible
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>CentSible – Student Budget Dashboard</title>
<style>
:root{box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px);
--bg:#eef2f8;--card:#fff;--ink:#111827;--mut:#6b7280;--line:#d5dce8;--pri:#1e5aa8;--ok:#15803d;--warn:#f59e0b;--warnbg:#fef3c7;--warnink:#92400e;--bad:#c2410c;--offbg:#fdecec;--offink:#991b1b;--onbg:#e7f6ec;--onink:#166534}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#0f172a;--card:#1e293b;--ink:#f1f5f9;--mut:#94a3b8;--line:#334155;--pri:#3b82f6;--ok:#4ade80;--warnbg:#422006;--warnink:#fcd34d;--bad:#fb923c;--offbg:#450a0a;--offink:#fca5a5;--onbg:#052e16;--onink:#86efac}}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
,:before,*:after{box-sizing:inherit}
body{margin:0;background:var(--bg);color:var(--ink);font-family:system-ui,-apple-system,"Segoe UI",Roboto,sans-serif}
.app{max-width:440px;margin:0 auto;min-height:100vh;padding-bottom:84px}
header{background:var(--pri);color:#fff;padding:22px 20px 70px;border-radius:0 0 28px 28px}
header h1{margin:0;font-size:26px}header p{margin:4px 0 0;font-size:13px;opacity:.85}
.card{background:var(--card);border:1px solid var(--line);border-radius:18px;padding:16px;margin:0 16px 14px}
.hero{margin-top:-52px}
.row{display:flex;justify-content:space-between;align-items:center;padding:6px 0;font-size:14px;color:var(--mut)}
.row b{font-size:20px;color:var(--ink)}
.bar{height:12px;border-radius:6px;background:var(--line);overflow:hidden;margin:6px 0 10px}
.bar i{display:block;height:100%;background:var(--warn);border-radius:6px;transition:width .3s}
.alert{display:none;background:var(--warnbg);border:1px solid var(--warn);color:var(--warnink);border-radius:12px;padding:8px;text-align:center;font-weight:700;font-size:13px}
.btn{border:0;border-radius:14px;padding:14px;font-size:16px;font-weight:700;cursor:pointer;background:var(--pri);color:#fff;width:100%}
.btn.sec{background:transparent;color:var(--pri);border:1px solid var(--pri);font-size:13px;padding:8px}
.btn.sm{width:auto;padding:8px 12px;font-size:13px}
.big{margin:0 16px 16px;width:calc(100% - 32px)}
h2{font-size:15px;margin:0 0 10px}
.cat{display:flex;align-items:center;gap:10px;padding:9px 0;border-top:1px solid var(--line);font-size:14px}
.cat:first-of-type{border-top:0}
.dot{width:14px;height:14px;border-radius:50%}
.cat span:nth-child(2){flex:1}
.tabs{display:flex;gap:6px;margin-bottom:10px}
.tabs button{flex:1;padding:8px;border-radius:10px;border:1px solid var(--line);background:transparent;color:var(--ink);cursor:pointer;font-size:13px}
.tabs button.on{background:var(--pri);color:#fff;border-color:var(--pri)}
.ent{display:flex;justify-content:space-between;font-size:13px;padding:6px 0;border-top:1px solid var(--line)}
.ent small{color:var(--mut);display:block}
.pend{color:var(--offink);font-size:11px}
.bch{display:flex;align-items:center;gap:8px;font-size:12px;margin:6px 0}
.bch .t{flex:1;height:10px;background:var(--line);border-radius:5px;overflow:hidden}.bch .t i{display:block;height:100%}
.bch .l{width:96px}.bch .v{width:56px;text-align:right}
#status{position:fixed;left:0;right:0;bottom:0;padding:14px 16px calc(14px + env(safe-area-inset-bottom,0px));display:flex;justify-content:center;align-items:center;gap:10px;font-size:13px;font-weight:700;cursor:pointer;border-top:1px solid var(--line);z-index:5}
#status.off{background:var(--offbg);color:var(--offink)}#status.on{background:var(--onbg);color:var(--onink)}
#status .d{width:12px;height:12px;border-radius:50%;background:currentColor}
.modal{position:fixed;inset:0;background:rgba(0,0,0,.5);display:none;align-items:flex-end;justify-content:center;z-index:10}
.modal.show{display:flex}
.sheet{background:var(--card);width:100%;max-width:440px;border-radius:22px 22px 0 0;padding:20px;padding-bottom:calc(20px + env(safe-area-inset-bottom,0px))}
.sheet h3{margin:0 0 12px}
label{display:block;font-size:12px;color:var(--mut);margin:10px 0 4px}
input,select{width:100%;padding:12px;border-radius:10px;border:1px solid var(--line);background:var(--bg);color:var(--ink);font-size:16px}
.err{color:var(--bad);font-size:12px;min-height:16px;margin-top:6px}
.btns{display:flex;gap:8px;margin-top:12px}
#toast{position:fixed;top:calc(12px + env(safe-area-inset-top,0px));left:50%;transform:translateX(-50%);background:#1f2937;color:#fff;padding:10px 16px;border-radius:12px;font-size:13px;z-index:20;display:none;max-width:90%;text-align:center}
.foot{text-align:center;font-size:11px;color:var(--mut);margin:8px 16px}
</style>
</head>
<body>
<div class="app">
<header><h1>CentSible</h1><p>LTC Student Allowance Tracker</p></header>

<div class="card hero">
  <div class="row"><span id="cycleLbl">Weekly Allowance</span><b id="allow">₱0.00</b></div>
  <div class="row"><span>Total Spent</span><b id="spent" style="color:var(--bad)">₱0.00</b></div>
  <div class="row"><span>Remaining Balance</span><b id="remain" style="color:var(--ok)">₱0.00</b></div>
  <div class="row"><span>Budget Used</span><b id="pct" style="font-size:15px">0%</b></div>
  <div class="bar"><i id="fill" style="width:0"></i></div>
  <div class="alert" id="alertBox">⚠️ BUDGET ALERT – 75% or more used</div>
  <div style="text-align:center;margin-top:10px"><button class="btn sec sm" id="setBtn">Set Allowance / Budget</button></div>
</div>

<button class="btn big" id="addBtn">+ ADD NEW EXPENSE</button>

<div class="card"><h2>Category Quick View</h2><div id="cats"></div></div>
<div class="card"><h2>Spending Breakdown</h2><div id="chart"></div></div>

<div class="card"><h2>Expenses Summary</h2>
  <div class="tabs" id="tabs"><button data-r="day" class="on">Day</button><button data-r="week">Week</button><button data-r="month">Month</button></div>
  <div class="row" style="padding-top:0"><span>Total in range</span><b id="rangeTotal" style="font-size:16px">₱0.00</b></div>
  <div id="list"></div>
</div>

<div class="foot">Prototype only – data is kept in this browser. <a href="#" id="reset" style="color:var(--pri)">Reset demo data</a></div>
</div>

<div id="status" class="on"><span class="d"></span><span id="stTxt"></span></div>
<div id="toast"></div>

<div class="modal" id="mExp"><div class="sheet">
  <h3>Add New Expense</h3>
  <label>Amount (₱)</label><input id="eAmt" type="number" inputmode="decimal" min="0" step="0.01" placeholder="0.00">
  <label>Category</label><select id="eCat"></select>
  <input id="eCustom" placeholder="New category name" style="display:none;margin-top:8px">
  <label>Note (optional)</label><input id="eNote" placeholder="e.g. jeepney to school">
  <div class="err" id="eErr"></div>
  <div class="btns"><button class="btn sec" id="eCancel" style="flex:1">Cancel</button><button class="btn" id="eSave" style="flex:2">Save Expense</button></div>
</div></div>

<div class="modal" id="mSet"><div class="sheet">
  <h3>Set Allowance</h3>
  <label>Allowance amount (₱)</label><input id="sAmt" type="number" inputmode="decimal" min="0" step="0.01">
  <label>Budget cycle</label><select id="sCyc"><option value="week">Weekly (default)</option><option value="month">Monthly</option></select>
  <div class="err" id="sErr"></div>
  <div class="btns"><button class="btn sec" id="sCancel" style="flex:1">Cancel</button><button class="btn" id="sSave" style="flex:2">Save</button></div>
</div></div>

<div class="modal" id="mWarn"><div class="sheet">
  <h3 id="wTitle">Warning</h3><p id="wMsg" style="color:var(--mut)"></p>
  <div class="btns"><button class="btn" id="wOk">OK</button></div>
</div></div>

<script>
(function(){
var COL={"Transportation":"#3b82f6","Meals":"#f97316","School Requirements":"#8b5cf6","Mobile Load/Data":"#10b981"};
var EXTRA=["#ec4899","#eab308","#06b6d4","#84cc16"];
var K="centsible_v1";
var $=function(i){return document.getElementById(i)};
var peso=function(n){return "₱"+Number(n).toLocaleString("en-PH",{minimumFractionDigits:2,maximumFractionDigits:2})};
function todayISO(){var d=new Date();return d.getFullYear()+"-"+String(d.getMonth()+1).padStart(2,"0")+"-"+String(d.getDate()).padStart(2,"0")}
function seed(){var t=todayISO();return {allowance:800,cycle:"week",online:false,lastSync:null,cats:Object.keys(COL),range:"day",alerted:true,
 exp:[{id:1,amt:120,cat:"Transportation",date:t,note:"Fare",synced:true},{id:2,amt:250,cat:"Meals",date:t,note:"",synced:true},{id:3,amt:200,cat:"School Requirements",date:t,note:"",synced:false},{id:4,amt:50,cat:"Mobile Load/Data",date:t,note:"",synced:false}]}}
var S;
try{S=JSON.parse(localStorage.getItem(K))}catch(e){S=null}
if(!S||!S.exp){S=seed()}
function save(){try{localStorage.setItem(K,JSON.stringify(S))}catch(e){}}
function toast(m){var t=$("toast");t.textContent=m;t.style.display="block";clearTimeout(toast.h);toast.h=setTimeout(function(){t.style.display="none"},2600)}
function color(c){if(COL[c])return COL[c];var i=S.cats.indexOf(c);return EXTRA[(i-4)%EXTRA.length]||"#64748b"}
function parse(d){var p=d.split("-");return new Date(+p[0],+p[1]-1,+p[2])}
function inRange(e,r){var n=new Date(),d=parse(e.date);
 if(r==="day")return e.date===todayISO();
 if(r==="month")return d.getMonth()===n.getMonth()&&d.getFullYear()===n.getFullYear();
 var s=new Date(n.getFullYear(),n.getMonth(),n.getDate());var dow=(s.getDay()+6)%7;s.setDate(s.getDate()-dow);
 var e2=new Date(s);e2.setDate(s.getDate()+7);return d>=s&&d<e2}
function periodExp(){return S.exp.filter(function(e){return inRange(e,S.cycle)})}
function sum(a){return a.reduce(function(t,e){return t+e.amt},0)}
function render(){
 var pe=periodExp(),spent=sum(pe),rem=S.allowance-spent,pct=S.allowance>0?spent/S.allowance*100:0;
 $("cycleLbl").textContent=(S.cycle==="week"?"Weekly":"Monthly")+" Allowance";
 $("allow").textContent=peso(S.allowance);$("spent").textContent=peso(spent);
 $("remain").textContent=peso(rem);$("remain").style.color=rem<0?"var(--bad)":"var(--ok)";
 $("pct").textContent=pct.toFixed(1)+"%";$("fill").style.width=Math.min(pct,100)+"%";
 $("alertBox").style.display=pct>=75?"block":"none";
 var by={};pe.forEach(function(e){by[e.cat]=(by[e.cat]||0)+e.amt});
 $("cats").innerHTML=S.cats.map(function(c){return '<div class="cat"><span class="dot" style="background:'+color(c)+'"></span><span>'+esc(c)+'</span><b>'+peso(by[c]||0)+'</b></div>'}).join("");
 var mx=Math.max.apply(null,[1].concat(S.cats.map(function(c){return by[c]||0})));
 $("chart").innerHTML=S.cats.map(function(c){var v=by[c]||0;return '<div class="bch"><span class="l">'+esc(c)+'</span><span class="t"><i style="width:'+(v/mx*100)+'%;background:'+color(c)+'"></i></span><span class="v">'+peso(v).replace(".00","")+'</span></div>'}).join("");
 var re=S.exp.filter(function(e){return inRange(e,S.range)}).sort(function(a,b){return b.id-a.id});
 $("rangeTotal").textContent=peso(sum(re));
 $("list").innerHTML=re.length?re.map(function(e){return '<div class="ent"><div>'+esc(e.cat)+'<small>'+e.date+(e.note?' · '+esc(e.note):'')+(e.synced?'':' <span class="pend">(pending sync)</span>')+'</small></div><b>'+peso(e.amt)+'</b></div>'}).join(""):'<div style="color:var(--mut);font-size:13px;padding:8px 0">No expenses in this range.</div>';
 Array.prototype.forEach.call($("tabs").children,function(b){b.classList.toggle("on",b.dataset.r===S.range)});
 var pend=S.exp.filter(function(e){return !e.synced}).length,st=$("status");
 st.className=S.online?"on":"off";
 $("stTxt").textContent=S.online?("Online • All synced"+(S.lastSync?" • Last sync "+S.lastSync:"")+(pend?" • Pending: "+pend:"")):("Offline Mode • Pending to Sync: "+pend+" "+(pend===1?"Entry":"Entries")+" (tap to go online)");
 if(S.online)$("stTxt").textContent=pend?"Syncing "+pend+" entries…":"Online • All synced"+(S.lastSync?" • Last sync "+S.lastSync:"");
 save()}
function esc(s){return String(s).replace(/[&<>"]/g,function(c){return {"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;"}[c]})}
function sync(){var p=S.exp.filter(function(e){return !e.synced});
 if(!S.online||!p.length)return;
 setTimeout(function(){if(!S.online)return;S.exp.forEach(function(e){e.synced=true});S.lastSync=new Date().toLocaleTimeString([],{hour:"2-digit",minute:"2-digit"});render();toast("Synced "+p.length+" entries to the server")},1200)}
function show(id){$(id).classList.add("show")}
function hide(id){$(id).classList.remove("show")}
function warn(t,m){$("wTitle").textContent=t;$("wMsg").textContent=m;show("mWarn")}
function fillCats(){$("eCat").innerHTML=S.cats.map(function(c){return '<option>'+esc(c)+'</option>'}).join("")+'<option value="__new">+ Add custom category…</option>'}

$("status").onclick=function(){S.online=!S.online;render();toast(S.online?"Internet connected":"You are offline");sync()};
$("addBtn").onclick=function(){fillCats();$("eAmt").value="";$("eNote").value="";$("eCustom").value="";$("eCustom").style.display="none";$("eErr").textContent="";show("mExp");setTimeout(function(){$("eAmt").focus()},50)};
$("eCat").onchange=function(){$("eCustom").style.display=this.value==="__new"?"block":"none"};
$("eCancel").onclick=function(){hide("mExp")};
$("eSave").onclick=function(){
 var amt=parseFloat($("eAmt").value),cat=$("eCat").value;
 if(!(amt>0)){$("eErr").textContent="Amount must be greater than 0.";return}
 if(cat==="__new"){cat=$("eCustom").value.trim();if(!cat){$("eErr").textContent="Enter a name for the new category.";return}
  if(S.cats.indexOf(cat)<0)S.cats.push(cat)}
 var remBefore=S.allowance-sum(periodExp());
 var go=function(){
  var before=S.allowance>0?sum(periodExp())/S.allowance*100:0;
  S.exp.push({id:Date.now(),amt:amt,cat:cat,date:todayISO(),note:$("eNote").value.trim(),synced:S.online});
  var after=S.allowance>0?sum(periodExp())/S.allowance*100:0;
  hide("mExp");render();
  toast(S.online?"Expense saved & synced":"Saved offline – will sync when connected");
  if(before<75&&after>=75)setTimeout(function(){warn("⚠️ Budget Alert","You have used "+after.toFixed(1)+"% of your allowance. Spend carefully!")},400)};
 if(amt>remBefore){if(confirm("This amount ("+peso(amt)+") is more than your remaining allowance ("+peso(Math.max(remBefore,0))+"). Save anyway?"))go()}else go()};
$("setBtn").onclick=function(){$("sAmt").value=S.allowance;$("sCyc").value=S.cycle;$("sErr").textContent="";show("mSet")};
$("sCancel").onclick=function(){hide("mSet")};
$("sSave").onclick=function(){var a=parseFloat($("sAmt").value);if(!(a>0)){$("sErr").textContent="Allowance must be greater than 0.";return}
 S.allowance=a;S.cycle=$("sCyc").value;hide("mSet");render();toast("Allowance updated")};
$("wOk").onclick=function(){hide("mWarn")};
$("tabs").onclick=function(e){if(e.target.dataset.r){S.range=e.target.dataset.r;render()}};
$("reset").onclick=function(e){e.preventDefault();if(confirm("Reset all demo data?")){S=seed();render();toast("Demo data reset")}};
render();
})();
</script>
</body>
</html>
