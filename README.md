<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">
<title>MMA Career Universe 7</title>

<style>
body{
margin:0;
background:#0b0b0b;
font-family:Arial,sans-serif;
color:white;
text-align:center;
}
.app{
max-width:430px;
margin:auto;
padding:10px;
}
.card{
background:#181818;
border-radius:18px;
padding:14px;
margin:12px 0;
box-shadow:0 0 12px #000;
}
h1{color:gold}
button,input,select{
padding:11px;
margin:5px;
border:none;
border-radius:10px;
font-weight:bold;
}
button{background:gold}
.stats{
display:grid;
grid-template-columns:1fr 1fr;
gap:8px;
}
.stat{
background:#292929;
padding:10px;
border-radius:12px;
}
.bar{
height:18px;
background:#444;
border-radius:10px;
overflow:hidden;
margin:8px 0;
}
.fill{
height:100%;
width:100%;
background:limegreen;
transition:.25s;
}
.energyFill{background:#00cfff}
.popFill{background:orange}
#fightScreen,#pressBox,#escapeBox{display:none}
.arena{
position:relative;
height:290px;
background:linear-gradient(#222,#101010);
border-radius:20px;
overflow:hidden;
margin-top:10px;
}
.floor{
position:absolute;
bottom:0;
height:70px;
width:100%;
background:#2d2d2d;
}
.fighter{
position:absolute;
bottom:70px;
width:85px;
height:160px;
transition:.18s;
}
.body{
position:absolute;
bottom:0;
width:85px;
height:120px;
background:#c28b5c;
border-radius:40px 40px 18px 18px;
}
.body:before{
content:"";
position:absolute;
top:-40px;
left:22px;
width:40px;
height:40px;
background:#f1c27d;
border-radius:50%;
}
.shorts{
position:absolute;
bottom:8px;
left:12px;
width:58px;
height:28px;
border-radius:8px;
background:red;
}
.hitAnim{transform:translateX(30px) rotate(4deg)}
.kickAnim{transform:translateY(-15px) rotate(12deg)}
.takedownAnim{transform:translateX(45px) rotate(-18deg) scale(.9)}
.submissionAnim{transform:scale(.85) rotate(-25deg)}
.blockAnim{filter:brightness(1.6)}
.enemyHit{animation:enemyShake .22s}
@keyframes enemyShake{
0%{transform:translateX(0)}
25%{transform:translateX(-8px)}
50%{transform:translateX(8px)}
75%{transform:translateX(-6px)}
100%{transform:translateX(0)}
}
#log{
background:black;
border-radius:12px;
padding:10px;
height:200px;
overflow-y:auto;
font-size:13px;
text-align:left;
}
.gold{color:gold;font-weight:bold}
.redText{color:#ff4444;font-weight:bold}
.small{font-size:12px;opacity:.8}
</style>
</head>

<body>
<div class="app">

<h1>MMA Career Universe 7</h1>

<div class="card" id="creator">
<h2>Создай бойца</h2>

<input id="fighterName" value="Мой Боец">

<select id="style">
<option value="striker">Ударник</option>
<option value="wrestler">Борец</option>
<option value="balanced">Универсал</option>
</select>

<select id="weight">
<option value="lightweight">Лёгкий вес</option>
<option value="middleweight">Средний вес</option>
<option value="heavyweight">Тяжёлый вес</option>
</select>

<select id="shorts">
<option value="red">Красные шорты</option>
<option value="blue">Синие шорты</option>
<option value="black">Чёрные шорты</option>
<option value="gold">Золотые шорты</option>
</select>

<button onclick="createPlayer()">Начать карьеру</button>
</div>

<div class="card">
<h2 id="playerName">Боец</h2>
<div id="status"></div>

<div class="stats">
<div class="stat">Рейтинг<br><b id="rating">50</b></div>
<div class="stat">Деньги<br><b id="money">1000</b>$</div>
<div class="stat">Фанаты<br><b id="fans">0</b></div>
<div class="stat">Стрик<br><b id="streak">0</b></div>
<div class="stat">Победы<br><b id="wins">0</b></div>
<div class="stat">Поражения<br><b id="losses">0</b></div>
<div class="stat">Сила<br><b id="power">10</b></div>
<div class="stat">Грэпплинг<br><b id="grappling">10</b></div>
<div class="stat">Защита<br><b id="defense">10</b></div>
<div class="stat">Энергия<br><b id="energy">100</b></div>
</div>

<div class="bar"><div id="popularityBar" class="fill popFill"></div></div>
</div>

<div class="card">
<h2>Тренировки</h2>
<button onclick="train('power')">🥊 Сила</button>
<button onclick="train('grappling')">🤼 Грэпплинг</button>
<button onclick="train('defense')">🛡️ Защита</button>
<p class="small">Если ты чемпион, тренировки дороже.</p>
</div>

<div class="card">
<h2>Энергия</h2>
<button onclick="recoverEnergy()">⚡ Восстановить энергию (500$)</button>
</div>

<div class="card">
<h2>Весовая категория</h2>
<button onclick="changeWeight('lightweight')">Лёгкий вес</button>
<button onclick="changeWeight('middleweight')">Средний вес</button>
<button onclick="changeWeight('heavyweight')">Тяжёлый вес</button>
<p class="small">Менять вес может только чемпион.</p>
</div>

<div class="card">
<h2>Карьера</h2>
<button onclick="findOpponent()">🔍 Найти соперника</button>
<p id="opponentText">Соперник не найден</p>
<button onclick="startPressConference()">🎤 Пресс-конференция</button>
<button onclick="startFight()">🔥 Начать бой</button>
</div>

<div class="card" id="pressBox">
<h2>Пресс-конференция</h2>
<p id="pressQuestion">Журналисты ждут ответа...</p>
<button onclick="pressAnswer('respect')">🤝 Уважительно</button>
<button onclick="pressAnswer('trash')">😈 Трэшток</button>
<button onclick="pressAnswer('confident')">🔥 Уверенно</button>
</div>

<div class="card" id="fightScreen">
<h2 id="fightTitle">БОЙ</h2>

<div class="bar"><div id="playerHP" class="fill"></div></div>
<div class="bar"><div id="enemyHP" class="fill"></div></div>
<div class="bar"><div id="energyBar" class="fill energyFill"></div></div>

<div class="arena">
<div id="fighter1" class="fighter" style="left:40px;">
<div class="body"><div id="shortsColor" class="shorts"></div></div>
</div>

<div id="fighter2" class="fighter" style="right:40px;">
<div class="body"><div class="shorts" style="background:blue"></div></div>
</div>

<div class="floor"></div>
</div>

<button onclick="attack('punch')">👊 Удар</button>
<button onclick="attack('kick')">🦵 Нога</button>
<button onclick="attack('block')">🛡️ Блок</button>
<button onclick="attack('takedown')">🤼 Тейкдаун</button>
<button onclick="attack('submission')">🔒 Сабмишен</button>
<button onclick="attack('special')">🔥 Добивание</button>

<div id="escapeBox">
<button onclick="tryEscape()">🆘 ВЫЙТИ ИЗ БОРЬБЫ</button>
</div>
</div>

<div class="card">
<h2>Новости</h2>
<div id="log">Создай бойца.</div>
</div>

</div>

<script>
let player={
name:"Боец",
rating:50,
money:1000,
fans:0,
wins:0,
losses:0,
streak:0,
power:10,
grappling:10,
defense:10,
energy:100,
style:"balanced",
weight:"lightweight",
shorts:"red",
belts:[]
};

let enemy=null;
let fight=false;
let blocking=false;
let titleFight=false;
let playerGrounded=false;
let enemyGrounded=false;

let playerHp=100;
let enemyHp=100;

const enemies=[
"Рико Шторм",
"Макс Орёл",
"Титан Блэйз",
"Хан Волк",
"Рэй Торнадо",
"Перейра Стоун",
"Лео Блэйд",
"Кейн Фрост",
"Гром Стоун",
"Озон Кинг"
];

const pressQuestions=[
"Что скажешь о сопернике?",
"Готов ли ты к мейн-ивенту?",
"Будет ли нокаут?",
"Ты достоин титульного боя?"
];

function createPlayer(){
player.name=document.getElementById("fighterName").value;
player.style=document.getElementById("style").value;
player.weight=document.getElementById("weight").value;
player.shorts=document.getElementById("shorts").value;

if(player.style==="striker"){
player.power=15;
player.grappling=8;
player.defense=10;
}

if(player.style==="wrestler"){
player.power=10;
player.grappling=16;
player.defense=12;
}

if(player.style==="balanced"){
player.power=12;
player.grappling=12;
player.defense=12;
}

document.getElementById("creator").style.display="none";
updateUI();
log("Карьера началась.");
}

function updateUI(){
document.getElementById("playerName").textContent=player.name;
document.getElementById("rating").textContent=player.rating;
document.getElementById("money").textContent=player.money;
document.getElementById("fans").textContent=player.fans;
document.getElementById("wins").textContent=player.wins;
document.getElementById("losses").textContent=player.losses;
document.getElementById("streak").textContent=player.streak;
document.getElementById("power").textContent=player.power;
document.getElementById("grappling").textContent=player.grappling;
document.getElementById("defense").textContent=player.defense;
document.getElementById("energy").textContent=player.energy;

let status="Претендент";
if(player.belts.length===1) status="<span class='gold'>🏆 Чемпион</span>";
if(player.belts.length>=2) status="<span class='redText'>🏆🏆 Двойной чемпион</span>";

document.getElementById("status").innerHTML=
status+"<br>"+player.belts.map(b=>"Пояс: "+b).join("<br>");

document.getElementById("popularityBar").style.width=
Math.min(player.fans/10,100)+"%";

saveGame();
}

function trainingCost(){
return player.belts.length>0 ? 800 : 300;
}

function train(type){
const cost=trainingCost();

if(player.money<cost){
log("Не хватает денег.");
return;
}

if(player.energy<20){
log("Не хватает энергии.");
return;
}

player.money-=cost;
player.energy-=20;

const limit=Math.floor(player.rating/4)+25;

if(type==="power" && player.power<limit) player.power++;
if(type==="grappling" && player.grappling<limit) player.grappling++;
if(type==="defense" && player.defense<limit) player.defense++;

player.rating++;
log("Тренировка завершена. Соперники тоже становятся сильнее.");
updateUI();
}

function recoverEnergy(){
if(player.money<500){
log("Не хватает денег.");
return;
}

player.money-=500;
player.energy=100;
log("Энергия восстановлена.");
updateUI();
}

function changeWeight(newWeight){
if(player.belts.length===0){
log("Только чемпион может менять весовую категорию.");
return;
}

player.weight=newWeight;
enemy=null;
document.getElementById("opponentText").textContent="Соперник не найден";
log("Ты перешёл в новую весовую категорию.");
updateUI();
}

function generateEnemy(){
const scale=Math.floor(player.rating/5);
return{
name:enemies[Math.floor(Math.random()*enemies.length)],
power:10+scale+Math.floor(Math.random()*5),
grappling:10+scale+Math.floor(Math.random()*5),
defense:10+scale+Math.floor(Math.random()*5),
hp:100+scale*4,
weight:player.weight
};
}

function checkTitleFight(){
return player.streak>=3 && player.fans>=30;
}

function findOpponent(){
enemy=generateEnemy();
titleFight=checkTitleFight();

document.getElementById("opponentText").innerHTML=
(titleFight ? "🏆 ТИТУЛЬНЫЙ БОЙ<br>" : "")+
enemy.name+
"<br>Сила: "+enemy.power+
"<br>Грэпплинг: "+enemy.grappling+
"<br>Защита: "+enemy.defense;

log("Найден соперник: "+enemy.name);
}

function startPressConference(){
document.getElementById("pressBox").style.display="block";
document.getElementById("pressQuestion").textContent=
pressQuestions[Math.floor(Math.random()*pressQuestions.length)];
}

function pressAnswer(type){
document.getElementById("pressBox").style.display="none";

if(type==="respect"){
player.fans+=5;
log("Фанаты уважают тебя.");
}

if(type==="trash"){
player.fans+=15;
log("Трэшток сделал тебя популярнее.");
}

if(type==="confident"){
player.fans+=10;
log("Ты выглядишь уверенно.");
}

updateUI();
}

function startFight(){
if(player.energy<15){
log("Мало энергии.");
return;
}

if(!enemy) findOpponent();

fight=true;
blocking=false;
playerGrounded=false;
enemyGrounded=false;

playerHp=100+player.defense*2;
enemyHp=enemy.hp;

document.getElementById("escapeBox").style.display="none";
document.getElementById("fightScreen").style.display="block";
document.getElementById("shortsColor").style.background=player.shorts;
document.getElementById("fightTitle").innerHTML=
titleFight ? "🏆 ТИТУЛЬНЫЙ БОЙ" : "БОЙ";

updateFightBars();
log("Бой начался.");
}

function attack(type){
if(!fight)return;

if(playerGrounded && type!=="block"){
log("Ты в борьбе. Сначала выйди из борьбы!");
return;
}

if(player.energy<=0){
log("Нет энергии.");
return;
}

const fighter=document.getElementById("fighter1");
const enemySprite=document.getElementById("fighter2");

if(type==="block"){
blocking=true;
fighter.classList.add("blockAnim");
setTimeout(()=>{
blocking=false;
fighter.classList.remove("blockAnim");
},700);
log("Ты поставил блок.");
return;
}

let dmg=0;
let energyCost=0;

if(type==="punch"){
fighter.classList.add("hitAnim");
setTimeout(()=>fighter.classList.remove("hitAnim"),160);
dmg=player.power+Math.random()*8;
energyCost=8;
}

if(type==="kick"){
fighter.classList.add("kickAnim");
setTimeout(()=>fighter.classList.remove("kickAnim"),180);
dmg=player.power+12+Math.random()*8;
energyCost=15;
}

if(type==="takedown"){
fighter.classList.add("takedownAnim");
setTimeout(()=>fighter.classList.remove("takedownAnim"),220);
energyCost=20;

const success=(Math.random()*100)+player.grappling > 50+enemy.grappling;

if(success){
enemyGrounded=true;
dmg=player.grappling+10;
log("🤼 Успешный тейкдаун! Теперь можно делать сабмишен.");
}else{
dmg=0;
log("Соперник защитился от тейкдауна.");
}
}

if(type==="submission"){
if(!enemyGrounded){
log("Сначала сделай тейкдаун!");
return;
}

fighter.classList.add("submissionAnim");
setTimeout(()=>fighter.classList.remove("submissionAnim"),250);
energyCost=28;

const success=(Math.random()*100)+player.grappling > 55+enemy.grappling;

if(success){
dmg=enemyHp;
log("🔒 Сабмишен прошёл!");
}else{
dmg=0;
enemyGrounded=false;
log("Соперник вышел из сабмишена.");
}
}

if(type==="special"){
fighter.classList.add("kickAnim");
setTimeout(()=>fighter.classList.remove("kickAnim"),220);
dmg=player.power+28+Math.random()*10;
energyCost=35;
}

player.energy-=energyCost;
if(player.energy<0) player.energy=0;

enemySprite.classList.add("enemyHit");
setTimeout(()=>enemySprite.classList.remove("enemyHit"),220);

enemyHp-=Math.floor(dmg);
if(enemyHp<0) enemyHp=0;

if(dmg>0) log("Ты нанёс "+Math.floor(dmg)+" урона.");

updateFightBars();

if(enemyHp<=0){
winFight();
return;
}

enemyAttack();
}

function enemyAttack(){
if(!fight)return;

const enemySprite=document.getElementById("fighter2");

if(!playerGrounded && Math.random()<0.28){
log(enemy.name+" пытается сделать тейкдаун!");

const success=(Math.random()*100)+enemy.grappling > 55+player.grappling;

if(success){
playerGrounded=true;
document.getElementById("escapeBox").style.display="block";
log("🤼 Тебя перевели в партер!");
}else{
log("Ты защитился от тейкдауна!");
}
return;
}

if(playerGrounded){
groundDamage();
return;
}

enemySprite.classList.add("hitAnim");
setTimeout(()=>enemySprite.classList.remove("hitAnim"),160);

let dmg=enemy.power+Math.random()*8;

if(blocking) dmg=Math.floor(dmg/2);

playerHp-=Math.floor(dmg);
if(playerHp<0) playerHp=0;

log(enemy.name+" ударил на "+Math.floor(dmg));

updateFightBars();

if(playerHp<=0) loseFight();
}

function tryEscape(){
if(!playerGrounded)return;

let baseChance=50;

if(enemy.grappling > player.grappling){
baseChance=30;
log("У соперника грэпплинг лучше: шанс выхода 30/70.");
}else{
log("Шанс выхода 50/50.");
}

const roll=Math.random()*100;

if(roll < baseChance){
playerGrounded=false;
document.getElementById("escapeBox").style.display="none";
log("✅ Ты вышел из борьбы!");
}else{
log("❌ Не получилось выйти!");
groundDamage();
}
}

function groundDamage(){
let dmg=enemy.grappling+Math.floor(Math.random()*12);
playerHp-=dmg;
if(playerHp<0) playerHp=0;

log("👊 Ground-and-pound: -"+dmg+" HP");

if(Math.random()<0.25){
const subSuccess=(Math.random()*100)+enemy.grappling > 60+player.grappling;

if(subSuccess){
playerHp=0;
updateFightBars();
log("🔒 Тебя засабмитили!");
loseFight();
return;
}else{
log("Ты защитился от сабмишена.");
}
}

updateFightBars();

if(playerHp<=0) loseFight();
}

function recoverFightStamina(){
if(!fight)return;

if(player.energy<100){
if(playerGrounded){
player.energy+=1;
}else{
player.energy+=3;
}

if(player.energy>100) player.energy=100;
updateFightBars();
updateUI();
}
}

setInterval(recoverFightStamina,1000);

function updateFightBars(){
document.getElementById("playerHP").style.width=
(playerHp/(100+player.defense*2))*100+"%";

document.getElementById("enemyHP").style.width=
(enemyHp/enemy.hp)*100+"%";

document.getElementById("energyBar").style.width=
player.energy+"%";
}

function winFight(){
fight=false;

player.wins++;
player.streak++;

const reward=titleFight ? 5000 : 1000;

player.money+=reward;
player.rating+=5;
player.fans+=10;

if(titleFight && !player.belts.includes(player.weight)){
player.belts.push(player.weight);
log("🏆 Ты стал чемпионом!");
}

log("Победа! +"+reward+"$");

document.getElementById("fightScreen").style.display="none";
document.getElementById("escapeBox").style.display="none";

enemy=null;
updateUI();
}

function loseFight(){
fight=false;

player.losses++;
player.streak=0;
player.rating=Math.max(40,player.rating-5);
player.fans=Math.max(0,player.fans-5);

if(titleFight && player.belts.includes(player.weight)){
player.belts=player.belts.filter(b=>b!==player.weight);
log("Ты потерял пояс.");
}

log("Ты проиграл бой.");

document.getElementById("fightScreen").style.display="none";
document.getElementById("escapeBox").style.display="none";

enemy=null;
updateUI();
}

function log(text){
document.getElementById("log").innerHTML=
text+"<br>"+document.getElementById("log").innerHTML;
}

function saveGame(){
localStorage.setItem("mmaCareerUniverse7",JSON.stringify(player));
}

function loadGame(){
const save=localStorage.getItem("mmaCareerUniverse7");

if(save){
player=JSON.parse(save);
document.getElementById("creator").style.display="none";
updateUI();
log("Сохранение загружено.");
}
}

loadGame();
</script>

</body>
</html>
