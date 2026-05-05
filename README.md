<html lang="es">
<head>
<meta charset="UTF-8">
<title>Drones RPAS</title>
<link rel="stylesheet" href="styles.css">
</head>
<body>
<h1>🚁 Ecommerce Drones RPAS</h1>
<select id="categoria">
  <option value="todos">Todos</option>
  <option value="drone">Drones</option>
  <option value="accesorio">Accesorios</option>
</select>
<div id="productos"></div>
<div class="carrito">
  <h2>🧾 Carrito (<span id="cantidad">0</span>)</h2>
  <ul id="lista-carrito"></ul>
  <p>Subtotal: <span id="subtotal">$0</span></p>
  <p>Total: <span id="total">$0</span></p>
  <h3>💳 Método de pago</h3>
  <select id="metodo-pago">
    <option value="">Seleccionar</option>
    <option value="whatsapp">WhatsApp</option>
  </select>

<select id="categoria">
  <option value="todos">Todos</option>
  <option value="recreacion">🚀 Recreación</option>
  <option value="cartografia">🗺️ Cartografía</option>
  <option value="agricultura">🌱 Agricultura</option>
  <option value="accesorio">🔧 Accesorios</option>
</select>
  <p id="metodo-seleccionado"></p>
  <button id="btn-whatsapp">📲 Pagar por WhatsApp</button>
</div>
<script src="app.js"></script>
</body>
</html>body{
  font-family: system-ui;
  background:
#f3f4e5;
  text-align:center;
}
.producto{
  border:1px solid #ddd;
  border-radius:8px;
  padding:10px;
  margin:10px;
  display:inline-block;
  width:220px;
}
.producto img{
  width:100%;
  height:140px;
  object-fit:cover;
}
button{
  background:
#0d6efd;
  color:white;
  border:none;
  padding:8px;
  border-radius:6px;
  cursor:pointer;
}
.carrito{
  margin-top:20px;
  padding:15px;
  border:1px solid #ccc;
  display:inline-block;
  text-align:left;
}
.qty button{
  background:#333;
}const productos = [
{ id:1, nombre:"Dron hexacoptero", precio:250000, categoria:"drone", img:"https://aeromind.pl/hpeciai/6bfb43cfdf4c6b3e433ae5a0b695f807/eng_pl_Yuneec-H520E-Hexacopter-Drone-Black-YUNH520EEUB-13792_1.jpg"},
{ id:2, nombre:"Dron Ala fija", precio:180000, categoria:"drone", img:"https://elvuelodeldrone.com/wp-content/uploads/2018/09/drone-industrial-Delair-UX11.jpg"},
{ id:3, nombre:"Camara drone", precio:300000, categoria:"accesorio", img:"https://elvuelodeldrone.com/wp-content/uploads/2019/01/Modulo-4KGC3-Camara-4K-impermeable-con-gimbal-3-ejes-800x600.jpg"},
];
let carrito = new Map();
const money = new Intl.NumberFormat('es-CO',{style:'currency',currency:'COP'});
const cont = document.getElementById("productos");
const lista = document.getElementById("lista-carrito");
function renderProductos(listaProd){
  cont.innerHTML="";
  listaProd.forEach(p=>{
    cont.innerHTML+=
    <div class="producto">
      <img src="${p.img}">
      <h3>${p.nombre}</h3>
      <p>${money.format(p.precio)}</p>
      <button data-add="${p.id}">Agregar</button>
    </div>;
  });
}
function pintarCarrito(){
  lista.innerHTML="";
  let total=0;
  let cantidad=0;
  carrito.forEach(p=>{
    total += p.precio * p.cantidad;
    cantidad += p.cantidad;
    lista.innerHTML+=
    <li>
      ${p.nombre} x${p.cantidad} - ${money.format(p.precio*p.cantidad)}
      <div class="qty">
        <button data-dec="${p.id}">-</button>
        <button data-inc="${p.id}">+</button>
        <button data-del="${p.id}">❌</button>
      </div>
    </li>;
  });
  document.getElementById("total").textContent = money.format(total);
  document.getElementById("subtotal").textContent = money.format(total);
  document.getElementById("cantidad").textContent = cantidad;
}
function add(id){
  const p = productos.find(x=>x.id==id);
  if(carrito.has(id)){
    carrito.get(id).cantidad++;
  }else{
    carrito.set(id,{...p,cantidad:1});
  }
  pintarCarrito();
}
function inc(id){ add(id); }
function dec(id){
  let p = carrito.get(id);
  p.cantidad--;
  if(p.cantidad<=0) carrito.delete(id);
  pintarCarrito();
}
function del(id){
  carrito.delete(id);
  pintarCarrito();
}
document.addEventListener("click",e=>{
  if(e.target.dataset.add) add(e.target.dataset.add);
  if(e.target.dataset.inc) inc(e.target.dataset.inc);
  if(e.target.dataset.dec) dec(e.target.dataset.dec);
  if(e.target.dataset.del) del(e.target.dataset.del);
});
document.getElementById("categoria").addEventListener("change",e=>{
  let val=e.target.value;
  if(val=="todos") renderProductos(productos);
  else renderProductos(productos.filter(p=>p.categoria==val));
});
document.getElementById("metodo-pago").addEventListener("change",e=>{
  document.getElementById("metodo-seleccionado").textContent =
    "Método seleccionado: " + e.target.value;
});
document.getElementById("btn-whatsapp").addEventListener("click",()=>{
  if(carrito.size===0){
    alert("Carrito vacío");
    return;
  }
  let mensaje="Hola, quiero comprar:%0A";
  carrito.forEach(p=>{
    mensaje+=- ${p.nombre} x${p.cantidad}%0A;
  });
  mensaje+=Total: ${document.getElementById("total").textContent};
  window.open(https://wa.me/573227321612?text=${mensaje});
});
renderProductos(productos);
