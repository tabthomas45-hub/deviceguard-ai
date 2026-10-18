
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<meta name="theme-color" content="#101b30">
<title>DeviceGuard AI</title>
<style>
:root{color-scheme:dark;--bg:#0b1220;--panel:#131f33;--line:#293952;--blue:#62d6f5;--text:#edf4ff;--muted:#a5b4cc}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--text);font:15px system-ui,Arial}
header{padding:22px 16px;background:#101b30;border-bottom:1px solid var(--line)}
h1{margin:0;font-size:25px}h2{font-size:19px;margin-top:0}
p{line-height:1.55}.muted,small{color:var(--muted)}
main{max-width:1000px;margin:auto;padding:16px}
nav{display:flex;gap:8px;overflow:auto;padding:12px 0}
button,a.btn{border:1px solid var(--line);border-radius:10px;padding:10px 13px;background:#1a2b43;color:var(--text);cursor:pointer;text-decoration:none}
button:hover,a.btn:hover{border-color:var(--blue)}
.primary{background:#075a79;color:white;border-color:#1685a9}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(145px,1fr));gap:10px}
.card{background:var(--panel);border:1px solid var(--line);border-radius:14px;padding:15px;margin-bottom:12px;min-width:0}
.stat{font-size:24px;font-weight:700;color:var(--blue)}
input,textarea,select{width:100%;padding:12px;margin:7px 0 12px;background:#0b1220;color:white;border:1px solid var(--line);border-radius:8px}
label{display:block;color:var(--muted)}
section{scroll-margin-top:10px}
pre{white-space:pre-wrap;overflow-wrap:anywhere;background:#0b1220;padding:12px;border-radius:8px}
footer{text-align:center;color:var(--muted);padding:25px}
.hidden{display:none}.ok{color:#70e0a0}.warn{color:#ffd080}
</style>
</head>
<body>
<header>
  <h1>🛡️ DeviceGuard AI</h1>
  <p class="muted">Advanced IT • Device safety • Cybersecurity learning</p>
  <small>Version 0.1 · Local demo · No account connected</small>
</header>
<main>
<nav aria-label="Main navigation">
<button onclick="show('home')">Home</button>
<button onclick="show('devices')">Devices</button>
<button onclick="show('gps')">GPS</button>
<button onclick="show('security')">Security</button>
<button onclick="show('labs')">IT Labs</button>
<button onclick="show('reports')">Reports</button>
</nav>

<section id="home">
<div class="card">
<h2>Welcome to your IT command centre</h2>
<p>Learn, test and manage your own device records from one dashboard.</p>
<div class="grid">
<div class="card"><small>Registered devices</small><div class="stat" id="deviceCount">0</div></div>
<div class="card"><small>Saved reports</small><div class="stat" id="reportCount">0</div></div>
<div class="card"><small>Training score</small><div class="stat" id="score">0</div></div>
</div>
<p class="warn">Demo mode: records stay in this browser. No remote tracking is active.</p>
</div>
<div class="card">
<h2>All ten IT areas</h2>
<div class="grid" id="areas"></div>
</div>
</section>

<section id="devices" class="hidden">
<div class="card">
<h2>Device registry</h2>
<label>Device nickname</label><input id="nickname" placeholder="e.g. My Infinix HOT 60">
<button class="primary" onclick="addDevice()">Register this device</button>
<div id="deviceList"></div>
<p class="muted">IDs are app-generated identifiers, not IMEI numbers. This demo does not verify physical device identity.</p>
</div>
</section>

<section id="gps" class="hidden">
<div class="card">
<h2>Authorized GPS location</h2>
<p>Press the button to request your browser's location permission. Location is displayed here and is not uploaded.</p>
<button class="primary" onclick="locate()">Get my location</button>
<div id="locationResult" aria-live="polite">No location requested yet.</div>
</div>
</section>

<section id="security" class="hidden">
<div class="card">
<h2>Security learning lab</h2>
<p>Practice identifying suspicious activity using sample events. These are fictional logs, not a scan of your phone.</p>
<button onclick="analyze()">Analyze sample alerts</button>
<pre id="alerts">No analysis run yet.</pre>
</div>
<div class="card">
<h2>Cybersecurity quiz</h2>
<p id="question"></p>
<div id="answers" class="grid"></div>
<p id="feedback" aria-live="polite"></p>
<button onclick="nextQuestion()">Next question</button>
</div>
</section>

<section id="labs" class="hidden">
<div class="card">
<h2>Networking lab</h2>
<p>IP addresses identify network interfaces. DNS helps translate domain names into IP addresses. HTTPS encrypts web traffic between a client and server.</p>
<label>Example IPv4 address</label><input id="ip" value="192.168.1.10">
<button onclick="checkIP()">Validate example</button>
<p id="ipResult"></p>
</div>
<div class="card">
<h2>IoT simulator</h2>
<p>Toggle fictional devices to learn how a dashboard might monitor sensors. Nothing physical is controlled.</p>
<div id="iot"></div>
</div>
<div class="card">
<h2>App development and cloud</h2>
<p>This app uses HTML for structure, CSS for design and JavaScript for interactions. GitHub Pages can host static files; a backend will be needed for authenticated cloud records.</p>
<p><a class="btn" href="https://developer.mozilla.org/en-US/docs/Learn_web_development" target="_blank" rel="noopener">Free web development lessons</a></p>
</div>
<div class="card">
<h2>Database and forensics lab</h2>
<p>Review the sample incident below and practice recording what happened, when it happened, and what evidence needs preservation.</p>
<pre>Sample incident
Time: 09:42 UTC
Event: Repeated failed sign-in attempts
Source: Simulated training log
Action: Verify the account owner, review logs,
and enable stronger authentication if needed.</pre>
</div>
<div class="card">
<h2>AI alert explainer (rule-based demo)</h2>
<label>Paste a harmless sample log</label>
<textarea id="sampleLog" rows="3" placeholder="e.g. 8 failed login attempts"></textarea>
<button onclick="explainLog()">Explain sample</button>
<p id="explanation"></p>
<p class="muted">This uses simple rules, not a trained AI model.</p>
</div>
</section>

<section id="reports" class="hidden">
<div class="card">
<h2>Lost-device and incident reports</h2>
<label>Report type</label>
<select id="reportType"><option>Lost device</option><option>Suspicious activity</option><option>Security incident</option></select>
<label>Device ID or nickname (optional)</label>
<input id="reportDevice" placeholder="Your registered device">
<label>Details</label>
<textarea id="reportDetails" rows="4" placeholder="Describe the incident. Avoid passwords and secret keys."></textarea>
<button class="primary" onclick="saveReport()">Save report on this browser</button>
<div id="reportList"></div>
<p class="muted">For a stolen phone, use the manufacturer's official recovery tools and contact local authorities as appropriate. This form does not contact anyone.</p>
</div>
</section>
</main>
<footer>DeviceGuard AI · Learn safely. Protect privacy. Test only authorized systems.</footer>

<script>
const $=id=>document.getElementById(id);
const read=k=>{try{return JSON.parse(localStorage.getItem(k)||"[]")}catch{return []}};
const write=(k,v)=>localStorage.setItem(k,JSON.stringify(v));
const areas=[
["Cybersecurity","Security concepts and safe labs"],
["GPS & recovery","Permission-based location"],
["Networking","IP, DNS, Wi-Fi and routers"],
["App development","HTML, CSS and JavaScript"],
["Cloud & DevOps","Hosting and deployment"],
["Digital forensics","Incident evidence and logs"],
["AI","Rule-based alert explanations"],
["Databases","Structured local records"],
["IoT","Simulated sensors and devices"],
["UI/UX","Mobile-friendly design"]
];
$("areas").innerHTML=areas.map(a=>`<div class="card"><b>${a[0]}</b><p class="muted">${a[1]}</p></div>`).join("");
function show(id){document.querySelectorAll("main section").forEach(s=>s.classList.add("hidden"));$(id).classList.remove("hidden");window.scrollTo(0,0)}
function addDevice(){
 let name=$("nickname").value.trim();if(!name)return alert("Enter a nickname first.");
 let d=read("dgDevices");d.push({id:"DG-"+crypto.randomUUID().slice(0,8).toUpperCase(),name,time:new Date().toLocaleString()});
 write("dgDevices",d);$("nickname").value="";render();
}
function render(){
 let d=read("dgDevices"),r=read("dgReports");
 $("deviceCount").textContent=d.length;$("reportCount").textContent=r.length;
 $("deviceList").innerHTML=d.map(x=>`<div class="card"><b>${esc(x.name)}</b><p>${esc(x.id)}</p><small>${esc(x.time)}</small></div>`).join("")||"<p>No devices registered yet.</p>";
 $("reportList").innerHTML=r.map(x=>`<div class="card"><b>${esc(x.type)}</b><p>${esc(x.details)}</p><small>${esc(x.device)} · ${esc(x.time)}</small></div>`).join("")||"<p>No reports saved.</p>";
}
function esc(s){return String(s).replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]))}
function locate(){
 $("locationResult").textContent="Waiting for permission and location…";
 if(!navigator.geolocation){$("locationResult").textContent="Geolocation is not supported by this browser.";return}
 navigator.geolocation.getCurrentPosition(p=>{
 const lat=p.coords.latitude,lon=p.coords.longitude,acc=Math.round(p.coords.accuracy);
 $("locationResult").innerHTML=`<p class="ok">Location received.</p><p>Latitude: ${lat}<br>Longitude: ${lon}<br>Reported accuracy: about ${acc} metres<br>Time: ${new Date(p.timestamp).toLocaleString()}</p><a class="btn" target="_blank" rel="noopener" href="https://www.openstreetmap.org/?mlat=${lat}&mlon=${lon}#map=16/${lat}/${lon}">View map</a><p class="muted">Coordinates remain in this page unless you choose to share them.</p>`;
 },e=>{$("locationResult").textContent="Location unavailable: "+e.message}, {enableHighAccuracy:true,timeout:20000,maximumAge:0});
}
function analyze(){
 $("alerts").textContent="1. Repeated failed sign-ins: review account logs and enable MFA.\n2. Unexpected software installation: verify the source and scan with trusted security software.\n3. Unknown network device: check your own router's connected-device list.\n\nThese are fictional training examples; no real device was scanned.";
}
const qs=[
{q:"What should you do before testing a network?",a:["Get explicit authorization","Hide your identity","Try every password"],c:0,e:"Only test systems you own or have permission to assess."},
{q:"What does DNS mainly do?",a:["Charge a battery","Resolve domain names","Track a stolen phone secretly"],c:1,e:"DNS helps find the IP address associated with a domain name."},
{q:"What does GPS accuracy in metres mean?",a:["An estimated uncertainty radius","The phone's serial number","A guaranteed exact position"],c:0,e:"Accuracy is an estimate, not a guarantee."},
{q:"Which is safer for accounts?",a:["Reuse one password","Share passwords","Use unique passwords and MFA"],c:2,e:"Unique passwords and multi-factor authentication reduce account risk."}
];
let qi=0,answered=false;
function drawQ(){
 answered=false;let q=qs[qi%qs.length];$("question").textContent=q.q;
 $("answers").innerHTML=q.a.map((a,i)=>`<button onclick="answer(${i})">${a}</button>`).join("");$("feedback").textContent="";
}
function answer(i){
 if(answered)return;answered=true;let q=qs[qi%qs.length],ok=i===q.c;
 if(ok){let n=Number(localStorage.getItem("dgScore")||0)+1;localStorage.setItem("dgScore",n);$("score").textContent=n}
 $("feedback").textContent=(ok?"Correct! ":"Not quite. ")+q.e;
}
function nextQuestion(){qi++;drawQ()}
function checkIP(){
 let s=$("ip").value.trim(),p=s.split("."),ok=p.length===4&&p.every(n=>/^\d+$/.test(n)&&+n>=0&&+n<=255);
 $("ipResult").textContent=ok?"Valid IPv4 format (not proof that the address is reachable).":"Invalid IPv4 format.";
}
const iot=[["Temperature sensor",true],["Door sensor",false],["Demo smart light",true]];
$("iot").innerHTML=iot.map((x,i)=>`<p><label><input type="checkbox" style="width:auto" ${x[1]?"checked":""} onchange="iotState(${i},this.checked)"> ${x[0]} — simulated only</label></p>`).join("");
function iotState(i,v){$("iot").querySelectorAll("p")[i].lastChild.textContent=" "+iot[i][0]+" — "+(v?"ON":"OFF")+" (simulated only)"}
function explainLog(){
 let t=$("sampleLog").value.toLowerCase(),msg="No obvious pattern matched. Review the log manually and preserve its context.";
 if(t.includes("failed")||t.includes("login"))msg="Possible authentication issue. Check timestamps, account activity and authorized access; do not assume an attack from one log alone.";
 if(t.includes("malware")||t.includes("virus"))msg="Possible malware indicator. Avoid opening suspicious files, preserve relevant evidence and use trusted security tools.";
 if(t.includes("unknown device")||t.includes("new device"))msg="Verify the device through your own router or account security settings before deciding whether it is unauthorized.";
 $("explanation").textContent=msg;
}
function saveReport(){
 let details=$("reportDetails").value.trim();if(!details)return alert("Describe the report first.");
 let r=read("dgReports");r.unshift({type:$("reportType").value,device:$("reportDevice").value.trim()||"Not specified",details,time:new Date().toLocaleString()});
 write("dgReports",r);$("reportDetails").value="";render();alert("Saved locally in this browser.");
}
$("score").textContent=localStorage.getItem("dgScore")||"0";
render();drawQ();
</script>
</body>
</html>
# deviceguard-ai
DeviceGuard AI — Advanced IT learning and device safety
