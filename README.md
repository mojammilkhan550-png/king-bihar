<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>SmartBuy AI | MODASSIR KHAN</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#f3f6fc;color:#182230}
header{background:#111a2d;color:white;padding:15px;position:sticky;top:0;z-index:5}
.brand{font-size:24px;font-weight:bold;margin-bottom:12px}
.brand span{color:#a49aff}
.search{display:flex;gap:7px}
input,select,button{font:inherit;padding:12px;border-radius:10px;border:1px solid #d0d5dd}
.search input{width:100%;min-width:0}
button{cursor:pointer;background:#635bff;color:white;border:0;font-weight:bold}
nav{display:flex;gap:8px;overflow:auto;padding-top:12px}
nav button{white-space:nowrap;background:#29344c}
.hero{padding:32px 20px;color:white;background:linear-gradient(120deg,#111a2d,#303c91,#155363)}
.hero h1{font-size:clamp(32px,7vw,52px);margin:10px 0}
.hero p{line-height:1.6}
.wrap{max-width:1200px;margin:auto;padding:18px}
.panel{background:white;padding:18px;border-radius:16px;margin-bottom:18px;box-shadow:0 5px 25px #1822300c}
h2{margin-top:0}
.categories{display:flex;gap:8px;overflow:auto;margin:12px 0}
.categories button{background:#eeedff;color:#4c43c7;white-space:nowrap}
.grid{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:14px}
.product{background:white;border:1px solid #e0e5ee;border-radius:14px;overflow:hidden}
.photo{height:145px;display:grid;place-items:center;background:#eef0ff;font-size:55px}
.product img{width:100%;height:100%;object-fit:contain}
.details{padding:13px}
.details h3{font-size:16px;min-height:38px}
.price{font-size:22px;font-weight:bold;color:#4c43c7}
.muted{font-size:12px;color:#667085;line-height:1.6}
.actions{display:flex;gap:7px;flex-wrap:wrap;margin-top:12px}
.actions button,.actions a{flex:1;text-align:center;text-decoration:none;padding:10px 7px;border-radius:8px;font-size:12px}
.secondary{background:#edf0f7;color:#243047}
a{color:#4c43c7}
#results{margin:12px 0;color:#667085}
.external{display:flex;gap:8px;flex-wrap:wrap;margin-top:12px}
.external a{display:inline-block;padding:10px;background:#eeedff;border-radius:9px;font-weight:bold}
input,select{max-width:100%}
.field{display:grid;gap:6px;margin:12px 0}
.field input{width:100%}
canvas{width:100%;height:220px;border:1px solid #e4e7ec;border-radius:10px}
#detailImage{font-size:65px}
footer{text-align:center;padding:30px;color:#667085;font-size:12px}
@media(max-width:750px){.grid{grid-template-columns:repeat(2,minmax(0,1fr))}.wrap{padding:12px}.hero{padding:25px 16px}.photo{height:115px}.details{padding:10px}.details h3{font-size:14px}}
@media(max-width:390px){.grid{grid-template-columns:1fr}.photo{height:180px}}
</style>
</head>
<body>
<header>
 <div class="brand">SmartBuy <span>AI</span></div>
 <div class="search">
  <input id="q" placeholder="Search any product, e.g. iPhone 16"
   onkeydown="if(event.key==='Enter')search()">
  <button onclick="search()">Search</button>
 </div>
 <nav>
  <button onclick="home()">Discover</button>
  <button onclick="wishlistPage()">♡ Wishlist</button>
  <button onclick="showAdd()">＋ Add product</button>
  <button onclick="profile()">My Profile</button>
  <button onclick="assistant()">AI Assistant</button>
 </nav>
</header>

<section class="hero">
 <small>SMART SHOPPING • PRICE TRACKING</small>
 <h1>Find the right deal.<br><span style="color:#aaa3ff">Save more with AI.</span></h1>
 <p>Search products, compare saved prices, explore store links and track price history.</p>
 <button onclick="document.getElementById('catalog').scrollIntoView()">Explore products ↓</button>
</section>

<main class="wrap">
 <section class="panel">
  <h2>Discover products</h2>
  <div class="categories" id="categories"></div>
  <div style="display:flex;gap:8px;flex-wrap:wrap">
   <select id="sort" onchange="render()">
    <option value="featured">Featured</option>
    <option value="low">Price: low to high</option>
    <option value="high">Price: high to low</option>
   </select>
   <button class="secondary" onclick="showAdd()">＋ Add your product</button>
  </div>
  <div id="results"></div>
  <div id="catalog" class="grid"></div>
  <p class="muted">Sample prices are illustrative, not verified live offers.</p>
 </section>

 <section class="panel" id="productDetail" hidden>
  <h2>Product details</h2>
  <div id="detailImage"></div>
  <h3 id="detailName"></h3>
  <div id="detailPrice" class="price"></div>
  <p id="detailDesc"></p>
  <p id="detailLaunch" class="muted"></p>
  <div class="external" id="storeLinks"></div>
  <h3>Price history</h3>
  <p class="muted">Only saved history points are shown. Initial demo points are illustrative.</p>
  <canvas id="chart" width="700" height="240"></canvas>
  <div class="field">
   <label>Record a price (₹)</label>
   <input type="number" id="newPrice" min="1" placeholder="Enter observed price">
   <button onclick="recordPrice()">Save price history</button>
  </div>
 </section>

 <section class="panel" id="addPanel" hidden>
  <h2>Add a product</h2>
  <div class="field"><label>Product name</label><input id="n" placeholder="iPhone 16"></div>
  <div class="field"><label>Price in rupees</label><input id="p" type="number" min="1" placeholder="69900"></div>
  <div class="field"><label>Category</label>
   <select id="cat"><option>Mobiles</option><option>Laptops</option><option>Audio</option><option>TV</option><option>Fashion</option><option>Home</option><option>Other</option></select>
  </div>
  <div class="field"><label>Image URL (optional)</label><input id="img" type="url" placeholder="https://..."></div>
  <div class="field"><label>Launch date (optional)</label><input id="launch" type="date"></div>
  <div class="field"><label>Store product URL (optional)</label><input id="url" type="url" placeholder="https://..."></div>
  <div class="field"><label>Specifications / description</label><input id="desc" placeholder="Storage, colour, RAM..."></div>
  <button onclick="addProduct()">Save product</button>
 </section>

 <section class="panel" id="wishlistPanel" hidden>
  <h2>♡ My Wishlist</h2><div id="wishlist" class="grid"></div>
 </section>

 <section class="panel" id="profilePanel" hidden>
  <h2>My Profile</h2>
  <div class="field"><label>Name</label><input id="profileName"></div>
  <div class="field"><label>Profile ID</label><input id="profileId"></div>
  <div class="field"><label>Upload photo</label><input id="profilePhoto" type="file" accept="image/*"></div>
  <button onclick="saveProfile()">Save profile</button>
  <p class="muted">This is a local profile, not secure account authentication.</p>
 </section>

 <section class="panel" id="aiPanel" hidden>
  <h2>✦ Shopping Assistant</h2>
  <p class="muted">This demo recommends products from the saved catalogue. It is not connected to a real AI API yet.</p>
  <div class="field"><input id="ask" placeholder="Suggest a phone under ₹20000"></div>
  <button onclick="askAI()">Get suggestions</button>
  <p id="answer"></p>
 </section>
</main>
<footer>SmartBuy AI • MODASSIR KHAN<br>Verify prices, seller, warranty and specifications on the store site.</footer>

<script>
const $=id=>document.getElementById(id);
const demo=[
 {id:1,name:"Apple iPhone 16 128GB",price:69900,cat:"Mobiles",icon:"📱",launch:"2024-09-20",desc:"Demo listing. Check exact specifications and current price.",url:"https://www.amazon.in/s?k=iPhone+16",history:[{d:"2026-08-01",p:72900},{d:"2026-09-01",p:70900},{d:"2026-10-01",p:69900}]},
 {id:2,name:"Apple iPhone 15 128GB",price:59900,cat:"Mobiles",icon:"📱",launch:"2023-09-22",desc:"Demo listing. Verify seller and warranty.",url:"https://www.amazon.in/s?k=iPhone+15",history:[{d:"2026-08-01",p:62900},{d:"2026-09-01",p:61900},{d:"2026-10-01",p:59900}]},
 {id:3,name:"Samsung Galaxy S24",price:54999,cat:"Mobiles",icon:"📱",launch:"2024-01-31",desc:"Demo listing. Check configuration before purchase.",url:"https://www.amazon.in/s?k=Samsung+Galaxy+S24",history:[{d:"2026-08-01",p:57999},{d:"2026-09-01",p:55999},{d:"2026-10-01",p:54999}]},
 {id:4,name:"OnePlus 12R",price:39999,cat:"Mobiles",icon:"📱",launch:"2024-01-23",desc:"Demo listing.",url:"https://www.flipkart.com/search?q=OnePlus+12R",history:[]},
 {id:5,name:"HP Pavilion Laptop",price:52990,cat:"Laptops",icon:"💻",desc:"Example listing.",url:"https://www.amazon.in/s?k=HP+Pavilion+Laptop",history:[]},
 {id:6,name:"Sony Wireless Headphones",price:3990,cat:"Audio",icon:"🎧",desc:"Example listing.",url:"https://www.flipkart.com/search?q=Sony+headphones",history:[]},
 {id:7,name:"Smart Watch",price:1799,cat:"Accessories",icon:"⌚",desc:"Example listing.",url:"https://www.amazon.in/s?k=smart+watch",history:[]},
 {id:8,name:"Smart TV",price:24999,cat:"TV",icon:"📺",desc:"Example listing.",url:"https://www.flipkart.com/search?q=smart+tv",history:[]}
];
let products;
try{products=JSON.parse(localStorage.getItem("sb_v3_products"))||demo}catch(e){products=demo}
let wishes=JSON.parse(localStorage.getItem("sb_v3_wishes")||"[]");
let selected=null,category="All";
function persist(){localStorage.setItem("sb_v3_products",JSON.stringify(products));localStorage.setItem("sb_v3_wishes",JSON.stringify(wishes))}
function rupees(n){return "₹"+Number(n).toLocaleString("en-IN")}
function esc(s){return String(s||"").replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]))}
function externalLinks(query){let q=encodeURIComponent(query);return `<a target="_blank" rel="noopener" href="https://www.google.com/search?tbm=shop&q=${q}">Google Shopping ↗</a><a target="_blank" rel="noopener" href="https://www.amazon.in/s?k=${q}">Amazon ↗</a><a target="_blank" rel="noopener" href="https://www.flipkart.com/search?q=${q}">Flipkart ↗</a>`}
function showOnly(id){["productDetail","addPanel","wishlistPanel","profilePanel","aiPanel"].forEach(x=>$(x).hidden=x!==id)}
function home(){showOnly("");$("productDetail").hidden=true;render()}
function categories(){let cats=["All",...new Set(products.map(p=>p.cat))];$("categories").innerHTML=cats.map(c=>`<button onclick="category='${esc(c)}';render()">${esc(c)}</button>`).join("")}
function card(p){return `<article class="product"><div class="photo">${p.img?`<img src="${esc(p.img)}" alt="${esc(p.name)}" onerror="this.parentElement.textContent='🛍️'">`:p.icon||"🛍️"}</div><div class="details"><div class="muted">${esc(p.cat)}</div><h3>${esc(p.name)}</h3><div class="price">${rupees(p.price)}</div><div class="muted">${esc(p.desc)}</div><div class="actions"><button onclick="wish(${p.id})" class="secondary">${wishes.includes(p.id)?"♥ Saved":"♡ Wishlist"}</button><button onclick="details(${p.id})">Details</button></div><div class="external"><a target="_blank" rel="noopener" href="https://www.amazon.in/s?k=${encodeURIComponent(p.name)}">Amazon</a><a target="_blank" rel="noopener" href="https://www.flipkart.com/search?q=${encodeURIComponent(p.name)}">Flipkart</a></div></div></article>`}
function render(){categories();let q=$("q").value.trim().toLowerCase();let list=products.filter(p=>(category==="All"||p.cat===category)&&(!q||(p.name+" "+p.cat+" "+(p.desc||"")).toLowerCase().includes(q)));if($("sort").value==="low")list.sort((a,b)=>a.price-b.price);if($("sort").value==="high")list.sort((a,b)=>b.price-a.price);$("results").textContent=list.length+" products found";$("catalog").innerHTML=list.map(card).join("");if(q&&!list.length){$("catalog").innerHTML=`<div class="panel" style="grid-column:1/-1"><h3>No exact match in your catalogue.</h3><p>Search this product on shopping sites:</p><div class="external">${externalLinks(q)}</div><p class="muted">These links show external search results. Prices are not fetched into this website.</p></div>`}}
function search(){render();$("catalog").scrollIntoView({behavior:"smooth",block:"start"})}
$("q").addEventListener("input",render);
function wish(id){wishes=wishes.includes(id)?wishes.filter(x=>x!==id):[...wishes,id];persist();render()}
function wishlistPage(){showOnly("wishlistPanel");$("wishlist").innerHTML=products.filter(p=>wishes.includes(p.id)).map(card).join("")||"No saved products yet."}
function details(id){selected=products.find(p=>p.id===id);showOnly("productDetail");$("detailImage").textContent=selected.icon||"🛍️";$("detailName").textContent=selected.name;$("detailPrice").textContent=rupees(selected.price);$("detailDesc").textContent=selected.desc||"No specifications provided.";$("detailLaunch").textContent="Launch date: "+(selected.launch||"Not provided");$("storeLinks").innerHTML=externalLinks(selected.name)+(selected.url?`<a target="_blank" rel="noopener" href="${esc(selected.url)}">Product link ↗</a>`:"");drawChart();$("productDetail").scrollIntoView({behavior:"smooth"})}
function drawChart(){let c=$("chart"),ctx=c.getContext("2d"),w=c.width,h=c.height;ctx.clearRect(0,0,w,h);let points=selected.history||[];if(!points.length){ctx.fillStyle="#667085";ctx.font="16px Arial";ctx.fillText("No saved price history yet",20,35);return}let vals=points.map(x=>x.p),min=Math.min(...vals)*.95,max=Math.max(...vals)*1.05;if(min===max)max=min+1;ctx.strokeStyle="#d7dce5";ctx.fillStyle="#667085";ctx.font="12px Arial";for(let i=0;i<5;i++){let y=20+i*(h-55)/4;ctx.beginPath();ctx.moveTo(45,y);ctx.lineTo(w-15,y);ctx.stroke();ctx.fillText(rupees(Math.round(max-(max-min)*i/4)),2,y+4)}ctx.strokeStyle="#635bff";ctx.lineWidth=3;ctx.beginPath();points.forEach((p,i)=>{let x=50+i*(w-80)/Math.max(1,points.length-1),y=20+(max-p.p)/(max-min)*(h-55);i?ctx.lineTo(x,y):ctx.moveTo(x,y)});ctx.stroke();points.forEach((p,i)=>{let x=50+i*(w-80)/Math.max(1,points.length-1),y=20+(max-p.p)/(max-min)*(h-55);ctx.fillStyle="#635bff";ctx.beginPath();ctx.arc(x,y,4,0,Math.PI*2);ctx.fill();ctx.fillStyle="#667085";ctx.fillText(p.d.slice(5),x-18,h-12)})}
function recordPrice(){if(!selected)return;let p=Number($("newPrice").value);if(!p||p<1){alert("Enter a valid price");return}selected.history=selected.history||[];selected.history.push({d:new Date().toISOString().slice(0,10),p});selected.price=p;persist();details(selected.id)}
function showAdd(){showOnly("addPanel")}
function addProduct(){let name=$("n").value.trim(),price=Number($("p").value);if(!name||!price||price<1){alert("Enter product name and valid price");return}products.unshift({id:Date.now(),name,price,cat:$("cat").value,icon:"🛍️",img:$("img").value,launch:$("launch").value,url:$("url").value,desc:$("desc").value,history:[{d:new Date().toISOString().slice(0,10),p:price}]});persist();["n","p","img","launch","url","desc"].forEach(x=>$(x).value="");home();alert("Product saved")}
function profile(){showOnly("profilePanel");let p=JSON.parse(localStorage.getItem("sb_v3_profile")||'{"name":"MODASSIR KHAN","id":"MODASSIR-001"}');$("profileName").value=p.name;$("profileId").value=p.id}
function saveProfile(){let p={name:$("profileName").value.trim(),id:$("profileId").value.trim()};localStorage.setItem("sb_v3_profile",JSON.stringify(p));alert("Profile saved on this browser")}
function assistant(){showOnly("aiPanel")}
function askAI(){let q=$("ask").value.toLowerCase(),m=q.match(/(?:under|below|within)\s*₹?\s*([\d,]+)/),budget=m?Number(m[1].replace(/,/g,"")):Infinity;let list=products.filter(p=>p.price<=budget).sort((a,b)=>a.price-b.price).slice(0,5);$("answer").textContent=list.length?"Local catalogue suggestions: "+list.map(p=>p.name+" ("+rupees(p.price)+")").join("; ")+". This is a demo assistant, not real AI.":"No matching saved products. Try another budget or search external stores."}
categories();render();
</script>
</body>
</html>
