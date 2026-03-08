# Pro5
<!DOCTYPE html>
<html>
<head>
<title>Smart Product Recommendation System</title>

<style>

body{
font-family:Arial;
margin:0;
background:#eef2f7;
}

header{
background:#2563eb;
color:white;
padding:18px;
text-align:center;
font-size:24px;
}

.container{
padding:25px;
text-align:center;
}

.page{
display:none;
animation:fade 0.7s ease;
}

@keyframes fade{
from{opacity:0;transform:translateY(10px);}
to{opacity:1;transform:translateY(0);}
}

input,select{
padding:10px;
margin:6px;
border-radius:6px;
border:1px solid #ccc;
width:220px;
}

button{
padding:10px 15px;
border:none;
background:#2563eb;
color:white;
border-radius:6px;
cursor:pointer;
margin:6px;
}

button:hover{
background:#1e40af;
}

.products{
display:grid;
grid-template-columns:repeat(auto-fill,minmax(250px,1fr));
gap:20px;
margin-top:25px;
}

.card{
background:white;
border-radius:12px;
padding:15px;
box-shadow:0 4px 15px rgba(0,0,0,0.1);
}

.card img{
width:100%;
height:170px;
object-fit:cover;
border-radius:8px;
}

.pricebox{
background:#eef2ff;
padding:6px;
margin-top:5px;
border-radius:5px;
}

