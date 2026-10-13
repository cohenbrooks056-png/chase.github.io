.
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Chase Wallet</title>
<style>
:root {
  --navy:#073b75;
  --blue:#126bc5;
  --pale:#eaf3fc;
  --bg:#f4f7fb;
  --text:#172b4d;
  --muted:#738198;
  --border:#e5eaf1;
  --green:#16845b;
  --red:#c74343;
}
* { box-sizing:border-box }
body { margin:0; font-family:Arial,Helvetica,sans-serif; background:var(--bg); color:var(--text) }
button,input,select { font:inherit }
button { cursor:pointer }
button:focus-visible,input:focus-visible,select:focus-visible { outline:2px solid var(--blue); outline-offset:2px }
.topbar { height:68px; padding:0 5%; background:white; border-bottom:1px solid var(--border); display:flex; align-items:center; justify-content:space-between; position:sticky; top:0; z-index:10 }
.brand { display:flex; align-items:center; gap:11px; font-size:22px; font-weight:800; color:var(--navy) }
.logo { width:37px; height:37px; display:grid; place-items:center; border-radius:10px; background:var(--navy); color:white; font-size:22px }
.profile { display:flex; align-items:center; gap:12px; font-size:13px; font-weight:700 }
.avatar { width:37px; height:37px; border-radius:50%; display:grid; place-items:center; color:var(--navy); background:var(--pale) }
.layout { display:grid; grid-template-columns:225px minmax(0,1fr); max-width:1600px; margin:auto }
.sidebar { background:white; border-right:1px solid var(--border); min-height:calc(100vh - 68px); padding:25px 14px }
.nav-label { margin:14px 12px 9px; font-size:10px; font-weight:800; letter-spacing:1.4px; color:#96a1b2 }
.nav-btn { display:flex; align-items:center; gap:12px; width:100%; padding:13px 12px; margin:3px 0; border:0; border-radius:9px; background:transparent; text-align:left; color:#66758b; font-size:13px }
.nav-btn:hover,.nav-btn.active { background:var(--pale); color:var(--navy); font-weight:700 }
.ico { width:20px; text-align:center; font-size:17px }
.main { padding:32px; min-width:0 }
.page { display:none }
.page.active { display:block; animation:fade .18s ease }
@keyframes fade { from { opacity:.5 } to { opacity:1 } }
.heading { display:flex; justify-content:space-between; align-items:center; gap:14px; margin-bottom:24px }
h1 { font-size:27px; letter-spacing:-.6px; margin:0 0 7px }
h2 { font-size:17px; margin:0 }
.subtitle { color:var(--muted); font-size:13px; line-height:1.5 }
.grid { display:grid; gap:18px }
.two { grid-template-columns:repeat(2,minmax(0,1fr)) }
.three { grid-template-columns:repeat(3,minmax(0,1fr)) }
.card { padding:22px; background:white; border:1px solid var(--border); border-radius:14px; min-width:0 }
.balance-card { padding:24px; color:white; background:linear-gradient(125deg,#073b75,#126bc5); border:0; position:relative; overflow:hidden; min-height:220px }
.balance-card:after { content:""; position:absolute; width:230px; height:230px; right:-75px; top:-100px; border:1px solid #ffffff30; border-radius:50%; box-shadow:0 0 0 30px #ffffff0b,0 0 0 60px #ffffff08; pointer-events:none }
.balance-label { font-size:13px; opacity:.83 }
.balance-amount { font-size:clamp(30px,4vw,42px); font-weight:800; letter-spacing:-1px; margin:18px 0 9px; overflow-wrap:anywhere }
.balance-foot { font-size:12px; opacity:.75 }
.actions { display:flex; gap:10px; margin-top:22px; position:relative; z-index:1 }
.btn { padding:11px 15px; border:0; border-radius:8px; background:var(--blue); color:white; font-weight:700; font-size:12px }
.btn:hover { filter:brightness(.94) }
.btn-white { background:white; color:var(--navy) }
.btn-light { background:var(--pale); color:var(--navy) }
.btn-outline { background:white; border:1px solid var(--border); color:var(--navy) }
.btn-danger { background:#fff0f0; color:var(--red) }
.account-icon { width:43px; height:43px; display:grid; place-items:center; border-radius:11px; background:var(--pale); color:var(--navy); font-size:20px }
.account-head { display:flex; align-items:center; justify-content:space-between }
.account-name { margin-top:16px; font-weight:700; font-size:14px }
.account-number { margin-top:5px; color:var(--muted); font-size:12px }
.metric { font-size:26px; font-weight:800; letter-spacing:-.5px; margin:16px 0 6px; overflow-wrap:anywhere }
.metric-note { font-size:12px; color:var(--muted) }
.account-balance { font-size:26px; font-weight:800; margin:22px 0; overflow-wrap:anywhere }
.account-bottom { display:flex; align-items:center; justify-content:space-between; gap:10px }
.pill,.status { display:inline-block; border-radius:20px; padding:5px 9px; font-size:10px; font-weight:700; background:#e7f7ef; color:var(--green) }
.section-head { display:flex; align-items:center; justify-content:space-between; gap:12px; margin-bottom:18px }
.link-btn { border:0; background:transparent; color:var(--blue); font-size:12px; font-weight:700 }
.table-wrap { overflow-x:auto }
table { width:100%; border-collapse:collapse; white-space:nowrap }
th { padding:12px 9px; border-bottom:1px solid var(--border); color:#8793a5; text-align:left; font-size:10px; letter-spacing:.6px; text-transform:uppercase }
td { padding:14px 9px; border-bottom:1px solid #edf0f5; font-size:12px }
tr:last-child td { border-bottom:0 }
.tx-title { display:flex; align-items:center; gap:9px }
.tx-icon { width:33px; height:33px; display:grid; place-items:center; border-radius:9px; background:#f0f4fa }
.tx-sub { margin-top:4px; color:var(--muted); font-size:10px }
.positive { color:var(--green); font-weight:800 }
.negative { font-weight:800 }
.form-grid { display:grid; grid-template-columns:1fr 1fr; gap:16px }
.field { display:flex; flex-direction:column; gap:7px; min-width:0 }
.field.full { grid-column:1/-1 }
.field label { font-size:12px; font-weight:700; color:#485b75 }
.field input,.field select { width:100%; padding:12px; border:1px solid #dce3ed; border-radius:8px; background:white; color:var(--text); min-width:0 }
.form-note,.notice { padding:13px; border-radius:9px; background:#f0f6ff; color:#45617e; font-size:12px; line-height:1.6 }
.setting-row { display:flex; align-items:center; justify-content:space-between; gap:15px; padding:16px 0; border-bottom:1px solid var(--border) }
.setting-row:last-child { border-bottom:0 }
.setting-title { font-size:12px; font-weight:700 }
.setting-desc { margin-top:5px; font-size:11px; line-height:1.5; color:var(--muted) }
.switch { width:40px; height:22px; position:relative; flex-shrink:0 }
.switch input { display:none }
.slider { position:absolute; inset:0; background:#c8d1dd; border-radius:20px; transition:.2s }
.slider:before { content:""; position:absolute; left:3px; top:3px; width:16px; height:16px; border-radius:50%; background:white; transition:.2s }
.switch input:checked+.slider { background:var(--blue) }
.switch input:checked+.slider:before { transform:translateX(18px) }
.card-visual { min-height:200px; padding:23px; border-radius:16px; color:white; background:linear-gradient(130deg,#102d50,#1766ae); position:relative; overflow:hidden }
.card-brand { display:flex; justify-content:space-between; font-size:13px; font-weight:800 }
.card-chip { margin-top:27px; font-size:23px }
.card-number { margin-top:16px; letter-spacing:2px; font-size:15px }
.card-footer { display:flex; justify-content:space-between; gap:8px; margin-top:19px; font-size:9px; opacity:.8 }
.empty { padding:26px; text-align:center; color:var(--muted); font-size:12px }
.mobile-menu { display:none }
.watermark { position:fixed; bottom:7px; right:10px; z-index:20; font-size:9px; letter-spacing:1.3px; color:#64748b; opacity:.55; pointer-events:none }
.toast { display:none; position:fixed; right:18px; top:82px; z-index:100000; max-width:340px; padding:14px 17px; background:#163d61; color:white; border-radius:9px; font-size:12px; box-shadow:0 8px 30px #0002 }
.key-gate { position:fixed; inset:0; z-index:99999; display:flex; align-items:center; justify-content:center; padding:20px; background:#f4f7fb }
.key-box { width:100%; max-width:390px; padding:34px; background:white; border:1px solid var(--border); border-radius:17px; box-shadow:0 18px 55px #163d6114 }
.key-logo { width:47px; height:47px; display:grid; place-items:center; border-radius:12px; background:var(--navy); color:white; font-size:25px; font-weight:bold }
.key-box h1 { margin-top:22px; font-size:26px }
.key-box p { margin-bottom:25px; color:var(--muted); font-size:13px; line-height:1.6 }
.key-box label { display:block; margin-bottom:8px; font-size:10px; font-weight:bold; letter-spacing:1px }
.key-box input { width:100%; padding:13px; border:1px solid #dce3ed; border-radius:8px; margin-bottom:13px }
.key-box button { width:100%; padding:13px; background:var(--navy); color:white; border:0; border-radius:8px; font-weight:700 }
.key-box button:hover { background:var(--blue) }
#keyError { min-height:18px; margin-top:12px; color:var(--red); font-size:12px }
.key-box small { display:block; margin-top:12px; text-align:center; color:var(--muted); font-size:10px }
@media(max-width:900px) {
  .layout { grid-template-columns:1fr }
  .sidebar { display:none; min-height:auto; border-right:0; border-bottom:1px solid var(--border); padding:12px }
  .sidebar.open { display:block }
  .main { padding:22px 17px }
  .mobile-menu { display:inline-block }
  .three { grid-template-columns:1fr 1fr }
}
@media(max-width:600px) {
  .topbar { height:62px; padding:0 14px }
  .brand { font-size:19px }
  .profile>span { display:none }
  .main { padding:20px 12px }
  .two,.three,.form-grid { grid-template-columns:1fr }
  .field.full { grid-column:auto }
  .card { padding:17px }
  .balance-card { padding:20px }
  .heading { align-items:flex-start }
  h1 { font-size:24px }
  .key-box { padding:26px }
}
</style>
</head>
<body>

<div id="keyGate" class="key-gate">
  <form id="keyForm" class="key-box">
    <div class="key-logo">↗</div>
    <h1>Chase Wallet</h1>
    <p>Enter your access key to continue. If you dont have a key dm me on TikTok: @larpedtheledgerr.</p>
    <label for="accessKey">ACCESS KEY</label>
    <input id="accessKey" type="password" placeholder="CHASE-000000" autocomplete="off" required>
    <button type="submit">Unlock Wallet →</button>
    <div id="keyError" role="alert"></div>
    <small>Authorized access only</small>
  </form>
</div>

<header class="topbar">
  <div class="brand"><div class="logo">↗</div> Chase Wallet</div>
  <div class="profile">
    <button class="btn btn-light mobile-menu" onclick="toggleMenu()">☰</button>
    <span>My Wallet</span><div class="avatar">MW</div>
  </div>
</header>

<div class="layout">
<aside class="sidebar" id="sidebar">
  <div class="nav-label">WALLET</div>
  <button class="nav-btn active" data-page="dashboard" onclick="showPage('dashboard')"><span class="ico">▦</span>Dashboard</button>
  <button class="nav-btn" data-page="accounts" onclick="showPage('accounts')"><span class="ico">▤</span>Accounts</button>
  <button class="nav-btn" data-page="transactions" onclick="showPage('transactions')"><span class="ico">↕</span>Transactions</button>
  <div class="nav-label">PAYMENTS</div>
  <button class="nav-btn" data-page="send" onclick="showPage('send')"><span class="ico">↗</span>Send Money</button>
  <button class="nav-btn" data-page="receive" onclick="showPage('receive')"><span class="ico">↙</span>Receive Money</button>
  <div class="nav-label">MANAGE</div>
  <button class="nav-btn" data-page="cards" onclick="showPage('cards')"><span class="ico">▰</span>Cards</button>
  <button class="nav-btn" data-page="settings" onclick="showPage('settings')"><span class="ico">⚙</span>Settings</button>
</aside>

<main class="main">
<section class="page active" id="page-dashboard">
  <div class="heading">
    <div><h1>Good morning</h1><div class="subtitle">Here's what's happening with your wallet.</div></div>
    <button class="btn btn-outline" onclick="showPage('transactions')">View activity ↗</button>
  </div>
  <div class="grid two">
    <div class="card balance-card">
      <div class="balance-label">Total wallet balance</div>
      <div class="balance-amount" id="mainBalance">$285,198.62</div>
      <div class="balance-foot">Combined account balance</div>
      <div class="actions"><button class="btn btn-white" onclick="showPage('send')">↗ Send</button><button class="btn btn-white" onclick="showPage('receive')">↙ Receive</button></div>
    </div>
    <div class="grid" style="gap:14px">
      <div class="card">
        <div class="account-head"><div class="account-icon">▤</div><span class="pill">Active</span></div>
        <div class="account-name">Checking account</div><div class="metric" id="checkingMetric">$185,198.62</div><div class="metric-note">•••• 4821</div>
      </div>
      <div class="card">
        <div class="account-head"><div class="account-icon">◈</div><span class="pill">Active</span></div>
        <div class="account-name">Savings account</div><div class="metric" id="savingsMetric">$100,000.00</div><div class="metric-note">•••• 9064</div>
      </div>
    </div>
  </div>
  <div class="grid three" style="margin-top:18px">
    <div class="card"><div class="subtitle">Money received</div><div class="metric" id="receivedMetric">$0.00</div><div class="metric-note">This session</div></div>
    <div class="card"><div class="subtitle">Money sent</div><div class="metric" id="sentMetric">$0.00</div><div class="metric-note">This session</div></div>
    <div class="card"><div class="subtitle">Transactions</div><div class="metric" id="countMetric">0</div><div class="metric-note">Recorded this session</div></div>
  </div>
  <div class="card" style="margin-top:18px">
    <div class="section-head"><h2>Recent activity</h2><button class="link-btn" onclick="showPage('transactions')">See all</button></div>
    <div class="table-wrap"><table><thead><tr><th>Transaction</th><th>Date</th><th>Account</th><th>Amount</th><th>Status</th></tr></thead><tbody id="recentTransactions"></tbody></table></div>
  </div>
</section>

<section class="page" id="page-accounts">
  <div class="heading"><div><h1>Accounts</h1><div class="subtitle">Your wallet account overview.</div></div></div>
  <div class="grid two">
    <div class="card">
      <div class="account-head"><div class="account-icon">▤</div><span class="pill">Active</span></div>
      <div class="account-name">Checking</div><div class="account-number">Account ending in 4821</div>
      <div class="account-balance" id="checkingAccountBalance">$185,198.62</div>
      <div class="account-bottom"><span class="subtitle">Checking account</span><button class="btn btn-light" onclick="openSendFor('checking')">Send money</button></div>
    </div>
    <div class="card">
      <div class="account-head"><div class="account-icon">◈</div><span class="pill">Active</span></div>
      <div class="account-name">Savings</div><div class="account-number">Account ending in 9064</div>
      <div class="account-balance" id="savingsAccountBalance">$100,000.00</div>
      <div class="account-bottom"><span class="subtitle">Savings account</span><button class="btn btn-light" onclick="openSendFor('savings')">Send money</button></div>
    </div>
  </div>
  <div class="card" style="margin-top:18px">
    <h2>Account details</h2>
    <div class="setting-row"><div><div class="setting-title">Combined balance</div><div class="setting-desc">Total across wallet accounts</div></div><strong id="combinedAccountBalance">$285,198.62</strong></div>
    <div class="setting-row"><div><div class="setting-title">Currency</div><div class="setting-desc">Display currency</div></div><strong>USD ($)</strong></div>
    <div class="setting-row"><div><div class="setting-title">Wallet type</div><div class="setting-desc">Local interactive interface</div></div><span class="pill">Simulation</span></div>
  </div>
</section>

<section class="page" id="page-transactions">
  <div class="heading"><div><h1>Transactions</h1><div class="subtitle">Review recorded wallet activity.</div></div><button class="btn btn-outline" onclick="clearTransactions()">Clear activity</button></div>
  <div class="card">
    <div class="section-head"><h2>All activity</h2>
      <select id="transactionFilter" onchange="renderTransactions()" aria-label="Filter transactions">
        <option value="all">All activity</option><option value="sent">Money sent</option><option value="received">Money received</option>
      </select>
    </div>
    <div class="table-wrap"><table><thead><tr><th>Transaction</th><th>Date</th><th>Account</th><th>Amount</th><th>Status</th></tr></thead><tbody id="allTransactions"></tbody></table></div>
    <div id="emptyTransactions" class="empty" style="display:none">No transactions match this filter.</div>
  </div>
</section>

<section class="page" id="page-send">
  <div class="heading"><div><h1>Send money</h1><div class="subtitle">Record a simulated transfer from your wallet.</div></div></div>
  <div class="grid two">
    <div class="card">
      <div class="section-head"><h2>Transfer details</h2></div>
      <form id="sendForm" class="form-grid">
        <div class="field full"><label for="recipient">Recipient name</label><input id="recipient" required maxlength="70" placeholder="Enter recipient name"></div>
        <div class="field full"><label for="recipientDetails">Recipient email or note (optional)</label><input id="recipientDetails" maxlength="100" placeholder="name@example.com"></div>
        <div class="field"><label for="sendAmount">Amount (USD)</label><input id="sendAmount" type="number" min=".01" max="1000000" step=".01" required placeholder="0.00"></div>
        <div class="field"><label for="sendFrom">From account</label><select id="sendFrom"><option value="checking">Checking ••4821</option><option value="savings">Savings ••9064</option></select></div>
        <div class="field full"><label for="sendNote">Memo (optional)</label><input id="sendNote" maxlength="100" placeholder="What's this for?"></div>
        <div class="field full"><div class="form-note">Local simulation only. No bank account is connected and no real money is transferred.</div></div>
        <div class="field full"><button class="btn" type="submit">Record simulated transfer →</button></div>
      </form>
    </div>
    <div class="card">
      <h2>Available balances</h2>
      <div class="setting-row"><div><div class="setting-title">Checking</div><div class="setting-desc">Account ••4821</div></div><strong id="sendCheckingBalance">$185,198.62</strong></div>
      <div class="setting-row"><div><div class="setting-title">Savings</div><div class="setting-desc">Account ••9064</div></div><strong id="sendSavingsBalance">$100,000.00</strong></div>
      <div class="notice" style="margin-top:18px">Transfers only change the balances shown in this page.</div>
    </div>
  </div>
</section>

<section class="page" id="page-receive">
  <div class="heading"><div><h1>Receive money</h1><div class="subtitle">Record a simulated incoming payment.</div></div></div>
  <div class="grid two">
    <div class="card">
      <div class="section-head"><h2>Incoming payment</h2></div>
      <form id="receiveForm" class="form-grid">
        <div class="field full"><label for="sender">Sender name</label><input id="sender" required maxlength="70" placeholder="Enter sender name"></div>
        <div class="field"><label for="receiveAmount">Amount (USD)</label><input id="receiveAmount" type="number" min=".01" max="1000000" step=".01" required placeholder="0.00"></div>
        <div class="field"><label for="receiveTo">Deposit into</label><select id="receiveTo"><option value="checking">Checking ••4821</option><option value="savings">Savings ••9064</option></select></div>
        <div class="field full"><label for="receiveNote">Memo (optional)</label><input id="receiveNote" maxlength="100" placeholder="Payment description"></div>
        <div class="field full"><div class="form-note">This records a fictional incoming payment in the local interface only.</div></div>
        <div class="field full"><button class="btn" type="submit">Record incoming payment →</button></div>
      </form>
    </div>
    <div class="card">
      <h2>Wallet details</h2>
      <div class="setting-row"><div><div class="setting-title">Wallet reference</div><div class="setting-desc">Fictional local reference</div></div><strong>WLT-4821</strong></div>
      <div class="setting-row"><div><div class="setting-title">Currency</div><div class="setting-desc">US dollars</div></div><strong>USD</strong></div>
      <div class="notice" style="margin-top:18px">These are not real bank transfer instructions and cannot receive actual payments.</div>
    </div>
  </div>
</section>

<section class="page" id="page-cards">
  <div class="heading"><div><h1>Cards</h1><div class="subtitle">Manage your wallet's visual card settings.</div></div></div>
  <div class="grid two">
    <div class="card">
      <div class="card-visual" id="visualCard">
        <div class="card-brand"><span>CHASE WALLET</span><span>◉</span></div>
        <div class="card-chip">▦</div>
        <div class="card-number">•••• &nbsp; •••• &nbsp; •••• &nbsp; 4821</div>
        <div class="card-footer"><span>WALLET HOLDER</span><span>VIRTUAL DISPLAY</span></div>
      </div>
      <div class="setting-row"><div><div class="setting-title">Visual wallet card</div><div class="setting-desc">Display-only card; not a payment card</div></div><span class="pill">Active</span></div>
      <div class="notice" style="margin-top:14px">This visual card cannot make purchases or access a bank account.</div>
    </div>
    <div class="card">
      <h2>Card controls</h2>
      <div class="setting-row"><div><div class="setting-title">Show card</div><div class="setting-desc">Display card on this page</div></div><label class="switch"><input type="checkbox" checked onchange="toggleCardDisplay(this.checked)"><span class="slider"></span></label></div>
      <div class="setting-row"><div><div class="setting-title">Online purchases</div><div class="setting-desc">Visual preference only</div></div><label class="switch"><input type="checkbox" checked><span class="slider"></span></label></div>
      <div class="setting-row"><div><div class="setting-title">Contactless</div><div class="setting-desc">Visual preference only</div></div><label class="switch"><input type="checkbox" checked><span class="slider"></span></label></div>
    </div>
  </div>
</section>

<section class="page" id="page-settings">
  <div class="heading"><div><h1>Settings</h1><div class="subtitle">Personalize the wallet interface.</div></div></div>
  <div class="grid two">
    <div class="card">
      <h2>Preferences</h2>
      <div class="setting-row"><div><div class="setting-title">Email notifications</div><div class="setting-desc">Display preference</div></div><label class="switch"><input type="checkbox" checked><span class="slider"></span></label></div>
      <div class="setting-row"><div><div class="setting-title">Activity alerts</div><div class="setting-desc">Show a message when activity is recorded</div></div><label class="switch"><input type="checkbox" id="alertsEnabled" checked><span class="slider"></span></label></div>
      <div class="setting-row"><div><div class="setting-title">Currency</div><div class="setting-desc">Display currency</div></div><strong>USD ($)</strong></div>
    </div>
    <div class="card">
      <h2>Wallet information</h2>
      <div class="setting-row"><div><div class="setting-title">Wallet name</div><div class="setting-desc">Interface title</div></div><strong>Chase Wallet</strong></div>
      <div class="setting-row"><div><div class="setting-title">Initial balance</div><div class="setting-desc">Fictional starting amount</div></div><strong>$285,198.62</strong></div>
      <div class="setting-row"><div><div class="setting-title">Storage</div><div class="setting-desc">Balances reset when page reloads</div></div><strong>Local</strong></div>
      <button class="btn btn-danger" style="margin-top:18px" onclick="resetWallet()">Reset wallet</button>
    </div>
  </div>
</section>
</main>
</div>

<div class="watermark">SIMULATION</div>
<div class="toast" id="toast" role="status" aria-live="polite"></div>

<script>
/* ACCESS GATE: casual client-side access only, not secure authentication. */
const validAccessKeys = new Set([
"CHASE-484957","CHASE-729184","CHASE-315862","CHASE-906473","CHASE-582019",
"CHASE-147638","CHASE-863205","CHASE-294751","CHASE-671492","CHASE-438026",
"CHASE-915284","CHASE-362817","CHASE-750193","CHASE-284609","CHASE-593741",
"CHASE-826350","CHASE-174926","CHASE-640582","CHASE-357104","CHASE-982613",
"CHASE-461825","CHASE-738290","CHASE-205467","CHASE-894136","CHASE-516702",
"CHASE-329845","CHASE-670318","CHASE-142579","CHASE-953861","CHASE-487203",
"CHASE-716954","CHASE-268431","CHASE-839572","CHASE-504186","CHASE-193728",
"CHASE-625940","CHASE-371586","CHASE-948217","CHASE-256803","CHASE-780462",
"CHASE-413695","CHASE-862741","CHASE-597320","CHASE-134857","CHASE-925604",
"CHASE-348162","CHASE-701539","CHASE-486291","CHASE-259714","CHASE-674803"
]);

document.getElementById("keyForm").addEventListener("submit", function(event) {
  event.preventDefault();
  const input = document.getElementById("accessKey");
  const key = input.value.trim().toUpperCase();
  if (!validAccessKeys.has(key)) {
    document.getElementById("keyError").textContent = "Invalid access key. Please try again.";
    input.value = "";
    input.focus();
    return;
  }
  document.getElementById("keyError").textContent = "";
  document.getElementById("keyGate").remove();
});

/* WALLET */
const startingChecking = 185198.62;
const startingSavings = 100000.00;
let checking = startingChecking;
let savings = startingSavings;
let sentTotal = 0;
let receivedTotal = 0;
let transactions = [];
let toastTimer;

const money = amount => amount.toLocaleString("en-US", {
  style:"currency", currency:"USD"
});
const getAccount = account => account === "savings" ? savings : checking;
function setAccount(account, amount) {
  if (account === "savings") savings = amount;
  else checking = amount;
}
function accountName(account) {
  return account === "savings" ? "Savings ••9064" : "Checking ••4821";
}
function escapeHTML(value) {
  return String(value).replace(/[&<>"']/g, char => ({
    "&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"
  }[char]));
}
function showPage(name) {
  document.querySelectorAll(".page").forEach(p => p.classList.remove("active"));
  document.querySelectorAll(".nav-btn").forEach(b => b.classList.toggle("active", b.dataset.page === name));
  const page = document.getElementById("page-" + name);
  if (page) page.classList.add("active");
  document.getElementById("sidebar").classList.remove("open");
  window.scrollTo({top:0,behavior:"smooth"});
}
function toggleMenu() {
  document.getElementById("sidebar").classList.toggle("open");
}
function notify(message) {
  const alerts = document.getElementById("alertsEnabled");
  if (alerts && !alerts.checked) return;
  const toast = document.getElementById("toast");
  toast.textContent = message;
  toast.style.display = "block";
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => toast.style.display = "none", 3200);
}
function renderBalances() {
  const values = {
    mainBalance:checking+savings,
    checkingMetric:checking,
    savingsMetric:savings,
    checkingAccountBalance:checking,
    savingsAccountBalance:savings,
    combinedAccountBalance:checking+savings,
    sendCheckingBalance:checking,
    sendSavingsBalance:savings
  };
  Object.entries(values).forEach(([id,value]) => {
    const el = document.getElementById(id);
    if (el) el.textContent = money(value);
  });
  document.getElementById("receivedMetric").textContent = money(receivedTotal);
  document.getElementById("sentMetric").textContent = money(sentTotal);
  document.getElementById("countMetric").textContent = transactions.length;
}
function transactionRow(tx) {
  const incoming = tx.type === "received";
  return `<tr>
    <td><div class="tx-title"><div class="tx-icon">${incoming?"↙":"↗"}</div>
    <div><strong>${escapeHTML(tx.name)}</strong><div class="tx-sub">${escapeHTML(tx.note || (incoming?"Incoming payment":"Outgoing transfer"))}</div></div></div></td>
    <td>${escapeHTML(tx.date)}</td>
    <td>${escapeHTML(accountName(tx.account))}</td>
    <td class="${incoming?"positive":"negative"}">${incoming?"+":"−"}${money(tx.amount)}</td>
    <td><span class="status">Recorded locally</span></td>
  </tr>`;
}
function renderTransactions() {
  const filter = document.getElementById("transactionFilter")?.value || "all";
  const filtered = transactions.filter(tx => filter === "all" || tx.type === filter);
  document.getElementById("allTransactions").innerHTML = filtered.map(transactionRow).join("");
  document.getElementById("emptyTransactions").style.display = filtered.length ? "none" : "block";
  document.getElementById("recentTransactions").innerHTML =
    transactions.slice(0,5).map(transactionRow).join("") ||
    '<tr><td colspan="5" class="empty">No activity recorded yet.</td></tr>';
}
function render() {
  renderBalances();
  renderTransactions();
}
function openSendFor(account) {
  document.getElementById("sendFrom").value = account;
  showPage("send");
}
document.getElementById("sendForm").addEventListener("submit", event => {
  event.preventDefault();
  const name = document.getElementById("recipient").value.trim();
  const amount = Math.round(Number(document.getElementById("sendAmount").value)*100)/100;
  const account = document.getElementById("sendFrom").value;
  const note = document.getElementById("sendNote").value.trim();
  if (!name || !Number.isFinite(amount) || amount <= 0) {
    notify("Enter a valid recipient and amount."); return;
  }
  if (amount > getAccount(account)) {
    notify("Insufficient displayed balance in that account."); return;
  }
  setAccount(account,getAccount(account)-amount);
  sentTotal += amount;
  transactions.unshift({type:"sent",name,amount,account,note,date:new Date().toLocaleDateString("en-US")});
  event.target.reset();
  render();
  notify("Simulated transfer recorded. No real money was sent.");
  showPage("transactions");
});
document.getElementById("receiveForm").addEventListener("submit", event => {
  event.preventDefault();
  const name = document.getElementById("sender").value.trim();
  const amount = Math.round(Number(document.getElementById("receiveAmount").value)*100)/100;
  const account = document.getElementById("receiveTo").value;
  const note = document.getElementById("receiveNote").value.trim();
  if (!name || !Number.isFinite(amount) || amount <= 0) {
    notify("Enter a valid sender and amount."); return;
  }
  setAccount(account,getAccount(account)+amount);
  receivedTotal += amount;
  transactions.unshift({type:"received",name,amount,account,note,date:new Date().toLocaleDateString("en-US")});
  event.target.reset();
  render();
  notify("Simulated incoming payment recorded.");
  showPage("transactions");
});
function clearTransactions() {
  if (!transactions.length) { notify("There is no activity to clear."); return; }
  if (!confirm("Clear all recorded activity? Balances will remain unchanged.")) return;
  transactions = [];
  sentTotal = 0;
  receivedTotal = 0;
  render();
  notify("Activity cleared.");
}
function resetWallet() {
  if (!confirm("Reset balances and clear all session activity?")) return;
  checking = startingChecking;
  savings = startingSavings;
  sentTotal = 0;
  receivedTotal = 0;
  transactions = [];
  render();
  notify("Wallet reset to starting balances.");
  showPage("dashboard");
}
function toggleCardDisplay(visible) {
  document.getElementById("visualCard").style.display = visible ? "block" : "none";
}
render();
</script>
</body>
</html>
```
