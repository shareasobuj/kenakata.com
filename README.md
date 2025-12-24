# kenakata.com
<html lang="bn">
<head>
<meta charset="UTF-8">
<title>কেনাকাটা.কম</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
body{font-family:Arial;margin:0;background:rgb(185, 14, 223)}
header{background:#0a7cff;color:#fff;padding:12px;text-align:center;position:sticky;top:0}
button{padding:8px 12px;border:none;border-radius:5px;background:#0a7cff;color:#fff}
input{padding:8px;width:100%;margin:5px 0}
section{display:none;padding:15px}
.show{display:block}
.card{background:#fff;padding:10px;margin:10px 0;border-radius:8px}
nav button{margin:4px}
.admin{background:#ffeeba}
</style>
</head>

<body>

<header>
<h2 id="title">কেনাকাটা.কম</h2>
<button onclick="toggleLang()">BN/EN</button>
</header>
<marquee>কেনাকাটা.কম এ আপনাকে স্বাগতম</marquee>

<section id="login" class="show">
<h3 id="loginTitle">লগইন</h3>
<input id="luser" placeholder="ইউজারনেম">
<input id="lpass" type="password" placeholder="পাসওয়ার্ড">
<button onclick="login()">Login</button>
<p onclick="show('signup')">Signup</p>
</section>

<section id="signup">
<h3 id="signupTitle">সাইন আপ</h3>
<input id="suser" placeholder="ইউজারনেম">
<input id="spass" type="password" placeholder="পাসওয়ার্ড">
<button onclick="signup()">Signup</button>
</section>

<section id="main">
<nav>
<button onclick="tab('home')">Home</button>
<button onclick="tab('cart')">Cart</button>
<button onclick="tab('orders')">Orders</button>
<button onclick="tab('admin')">Admin</button>
<button onclick="logout()">Logout</button>
</nav>

<!-- HOME -->
<div id="home">
<input placeholder="Search product" onkeyup="search(this.value)">
<div id="products"></div>
</div>

<!-- CART -->
<div id="cart" style="display:none">
<h3>Cart</h3>
<div id="cartList"></div>
<button onclick="placeOrder()">Place Order</button>
</div>

<!-- ORDERS -->
<div id="orders" style="display:none">
<h3>Orders</h3>
<div id="orderList"></div>
<button onclick="downloadCSV()">Download CSV</button>
</div>

<!-- ADMIN -->
<div id="admin" style="display:none" class="admin">
<h3>Admin - Add Product</h3>
<input id="pname" placeholder="Product Name">
<input id="pprice" type="number" placeholder="Price">
<button onclick="addProduct()">Add</button>
</div>
</section>

<script>
let lang="bn";
let products=JSON.parse(localStorage.getItem("products")||'[{"n":"Shirt","p":500},{"n":"Shoe","p":1200}]');
let cart=JSON.parse(localStorage.getItem("cart")||"[]");
let orders=JSON.parse(localStorage.getItem("orders")||"[]");

function show(id){
document.querySelectorAll("section").forEach(s=>s.classList.remove("show"));
document.getElementById(id).classList.add("show");
}
function tab(id){
["home","cart","orders","admin"].forEach(d=>document.getElementById(d).style.display="none");
document.getElementById(id).style.display="block";
render();
}
function signup(){
localStorage.user=suser.value; localStorage.pass=spass.value;
alert("Signup Done"); show("login");
}
function login(){
if(luser.value==localStorage.user && lpass.value==localStorage.pass){
show("main"); tab("home");
}else alert("Wrong Info");
}
function logout(){show("login")}
function render(){
productsDiv=document.getElementById("products");
productsDiv.innerHTML=products.map((i,x)=>`
<div class="card">
<b>${i.n}</b> - ৳${i.p}
<button onclick="addCart(${x})">Add</button>
</div>`).join("");
cartList.innerHTML=cart.map(i=>`<p>${i.n} - ৳${i.p}</p>`).join("");
orderList.innerHTML=orders.map(o=>`<p>${o}</p>`).join("");
}
function addCart(i){
cart.push(products[i]);
localStorage.cart=JSON.stringify(cart);
alert("Added to cart");
}
function placeOrder(){
if(cart.length==0) return alert("Cart Empty");
orders.push("Order: "+cart.length+" items | "+new Date().toLocaleString());
cart=[];
localStorage.orders=JSON.stringify(orders);
localStorage.cart="[]";
render();
alert("Order Placed");
}
function addProduct(){
products.push({n:pname.value,p:pprice.value});
localStorage.products=JSON.stringify(products);
render(); alert("Product Added");
}
function search(t){
t=t.toLowerCase();
document.getElementById("products").innerHTML=products
.filter(i=>i.n.toLowerCase().includes(t))
.map(i=>`<div class="card">${i.n} - ৳${i.p}</div>`).join("");
}
function downloadCSV(){
let csv="Order\n"+orders.join("\n");
let a=document.createElement("a");
a.href=URL.createObjectURL(new Blob([csv]));
a.download="orders.csv"; a.click();
}
function toggleLang(){
if(lang=="bn"){
title.innerText="Kenakata.com";
loginTitle.innerText="Login";
signupTitle.innerText="Signup";
lang="en";
}else location.reload();
}
render();
</script>