.rating{color:#f59e0b;}

.best{
background:#16a34a;
color:white;
padding:4px;
border-radius:5px;
display:inline-block;
margin-top:5px;
}

</style>
</head>

<body>

<header>Smart Product Recommendation System</header>

<!-- ROLE PAGE -->

<div id="rolePage" class="container page" style="display:block">
<h2>Select Login</h2>
<button onclick="openUser()">User</button>
<button onclick="openAdmin()">Admin</button>
</div>

<!-- USER LOGIN -->

<div id="userLoginPage" class="container page">
<h2>User Login</h2>
<input id="mobile" placeholder="Mobile Number"><br>

<select id="location">
<option value="">Select Location</option>
<option>Chennai</option>
<option>Madurai</option>
<option>Trichy</option>
<option>Coimbatore</option>
</select><br>

<button onclick="login()">Enter Website</button>
</div>

<!-- ADMIN LOGIN -->

<div id="adminLoginPage" class="container page">
<h2>Admin Login</h2>
<input id="adminUser" placeholder="Admin Username"><br>
<input id="adminPass" type="password" placeholder="Password"><br>
<button onclick="adminLogin()">Login</button>
</div>

<!-- USER PAGE -->

<div id="mainPage" class="container page">

<h3>Search Product</h3>

<input id="searchInput" placeholder="Search mobile, tv, face wash, shirt">
<button onclick="searchProduct()">Search</button>
<button onclick="resetSearch()">Reset</button>

<div id="dynamicFilters"></div>

<div class="products" id="productList"></div>

</div>

<!-- ADMIN -->

<div id="adminPage" class="container page">
<h3>Admin Add Product</h3>

<input id="pname" placeholder="Product Name"><br>
<input id="ptype" placeholder="Product Type"><br>
<input id="pimage" placeholder="Image URL"><br>

<button onclick="addProduct()">Add Product</button>

</div>

<script>

/* USER STORAGE */

function saveUser(mobile,location){
localStorage.setItem("user_"+mobile,JSON.stringify({mobile,location}))
}

/* PRODUCT DATA */

let products=[

{
name:"iPhone 14",
type:"mobile",
brand:"Apple",
image:"https://images.unsplash.com/photo-1603898037225",
rating:"4.8",
review:"Best camera phone",
online:{amazon:78999,flipkart:78499},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[
{name:"Reliance Digital",price:79000,map:"https://maps.google.com/?q=Reliance+Digital"}
]
},

{
name:"Samsung Galaxy S23",
type:"mobile",
brand:"Samsung",
image:"https://images.unsplash.com/photo-1610945265064",
rating:"4.7",
review:"Flagship performance",
online:{amazon:70999,flipkart:69999},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[
{name:"Mobile World",price:70500,map:"https://maps.google.com/?q=Mobile+Shop"}
]
},

{
name:"Realme 11 Pro",
type:"mobile",
brand:"Realme",
image:"https://images.unsplash.com/photo-1598327105666",
rating:"4.5",
review:"Budget 5G phone",
online:{amazon:22999,flipkart:21999},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[
{name:"Poorvika Mobiles",price:21500,map:"https://maps.google.com/?q=Poorvika"}
]
},

{
name:"LG Smart TV",
type:"appliance",
brand:"LG",
image:"https://images.unsplash.com/photo-1593784991095",
rating:"4.6",
review:"4K Smart TV",
online:{amazon:45999,flipkart:44999},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[
{name:"Reliance Digital",price:44500,map:"https://maps.google.com/?q=Reliance+Digital"}
]
},

{
name:"Sony Bravia TV",
type:"appliance",
brand:"Sony",
image:"https://images.unsplash.com/photo-1601944177325",
rating:"4.7",
review:"Premium smart TV",
online:{amazon:65999,flipkart:64999},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[
{name:"Sony Store",price:64000,map:"https://maps.google.com/?q=Sony+Store"}
]
},

{
name:"Nivea Face Wash",
type:"beauty",
skin:"Normal",
image:"https://images.unsplash.com/photo-1596755389378",
rating:"4.4",
review:"Best for normal skin",
online:{amazon:249,flipkart:239},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[
{name:"Apollo Pharmacy",price:240,map:"https://maps.google.com/?q=Apollo+Pharmacy"}
]
},

{
name:"Himalaya Face Wash",
type:"beauty",
skin:"Dry",
image:"https://images.unsplash.com/photo-1608248597279",
rating:"4.3",
review:"Best for dry skin",
online:{amazon:199,flipkart:189},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[
{name:"Medical Shop",price:185,map:"https://maps.google.com/?q=Medical+Shop"}
]
},

{
name:"Cricket Bat",
type:"sports",
color:"Red",
image:"https://images.unsplash.com/photo-1599058917765",
rating:"4.5",
review:"Professional cricket bat",
online:{amazon:1500,flipkart:1450},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[
{name:"Sports Hub",price:1400,map:"https://maps.google.com/?q=Sports+Shop"}
]
},

{
name:"Men Shirt",
type:"clothing",
size:"L",
image:"https://images.unsplash.com/photo-1520975698519",
rating:"4.4",
review:"Stylish shirt",
online:{amazon:999,flipkart:899},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[
{name:"Fashion Store",price:850,map:"https://maps.google.com/?q=Clothing+Shop"}
]
}

]

/* LOGIN FUNCTIONS */

function openUser(){
rolePage.style.display="none"
userLoginPage.style.display="block"
}

function openAdmin(){
rolePage.style.display="none"
adminLoginPage.style.display="block"
}

function login(){

let mobile=document.getElementById("mobile").value
let location=document.getElementById("location").value

if(mobile.length>=10 && location!=""){

saveUser(mobile,location)

userLoginPage.style.display="none"
mainPage.style.display="block"

displayProducts(products)

}else{
alert("Enter valid details")
}

}

function adminLogin(){

if(adminUser.value=="admin" && adminPass.value=="1234"){

adminLoginPage.style.display="none"
adminPage.style.display="block"

}else{
alert("Invalid login")
}

}

/* DISPLAY */

function displayProducts(list){

let html=""

list.forEach(p=>{

let shopHTML=""

p.shops.forEach(s=>{
shopHTML+=`
<div class="pricebox">
🏪 ${s.name} ₹${s.price}
<br>
<a href="${s.map}" target="_blank">View Map</a>
</div>`
})

html+=`

<div class="card">

<img src="${p.image}">

<h4>${p.name}</h4>

<p class="rating">⭐ ${p.rating}</p>

<p>${p.review}</p>

<div class="pricebox">
Amazon ₹${p.online.amazon}
<br>
<a href="${p.amazon}" target="_blank">Buy Amazon</a>
</div>

<div class="pricebox">
Flipkart ₹${p.online.flipkart}
<br>
<a href="${p.flipkart}" target="_blank">Buy Flipkart</a>
</div>

${shopHTML}

</div>

`

})

productList.innerHTML=html

}

/* SEARCH */

function searchProduct(){

let q=searchInput.value.toLowerCase()

let result=products.filter(p=>JSON.stringify(p).toLowerCase().includes(q))

displayProducts(result)

showFilters(q)

}

/* FILTERS */

function showFilters(q){

let html=""

if(q.includes("mobile")||q.includes("phone")){

html=`
<h4>Select Mobile Brand</h4>
<button onclick="filterBrand('Apple')">Apple</button>
<button onclick="filterBrand('Samsung')">Samsung</button>
<button onclick="filterBrand('Realme')">Realme</button>
`
}

else if(q.includes("tv")||q.includes("fridge")||q.includes("washing")){

html=`
<h4>Select Electronics Brand</h4>
<button onclick="filterBrand('LG')">LG</button>
<button onclick="filterBrand('Samsung')">Samsung</button>
<button onclick="filterBrand('Sony')">Sony</button>
`
}

else if(q.includes("face")||q.includes("cream")||q.includes("soap")){

html=`
<h4>Select Skin Type</h4>
<button onclick="filterSkin('Normal')">Normal</button>
<button onclick="filterSkin('Dry')">Dry</button>
`
}

else if(q.includes("sports")||q.includes("bat")||q.includes("ball")){

html=`
<h4>Select Color</h4>
<button onclick="filterColor('Red')">Red</button>
<button onclick="filterColor('Yellow')">Yellow</button>
`
}

else if(q.includes("shirt")||q.includes("dress")){

html=`
<h4>Select Size</h4>
<button onclick="filterSize('S')">S</button>
<button onclick="filterSize('M')">M</button>
<button onclick="filterSize('L')">L</button>
<button onclick="filterSize('XL')">XL</button>
`
}

dynamicFilters.innerHTML=html

}

/* FILTER FUNCTIONS */

function filterBrand(b){
displayProducts(products.filter(p=>p.brand==b))
}

function filterSkin(s){
displayProducts(products.filter(p=>p.skin==s))
}

function filterColor(c){
displayProducts(products.filter(p=>p.color==c))
}

function filterSize(s){
displayProducts(products.filter(p=>p.size==s))
}

/* ADMIN */

function addProduct(){

let name=pname.value
let type=ptype.value
let image=pimage.value

products.push({
name:name,
type:type,
image:image,
rating:"4",
review:"New product",
online:{amazon:100,flipkart:95},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[{name:"Local Shop",price:90,map:"https://maps.google.com"}]
})

alert("Product Added")

}

</script>

</body>
</html>
