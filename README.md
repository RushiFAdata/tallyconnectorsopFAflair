<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Tally Onboarding Tracker</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Archivo:wght@500;600;700&family=IBM+Plex+Sans:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{
  color-scheme: light;
  --navy:#1B2A4A; --accent:#E8773A;
  --ink:#1A2230; --muted:#6B7585; --faint:#8A93A3; --line:#DDE2EA;
  --surface:#FFFFFF; --canvas:#F3F5F9; --raised:#E9EDF4;
  --good:#2E7D5B; --goodbg:#E7F2EC; --warn:#B07D2A; --warnbg:#FBF2E0;
  --crit:#B3453A;
  --display:'Archivo',system-ui,sans-serif;
  --body:'IBM Plex Sans',system-ui,-apple-system,sans-serif;
  --r:10px;
}
@media (prefers-color-scheme: dark){
  :root:not([data-theme="light"]){
    color-scheme: dark;
    --navy:#C8D4EA; --accent:#F08A4F;
    --ink:#E9EDF4; --muted:#95A1B5; --faint:#7C879B; --line:#2C3545;
    --surface:#161C27; --canvas:#0E131C; --raised:#1E2635;
    --good:#5BBF8F; --goodbg:#17301F; --warn:#D7A54A; --warnbg:#2E2514;
    --crit:#E0695C;
  }
}
:root[data-theme="dark"]{
  color-scheme: dark;
  --navy:#C8D4EA; --accent:#F08A4F;
  --ink:#E9EDF4; --muted:#95A1B5; --faint:#7C879B; --line:#2C3545;
  --surface:#161C27; --canvas:#0E131C; --raised:#1E2635;
  --good:#5BBF8F; --goodbg:#17301F; --warn:#D7A54A; --warnbg:#2E2514;
  --crit:#E0695C;
}
*{box-sizing:border-box}
html,body{margin:0}
body{background:var(--canvas); color:var(--ink); font-family:var(--body);
  font-size:15px; line-height:1.5; -webkit-font-smoothing:antialiased}
.wrap{max-width:1140px; margin:0 auto; padding:24px 16px 72px}
h1,h2,h3{font-family:var(--display); text-wrap:balance; margin:0}
.top{display:flex; flex-wrap:wrap; gap:12px; align-items:center; justify-content:space-between; margin-bottom:16px}
.top-r{display:flex; flex-wrap:wrap; gap:8px; align-items:center}
h1{font-size:25px; font-weight:700; letter-spacing:-.01em; color:var(--navy)}
.sub{color:var(--muted); font-size:13px; margin-top:3px}
.seg{display:inline-flex; background:var(--raised); border:1px solid var(--line); border-radius:999px; padding:3px}
.seg button{background:transparent; color:var(--muted); padding:6px 16px; border-radius:999px;
  font-size:13px; border:none; cursor:pointer; font-weight:600; font-family:var(--body)}
.seg button[aria-pressed="true"]{background:var(--surface); color:var(--ink); box-shadow:0 1px 2px rgba(20,30,50,.1)}
.seg button:disabled{opacity:.4; cursor:not-allowed}
.card{background:var(--surface); border:1px solid var(--line); border-radius:var(--r); padding:15px; margin-bottom:14px}
h3.ch{font-size:12px; text-transform:uppercase; letter-spacing:.08em; color:var(--muted);
  font-weight:600; margin-bottom:11px}
label{display:block; font-size:11px; text-transform:uppercase; letter-spacing:.06em;
  color:var(--muted); font-weight:600; margin-bottom:5px}
input,select,button,textarea{font-family:var(--body); font-size:14px}
input[type=text],input[type=password],input[type=search],select{width:100%; padding:8px 9px; border:1px solid var(--line);
  border-radius:7px; background:var(--surface); color:var(--ink)}
