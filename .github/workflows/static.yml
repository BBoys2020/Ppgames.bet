<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PPGames - Betting Platform</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Arial}
body{background:#08080f;color:#fff}
header{background:#111122;padding:12px 18px;display:flex;justify-content:space-between;align-items:center;border-bottom:2px solid #7c5cff;position:sticky;top:0;z-index:10}
.logo{font-weight:900;font-size:22px}.logo span{color:#7c5cff}
.saldoBox{background:#1a1a33;padding:8px 14px;border-radius:20px;border:1px solid #7c5cff;font-weight:bold}
nav{display:flex;gap:8px;padding:12px;background:#0f0f1f;overflow:auto}
nav button{background:#1e1e3a;color:#fff;border:none;padding:8px 14px;border-radius:20px;cursor:pointer;white-space:nowrap}
nav button.active{background:#7c5cff}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(160px,1fr));gap:12px;padding:16px}
.gameCard{background:#16162c;border-radius:14px;overflow:hidden;cursor:pointer;border:1px solid #222;transition:.2s}
.gameCard:hover{border-color:#7c5cff;transform:translateY(-3px)}
.gameThumb{height:120px;background:linear-gradient(135deg,#1e1e3a,#7c5cff);display:flex;align-items:center;justify-content:center;font-size:40px;position:relative}
.rtp{position:absolute;top:6px;right:6px;background:#00ff88;color:#000;font-size:10px;padding:3px 6px;border-radius:10px;font-weight:bold}
.gameInfo{padding:8px}.gameInfo h4{font-size:13px}
#modal{position:fixed;inset:0;background:rgba(0,0,0,.85);display:none;z-index:100;align-items:center;justify-content:center;padding:15px}
#modal.active{display:flex}
.modalBox{background:#15152c;width:100%;max-width:420px;border-radius:18px;padding:18px;border:1px solid #7c5cff}
.input{width:100%;padding:12px;background:#0f0f1f;border:1px solid #333;border-radius:10px;color:#fff;margin:6px 0}
.btn{background:#7c5cff;color:#fff;border:none;padding:12px;width:100%;border-radius:10px;font-weight:bold;cursor:pointer;margin-top:8px}
.btnGreen{background:#00ff88;color:#000}
.adminGrid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:10px}
.statBox{background:#1e1e3a;padding:12px;border-radius:10px;text-align:center}
canvas{background:#0f0f1f;border-radius:10px;display:block;margin:10px auto;border:1px solid #333}
</style>
</head>
<body>
<header>
<div class="logo">PP<span>GAMES</span> BET</div>
<div style="display:flex;gap:8px;align-items:center">
<div class="saldoBox">R$ <span id="saldo">100.00</span></div>
<button onclick="openModal('dep')" style="background:#00ff88;color:#000;border:none;padding:8px 12px;border-radius:20px;font-weight:bold;cursor:pointer">+ Depósito</button>
<button onclick="openModal('adm')" style="background:#222;color:#fff;border:none;padding:8px 10px;border-radius:50%;cursor:pointer">⚙️</button>
</div>
</header>

<nav>
<button class="active" onclick="filtrar('todos')">Todos 15</button>
<button onclick="filtrar('crash')">Crash</button>
<button onclick="filtrar('slots')">Slots</button>
<button onclick="filtrar('roleta')">Roleta</button>
<button onclick="openModal('saque')">Saque</button>
</nav>

<div class="grid" id="grid"></div>

<div id="modal">
<div class="modalBox" id="modalContent"></div>
</div>

<script>
let saldo = parseFloat(localStorage.getItem('pp_saldo')||'100.00');
let config = JSON.parse(localStorage.getItem('pp_config')||'{"pix":"seu-pix@email.com","rtp":85,"minDep":20,"minSaque":50}');
document.getElementById('saldo').innerText=saldo.toFixed(2);

let jogos = [
{id:1,nome:'Crash Aviator',tipo:'crash',icon:'🚀',rtp:config.rtp},
{id:2,nome:'Double Roleta',tipo:'roleta',icon:'🎡',rtp:config.rtp},
{id:3,nome:'Mines',tipo:'slots',icon:'💣',rtp:config.rtp},
{id:4,nome:'Fortune Tiger',tipo:'slots',icon:'🐯',rtp:config.rtp},
{id:5,nome:'Crash Ball',tipo:'crash',icon:'⚽',rtp:config.rtp},
{id:6,nome:'Roleta Brasileira',tipo:'roleta',icon:'🎰',rtp:config.rtp},
{id:7,nome:'Ninja Crash',tipo:'crash',icon:'🥷',rtp:config.rtp},
{id:8,nome:'Fruit Slots',tipo:'slots',icon:'🍒',rtp:config.rtp},
{id:9,nome:'Plinko',tipo:'crash',icon:'🔴',rtp:config.rtp},
{id:10,nome:'Blackjack',tipo:'roleta',icon:'🃏',rtp:config.rtp},
{id:11,nome:'Aviator 2x',tipo:'crash',icon:'✈️',rtp:config.rtp},
{id:12,nome:'Golden Slots',tipo:'slots',icon:'💰',rtp:config.rtp},
{id:13,nome:'Crash Moto',tipo:'crash',icon:'🏍️',rtp:config.rtp},
{id:14,nome:'Roleta PIX',tipo:'roleta',icon:'💸',rtp:config.rtp},
{id:15,nome:'Big Bass',tipo:'slots',icon:'🐟',rtp:config.rtp},
];

function render(filtro='todos'){
let g=document.getElementById('grid');g.innerHTML='';
jogos.filter(j=>filtro==='todos'||j.tipo===filtro).forEach(j=>{
g.innerHTML+=`<div class="gameCard" onclick="jogar(${j.id})"><div class="gameThumb">${j.icon}<span class="rtp">${j.rtp}% RTP</span></div><div class="gameInfo"><h4>${j.nome}</h4><p style="font-size:10px;opacity:.5">${j.tipo.toUpperCase()}</p></div></div>`;
});
}
render();
function filtrar(t){document.querySelectorAll('nav button').forEach(b=>b.classList.remove('active'));event.target.classList.add('active');render(t);}

function jogar(id){
let j=jogos.find(x=>x.id===id);
document.getElementById('modal').classList.add('active');
document.getElementById('modalContent').innerHTML=`
<h3>${j.icon} ${j.nome}</h3>
<p style="font-size:12px;opacity:.6;margin:6px 0">RTP Configurado: ${j.rtp}% | Aposta mínima: R$1</p>
<div style="display:flex;gap:6px;margin:10px 0"><button class="btn" style="width:auto;padding:8px 12px" onclick="setBet(1)">R$1</button><button class="btn" style="width:auto;padding:8px 12px" onclick="setBet(5)">R$5</button><button class="btn" style="width:auto;padding:8px 12px" onclick="setBet(10)">R$10</button><button class="btn" style="width:auto;padding:8px 12px" onclick="setBet(50)">R$50</button></div>
<input id="valorBet" class="input" type="number" value="5" min="1">
<canvas id="gameCanvas" width="340" height="200"></canvas>
<p style="text-align:center;font-size:12px" id="gameStatus">Clique em JOGAR</p>
<button class="btn btnGreen" onclick="rodarCrash()">🎮 JOGAR</button>
<button class="btn" style="background:#222" onclick="closeModal()">Fechar</button>
`;
}
let bet=5;function setBet(v){bet=v;document.getElementById('valorBet').value=v;}
function rodarCrash(){
let v=parseFloat(document.getElementById('valorBet').value);
if(saldo<v){alert('Saldo insuficiente! Faça depósito.');return;}
saldo-=v;updateSaldo();
let canvas=document.getElementById('gameCanvas'),ctx=canvas.getContext('2d'),mult=1,crashPoint=(Math.random()* (100-config.rtp)/20)+1; // RTP influencia
if(Math.random()*100 < config.rtp){crashPoint = 1.2 + Math.random()*5;} else {crashPoint = 1 + Math.random()*0.8;}
let interval=setInterval(()=>{
mult+=0.05;
ctx.clearRect(0,0,340,200);
ctx.fillStyle='#7c5cff';ctx.font='32px Arial';ctx.fillText(mult.toFixed(2)+'x',120,100);
document.getElementById('gameStatus').innerText='Multiplicador: '+mult.toFixed(2)+'x - Crash em '+crashPoint.toFixed(2)+'x';
if(mult>=crashPoint){
clearInterval(interval);
if(mult>=1.5 && Math.random()*100 < config.rtp){let ganho=v*mult;saldo+=ganho;updateSaldo();document.getElementById('gameStatus').innerText='GANHOU R$ '+ganho.toFixed(2)+'!';}
else{document.getElementById('gameStatus').innerText='CRASH! Perdeu.';}
}
},80);
}
function updateSaldo(){localStorage.setItem('pp_saldo',saldo);document.getElementById('saldo').innerText=saldo.toFixed(2);}

function openModal(t){
document.getElementById('modal').classList.add('active');
let c=document.getElementById('modalContent');
if(t==='dep'){
c.innerHTML=`<h3>Depósito PIX</h3><p style="font-size:12px;opacity:.6">PIX configurado pelo ADM: ${config.pix}</p><input id="depVal" class="input" type="number" placeholder="Valor mínimo R$ ${config.minDep}"><p style="font-size:11px;margin-top:6px">Chave PIX:</p><div style="background:#0f0f1f;padding:10px;border-radius:8px;font-size:13px;word-break:break-all">${config.pix}</div><button class="btn btnGreen" onclick="fazerDep()">Confirmar Depósito (DEMO)</button><button class="btn" style="background:#222" onclick="closeModal()">Fechar</button>`;
}
if(t==='saque'){
c.innerHTML=`<h3>Saque</h3><input id="saqVal" class="input" type="number" placeholder="Mínimo R$ ${config.minSaque}"><input class="input" placeholder="Sua chave PIX"><button class="btn btnGreen" onclick="fazerSaque()">Solicitar Saque</button><button class="btn" style="background:#222" onclick="closeModal()">Fechar</button>`;
}
if(t==='adm'){
c.innerHTML=`<h3>⚙️ PAINEL ADM</h3><p style="font-size:11px;opacity:.6">Senha padrão: admin123</p><input id="admPass" class="input" type="password" placeholder="Senha ADM"><button class="btn" onclick="loginAdm()">Entrar no ADM</button><button class="btn" style="background:#222" onclick="closeModal()">Fechar</button>`;
}
}
function fazerDep(){let v=parseFloat(document.getElementById('depVal').value);if(v<config.minDep){alert('Mínimo R$'+config.minDep);return;}saldo+=v;updateSaldo();alert('Depósito DEMO de R$'+v+' creditado!');closeModal();}
function fazerSaque(){let v=parseFloat(document.getElementById('saqVal').value);if(v>saldo){alert('Saldo insuficiente');return;}if(v<config.minSaque){alert('Mínimo R$'+config.minSaque);return;}saldo-=v;updateSaldo();alert('Saque de R$'+v+' solicitado!');closeModal();}
function loginAdm(){
if(document.getElementById('admPass').value!=='admin123'){alert('Senha errada');return;}
let c=document.getElementById('modalContent');
c.innerHTML=`
<h3>Painel ADM - PPGames</h3>
<div class="adminGrid"><div class="statBox"><h2>R$ ${saldo.toFixed(2)}</h2><p style="font-size:11px">Saldo Total Plataforma</p></div><div class="statBox"><h2>15</h2><p style="font-size:11px">Jogos Ativos</p></div></div>
<p style="margin-top:12px;font-weight:bold">Configurações:</p>
<label style="font-size:12px">Chave PIX Recebimento</label><input id="cfgPix" class="input" value="${config.pix}">
<label style="font-size:12px">RTP % (Quanto paga pros jogadores)</label><input id="cfgRtp" class="input" type="number" value="${config.rtp}">
<label style="font-size:12px">Depósito Mínimo</label><input id="cfgMinDep" class="input" type="number" value="${config.minDep}">
<label style="font-size:12px">Saque Mínimo</label><input id="cfgMinSaq" class="input" type="number" value="${config.minSaque}">
<label style="font-size:12px">Saldo Inicial Usuário</label><input id="cfgSaldo" class="input" type="number" value="100">
<button class="btn btnGreen" onclick="salvarCfg()">Salvar Configurações</button>
<button class="btn" onclick="zerar()">Zerar Plataforma</button>
<button class="btn" style="background:#222" onclick="closeModal()">Fechar</button>
`;
}
function salvarCfg(){config.pix=document.getElementById('cfgPix').value;config.rtp=parseInt(document.getElementById('cfgRtp').value);config.minDep=parseFloat(document.getElementById('cfgMinDep').value);config.minSaque=parseFloat(document.getElementById('cfgMinSaq').value);localStorage.setItem('pp_config',JSON.stringify(config));alert('Salvo! RTP agora: '+config.rtp+'%');render();closeModal();}
function zerar(){localStorage.clear();location.reload();}
function closeModal(){document.getElementById('modal').classList.remove('active');}
</script>
</body>
</html>
