<!doctype html>
<html lang="ko">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="theme-color" content="#2d211c">
<title>AI름다운 가을 | Autumn Interest</title>
<style>
:root{
  --bg:#f4eadb; --paper:#fffaf2; --ink:#2d211c; --muted:#796a60;
  --line:#d9c4ad; --accent:#8a3f2d; --accent2:#b96f3f; --dark:#34251f;
}
*{box-sizing:border-box}
body{
  margin:0; min-height:100vh; color:var(--ink);
  font-family: Pretendard, "Noto Sans KR", system-ui, -apple-system, sans-serif;
  background:
    radial-gradient(circle at 10% 8%, rgba(185,111,63,.16), transparent 27%),
    radial-gradient(circle at 90% 92%, rgba(138,63,45,.12), transparent 25%),
    var(--bg);
}
button{font:inherit}
.app{max-width:560px; min-height:100vh; margin:auto; padding:24px 18px 38px; display:flex; align-items:center}
.card{
  width:100%; background:rgba(255,250,242,.94); border:1px solid var(--line);
  border-radius:28px; padding:28px 22px; box-shadow:0 20px 55px rgba(64,42,30,.13);
  position:relative; overflow:hidden;
}
.card:before,.card:after{position:absolute; font-size:48px; opacity:.13}
.card:before{content:"🍂"; top:8px; right:12px; transform:rotate(18deg)}
.card:after{content:"🍁"; bottom:8px; left:10px; transform:rotate(-18deg)}
.screen{display:none; position:relative; z-index:1}
.screen.active{display:block; animation:fade .35s ease}
@keyframes fade{from{opacity:0; transform:translateY(8px)}to{opacity:1; transform:none}}
.eyebrow{letter-spacing:.18em; font-size:12px; font-weight:800; color:var(--accent); text-transform:uppercase}
h1{font-size:42px; line-height:1.05; margin:12px 0 8px; letter-spacing:-.045em}
h2{font-size:27px; line-height:1.28; margin:10px 0 22px; letter-spacing:-.035em}
p{line-height:1.65}
.sub{color:var(--muted); margin:0 0 26px}
.badge{display:inline-block; padding:8px 12px; border:1px solid var(--line); border-radius:999px; font-size:12px; background:#fff7ec}
.primary,.option,.ghost{
  width:100%; border-radius:16px; cursor:pointer; transition:.15s ease;
}
.primary{border:0; padding:16px; background:var(--dark); color:white; font-weight:800; margin-top:10px}
.primary:active,.option:active,.ghost:active{transform:scale(.985)}
.option{
  text-align:left; padding:15px 16px; margin:9px 0; border:1px solid var(--line);
  background:#fffdf8; color:var(--ink); line-height:1.4;
}
.option:hover{border-color:var(--accent2); background:#fff7ec}
.ghost{padding:14px; border:1px solid var(--line); background:transparent; color:var(--ink); margin-top:10px}
.progress{height:7px; border-radius:999px; background:#eadccd; overflow:hidden; margin:8px 0 24px}
.progress > div{height:100%; background:linear-gradient(90deg,var(--accent2),var(--accent)); transition:width .3s}
.qmeta{display:flex; justify-content:space-between; color:var(--muted); font-size:12px}
.loader{width:78px;height:78px;border:7px solid #eadccd;border-top-color:var(--accent);border-radius:50%;margin:28px auto;animation:spin .8s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}
.center{text-align:center}
.scan{font-family:ui-monospace,SFMono-Regular,Menlo,monospace;color:var(--accent);font-weight:800;letter-spacing:.08em}
.result-icon{font-size:68px; margin:10px 0 4px}
.result-name{font-size:34px; margin:0 0 10px}
.result-desc{color:var(--muted); margin:0 auto 22px; max-width:430px}
.menu-box{border:1px solid var(--line); border-radius:20px; padding:18px; background:#fff7ec; margin:16px 0}
.menu-label{font-size:12px; letter-spacing:.14em; font-weight:900; color:var(--accent)}
.menu{font-size:25px; font-weight:900; margin:6px 0}
.match{font-size:13px;color:var(--muted)}
.footer{font-size:11px;color:#8c7d73;text-align:center;margin-top:20px}
.tiny{font-size:12px;color:var(--muted)}
</style>
</head>
<body>
<main class="app">
<section class="card">
  <div id="start" class="screen active center">
    <div class="eyebrow">AUTUMN INTEREST</div>
    <h1>AI름다운<br>가을 🍂</h1>
    <p class="sub">6개의 질문으로 분석하는<br><strong>나의 가을 취향 & AI 추천 메뉴</strong></p>
    <span class="badge">AI CONVERGENCE × AUTUMN</span>
    <button class="primary" onclick="begin()">AI 분석 시작하기</button>
    <div class="footer">당신의 Autumn Interest를 분석합니다.</div>
  </div>

  <div id="quiz" class="screen">
    <div class="qmeta"><span id="qcount"></span><span>AUTUMN SCAN</span></div>
    <div class="progress"><div id="bar"></div></div>
    <div class="eyebrow">INTEREST QUESTION</div>
    <h2 id="question"></h2>
    <div id="options"></div>
  </div>

  <div id="loading" class="screen center">
    <div class="eyebrow">AI ANALYSIS</div>
    <div class="loader"></div>
    <h2>당신의 가을 취향을<br>분석하고 있습니다.</h2>
    <p id="loadingText" class="scan">ANALYZING... 18%</p>
    <p class="tiny">FOOD · MOOD · AUTUMN · VIBE</p>
  </div>

  <div id="result" class="screen center">
    <div class="eyebrow">AUTUMN INTEREST FOUND</div>
    <div id="ricon" class="result-icon"></div>
    <h2 id="rname" class="result-name"></h2>
    <p id="rdesc" class="result-desc"></p>
    <div class="menu-box">
      <div class="menu-label">AI MENU MATCH</div>
      <div id="rmenu" class="menu"></div>
      <div id="rmatch" class="match"></div>
    </div>
    <p class="tiny">오늘 당신의 가을 취향에 어울리는 메뉴입니다.</p>
    <button class="primary" onclick="restart()">다시 분석하기</button>
  </div>
</section>
</main>

<script>
const TYPES = {
 energy:{name:"단풍 직진형",icon:"🍁",menu:"소시지 + 감튀",desc:"고민은 짧게, 재미는 확실하게. 친구들과 활기차게 움직이고 가을을 제대로 즐기는 직진형입니다."},
 romance:{name:"가을밤 낭만형",icon:"🌙",menu:"육전",desc:"선선한 밤공기와 좋은 대화를 좋아하는 감성파. 북적임 속에서도 분위기와 낭만을 놓치지 않습니다."},
 bold:{name:"화끈한 불꽃형",icon:"🔥",menu:"닭꼬치",desc:"강렬한 맛과 뜨거운 분위기에 끌리는 타입. 술자리의 온도를 한 단계 올리는 에너지가 있습니다."},
 comfort:{name:"느긋한 낙엽형",icon:"🍂",menu:"두부김치",desc:"빠르게 흘러가기보다 편안하게 오래 즐기는 타입. 익숙하고 든든한 분위기에서 가장 행복합니다."},
 cozy:{name:"포근한 담요형",icon:"🧣",menu:"콘치즈",desc:"쌀쌀한 날엔 따뜻하고 부드러운 것이 최고. 포근하고 고소한 행복을 좋아하는 타입입니다."},
 fresh:{name:"새콤한 첫바람형",icon:"🌬️",menu:"요구르트 샤베트",desc:"무겁기보다 산뜻하고 상쾌한 분위기를 선호합니다. 선선한 첫 가을바람처럼 깔끔한 매력의 소유자입니다."},
 crispy:{name:"바삭한 산책형",icon:"👟",menu:"나초 + 감튀",desc:"친구들과 돌아다니며 이것저것 즐기는 것을 좋아하는 타입. 가볍고 바삭하게 계속 손이 가는 재미를 선호합니다."},
 sunset:{name:"달콤한 노을형",icon:"🌇",menu:"화채",desc:"예쁜 풍경과 달콤한 순간을 오래 기억하는 타입. 기분 좋은 마무리와 함께하는 시간을 중요하게 생각합니다."}
};

const Q = [
 {q:"선선한 가을 저녁, 가장 끌리는 계획은?", a:[
  ["친구들이랑 축제 한복판에서 신나게 놀기",{energy:3,bold:1}],
  ["야경 보면서 천천히 산책하기",{romance:3,sunset:1}],
  ["맛있는 거 먹으면서 오래 이야기하기",{comfort:2,cozy:2}],
  ["시원한 바람 맞으며 여기저기 구경하기",{fresh:2,crispy:2}]
 ]},
 {q:"술자리에서 나와 가장 가까운 모습은?", a:[
  ["분위기 띄우고 먼저 건배하기",{energy:2,bold:2}],
  ["친한 사람과 깊은 이야기하기",{romance:3,comfort:1}],
  ["안주 하나씩 계속 집어 먹기",{crispy:3,cozy:1}],
  ["달달하고 시원한 메뉴로 쉬어가기",{fresh:2,sunset:2}]
 ]},
 {q:"지금 가장 당기는 맛은?", a:[
  ["짭짤하고 강렬한 맛",{bold:3,energy:1}],
  ["따뜻하고 든든한 맛",{comfort:3,romance:1}],
  ["고소하고 부드러운 맛",{cozy:3,crispy:1}],
  ["달콤하고 상쾌한 맛",{sunset:2,fresh:2}]
 ]},
 {q:"가을의 한 장면을 고른다면?", a:[
  ["붉게 물든 단풍길",{energy:3,bold:1}],
  ["조명 아래 선선한 가을밤",{romance:3,comfort:1}],
  ["담요 덮고 따뜻하게 쉬는 시간",{cozy:3,sunset:1}],
  ["바람 맞으며 걷는 축제 거리",{fresh:2,crispy:2}]
 ]},
 {q:"안주를 고를 때 더 중요한 것은?", a:[
  ["한입 먹자마자 확 오는 맛",{bold:3,energy:1}],
  ["술과 천천히 즐기기 좋은 맛",{romance:2,comfort:2}],
  ["계속 손이 가는 바삭하고 고소한 맛",{crispy:3,cozy:1}],
  ["입안을 시원하게 바꿔주는 맛",{fresh:2,sunset:2}]
 ]},
 {q:"오늘 밤의 엔딩으로 가장 좋은 것은?", a:[
  ["마지막까지 텐션 올리고 놀기",{energy:2,bold:2}],
  ["좋은 사람과 여운 남는 대화",{romance:2,comfort:2}],
  ["맛있는 거 하나 더 시켜 나눠 먹기",{crispy:2,cozy:2}],
  ["달콤하고 시원하게 마무리하기",{sunset:3,fresh:1}]
 ]}
];

let idx=0;
let scores={};

function show(id){
 document.querySelectorAll(".screen").forEach(x=>x.classList.remove("active"));
 document.getElementById(id).classList.add("active");
}
function begin(){
 idx=0; scores={};
 Object.keys(TYPES).forEach(k=>scores[k]=0);
 show("quiz"); render();
}
function render(){
 const item=Q[idx];
 document.getElementById("qcount").textContent=`${idx+1} / ${Q.length}`;
 document.getElementById("bar").style.width=`${((idx+1)/Q.length)*100}%`;
 document.getElementById("question").textContent=item.q;
 const box=document.getElementById("options"); box.innerHTML="";
 item.a.forEach(([label,points])=>{
   const b=document.createElement("button"); b.className="option"; b.textContent=label;
   b.onclick=()=>answer(points); box.appendChild(b);
 });
}
function answer(points){
 Object.entries(points).forEach(([k,v])=>scores[k]+=v);
 idx++;
 if(idx<Q.length) render(); else analyze();
}
function analyze(){
 show("loading");
 const steps=[18,37,59,78,94,100]; let i=0;
 const el=document.getElementById("loadingText");
 const timer=setInterval(()=>{
   el.textContent=`ANALYZING... ${steps[i]}%`; i++;
   if(i===steps.length){ clearInterval(timer); setTimeout(result,450); }
 },320);
}
function result(){
 const max=Math.max(...Object.values(scores));
 let winners=Object.keys(scores).filter(k=>scores[k]===max);
 // 동점이면 해당 세션에서 동점 유형 중 하나를 선택
 const key=winners[Math.floor(Math.random()*winners.length)];
 const r=TYPES[key];
 const total=Object.values(scores).reduce((a,b)=>a+b,0);
 const dominance= total ? scores[key]/total : 0;
 const match=Math.min(99, Math.max(91, Math.round(89 + dominance*35)));
 document.getElementById("ricon").textContent=r.icon;
 document.getElementById("rname").textContent=r.name;
 document.getElementById("rdesc").textContent=r.desc;
 document.getElementById("rmenu").textContent=r.menu;
 document.getElementById("rmatch").textContent=`MATCH RATE ${match}%`;
 show("result");
}
function restart(){ show("start"); }
</script>
</body>
</html>