input:focus-visible,select:focus-visible,button:focus-visible,textarea:focus-visible{outline:2px solid var(--accent); outline-offset:1px}
button{cursor:pointer; border-radius:7px; border:1px solid transparent; font-weight:600}
.btn{background:var(--surface); border:1px solid var(--line); color:var(--muted); padding:7px 13px; font-size:13px}
.btn:hover{color:var(--ink)}
.btn:disabled{opacity:.5; cursor:not-allowed}
.btn-primary{background:var(--accent); color:#fff; padding:9px 17px; border:none}
.btn-primary:hover{filter:brightness(.94)}
.btn-sync{display:inline-flex; align-items:center; gap:6px}
.dot{width:8px; height:8px; border-radius:50%; background:var(--faint); display:inline-block}
.dot.on{background:var(--good)}
.row-acts{display:flex; gap:7px; flex-wrap:wrap}

/* modal */
.modal-bg{position:fixed; inset:0; background:rgba(14,19,28,.55); display:grid; place-items:center;
  padding:16px; z-index:20}
.modal{background:var(--surface); border:1px solid var(--line); border-radius:var(--r);
  width:100%; max-width:440px; padding:18px; box-shadow:0 16px 40px rgba(0,0,0,.25)}
.modal h2{font-size:18px; color:var(--navy); margin-bottom:4px}
.modal .fg{margin-top:11px}
.modal .mfoot{display:flex; justify-content:space-between; align-items:center; gap:8px;
  flex-wrap:wrap; margin-top:16px; padding-top:12px; border-top:1px solid var(--line)}

/* tiles */
.tiles{display:grid; grid-template-columns:repeat(4,1fr); gap:10px; margin-bottom:14px}
@media (max-width:620px){ .tiles{grid-template-columns:repeat(2,1fr)} }
.tile{background:var(--surface); border:1px solid var(--line); border-radius:var(--r);
  padding:12px 13px; position:relative; overflow:hidden}
.tile::before{content:""; position:absolute; inset:0 auto 0 0; width:3px; background:var(--line)}
.tile.good::before{background:var(--good)} .tile.warn::before{background:var(--warn)}
.tile.open::before{background:var(--accent)}
.tile .n{font-family:var(--display); font-size:26px; font-weight:700; line-height:1.1; font-variant-numeric:tabular-nums}
.tile.good .n{color:var(--good)} .tile.warn .n{color:var(--warn)}
.tile .k{font-size:11px; text-transform:uppercase; letter-spacing:.07em; color:var(--muted); font-weight:600; margin-top:2px}

/* table */
table{width:100%; border-collapse:collapse; font-size:13.5px}
th{text-align:left; font-size:10.5px; text-transform:uppercase; letter-spacing:.07em;
  color:var(--canvas); background:var(--navy); font-weight:600; padding:9px 10px; white-space:nowrap}
@media (prefers-color-scheme: dark){ :root:not([data-theme="light"]) th{color:#0E131C} }
:root[data-theme="dark"] th{color:#0E131C}
td{padding:9px 10px; border-bottom:1px solid var(--line); vertical-align:middle}
tbody tr{cursor:pointer}
tbody tr:hover{background:var(--raised)}
tbody tr:hover td:first-child{box-shadow:inset 3px 0 0 var(--accent)}
.num{text-align:right; font-variant-numeric:tabular-nums}
.mini-bar{height:6px; background:var(--raised); border-radius:99px; overflow:hidden; min-width:70px}
.mini-bar i{display:block; height:100%; background:var(--good); border-radius:99px}
.badge{display:inline-block; padding:2px 9px; border-radius:999px; font-size:11.5px; font-weight:600}
.badge.ok{background:var(--goodbg); color:var(--good)}
.badge.wip{background:var(--warnbg); color:var(--warn)}
.badge.no{background:var(--raised); color:var(--muted)}
.go{color:var(--faint); font-size:13px}

/* upload */
.up{display:grid; grid-template-columns:1fr auto; gap:12px; align-items:center}
@media (max-width:700px){ .up{grid-template-columns:1fr} }
.drop{border:1.5px dashed var(--line); border-radius:var(--r); padding:16px; text-align:center;
  color:var(--muted); font-size:13.5px; cursor:pointer; background:var(--canvas)}
.drop:hover,.drop.over{border-color:var(--accent); color:var(--ink)}
.drop strong{display:block; color:var(--ink); font-family:var(--display); font-size:14.5px; margin-bottom:2px}
.pick3{display:grid; grid-template-columns:1fr 1fr 150px auto; gap:10px; align-items:end}
@media (max-width:700px){ .pick3{grid-template-columns:1fr 1fr} }

/* checklist view */
.backbar{display:flex; gap:11px; align-items:center; flex-wrap:wrap; margin-bottom:13px}
.pick{display:grid; grid-template-columns:1fr 1.2fr; gap:12px}
@media (max-width:700px){ .pick{grid-template-columns:1fr} }
.det{display:grid; grid-template-columns:repeat(auto-fit,minmax(190px,1fr)); gap:1px;
  background:var(--line); border:1px solid var(--line); border-radius:8px; overflow:hidden; margin-top:12px}
.det div{background:var(--surface); padding:9px 11px; min-width:0}
.det .k{font-size:10.5px; text-transform:uppercase; letter-spacing:.06em; color:var(--faint); font-weight:600}
.det .v{font-size:14px; word-break:break-word}
.det .v a{color:var(--accent); text-decoration:none}
.prog{display:grid; grid-template-columns:auto 1fr; gap:16px; align-items:center}
.ring{--p:0; width:76px; height:76px; border-radius:50%; flex:none;
  background:conic-gradient(var(--good) calc(var(--p)*1%), var(--raised) 0);
  display:grid; place-items:center; position:relative}
.ring::after{content:""; position:absolute; inset:9px; border-radius:50%; background:var(--surface)}
.ring span{position:relative; font-family:var(--display); font-weight:700; font-size:18px; font-variant-numeric:tabular-nums}
.bars{display:flex; gap:7px; flex-wrap:wrap}
.pillc{display:inline-flex; align-items:center; gap:6px; border-radius:999px; padding:4px 11px;
  font-size:12.5px; font-weight:600}
.pillc i{width:8px; height:8px; border-radius:50%; display:inline-block}
.pillc.c{background:var(--goodbg); color:var(--good)} .pillc.c i{background:var(--good)}
.pillc.p{background:var(--warnbg); color:var(--warn)} .pillc.p i{background:var(--warn)}

/* stages */
.stage{border:1px solid var(--line); border-radius:var(--r); background:var(--surface);
  margin-bottom:11px; overflow:hidden}
.sh{display:grid; grid-template-columns:auto 1fr auto auto; gap:11px; align-items:center;
  padding:12px 14px; cursor:pointer; background:var(--surface); width:100%; border:none; text-align:left}
.sh:hover{background:var(--raised)}
.sh .tag{font-family:var(--display); font-weight:700; font-size:12px; color:var(--canvas);
  background:var(--navy); border-radius:5px; width:23px; height:23px; display:grid; place-items:center}
@media (prefers-color-scheme: dark){ :root:not([data-theme="light"]) .sh .tag{color:#0E131C} }
:root[data-theme="dark"] .sh .tag{color:#0E131C}
.sh .tag.full{background:var(--good)}
.sh .nm{font-family:var(--display); font-weight:600; font-size:15px; min-width:0}
.sh .mini{font-size:12px; color:var(--muted); font-variant-numeric:tabular-nums; white-space:nowrap}
.sh .car{color:var(--faint); font-size:12px; transition:transform .15s}
.stage[data-open="1"] .sh .car{transform:rotate(90deg)}
.sbody{display:none; border-top:1px solid var(--line)}
.stage[data-open="1"] .sbody{display:block}
.snote{padding:11px 14px; font-size:13px; color:var(--muted); background:var(--canvas);
  border-bottom:1px solid var(--line)}
.item{padding:12px 14px; border-bottom:1px solid var(--line);
  display:grid; grid-template-columns:1fr 268px; gap:13px; align-items:start}
.item:last-child{border-bottom:none}
@media (max-width:860px){ .item{grid-template-columns:1fr} }
.item.done{background:var(--goodbg)}
.itl{display:grid; grid-template-columns:19px 1fr; gap:11px; align-items:start; cursor:pointer}
input.cb{width:17px; height:17px; margin:3px 0 0; accent-color:var(--good); cursor:pointer}
.itx{font-size:14.5px; font-weight:500}
.item.done .itx{color:var(--good)}
.itx .ref{font-family:var(--display); font-size:11.5px; font-weight:700; color:var(--accent); margin-right:7px}
.item.done .itx .ref{color:var(--good)}
.id{font-size:12.8px; color:var(--muted); margin:3px 0 0 30px}
.src{font-size:11px; color:var(--faint); margin:4px 0 0 30px; letter-spacing:.02em}
.rm{width:100%; padding:6px 8px; font-size:13px; border:1px solid var(--line);
  border-radius:7px; background:var(--surface); color:var(--ink); resize:vertical; min-height:36px}

.state{background:var(--surface); border:1px solid var(--line); border-radius:var(--r);
  padding:34px 20px; text-align:center; color:var(--muted); font-size:14px}
.state strong{display:block; color:var(--ink); font-family:var(--display); font-size:16px; margin-bottom:5px}
.hide{display:none !important}
.toast{position:fixed; bottom:16px; left:50%; transform:translateX(-50%); background:var(--navy);
  color:var(--canvas); padding:7px 15px; border-radius:999px; font-size:12.5px; font-weight:600;
  opacity:0; transition:opacity .18s; pointer-events:none; z-index:30}
.toast.on{opacity:1}
@media (prefers-color-scheme: dark){ :root:not([data-theme="light"]) .toast{color:#0E131C} }
:root[data-theme="dark"] .toast{color:#0E131C}
.foot{margin-top:26px; color:var(--muted); font-size:12px; border-top:1px solid var(--line);
  padding-top:12px; display:flex; flex-wrap:wrap; gap:10px; justify-content:space-between; align-items:center}
@media (prefers-reduced-motion:reduce){ *{transition:none !important} }
</style>
</head>
<body>
<div class="wrap">

  <div class="top">
    <div>
      <h1>Tally Onboarding Tracker</h1>
      <div class="sub">FA Connect &middot; distributor-wise SOP validation</div>
    </div>
    <div class="top-r">
      <button type="button" class="btn btn-sync hide" id="ghPushBtn">↑ Sync to GitHub</button>
      <button type="button" class="btn btn-sync" id="ghSetBtn"><i class="dot" id="ghDot"></i> Sync Settings</button>
      <div class="seg" id="views" role="group" aria-label="View">
        <button type="button" data-v="sum" aria-pressed="true">Summary</button>
        <button type="button" data-v="chk" aria-pressed="false" id="btnchk" disabled>Checklist</button>
      </div>
    </div>
  </div>

  <!-- ===== SUMMARY (landing) ===== -->
  <div id="v-sum">
    <div class="tiles">
      <div class="tile open"><div class="n" id="s-tot">0</div><div class="k">Distributors</div></div>
      <div class="tile good"><div class="n" id="s-done">0</div><div class="k">Onboarding complete</div></div>
      <div class="tile warn"><div class="n" id="s-wip">0</div><div class="k">In progress</div></div>
      <div class="tile"><div class="n" id="s-not">0</div><div class="k">Not started</div></div>
    </div>

    <div class="card" style="padding:0;overflow-x:auto" id="sumcard"></div>

    <!-- DISTRIBUTOR MASTER: Collapsed once uploaded -->
    <div class="card">
      <div id="masterStatusBox" class="hide" style="display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px">
        <div>
          <div style="font-weight:700;color:var(--good);display:flex;align-items:center;gap:6px">
            <span style="font-size:16px">✔</span> Distributor Master Loaded & Permanently Saved
          </div>
          <div class="sub" id="masterStatusMeta" style="margin-top:2px">0 distributors loaded.</div>
        </div>
        <div class="row-acts">
          <button type="button" class="btn" id="toggleUploadBtn">Replace / Update File</button>
          <button type="button" class="btn" id="clearm">Clear Master</button>
        </div>
      </div>

      <div id="dropContainer">
        <h3 class="ch">Upload Distributor Master</h3>
        <div class="up">
          <div class="drop" id="drop" tabindex="0" role="button">
            <strong>Upload Distributor Master</strong>
            Drop an .xlsx or .csv here, or click to choose. (One-time activity — saved permanently).
          </div>
          <div class="row-acts">
            <button type="button" class="btn" id="tmpl">Download template</button>
          </div>
        </div>
        <div id="upinfo" class="sub hide" style="margin-top:10px"></div>
      </div>
    </div>

    <div class="card">
      <h3 class="ch">Add a distributor manually</h3>
      <div class="pick3">
        <div><label for="mn">Distributor name</label><input type="text" id="mn" placeholder="Firm name" autocomplete="off"></div>
        <div><label for="me">Email</label><input type="text" id="me" placeholder="name@company.com" autocomplete="off"></div>
        <div><label for="mi">ERP ID</label><input type="text" id="mi" placeholder="CUS000580" autocomplete="off"></div>
        <div><button type="button" class="btn-primary" id="manadd">Add</button></div>
      </div>
    </div>
  </div>

  <!-- ===== CHECKLIST ===== -->
  <div id="v-chk" class="hide">
    <div class="backbar">
      <button type="button" class="btn" id="back">&larr; Back to summary</button>
      <span class="sub" style="margin:0" id="crumb"></span>
    </div>

    <div class="card">
      <div class="pick">
        <div>
          <label for="q">Search distributor</label>
          <input type="search" id="q" placeholder="Name, ERP ID, email&hellip;" autocomplete="off">
        </div>
        <div>
          <label for="sel">Switch distributor</label>
          <select id="sel"></select>
        </div>
      </div>
      <div class="det hide" id="det"></div>
    </div>

    <div class="card hide" id="progcard">
      <div class="prog">
        <div class="ring" id="ring"><span>0%</span></div>
        <div>
          <div style="font-family:var(--display);font-weight:600;font-size:15px" id="progname">&nbsp;</div>
          <div class="sub" id="progline" style="margin-bottom:9px">&nbsp;</div>
          <div class="bars" id="bars"></div>
        </div>
      </div>
    </div>

    <div id="stages"></div>
    <div id="chkempty"></div>
  </div>

  <div class="foot">
    <span id="footn">Auto-saving active.</span>
    <span class="row-acts">
      <button type="button" class="btn" id="expcsv">Export progress (CSV)</button>
      <button type="button" class="btn" id="expjson">Download backup (JSON)</button>
      <button type="button" class="btn" id="impjson">Restore backup</button>
    </span>
  </div>
</div>

<!-- ===== GITHUB SYNC MODAL ===== -->
<div class="modal-bg hide" id="ghModal" role="dialog" aria-modal="true" aria-labelledby="ghTitle">
  <div class="modal">
    <h2 id="ghTitle">GitHub Sync Settings</h2>
    <div class="sub">Saved only in this browser's localStorage — never in the code.</div>
    <div class="fg">
      <label for="ghOwner">GitHub Owner (Username or Org)</label>
      <input type="text" id="ghOwner" placeholder="e.g. your-username" autocomplete="off">
    </div>
    <div class="fg">
      <label for="ghRepo">Repository Name</label>
      <input type="text" id="ghRepo" placeholder="e.g. tally-tracker" autocomplete="off">
    </div>
    <div class="fg">
      <label for="ghBranch">Branch</label>
      <input type="text" id="ghBranch" placeholder="main" value="main" autocomplete="off">
    </div>
    <div class="fg">
      <label for="ghToken">Fine-Grained Personal Access Token (Contents: Read & Write)</label>
      <input type="password" id="ghToken" placeholder="github_pat_..." autocomplete="off">
    </div>
    <div class="mfoot">
      <button type="button" class="btn" id="ghClear">Clear</button>
      <div class="row-acts">
        <button type="button" class="btn" id="ghClose">Close</button>
        <button type="button" class="btn" id="ghPullNow">Pull</button>
        <button type="button" class="btn-primary" id="ghSave">Save &amp; Connect</button>
      </div>
    </div>
  </div>
</div>

<input type="file" id="file" accept=".xlsx,.xls,.csv,text/csv" hidden>
<input type="file" id="bfile" accept="application/json" hidden>
<div class="toast" id="toast" role="status" aria-live="polite">Saved</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

<script>
var STAGES = [
 {id:"A", name:"Environment pre-checks", note:"Settle these before the request goes out — each one changes how the rest of the onboarding runs.", items:[
  {id:"A1", t:"Distributor machine is on Windows 10 or later", d:"Confirm the OS of the machine that actually runs Tally. The FieldAssistChecker installer is a Windows desktop installer.", src:"Added check"},
  {id:"A2", t:"A new company is being created in Tally for the integration", d:"The connector attaches to a dedicated company (reference setup: DMS Connect). Confirm they are creating a new one and not pointing it at their live billing company.", src:"Added check · SOP §5.2"},
  {id:"A3", t:"Tally database location checked — local, not Tally Cloud", d:"Just establish where the Tally company data sits. Local Tally installation is recommended.", src:"Added check"},
  {id:"A4", t:"DMS confirmed as the system of record", d:"If the distributor is still billing out of Tally, stop and fix that before going further.", src:"SOP §5 callout"},
  {id:"A5", t:"Distributor told the Tally company becomes read-only", d:"Once the connector is live, master alteration and manual transaction entry are blocked.", src:"SOP §5 callout"}
 ]},
 {id:"B", name:"FA admin — send the onboarding request", note:"All five steps in the FA Connect dashboard at fa-connect.fieldassist.io.", items:[
  {id:"B1", t:"Distributor located in Manage Distributors", d:"Search by name, ERP ID or city.", src:"SOP §4.2"},
  {id:"B2", t:"Software set to Tally on the distributor row", d:"The row will not accept a request until a software is set.", src:"SOP §4.3"},
  {id:"B3", t:"Sync-from date set to the go-live date, not today", d:"Configure → Sync External Secondary Invoices from.", src:"SOP §4.4 + callout"},
  {id:"B4", t:"Send Request clicked — status moved to Request Sent", d:"Action column → Send Request.", src:"SOP §4.5"},
  {id:"B5", t:"Onboarding email received by the distributor", d:"Carries the plugin download and credentials.", src:"SOP §4.5"}
 ]},
 {id:"C", name:"Distributor — Tally setup", note:"Runs on the distributor's own machine.", items:[
  {id:"C1", t:"Onboarding email opened and credentials retained", d:"Credentials needed again if Tally is reinstalled.", src:"SOP §5.1"},
  {id:"C2", t:"Company and ledgers created in Tally", d:"Company must exist before connector can attach.", src:"SOP §5.2"},
  {id:"C3", t:"FieldAssistChecker connector installed on the Tally machine", d:"Run the installer from the email on the Tally machine.", src:"SOP §5.3"},
  {id:"C4", t:"FieldAssist Integration visible in Gateway of Tally → Utilities", d:"If missing, installer did not finish.", src:"SOP §5.4"},
  {id:"C5", t:"Logged in through option 1 with the email credentials", d:"FieldAssist Integration → 1 (Login).", src:"SOP §5.5"},
  {id:"C6", t:"Import Ledgers run", d:"Pulls every buyer in DMS into Tally.", src:"SOP §5.6"},
  {id:"C7", t:"Import Stock Items run", d:"Pulls stock items into Tally.", src:"SOP §5.6"}
 ]},
 {id:"D", name:"Mapping and approval", note:"Check customer and stock item mapping.", items:[
  {id:"D1", t:"Status moved off Distributor Pending to Admin Pending", d:"Distributor side is done.", src:"SOP §2"},
  {id:"D2", t:"FA admin approval completed", d:"Clears Admin Pending.", src:"SOP §2"},
  {id:"D3", t:"Masters and fields mapped — status reads Mapping Done", d:"Data can flow from this point.", src:"SOP §2"},
  {id:"D4", t:"Buyers mapping at 100%", d:"Check Customers data sync.", src:"SOP §9.1"},
  {id:"D5", t:"Stock items / products mapped", d:"Check Product data sync.", src:"SOP §9.1"}
 ]},
 {id:"E", name:"Invoice validation before go-live", note:"Validate line-level data before go-live.", items:[
  {id:"E1", t:"Invoice number matches the Tally voucher exactly", d:"", src:"SOP §9.2"},
  {id:"E2", t:"Tally invoice date matches — not the sync date", d:"", src:"SOP §9.2"},
  {id:"E3", t:"Buyer firm correct and Buyer Status reads Mapped", d:"", src:"SOP §9.2"},
  {id:"E4", t:"Item and quantity match, in the same unit", d:"Verify units match.", src:"SOP §9.2"},
  {id:"E5", t:"Total value matches to the rupee", d:"", src:"SOP §9.2"},
  {id:"E6", t:"No invoice sitting in Failed status", d:"Failed status must be resolved.", src:"SOP §9.2"},
  {id:"E7", t:"Line-level check done via Download Excel", d:"Verify line items.", src:"SOP §9.2"}
 ]},
 {id:"F", name:"Go-live and handover", note:"Verify continuous sync.", items:[
  {id:"F1", t:"Auto-sync observed completing a cycle", d:"Confirms connector is active.", src:"SOP §6"},
  {id:"F2", t:"Status reads Synced", d:"Live sync is running.", src:"SOP §2"},
  {id:"F3", t:"Sync Health at 100% across document types", d:"GRNs, Sales Invoices, Credit Notes.", src:"SOP §2"},
  {id:"F4", t:"Daily monitoring routine handed to the owning CSM", d:"Handover complete.", src:"SOP §8"},
  {id:"F5", t:"Credentials handled safely", d:"Keep passwords secure.", src:"SOP §5.1"}
 ]}
];

(function(){
  "use strict";
  var KEY = "fa_tally_onboarding_data_v2";
  var GH_KEY = "fa_tally_gh_sync_cfg_v1";
  var GH_FILE = "tracker-data.json";
  var IDB_NAME = "FATallyTrackerDB";
  var IDB_STORE = "tracker_store";
  var ALL = []; STAGES.forEach(function(s){ s.items.forEach(function(i){ ALL.push(i); }); });
  var N = ALL.length;

  var S = { headers:[], rows:[], map:{}, data:{}, sel:"", view:"sum", open:{A:true}, savedAt:0 };

  var $ = function(id){ return document.getElementById(id); };

  function toast(m){
    var t = $("toast"); t.textContent = m; t.classList.add("on");
    clearTimeout(toast._t); toast._t = setTimeout(function(){ t.classList.remove("on"); }, 2200);
  }
  function esc(v){ return String(v==null?"":v).replace(/[&<>"]/g,function(c){
    return {"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;"}[c]; }); }
  function stamp(ts){
    if(!ts) return "";
    var d = new Date(ts);
    return d.toLocaleDateString(undefined,{day:"numeric",month:"short"}) + " " +
           d.toLocaleTimeString(undefined,{hour:"2-digit",minute:"2-digit"});
  }

  /* ---------- UTF-8 Safe Base64 ---------- */
  function toBase64(str){
    var bytes = new TextEncoder().encode(str);
    var bin = "";
    for(var i=0; i<bytes.length; i++) bin += String.fromCharCode(bytes[i]);
    return btoa(bin);
  }
  function fromBase64(b64){
    var bin = atob(b64.replace(/\s/g, ""));
    var bytes = new Uint8Array(bin.length);
    for(var i=0; i<bin.length; i++) bytes[i] = bin.charCodeAt(i);
    return new TextDecoder().decode(bytes);
  }

  /* ---------- GitHub Sync Engine (Token stored ONLY in localStorage) ---------- */
  function getGhCfg(){
    try{
      var c = JSON.parse(localStorage.getItem(GH_KEY) || "null");
      if(c && c.owner && c.repo && c.token) return c;
    }catch(e){}
    return null;
  }
  function paintGhUI(){
    var cfg = getGhCfg();
    $("ghDot").classList.toggle("on", !!cfg);
    $("ghPushBtn").classList.toggle("hide", !cfg);
  }
  function ghApiUrl(cfg){
    return "https://api.github.com/repos/" + encodeURIComponent(cfg.owner) + "/" +
           encodeURIComponent(cfg.repo) + "/contents/" + GH_FILE;
  }
  async function ghPull(quiet){
    var cfg = getGhCfg();
    if(!cfg){ if(!quiet) toast("Configure Sync Settings first"); return false; }
    try{
      var url = ghApiUrl(cfg) + "?ref=" + encodeURIComponent(cfg.branch || "main") + "&cb=" + Date.now();
      var res = await fetch(url, {
        headers: {
          "Authorization": "Bearer " + cfg.token,
          "Accept": "application/vnd.github+json"
        }
      });
      if(res.status === 404){
        if(!quiet) toast("No tracker-data.json in repo yet — click Sync to GitHub to create it");
        return false;
      }
      if(!res.ok) throw new Error("HTTP " + res.status);
      var meta = await res.json();
      var remote = JSON.parse(fromBase64(meta.content));
      if(remote && remote.rows){
        if(!quiet || !S.rows.length || (remote.savedAt || 0) > (S.savedAt || 0)){
          applySnapshot(remote);
          save(true);
          render();
          if(!quiet) toast("✔ Pulled latest data from GitHub");
        }
        return true;
      }
    }catch(err){
      if(!quiet) toast("GitHub pull failed: " + err.message);
    }
    return false;
  }
  async function ghPush(){
    var cfg = getGhCfg();
    if(!cfg){ openGhModal(); return; }
    var btn = $("ghPushBtn");
    btn.disabled = true; btn.textContent = "Syncing…";
    try{
      var branch = cfg.branch || "main";
      var sha = null;
      var getRes = await fetch(ghApiUrl(cfg) + "?ref=" + encodeURIComponent(branch) + "&cb=" + Date.now(), {
        headers: {
          "Authorization": "Bearer " + cfg.token,
          "Accept": "application/vnd.github+json"
        }
      });
      if(getRes.ok){
        var existing = await getRes.json();
        sha = existing.sha;
      }
      S.savedAt = Date.now();
      save(true);
      var body = {
        message: "Update " + GH_FILE + " (" + new Date().toISOString().slice(0,16) + ")",
        content: toBase64(JSON.stringify(getStateSnapshot(), null, 1)),
        branch: branch
      };
      if(sha) body.sha = sha;
      var putRes = await fetch(ghApiUrl(cfg), {
        method: "PUT",
        headers: {
          "Authorization": "Bearer " + cfg.token,
          "Accept": "application/vnd.github+json",
          "Content-Type": "application/json"
        },
        body: JSON.stringify(body)
      });
      if(!putRes.ok){
        var errTxt = "";
        try{ var ej = await putRes.json(); errTxt = ej.message || ""; }catch(e){}
        throw new Error("HTTP " + putRes.status + (errTxt ? " — " + errTxt : ""));
      }
      toast("✔ Synced to GitHub");
    }catch(err){
      toast("Sync failed: " + err.message);
    }finally{
      btn.disabled = false; btn.textContent = "↑ Sync to GitHub";
    }
  }
  function openGhModal(){
    var cfg = getGhCfg() || {};
    $("ghOwner").value = cfg.owner || "";
    $("ghRepo").value = cfg.repo || "";
    $("ghBranch").value = cfg.branch || "main";
    $("ghToken").value = cfg.token || "";
    $("ghModal").classList.remove("hide");
  }
  function closeGhModal(){ $("ghModal").classList.add("hide"); }

  /* ---------- Unlimited IndexedDB Engine ---------- */
  function openDatabase(){
    return new Promise(function(resolve){
      if(!window.indexedDB){ resolve(null); return; }
      var req = indexedDB.open(IDB_NAME, 1);
      req.onupgradeneeded = function(e){
        var db = e.target.result;
        if(!db.objectStoreNames.contains(IDB_STORE)) db.createObjectStore(IDB_STORE);
      };
      req.onsuccess = function(e){ resolve(e.target.result); };
      req.onerror = function(){ resolve(null); };
    });
  }
  function idbPut(key, val){
    return openDatabase().then(function(db){
      if(!db) return;
      return new Promise(function(resolve){
        var tx = db.transaction(IDB_STORE, "readwrite");
        tx.objectStore(IDB_STORE).put(val, key);
        tx.oncomplete = function(){ resolve(); };
        tx.onerror = function(){ resolve(); };
      });
    });
  }
  function idbGet(key){
    return openDatabase().then(function(db){
      if(!db) return null;
      return new Promise(function(resolve){
        var tx = db.transaction(IDB_STORE, "readonly");
        var req = tx.objectStore(IDB_STORE).get(key);
        req.onsuccess = function(){ resolve(req.result || null); };
        req.onerror = function(){ resolve(null); };
      });
    });
  }

  /* ---------- State Persistence ---------- */
  function getStateSnapshot(){
    return {
      headers: S.headers, rows: S.rows, map: S.map, data: S.data,
      sel: S.sel, view: S.view, open: S.open, savedAt: S.savedAt
    };
  }
  function applySnapshot(p){
    if(!p || typeof p !== "object") return false;
    S.headers = p.headers || [];
    S.rows = p.rows || [];
    S.map = p.map || {};
    S.data = p.data || {};
    S.sel = p.sel || "";
    S.view = (p.view === "chk" && p.sel) ? "chk" : "sum";
    S.open = p.open || {A:true};
    S.savedAt = p.savedAt || 0;
    return true;
  }
  function save(quiet){
    S.savedAt = Date.now();
    var snap = getStateSnapshot();
    idbPut(KEY, snap);
    try{ localStorage.setItem(KEY, JSON.stringify(snap)); }catch(e){}
    paintFoot();
    if(!quiet) toast("Saved");
  }

  /* ---------- Live Sync Across Tabs (Same Browser) ---------- */
  window.addEventListener("storage", function(e){
    if(e.key === KEY && e.newValue){
      try{ if(applySnapshot(JSON.parse(e.newValue))) render(); }catch(err){}
    }
    if(e.key === GH_KEY) paintGhUI();
  });

  /* ---------- Column detection ---------- */
  var PAT = {
    name  : /^(distributor\s*name|dist\s*name|firm\s*name|party\s*name|customer\s*name|name)$/i,
    erp   : /(erp\s*id|erpid|distributor\s*code|dist\s*code|customer\s*code|party\s*code|\bcode\b)/i,
    email : /(e-?mail)/i,
    mobile: /(mobile|phone|contact\s*no|contact\s*number)/i,
    city  : /^(city|town|location|hq)$/i,
    state : /^(state|region|zone)$/i
  };
  function detect(headers){
    var mp = {};
    Object.keys(PAT).forEach(function(k){
      for(var i=0;i<headers.length;i++){
        if(PAT[k].test(String(headers[i]).trim())){ mp[k]=headers[i]; return; } }
    });
    if(!mp.name){
      for(var i=0;i<headers.length;i++){ if(/name/i.test(headers[i])){ mp.name=headers[i]; break; } }
    }
    if(!mp.name && headers.length) mp.name = headers[0];
    return mp;
  }
  function m(k,r){ var c = S.map[k]; return c && r[c]!=null ? String(r[c]).trim() : ""; }
  function keyOf(r){
    return String(m("erp",r) || m("name",r) || "").toLowerCase() || JSON.stringify(r).slice(0,60);
  }
  function label(r){
    var n = m("name",r) || "(unnamed)", e = m("erp",r);
    return n + (e ? "  —  " + e : "");
  }

  /* ---------- Scoring & Checklists ---------- */
  function recOf(k){ return S.data[k] || {}; }
  function isDone(k,id){ var r = recOf(k)[id]; return !!(r && r.s === "c"); }
  function remarkOf(k,id){ var r = recOf(k)[id]; return (r && r.r) || ""; }
  function counts(k){
    var c = 0;
    ALL.forEach(function(it){ if(isDone(k,it.id)) c++; });
    return {c:c, left:N-c, total:N, pct: Math.round(c/N*100)};
  }
  function touched(k){
    var r = S.data[k]; if(!r) return false;
    return Object.keys(r).some(function(id){ return r[id].s === "c" || (r[id].r||"").trim(); });
  }
  function stageCounts(k,st){
    var c = 0;
    st.items.forEach(function(it){ if(isDone(k,it.id)) c++; });
    return {c:c, total:st.items.length, pct: Math.round(c/st.items.length*100)};
  }
  function setDone(k,id,on){
    S.data[k] = S.data[k] || {};
    S.data[k][id] = S.data[k][id] || {};
    S.data[k][id].s = on ? "c" : "";
    save(true); paintItem(k,id); renderProgress(); renderPicker();
  }
  var _remTimer = null;
  function setRemark(k,id,v){
    S.data[k] = S.data[k] || {};
    S.data[k][id] = S.data[k][id] || {};
    S.data[k][id].r = v;
    clearTimeout(_remTimer);
    _remTimer = setTimeout(function(){ save(true); }, 250);
  }
  function paintItem(k,id){
    var cb = document.querySelector('input.cb[data-i="'+id+'"]');
    if(!cb) return;
    var on = isDone(k,id);
    cb.checked = on;
    var item = cb.closest(".item");
    if(item) item.classList.toggle("done", on);
    var stEl = item && item.closest(".stage");
    if(stEl){
      var st = STAGES.filter(function(x){ return x.id === stEl.dataset.s; })[0];
      if(st){
        var sc = stageCounts(k,st);
        var mini = stEl.querySelector(".mini");
        if(mini) mini.textContent = sc.c + " of " + sc.total;
        var tag = stEl.querySelector(".tag");
        if(tag) tag.classList.toggle("full", sc.c === sc.total);
      }
    }
  }

  /* ---------- File Ingestion ---------- */
  function parseCSV(txt){
    var rows=[], row=[], f="", q=false;
    txt = txt.replace(/^\uFEFF/,"");
    for(var i=0;i<txt.length;i++){
      var ch=txt[i];
      if(q){
        if(ch==='"'){ if(txt[i+1]==='"'){ f+='"'; i++; } else q=false; }
        else f+=ch;
      }else{
        if(ch==='"') q=true;
        else if(ch===","){ row.push(f); f=""; }
        else if(ch==="\n"){ row.push(f); rows.push(row); row=[]; f=""; }
        else if(ch!=="\r") f+=ch;
      }
    }
    if(f.length||row.length){ row.push(f); rows.push(row); }
    rows = rows.filter(function(r){ return r.some(function(c){ return String(c).trim()!==""; }); });
    if(!rows.length) return {headers:[],rows:[]};
    var hs = rows[0].map(function(h){ return String(h).trim(); });
    return {headers:hs, rows:rows.slice(1).map(function(r){
      var o={}; hs.forEach(function(h,i){ o[h] = r[i]==null?"":String(r[i]).trim(); }); return o; })};
  }
  function ingest(headers, rows){
    headers = headers.filter(function(h){ return String(h).trim()!==""; });
    if(!headers.length || !rows.length){ toast("No rows found in that file"); return; }
    S.headers = headers;
    S.map = detect(headers);
    var validRows = [], seen = {};
    for(var i=0; i<rows.length; i++){
      var r = rows[i];
      if(!m("name",r) && !m("erp",r)) continue;
      var k = keyOf(r);
      if(seen[k]) continue;
      seen[k] = 1;
      validRows.push(r);
    }
    S.rows = validRows; S.sel = ""; S.view = "sum";
    save(false);
    toast("✔ Master loaded and saved!");
    render();
  }
  function readFile(f){
    if(!f) return;
    var isCSV = /\.csv$/i.test(f.name) || f.type==="text/csv";
    var fr = new FileReader();
    fr.onerror = function(){ toast("Could not read that file"); };
    if(isCSV){
      fr.onload = function(){ var p = parseCSV(String(fr.result)); ingest(p.headers, p.rows); };
      fr.readAsText(f); return;
    }
    if(typeof XLSX === "undefined"){ toast("Excel reader did not load — save as CSV"); return; }
    fr.onload = function(){
      try{
        var wb = XLSX.read(new Uint8Array(fr.result), {type:"array"});
        var aoa = XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]], {header:1, raw:false, defval:""});
        aoa = aoa.filter(function(r){ return r.some(function(c){ return String(c).trim()!==""; }); });
        if(!aoa.length){ toast("That sheet is empty"); return; }
        var hi = 0;
        for(var i=0;i<Math.min(aoa.length,8);i++){
          if(aoa[i].filter(function(c){ return String(c).trim()!==""; }).length >= 2){ hi=i; break; } }
        var hs = aoa[hi].map(function(h){ return String(h).trim(); });
        var rows = aoa.slice(hi+1).map(function(r){
          var o={}; hs.forEach(function(h,j){ o[h] = r[j]==null?"":String(r[j]).trim(); }); return o; });
        ingest(hs, rows);
      }catch(e){ toast("Could not read workbook — try CSV format"); }
    };
    fr.readAsArrayBuffer(f);
  }

  /* ---------- Manual Add ---------- */
  function ensureCols(){
    [["name","Distributor Name"],["erp","ERP ID"],["email","Email"]].forEach(function(p){
      if(!S.map[p[0]]){
        S.map[p[0]] = p[1];
        if(S.headers.indexOf(p[1]) < 0) S.headers.push(p[1]);
      }
    });
  }
  function addManual(name,email,erp){
    name=(name||"").trim(); email=(email||"").trim(); erp=(erp||"").trim();
    if(!name && !erp){ toast("Enter at least a distributor name"); return false; }
    ensureCols();
    var r = {};
    r[S.map.name]=name; r[S.map.email]=email; r[S.map.erp]=erp; r.__manual = 1;
    var k = keyOf(r);
    if(S.rows.some(function(x){ return keyOf(x)===k; })){ toast("Already in the list"); return false; }
    S.rows.push(r); save(true);
    toast("Added — " + (name||erp));
    return true;
  }
  function removeManual(k){
    var r = S.rows.filter(function(x){ return keyOf(x)===k; })[0];
    if(!r || !r.__manual) return;
    S.rows = S.rows.filter(function(x){ return keyOf(x)!==k; });
    if(S.sel===k){ S.sel=""; S.view="sum"; }
    save(true); toast("Removed distributor"); render();
  }

  /* ---------- Summary & Checklist Renderers ---------- */
  function renderSummary(){
    var host = $("sumcard");
    if(!S.rows.length){
      ["s-tot","s-done","s-wip","s-not"].forEach(function(i){ $(i).textContent = "0"; });
      host.innerHTML = '<div class="state"><strong>No distributors loaded yet</strong>'+
        'Upload your Distributor Master once below, pull from GitHub Sync, or add distributors manually.</div>';
      return;
    }
    var done=0, wip=0, not=0;
    var rows = S.rows.map(function(r){
      var k = keyOf(r), c = counts(k), t = touched(k);
      var st = !t ? "not" : (c.pct===100 ? "done" : "wip");
      if(st==="done") done++; else if(st==="wip") wip++; else not++;
      return {r:r, k:k, c:c, st:st};
    });
    $("s-tot").textContent = S.rows.length;
    $("s-done").textContent = done;
    $("s-wip").textContent = wip;
    $("s-not").textContent = not;
    rows.sort(function(a,b){
      var o = {wip:0, not:1, done:2};
      if(o[a.st]!==o[b.st]) return o[a.st]-o[b.st];
      if(a.c.pct!==b.c.pct) return b.c.pct-a.c.pct;
      return (m("name",a.r)||"").localeCompare(m("name",b.r)||"");
    });
    var bd = {done:'<span class="badge ok">Complete</span>',
              wip:'<span class="badge wip">In progress</span>',
              not:'<span class="badge no">Not started</span>'};
    host.innerHTML = '<table><thead><tr>'+
      '<th>Distributor</th><th>ERP ID</th><th>Email</th><th>Status</th>'+
      '<th style="min-width:116px">Progress</th><th class="num">Ticked</th>'+
      '<th class="num">Left</th><th></th></tr></thead><tbody>'+
      rows.map(function(x){
        return '<tr data-k="'+esc(x.k)+'" tabindex="0">'+
          '<td>'+esc(m("name",x.r)||"(unnamed)")+'</td>'+
          '<td>'+esc(m("erp",x.r)||"—")+'</td>'+
          '<td>'+esc(m("email",x.r)||"—")+'</td>'+
          '<td>'+bd[x.st]+'</td>'+
          '<td><div class="mini-bar"><i style="width:'+x.c.pct+'%"></i></div>'+
            '<span class="num" style="font-size:11.5px;color:var(--muted)">'+x.c.pct+'%</span></td>'+
          '<td class="num">'+x.c.c+' / '+N+'</td>'+
          '<td class="num">'+x.c.left+'</td>'+
          '<td class="go">→</td></tr>';
      }).join("")+'</tbody></table>';
  }
  function renderPicker(){
    var q = $("q").value.trim().toLowerCase(), sel = $("sel");
    sel.innerHTML = "";
    var list = S.rows.filter(function(r){
      if(!q) return true;
      return S.headers.some(function(h){ return String(r[h]||"").toLowerCase().indexOf(q)>=0; });
    });
    sel.appendChild(new Option(list.length ? "— " + list.length + " distributor(s) —" : "No match", ""));
    list.slice(0,2000).forEach(function(r){
      var k = keyOf(r), c = counts(k);
      var o = new Option(label(r) + "   " + c.c + "/" + N, k);
      if(k === S.sel) o.selected = true;
      sel.appendChild(o);
    });
    if(S.sel && !list.some(function(r){ return keyOf(r)===S.sel; })){
      var cur = selRow();
      if(cur){ var o2 = new Option(label(cur), S.sel); o2.selected = true; sel.appendChild(o2); }
    }
  }
  function selRow(){ return S.rows.filter(function(r){ return keyOf(r)===S.sel; })[0] || null; }
  function renderDetails(){
    var d = $("det"), r = selRow();
    if(!r){ d.classList.add("hide"); d.innerHTML=""; return; }
    d.classList.remove("hide");
    var pri = [["Distributor", m("name",r)], ["Email", m("email",r)], ["ERP ID", m("erp",r)]];
    d.innerHTML = pri.map(function(p){
      var v = String(p[1]||"").trim();
      if(!v) v = "—";
      else if(/^Email$/i.test(p[0]) && /@/.test(v)) v = '<a href="mailto:'+esc(v)+'">'+esc(v)+'</a>';
      else v = esc(v);
      return '<div><div class="k">'+esc(p[0])+'</div><div class="v">'+v+'</div></div>';
    }).join("") + (r.__manual ? '<div><div class="k">Source</div><div class="v">'+
      'Added manually <button type="button" class="btn" id="delman" '+
      'style="padding:2px 8px;font-size:11.5px;margin-left:6px">Remove</button></div></div>' : "");
  }
  function renderProgress(){
    var card = $("progcard"), r = selRow();
    if(!r){ card.classList.add("hide"); return; }
    card.classList.remove("hide");
    var c = counts(S.sel);
    var ring = $("ring");
    ring.style.setProperty("--p", c.pct);
    ring.querySelector("span").textContent = c.pct + "%";
    $("progname").textContent = m("name",r) || "(unnamed)";
    $("progline").textContent = c.pct === 100
      ? "All " + N + " checkpoints ticked — ready to mark onboarding complete."
      : c.c + " of " + N + " checkpoints ticked.";
    $("bars").innerHTML =
      '<span class="pillc c"><i></i>'+c.c+' ticked</span>'+
      (c.left ? '<span class="pillc p"><i></i>'+c.left+' left</span>' : '');
  }
  function renderChecklist(){
    var host = $("stages"), empty = $("chkempty");
    if(!S.sel){
      host.innerHTML = "";
      empty.innerHTML = '<div class="state"><strong>No distributor selected</strong>Go back to the summary and click a row.</div>';
      return;
    }
    empty.innerHTML = "";
    var k = S.sel;
    $("crumb").textContent = (m("name", selRow()) || "") + "  ·  " + counts(k).c + " of " + N + " ticked";
    host.innerHTML = STAGES.map(function(st){
      var sc = stageCounts(k,st);
      var items = st.items.map(function(it){
        var on = isDone(k,it.id);
        return '<div class="item'+(on?" done":"")+'">'+
          '<div><label class="itl">'+
            '<input type="checkbox" class="cb" data-i="'+it.id+'"'+(on?" checked":"")+
              ' aria-label="'+esc(it.id+" "+it.t)+'">'+
            '<span class="itx"><span class="ref">'+it.id+'</span>'+esc(it.t)+'</span>'+
          '</label>'+
          (it.d ? '<div class="id">'+esc(it.d)+'</div>' : '')+
          '<div class="src">'+esc(it.src)+'</div></div>'+
          '<div><textarea class="rm" rows="1" data-r="'+it.id+'" placeholder="Remarks">'+
            esc(remarkOf(k,it.id))+'</textarea></div></div>';
      }).join("");
      return '<section class="stage" data-s="'+st.id+'" data-open="'+(S.open[st.id]?"1":"0")+'">'+
        '<button type="button" class="sh" data-t="'+st.id+'">'+
          '<span class="tag'+(sc.c===sc.total?" full":"")+'">'+st.id+'</span>'+
          '<span class="nm">'+esc(st.name)+'</span>'+
          '<span class="mini">'+sc.c+' of '+sc.total+'</span>'+
          '<span class="car">▶</span>'+
        '</button>'+
        '<div class="sbody">'+(st.note?'<div class="snote">'+esc(st.note)+'</div>':'')+items+'</div>'+
      '</section>';
    }).join("");
  }
  function paintFoot(){
    var f = $("footn"); if(!f) return;
    var who = S.rows.length
      ? Object.keys(S.data).length + " of " + S.rows.length + " distributors have progress"
      : "Nothing saved yet";
    f.textContent = who + (S.savedAt ? " · last saved " + stamp(S.savedAt) : "") + " · auto-saved in this browser.";
  }
  function render(){
    paintGhUI();
    var hasRows = S.rows.length > 0;
    $("masterStatusBox").classList.toggle("hide", !hasRows);
    $("dropContainer").classList.toggle("hide", hasRows);
    if(hasRows) $("masterStatusMeta").textContent = S.rows.length + " distributors loaded · Master is permanently active.";
    $("btnchk").disabled = !S.sel;
    document.querySelectorAll("#views button").forEach(function(b){
      b.setAttribute("aria-pressed", String(b.dataset.v === S.view));
    });
    $("v-sum").classList.toggle("hide", S.view!=="sum");
    $("v-chk").classList.toggle("hide", S.view!=="chk");
    paintFoot();
    if(S.view==="sum") renderSummary();
    else { renderPicker(); renderDetails(); renderProgress(); renderChecklist(); }
  }
  function openDist(k){
    S.sel = k; S.view = "chk"; $("q").value = "";
    save(true); render();
    try{ window.scrollTo({top:0, behavior:"smooth"}); }catch(e){}
  }

  /* ---------- Event Listeners ---------- */
  $("ghSetBtn").addEventListener("click", openGhModal);
  $("ghClose").addEventListener("click", closeGhModal);
  $("ghModal").addEventListener("click", function(e){ if(e.target === $("ghModal")) closeGhModal(); });
  $("ghPushBtn").addEventListener("click", ghPush);
  $("ghPullNow").addEventListener("click", function(){ ghPull(false); closeGhModal(); });
  $("ghClear").addEventListener("click", function(){
    localStorage.removeItem(GH_KEY);
    paintGhUI(); closeGhModal(); toast("GitHub Sync settings cleared");
  });
  $("ghSave").addEventListener("click", function(){
    var owner = $("ghOwner").value.trim(),
        repo = $("ghRepo").value.trim(),
        branch = $("ghBranch").value.trim() || "main",
        token = $("ghToken").value.trim();
    if(!owner || !repo || !token){ toast("Fill in Owner, Repo, and Token"); return; }
    localStorage.setItem(GH_KEY, JSON.stringify({owner:owner, repo:repo, branch:branch, token:token}));
    paintGhUI(); closeGhModal();
    toast("Sync settings saved — checking repo…");
    ghPull(false);
  });

  $("toggleUploadBtn").addEventListener("click", function(){ $("dropContainer").classList.toggle("hide"); });
  $("drop").addEventListener("click", function(){ $("file").click(); });
  $("drop").addEventListener("keydown", function(e){ if(e.key==="Enter"||e.key===" "){ e.preventDefault(); $("file").click(); } });
  ["dragenter","dragover"].forEach(function(ev){
    $("drop").addEventListener(ev, function(e){ e.preventDefault(); $("drop").classList.add("over"); }); });
  ["dragleave","drop"].forEach(function(ev){
    $("drop").addEventListener(ev, function(e){ e.preventDefault(); $("drop").classList.remove("over"); }); });
  $("drop").addEventListener("drop", function(e){ if(e.dataTransfer && e.dataTransfer.files[0]) readFile(e.dataTransfer.files[0]); });
  $("file").addEventListener("change", function(e){ readFile(e.target.files[0]); e.target.value=""; });

  $("manadd").addEventListener("click", function(){
    if(addManual($("mn").value, $("me").value, $("mi").value)){
      $("mn").value = $("me").value = $("mi").value = "";
      render(); $("mn").focus();
    }
  });
  ["mn","me","mi"].forEach(function(id){
    $(id).addEventListener("keydown", function(e){ if(e.key === "Enter"){ e.preventDefault(); $("manadd").click(); } });
  });

  $("sumcard").addEventListener("click", function(e){ var tr = e.target.closest("tr[data-k]"); if(tr) openDist(tr.dataset.k); });
  $("sumcard").addEventListener("keydown", function(e){ if(e.key === "Enter"){ var tr = e.target.closest("tr[data-k]"); if(tr) openDist(tr.dataset.k); } });

  $("back").addEventListener("click", function(){ S.view = "sum"; save(true); render(); });
  $("views").addEventListener("click", function(e){
    var b = e.target.closest("button[data-v]");
    if(!b || b.disabled) return;
    S.view = b.dataset.v; save(true); render();
  });

  $("q").addEventListener("input", renderPicker);
  $("sel").addEventListener("change", function(){ var v = $("sel").value; if(v){ S.sel = v; save(true); render(); } });
  $("det").addEventListener("click", function(e){ if(e.target && e.target.id === "delman") removeManual(S.sel); });

  $("stages").addEventListener("click", function(e){
    var h = e.target.closest(".sh");
    if(h){ S.open[h.dataset.t] = !S.open[h.dataset.t]; save(true); renderChecklist(); }
  });
  $("stages").addEventListener("input", function(e){
    var t = e.target.closest("textarea[data-r]"); if(t) setRemark(S.sel, t.dataset.r, t.value);
  });
  $("stages").addEventListener("change", function(e){
    var cb = e.target.closest("input.cb"); if(cb){ setDone(S.sel, cb.dataset.i, cb.checked); return; }
    var t = e.target.closest("textarea[data-r]"); if(t) setRemark(S.sel, t.dataset.r, t.value);
  });

  $("clearm").addEventListener("click", function(){
    if(!confirm("Clear the distributor master? All distributors will be removed.")) return;
    S.headers=[]; S.rows=[]; S.map={}; S.sel=""; S.view="sum";
    save(true); toast("Master cleared"); render();
  });

  function dl(name, text, mime){
    var b = new Blob([text], {type:mime||"text/plain;charset=utf-8"});
    var a = document.createElement("a");
    a.href = URL.createObjectURL(b); a.download = name;
    document.body.appendChild(a); a.click(); a.remove();
    setTimeout(function(){ URL.revokeObjectURL(a.href); }, 2000);
  }
  function q(v){ v = String(v==null?"":v); return /[",\n]/.test(v) ? '"'+v.replace(/"/g,'""')+'"' : v; }

  $("tmpl").addEventListener("click", function(){
    dl("distributor-master-template.csv",
      "Distributor Name,ERP ID,Email\nShree Sales Agency,CUS000580,shree.sales@example.com\n", "text/csv;charset=utf-8");
  });
  $("expcsv").addEventListener("click", function(){
    if(!S.rows.length){ toast("Nothing to export yet"); return; }
    var out = [["Distributor","ERP ID","Email","Overall %","Stage","Stage name",
                "Ref","Checkpoint","Ticked","Remarks"].map(q).join(",")];
    S.rows.forEach(function(r){
      var k = keyOf(r), c = counts(k);
      STAGES.forEach(function(st){
        st.items.forEach(function(it){
          out.push([m("name",r), m("erp",r), m("email",r), c.pct, st.id, st.name,
                    it.id, it.t, isDone(k,it.id)?"Yes":"No", remarkOf(k,it.id)].map(q).join(","));
        });
      });
    });
    dl("tally-onboarding-progress-"+new Date().toISOString().slice(0,10)+".csv", out.join("\n"), "text/csv;charset=utf-8");
  });
  $("expjson").addEventListener("click", function(){
    dl("tally-onboarding-backup-"+new Date().toISOString().slice(0,10)+".json",
       JSON.stringify(getStateSnapshot(), null, 1), "application/json");
  });
  $("impjson").addEventListener("click", function(){ $("bfile").click(); });
  $("bfile").addEventListener("change", function(e){
    var f = e.target.files && e.target.files[0]; if(!f) return;
    var fr = new FileReader();
    fr.onload = function(){
      try{
        var p = JSON.parse(String(fr.result));
        if(applySnapshot(p)){ save(true); toast("Backup restored"); render(); }
      }catch(err){ toast("Could not read backup file"); }
    };
    fr.readAsText(f); e.target.value="";
  });

  /* ---------- Unified Boot Engine ---------- */
  async function boot(){
    try{
      var raw = localStorage.getItem(KEY);
      if(raw) applySnapshot(JSON.parse(raw));
    }catch(e){}

    try{
      var idbData = await idbGet(KEY);
      if(idbData && idbData.rows && idbData.rows.length){
        if(!S.savedAt || (idbData.savedAt || 0) >= S.savedAt) applySnapshot(idbData);
      }
    }catch(e){}

    render();

    // Auto-pull from GitHub API if configured, else fallback to static tracker-data.json
    if(getGhCfg()){
      await ghPull(true);
    }else{
      try{
        var res = await fetch("./" + GH_FILE + "?cb=" + Date.now());
        if(res.ok){
          var remote = await res.json();
          if(remote && remote.rows && (!S.rows.length || (remote.savedAt || 0) > (S.savedAt || 0))){
            if(applySnapshot(remote)){ save(true); render(); }
          }
        }
      }catch(e){}
    }
  }

  boot();
})();
</script>
</body>
</html>
