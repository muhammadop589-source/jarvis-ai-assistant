# jarvis-ai-assistant
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="theme-color" content="#4dc3ff">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="JARVIS">
<title>JARVIS — AI Assistant</title>
<style>
:root{
  --bg:#070a12; --panel:rgba(20,26,42,.72); --panel-solid:#141a2a;
  --border:rgba(120,180,255,.14); --accent:#4dc3ff; --accent2:#7CFC9A;
  --danger:#ff5b5b; --warn:#ffb84d; --text:#e6f0ff; --muted:#8296b3;
  --user:#4dc3ff; --jarvis:#7CFC9A;
}
*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%}
body{
  font-family:-apple-system,'Segoe UI',system-ui,sans-serif;
  background:var(--bg);color:var(--text);display:flex;flex-direction:column;
  overflow:hidden;background-image:
    radial-gradient(circle at 15% 20%,rgba(77,195,255,.12),transparent 45%),
    radial-gradient(circle at 85% 80%,rgba(124,252,154,.10),transparent 45%);
  -webkit-tap-highlight-color:transparent;
}
button{font-family:inherit;cursor:pointer;border:none;background:none;color:inherit}
input,select,textarea{font-family:inherit;outline:none}

/* header */
header{
  display:flex;align-items:center;gap:12px;padding:12px 18px;
  background:var(--panel);backdrop-filter:blur(14px);
  border-bottom:1px solid var(--border);z-index:10;flex-shrink:0;
}
.orb{
  width:34px;height:34px;border-radius:50%;flex-shrink:0;
  background:radial-gradient(circle at 30% 30%,#b6ecff,var(--accent) 45%,#1a6ea8 100%);
  box-shadow:0 0 16px rgba(77,195,255,.55),inset 0 0 12px rgba(255,255,255,.35);
  transition:transform .25s,background .25s;
}
.orb.listening{animation:pulse 1.2s infinite;background:radial-gradient(circle at 30% 30%,#ffd7d7,#ff5b5b 45%,#8a1f1f 100%)}
.orb.thinking{animation:spin 1.6s linear infinite}
@keyframes pulse{0%,100%{transform:scale(1);box-shadow:0 0 18px rgba(255,91,91,.6)}50%{transform:scale(1.15);box-shadow:0 0 34px rgba(255,91,91,.9)}}
@keyframes spin{to{transform:rotate(360deg)}}
header h1{font-size:16px;font-weight:600;letter-spacing:.4px;white-space:nowrap}
header h1 span{font-weight:300;color:var(--muted);font-size:12px;margin-left:4px}
header .status{
  font-size:11px;color:var(--muted);margin-left:auto;display:flex;gap:12px;align-items:center;
  white-space:nowrap;
}
header .status b{color:var(--accent2);font-weight:500}
header .status .dot{
  display:inline-block;width:7px;height:7px;border-radius:50%;
  background:#666;margin-right:4px;vertical-align:middle;
}
header .status .dot.on{background:var(--accent2);box-shadow:0 0 6px var(--accent2)}
header .status .dot.off{background:var(--danger)}
.hbtn{
  padding:8px 10px;border-radius:8px;border:1px solid var(--border);
  background:rgba(255,255,255,.02);font-size:14px;transition:.15s;
}
.hbtn:hover{background:rgba(77,195,255,.12);border-color:var(--accent)}
@media(max-width:600px){header h1 span{display:none}header .status .hide-sm{display:none}}

/* main */
main{flex:1;display:flex;overflow:hidden;position:relative}
#sidebar{
  width:290px;background:var(--panel);backdrop-filter:blur(14px);
  border-right:1px solid var(--border);display:flex;flex-direction:column;
  transition:width .2s,opacity .2s;overflow:hidden;flex-shrink:0;
}
#sidebar.collapsed{width:0;opacity:0;border-right:none}
.tabs{display:flex;padding:8px;gap:3px;border-bottom:1px solid var(--border);flex-shrink:0}
.tab{
  flex:1;padding:8px 4px;font-size:10px;text-align:center;
  border-radius:8px;color:var(--muted);user-select:none;transition:.15s;
  cursor:pointer;letter-spacing:.3px;
}
.tab:hover{background:rgba(255,255,255,.04);color:var(--text)}
.tab.active{background:rgba(77,195,255,.15);color:var(--accent);font-weight:600}
.panel{flex:1;overflow-y:auto;padding:12px;display:none}
.panel.active{display:block}
.panel h3{
  font-size:10px;text-transform:uppercase;color:var(--muted);
  letter-spacing:1.2px;margin-bottom:10px;font-weight:600;
}
.panel h3:not(:first-child){margin-top:20px}
.input-row{display:flex;gap:6px;margin-bottom:10px}
.input-row input{
  flex:1;background:rgba(0,0,0,.35);border:1px solid var(--border);
  color:var(--text);padding:8px 10px;border-radius:8px;font-size:12px;
}
.input-row input:focus{border-color:var(--accent)}
.input-row button{
  background:var(--accent);color:#001824;padding:0 12px;
  border-radius:8px;font-size:12px;font-weight:600;
}
.item{
  padding:8px 10px;background:rgba(255,255,255,.03);border:1px solid var(--border);
  border-radius:8px;font-size:12px;margin-bottom:6px;
  display:flex;justify-content:space-between;align-items:center;
  gap:8px;word-break:break-word;
}
.item small{color:var(--muted);font-size:10px;display:block;margin-top:2px}
.item .del{
  background:none;border:none;color:var(--muted);
  cursor:pointer;font-size:14px;padding:0 4px;flex-shrink:0;
}
.item .del:hover{color:var(--danger)}
.empty{color:var(--muted);font-size:12px;text-align:center;padding:20px 0;font-style:italic}

/* settings */
label.f{
  display:block;font-size:10px;color:var(--muted);
  margin:10px 0 4px;text-transform:uppercase;letter-spacing:.6px;font-weight:600;
}
input.f,select.f,textarea.f{
  width:100%;background:rgba(0,0,0,.35);border:1px solid var(--border);
  color:var(--text);padding:8px 10px;border-radius:8px;font-size:12px;
}
input.f:focus,select.f:focus,textarea.f:focus{border-color:var(--accent)}
textarea.f{resize:vertical;min-height:60px;font-family:ui-monospace,Consolas,monospace}
.btn{
  padding:9px 14px;border-radius:8px;background:var(--panel-solid);
  border:1px solid var(--border);color:var(--text);font-size:12px;
  transition:.15s;font-weight:500;
}
.btn:hover{background:rgba(77,195,255,.12);border-color:var(--accent)}
.btn.primary{background:var(--accent);color:#001824;border-color:var(--accent)}
.btn.primary:hover{background:#6fd0ff}
.btn.danger{color:var(--danger);border-color:rgba(255,91,91,.3)}
.btn.danger:hover{background:rgba(255,91,91,.15);border-color:var(--danger)}
.btn.warn{color:var(--warn);border-color:rgba(255,184,77,.3)}
.btn.warn:hover{background:rgba(255,184,77,.15);border-color:var(--warn)}
.btn.block{display:block;width:100%;text-align:center;margin-top:8px}
.btn-row{display:flex;gap:6px;margin-top:8px;flex-wrap:wrap}

/* stats grid */
.stats{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:10px}
.stat{
  padding:12px;background:rgba(77,195,255,.06);
  border:1px solid var(--border);border-radius:8px;
}
.stat .n{font-size:22px;font-weight:600;color:var(--accent);line-height:1.1}
.stat .l{font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:.5px;margin-top:4px}

/* log */
#adminLog{
  background:rgba(0,0,0,.4);border:1px solid var(--border);border-radius:8px;
  padding:8px;font-family:ui-monospace,Consolas,monospace;font-size:10px;
  max-height:220px;overflow-y:auto;line-height:1.5;
}
#adminLog .l{display:block;padding:2px 0;border-bottom:1px solid rgba(255,255,255,.03)}
#adminLog .t{color:var(--muted);margin-right:6px}
#adminLog .ok{color:var(--accent2)}
#adminLog .err{color:var(--danger)}
#adminLog .warn{color:var(--warn)}
#adminLog .info{color:var(--accent)}

/* chat */
#chatWrap{flex:1;display:flex;flex-direction:column;min-width:0;min-height:0}
#chat{
  flex:1;overflow-y:auto;padding:20px 22px;
  display:flex;flex-direction:column;gap:12px;scroll-behavior:smooth;
}
#chat::-webkit-scrollbar,#adminLog::-webkit-scrollbar,.panel::-webkit-scrollbar{width:8px}
#chat::-webkit-scrollbar-thumb,#adminLog::-webkit-scrollbar-thumb,.panel::-webkit-scrollbar-thumb{
  background:rgba(120,180,255,.2);border-radius:4px;
}
.msg{display:flex;gap:10px;max-width:82%;animation:fade .25s ease}
.msg.user{align-self:flex-end;flex-direction:row-reverse}
.msg .avatar{
  width:30px;height:30px;border-radius:50%;flex-shrink:0;
  display:flex;align-items:center;justify-content:center;
  font-size:12px;font-weight:700;
}
.msg.jarvis .avatar{background:linear-gradient(135deg,#0d7ab5,#4dc3ff);color:#001824}
.msg.user .avatar{background:linear-gradient(135deg,#3e8a5a,#7CFC9A);color:#001824}
.msg.system{
  align-self:center;max-width:70%;color:var(--muted);
  font-size:11px;font-style:italic;padding:4px 12px;
  background:rgba(255,255,255,.03);border-radius:20px;
}
.msg.system .avatar{display:none}
.bubble{
  padding:10px 14px;border-radius:14px;font-size:13.5px;
  line-height:1.5;background:var(--panel-solid);
  border:1px solid var(--border);word-wrap:break-word;
  white-space:pre-wrap;overflow-wrap:anywhere;
}
.msg.user .bubble{background:rgba(77,195,255,.14);border-color:rgba(77,195,255,.3)}
.msg.jarvis .bubble{background:rgba(124,252,154,.08);border-color:rgba(124,252,154,.2)}
.bubble .time{display:block;font-size:9px;color:var(--muted);margin-top:4px;opacity:.7}
@keyframes fade{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:none}}

/* input bar */
#inputBar{
  padding:12px 18px;background:var(--panel);backdrop-filter:blur(14px);
  border-top:1px solid var(--border);display:flex;gap:8px;
  align-items:center;flex-shrink:0;
}
#cmd{
  flex:1;background:rgba(0,0,0,.35);border:1px solid var(--border);
  color:var(--text);padding:11px 14px;border-radius:12px;
  font-size:13.5px;transition:.15s;
}
#cmd:focus{border-color:var(--accent);box-shadow:0 0 0 3px rgba(77,195,255,.12)}
.ibtn{
  background:var(--panel-solid);color:var(--text);
  border:1px solid var(--border);padding:11px 13px;
  border-radius:12px;font-size:14px;transition:.15s;
  display:flex;align-items:center;gap:6px;
}
.ibtn:hover{background:rgba(77,195,255,.12);border-color:var(--accent)}
.ibtn.active{background:rgba(255,91,91,.18);border-color:var(--danger);color:#ffb3b3}
.ibtn.primary{background:var(--accent);color:#001824;border-color:var(--accent);font-weight:600}
.ibtn.primary:hover{background:#6fd0ff}
@media(max-width:600px){
  #inputBar{padding:10px 12px;gap:6px}
  #cmd{padding:10px 12px;font-size:14px}
  .ibtn{padding:10px 11px}
  .ibtn .lbl{display:none}
}

/* toast */
#toasts{
  position:fixed;bottom:20px;right:20px;z-index:1000;
  display:flex;flex-direction:column;gap:8px;pointer-events:none;
}
.toast{
  padding:12px 18px;background:var(--panel-solid);
  border:1px solid var(--accent);border-radius:10px;
  font-size:13px;box-shadow:0 8px 24px rgba(0,0,0,.4);
  animation:slideIn .25s ease;max-width:340px;
}
.toast.err{border-color:var(--danger)}
.toast.warn{border-color:var(--warn)}
@keyframes slideIn{from{opacity:0;transform:translateX(20px)}to{opacity:1;transform:none}}

/* modal */
#modal{
  position:fixed;inset:0;background:rgba(0,0,0,.7);
  backdrop-filter:blur(4px);z-index:999;
  display:none;align-items:center;justify-content:center;padding:20px;
}
#modal.on{display:flex}
#modalBox{
  background:var(--panel-solid);border:1px solid var(--border);
  border-radius:14px;padding:24px;max-width:440px;width:100%;
  box-shadow:0 20px 60px rgba(0,0,0,.6);
}
#modalBox h2{font-size:16px;margin-bottom:10px}
#modalBox p{font-size:13px;color:var(--muted);margin-bottom:18px;line-height:1.5}
#modalBox .btns{display:flex;gap:8px;justify-content:flex-end}

/* mobile sidebar */
@media(max-width:700px){
  #sidebar{
    position:absolute;top:0;bottom:0;left:0;z-index:30;
    width:280px;box-shadow:4px 0 20px rgba(0,0,0,.4);
  }
  #sidebar.collapsed{width:0;opacity:0}
}
</style>
</head>
<body>

<header>
  <div class="orb" id="orb"></div>
  <h1>JARVIS<span>v3</span></h1>
  <div class="status">
    <span><span class="dot" id="statusDot"></span><span id="statusText">checking</span></span>
    <span class="hide-sm">AI: <b id="aiStatus">off</b></span>
  </div>
  <button class="hbtn" id="btnInstall" style="display:none" title="Install">⬇</button>
  <button class="hbtn" id="btnSidebar" title="Menu">☰</button>
</header>

<main>
  <aside id="sidebar">
    <div class="tabs">
      <div class="tab active" data-tab="chat">💬 Chat</div>
      <div class="tab" data-tab="notes">📝</div>
      <div class="tab" data-tab="tasks">✅</div>
      <div class="tab" data-tab="memory">🧠</div>
      <div class="tab" data-tab="setup">⚙️</div>
      <div class="tab" data-tab="admin">🔐</div>
    </div>

    <!-- NOTES -->
    <div class="panel active" id="panel-notes">
      <h3>Notes</h3>
      <div class="input-row">
        <input id="noteInput" placeholder="New note...">
        <button onclick="Notes.add()">+</button>
      </div>
      <div id="notesList"></div>
    </div>

    <!-- TASKS -->
    <div class="panel" id="panel-tasks">
      <h3>Tasks</h3>
      <div class="input-row">
        <input id="todoInput" placeholder="New task...">
        <button onclick="Tasks.add()">+</button>
      </div>
      <div id="todosList"></div>
    </div>

    <!-- MEMORY -->
    <div class="panel" id="panel-memory">
      <h3>Long-term memory</h3>
      <div id="memoryList"></div>
    </div>

    <!-- SETUP -->
    <div class="panel" id="panel-setup">
      <h3>Quick setup</h3>
      <label class="f">Your name</label>
      <input class="f" id="setUserName" placeholder="What should I call you?">
      <label class="f">Assistant name</label>
      <input class="f" id="setBotName" value="Jarvis">
      <label class="f">Wake word</label>
      <input class="f" id="setWakeWord" value="jarvis">
      <label class="f">Recognition language</label>
      <select class="f" id="setLang">
        <option value="en-US">English (US)</option>
        <option value="en-IN">English (India)</option>
        <option value="en-GB">English (UK)</option>
        <option value="hi-IN">हिन्दी</option>
        <option value="es-ES">Español</option>
        <option value="fr-FR">Français</option>
        <option value="de-DE">Deutsch</option>
        <option value="ja-JP">日本語</option>
        <option value="zh-CN">中文</option>
        <option value="ar-SA">العربية</option>
        <option value="pt-BR">Português</option>
        <option value="ru-RU">Русский</option>
      </select>
      <label class="f">Voice output</label>
      <select class="f" id="setVoice">
        <option value="on">Enabled</option>
        <option value="off">Disabled</option>
      </select>
      <button class="btn primary block" onclick="Settings.saveBasic()">Save</button>
      <p style="font-size:11px;color:var(--muted);margin-top:12px;line-height:1.5">
        For full control open the <b>Admin</b> tab.
      </p>
    </div>

    <!-- ADMIN -->
    <div class="panel" id="panel-admin">
      <h3>Gemini API</h3>
      <label class="f">API key</label>
      <input class="f" id="admKey" type="password" placeholder="AIza...">
      <label class="f">Model</label>
      <select class="f" id="admModel">
        <option value="gemini-2.0-flash">gemini-2.0-flash</option>
        <option value="gemini-2.0-flash-lite">gemini-2.0-flash-lite</option>
        <option value="gemini-1.5-flash">gemini-1.5-flash</option>
        <option value="gemini-1.5-pro">gemini-1.5-pro</option>
      </select>
      <label class="f">Default city (weather)</label>
      <input class="f" id="admCity" value="Delhi">
      <label class="f">System prompt</label>
      <textarea class="f" id="admSysPrompt" rows="3"></textarea>
      <button class="btn primary block" onclick="Admin.saveAI()">Save AI config</button>
      <button class="btn block" onclick="Admin.testAI()">Test connection</button>

      <h3>Statistics</h3>
      <div class="stats">
        <div class="stat"><div class="n" id="stNotes">0</div><div class="l">Notes</div></div>
        <div class="stat"><div class="n" id="stTasks">0</div><div class="l">Tasks</div></div>
        <div class="stat"><div class="n" id="stMemory">0</div><div class="l">Memories</div></div>
        <div class="stat"><div class="n" id="stChat">0</div><div class="l">Messages</div></div>
      </div>

      <h3>Data</h3>
      <button class="btn block" onclick="Admin.exportData()">⬇ Export as JSON</button>
      <button class="btn block" onclick="document.getElementById('importFile').click()">⬆ Import from JSON</button>
      <input type="file" id="importFile" accept=".json" style="display:none" onchange="Admin.importData(event)">
      <button class="btn warn block" onclick="Admin.clearChat()">🗑 Clear chat history</button>

      <h3>Danger zone</h3>
      <button class="btn danger block" onclick="Admin.wipeAll()">⚠ Wipe everything</button>

      <h3>Activity log</h3>
      <div id="adminLog"></div>
      <button class="btn block" style="margin-top:8px" onclick="Admin.clearLog()">Clear log</button>

      <h3>System</h3>
      <div style="font-size:11px;color:var(--muted);line-height:1.8">
        <div>Version: <b style="color:var(--text)">3.0.0</b></div>
        <div>Mode: <b style="color:var(--text)" id="sysMode">standalone</b></div>
        <div>Storage: <b style="color:var(--text)" id="sysStorage">–</b></div>
        <div>Browser: <b style="color:var(--text)" id="sysBrowser">–</b></div>
        <div>PWA: <b style="color:var(--text)" id="sysPWA">no</b></div>
      </div>
    </div>
  </aside>

  <section id="chatWrap">
    <div id="chat"></div>
    <div id="inputBar">
      <input id="cmd" placeholder="Ask anything… (Ctrl+M to speak)" autocomplete="off" spellcheck="false">
      <button class="ibtn" id="btnMic" title="Voice input"><span>🎤</span><span class="lbl">Mic</span></button>
      <button class="ibtn primary" id="btnSend" title="Send"><span>➤</span></button>
    </div>
  </section>
</main>

<div id="toasts"></div>

<div id="modal">
  <div id="modalBox">
    <h2 id="modalTitle">Confirm</h2>
    <p id="modalText"></p>
    <div class="btns">
      <button class="btn" onclick="Modal.close()">Cancel</button>
      <button class="btn danger" id="modalOk">Confirm</button>
    </div>
  </div>
</div>

<script>
/* ==========================================================
   STORAGE
========================================================== */
const Store = {
  prefix: 'jarvis_v3_',
  get(k, d){
    try{ const v = localStorage.getItem(this.prefix + k);
      return v === null ? d : JSON.parse(v); }
    catch{ return d; }
  },
  set(k, v){
    try{ localStorage.setItem(this.prefix + k, JSON.stringify(v)); return true; }
    catch(e){ Toast.show('Storage full', 'err'); return false; }
  },
  del(k){ try{ localStorage.removeItem(this.prefix + k); }catch{} },
  size(){
    let n = 0;
    for (const k in localStorage) if (k.startsWith(this.prefix)) n += (localStorage[k] || '').length;
    return n;
  }
};

/* ==========================================================
   APP STATE
========================================================== */
const App = {
  notes:    Store.get('notes', []),
  todos:    Store.get('todos', []),
  memory:   Store.get('memory', {}),
  chat:     Store.get('chat', []),
  history:  [],
  histIdx:  0,
  log:      [],
  config:   Store.get('config', {
    userName: '',
    botName: 'Jarvis',
    wakeWord: 'jarvis',
    lang: 'en-US',
    voice: 'on',
    apiKey: '',
    model: 'gemini-2.0-flash',
    city: 'Delhi',
    systemPrompt: 'You are JARVIS, a witty, helpful AI assistant. Answer concisely in 2-3 sentences, plain text, no markdown.'
  }),
  thinking: false,
  listening: false,
  recognition: null,
  save(){ Store.set('config', this.config); },
  saveChat(){ Store.set('chat', this.chat); },
  saveNotes(){ Store.set('notes', this.notes); },
  saveTodos(){ Store.set('todos', this.todos); },
  saveMemory(){ Store.set('memory', this.memory); }
};

/* ==========================================================
   LOGGING
========================================================== */
function log(type, msg){
  const entry = { t: Date.now(), type, msg: String(msg) };
  App.log.push(entry);
  if (App.log.length > 300) App.log.shift();
  const el = document.getElementById('adminLog');
  if (el){
    const div = document.createElement('span');
    div.className = 'l ' + type;
    div.innerHTML = `<span class="t">${new Date(entry.t).toLocaleTimeString()}</span>${esc(entry.msg)}`;
    el.appendChi
