# lao-game-topup
Lao Game TopUp Store

<!DOCTYPE html>
<html lang="lo">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Lao Game TopUp</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#100d1c;color:white;font-family:Arial,sans-serif}
header{background:#211638;padding:22px;text-align:center}
header h1{color:#c5a3ff;margin:0}
main{max-width:650px;margin:auto;padding:16px}
.card{background:#211b32;padding:18px;border-radius:16px;margin:14px 0}
h2{color:#c5a3ff;font-size:20px}
label{display:block;margin:12px 0 6px}
input,select{width:100%;padding:13px;border:1px solid #594477;border-radius:9px;background:#130f20;color:white}
button{border:0;padding:13px;border-radius:9px;background:#9b65ed;color:white;font-weight:bold;width:100%;margin-top:12px}
button:active{opacity:.8}
.small{color:#c4b8d8;font-size:13px}
.package{display:flex;justify-content:space-between;align-items:center;padding:12px;border:1px solid #594477;border-radius:9px;margin:8px 0;gap:10px}
.package button{width:auto;margin:0}
.order{border:1px solid #594477;padding:12px;border-radius:10px;margin:10px 0}
.hidden{display:none}
</style>
</head>
<body>
<header>
<h1>💜 LAO GAME TOPUP</h1>
<p>ຮ້ານເຕີມເກມອອນລາຍ</p>
</header>

<main>
<section class="card">
<h2>🎮 ເລືອກເກມ</h2>
<label>ເກມທີ່ຕ້ອງການ</label>
<select id="game" onchange="showPackages()">
<option value="Free Fire">Free Fire</option>
<option value="Mobile Legends">Mobile Legends</option>
<option value="Roblox">Roblox</option>
</select>

<h2>💎 ເລືອກແພັກເກດ</h2>
<div id="packages"></div>

<label>Player ID / User ID</label>
<input id="playerId" placeholder="ປ້ອນ ID ເກມ">

<label>ຊື່ຜູ້ສັ່ງ</label>
<input id="customer" placeholder="ຊື່ຂອງເຈົ້າ">

<label>ເບີໂທຕິດຕໍ່</label>
<input id="phone" placeholder="ເບີໂທ">

<button onclick="createOrder()">ສັ່ງເຕີມເງິນ</button>
<p class="small">ກະລຸນາກວດສອບ ID ເກມກ່ອນສັ່ງຊື້</p>
</section>

<section class="card" id="payment">
<h2>🏦 ຊຳລະຜ່ານ BCEL One</h2>
<p>ທະນາຄານ: BCEL</p>
<p>ບັນຊີ: <strong>ໃຫ້ຮ້ານໃສ່ເລກບັນຊີຂອງຕົນເອງ</strong></p>
<p class="small">ຢ່າໂອນເງິນຈົນກວ່າຈະກວດສອບຂໍ້ມູນຮ້ານແລ້ວ</p>
</section>

<section class="card">
<h2>📦 ກວດສອບອໍເດີ</h2>
<div id="confirmation">ຍັງບໍ່ມີອໍເດີ</div>
</section>

<section class="card">
<h2>🔐 Admin Demo</h2>
<p class="small">ສຳລັບທົດລອງໃນ Browser ເຄື່ອງນີ້ເທົ່ານັ້ນ</p>
<button onclick="showAdmin()">ເປີດໜ້າຈັດການອໍເດີ</button>
<div id="admin" class="hidden">
<h2>ລາຍການອໍເດີ</h2>
<div id="orders"></div>
</div>
</section>
</main>

<script>
// ແກ້ລາຄາໃຫ້ເປັນລາຄາຈິງຂອງຮ້ານກ່ອນເປີດໃຊ້
const packages = {
"Free Fire": [
{name:"100 Diamonds",price:0},
{name:"310 Diamonds",price:0},
{name:"520 Diamonds",price:0},
{name:"1060 Diamonds",price:0}
],
"Mobile Legends": [
{name:"86 Diamonds",price:0},
{name:"172 Diamonds",price:0},
{name:"257 Diamonds",price:0},
{name:"706 Diamonds",price:0}
],
"Roblox": [
{name:"Robux Package 1",price:0},
{name:"Robux Package 2",price:0},
{name:"Robux Package 3",price:0}
]
};

let selectedPackage = null;

function showPackages(){
 const game=document.getElementById("game").value;
 const area=document.getElementById("packages");
 selectedPackage=null;
 area.innerHTML="";
 packages[game].forEach((p,i)=>{
  const div=document.createElement("div");
  div.className="package";
  const info=document.createElement("span");
  info.textContent=p.name+" — "+(p.price>0?p.price.toLocaleString()+" ກີບ":"ລາຄາກຳລັງກຳນົດ");
  const btn=document.createElement("button");
  btn.textContent="ເລືອກ";
  btn.onclick=()=>{
   selectedPackage=p;
   document.querySelectorAll(".package").forEach(x=>x.style.borderColor="#594477");
   div.style.borderColor="#c5a3ff";
  };
  div.append(info,btn);
  area.appendChild(div);
 });
}
showPackages();

function getOrders(){
 return JSON.parse(localStorage.getItem("laoTopupOrders")||"[]");
}
function saveOrders(list){
 localStorage.setItem("laoTopupOrders",JSON.stringify(list));
}
function createOrder(){
 const game=document.getElementById("game").value;
 const playerId=document.getElementById("playerId").value.trim();
 const customer=document.getElementById("customer").value.trim();
 const phone=document.getElementById("phone").value.trim();

 if(!selectedPackage)return alert("ກະລຸນາເລືອກແພັກເກດ");
 if(!playerId||!customer||!phone)return alert("ກະລຸນາປ້ອນຂໍ້ມູນໃຫ້ຄົບ");
 if(selectedPackage.price<=0)return alert("ແພັກເກດນີ້ຍັງບໍ່ມີລາຄາ ກະລຸນາລໍຖ້າຮ້ານກຳນົດລາຄາ");

 const order={
  id:"LGT"+Date.now(),
  game,package:selectedPackage.name,
  price:selectedPackage.price,
  playerId,customer,phone,
  status:"ລໍຖ້າກວດສອບ",
  date:new Date().toLocaleString()
 };
 const list=getOrders();
 list.unshift(order);
 saveOrders(list);

 document.getElementById("confirmation").textContent=
 "ລະຫັດອໍເດີ: "+order.id+" | "+game+" | "+order.package+" | "+order.price.toLocaleString()+" ກີບ | ສະຖານະ: "+order.status;
 alert("ສ້າງອໍເດີສຳເລັດ!");
}
function showAdmin(){
 const admin=document.getElementById("admin");
 admin.classList.toggle("hidden");
 renderOrders();
}
function renderOrders(){
 const area=document.getElementById("orders");
 const list=getOrders();
 area.innerHTML="";
 if(!list.length){area.textContent="ຍັງບໍ່ມີອໍເດີ";return;}
 list.forEach((o,i)=>{
  const div=document.createElement("div");
  div.className="order";
  const info=document.createElement("p");
  info.textContent=o.id+" | "+o.customer+" | "+o.game+" | "+o.package+" | "+o.price.toLocaleString()+" ກີບ | "+o.status;
  const select=document.createElement("select");
  ["ລໍຖ້າກວດສອບ","ກຳລັງດຳເນີນການ","ສຳເລັດ","ຍົກເລີກ"].forEach(s=>{
   const op=document.createElement("option");
   op.value=s;op.textContent=s;
   if(o.status===s)op.selected=true;
   select.appendChild(op);
  });
  select.onchange=()=>{
   const all=getOrders();
   all[i].status=select.value;
   saveOrders(all);
   renderOrders();
  };
  div.append(info,select);
  area.appendChild(div);
 });
}
</script>
</body>
</html>
