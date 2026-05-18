<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Flam Be</title>

<style>

*{
  margin:0;
  padding:0;
  box-sizing:border-box;
  font-family:Arial, sans-serif;
}

body{
  background:#f5f5f5;
}

/* HEADER */

header{
  background:#ff5722;
  color:white;
  padding:18px;
  text-align:center;
  position:sticky;
  top:0;
}

.logo-box{
  display:flex;
  align-items:center;
  justify-content:center;
  gap:12px;
}

.logo{
  width:60px;
  height:60px;
  background:white;
  color:#ff5722;
  border-radius:50%;
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:30px;
}

header h1{
  font-size:32px;
}

.tagline{
  font-size:14px;
}

/* MAIN */

.container{
  width:95%;
  max-width:1200px;
  margin:auto;
  padding:20px;
}

h2{
  margin-bottom:15px;
  color:#333;
}

.food-grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
  gap:20px;
}

/* FOOD CARDS */

.card{
  background:white;
  border-radius:14px;
  overflow:hidden;
  box-shadow:0 4px 10px rgba(0,0,0,0.1);
}

.card img{
  width:100%;
  height:180px;
  object-fit:cover;
}

.card-content{
  padding:15px;
}

.food-name{
  font-size:20px;
  margin-bottom:8px;
}

.price{
  color:#ff5722;
  font-size:18px;
  margin-bottom:12px;
}

/* BUTTONS */

button{
  border:none;
  padding:10px;
  border-radius:8px;
  cursor:pointer;
  width:100%;
  font-weight:bold;
}

.order-btn{
  background:#ff5722;
  color:white;
}

.delete-btn{
  background:red;
  color:white;
  margin-top:10px;
}

.admin-btn{
  background:#222;
  color:white;
  margin-top:10px;
}

.open-admin-btn{
  background:#222;
  color:white;
  margin-top:20px;
}

/* PANELS */

.admin-panel,
.orders{
  background:white;
  margin-top:40px;
  padding:20px;
  border-radius:14px;
  box-shadow:0 4px 10px rgba(0,0,0,0.1);
}

.admin-panel{
  display:none;
}

.admin-panel input{
  width:100%;
  padding:12px;
  margin-bottom:12px;
  border:1px solid #ccc;
  border-radius:8px;
}

.order-item{
  border-bottom:1px solid #ddd;
  padding:12px 0;
}

</style>
</head>

<body>

<header>

  <div class="logo-box">

    <div class="logo">
      🔥
    </div>

    <div>
      <h1>Flam Be</h1>

      <p class="tagline">
        Hot & Fresh Food
      </p>
    </div>

  </div>

</header>

<div class="container">

  <h2>Menu</h2>

  <div class="food-grid" id="foodGrid"></div>

  <!-- OWNER LOGIN -->

  <button
    class="open-admin-btn"
    onclick="openAdmin()"
    id="loginBtn"
  >
    Owner Login
  </button>

  <!-- ADMIN PANEL -->

  <div class="admin-panel" id="adminPanel">

    <h2>Owner Admin Panel</h2>

    <input
      type="text"
      id="foodName"
      placeholder="Food Name"
    >

    <input
      type="number"
      id="foodPrice"
      placeholder="Food Price"
    >

    <input
      type="text"
      id="foodImage"
      placeholder="Optional Custom Image URL"
    >

    <button
      class="admin-btn"
      onclick="addFood()"
    >
      Add Food
    </button>

    <button
      class="admin-btn"
      onclick="changePassword()"
    >
      Change Password
    </button>

  </div>

  <!-- ORDERS -->

  <div
    class="orders"
    id="ordersSection"
    style="display:none;"
  >

    <h2>Customer Orders</h2>

    <div id="ordersList"></div>

  </div>

</div>

<script>

let foods = JSON.parse(localStorage.getItem("foods")) || [

  {
    name:"Burger",
    price:50,
    image:"https://images.unsplash.com/photo-1568901346375-23c9450c58cd"
  },

  {
    name:"Pizza",
    price:80,
    image:"https://images.unsplash.com/photo-1513104890138-7c749659a591"
  },

  {
    name:"Chicken",
    price:70,
    image:"https://images.unsplash.com/photo-1604503468506-a8da13d82791"
  }

];

let orders =
JSON.parse(localStorage.getItem("orders")) || [];

let ownerPassword =
localStorage.getItem("ownerPassword") || "flambe123";

let isOwner = false;

const foodGrid =
document.getElementById("foodGrid");

const ordersList =
document.getElementById("ordersList");

/* OWNER LOGIN + LOGOUT */

function openAdmin(){

  if(isOwner){

    isOwner = false;

    document.getElementById("adminPanel")
    .style.display = "none";

    document.getElementById("ordersSection")
    .style.display = "none";

    document.getElementById("loginBtn")
    .innerText = "Owner Login";

    displayFoods();

    alert("Logged Out");

    return;
  }

  const password =
  prompt("Enter Owner Password");

  if(password === ownerPassword){

    isOwner = true;

    document.getElementById("adminPanel")
    .style.display = "block";

    document.getElementById("ordersSection")
    .style.display = "block";

    document.getElementById("loginBtn")
    .innerText = "Owner Logout";

    displayFoods();

    alert("Welcome Owner");

  }

  else{

    alert("Wrong Password");

  }

}

/* CHANGE PASSWORD */

function changePassword(){

  const oldPassword =
  prompt("Enter Current Password");

  if(oldPassword !== ownerPassword){

    alert("Wrong Current Password");

    return;
  }

  const newPassword =
  prompt("Enter New Password");

  if(!newPassword){

    alert("Password not changed");

    return;
  }

  ownerPassword = newPassword;

  localStorage.setItem(
    "ownerPassword",
    newPassword
  );

  alert("Password Changed Successfully");

}

/* SAVE */

function saveFoods(){

  localStorage.setItem(
    "foods",
    JSON.stringify(foods)
  );

}

function saveOrders(){

  localStorage.setItem(
    "orders",
    JSON.stringify(orders)
  );

}

/* DISPLAY FOOD */

function displayFoods(){

  foodGrid.innerHTML = "";

  foods.forEach((food,index)=>{

    foodGrid.innerHTML += `

      <div class="card">

        <img src="${food.image}">

        <div class="card-content">

          <div class="food-name">
            ${food.name}
          </div>

          <div class="price">
            K${food.price}
          </div>

          ${
            !isOwner
            ?
            `
            <button
              class="order-btn"
              onclick="orderFood('${food.name}',${food.price})"
            >
              Order Now
            </button>
            `
            :
            ""
          }

          ${
            isOwner
            ?
            `
            <button
              class="delete-btn"
              onclick="deleteFood(${index})"
            >
              Delete Food
            </button>
            `
            :
            ""
          }

        </div>

      </div>

    `;
  });

}

/* ADD FOOD */

function addFood(){

  const name =
  document.getElementById("foodName").value;

  const price =
  document.getElementById("foodPrice").value;

  let image =
  document.getElementById("foodImage").value;

  if(name === "" || price === ""){

    alert("Fill all fields");

    return;
  }

  /* AUTOMATIC FOOD IMAGES */

  if(name.toLowerCase().includes("burger")){

    image =
    "https://images.unsplash.com/photo-1568901346375-23c9450c58cd";

  }

  else if(name.toLowerCase().includes("pizza")){

    image =
    "https://images.unsplash.com/photo-1513104890138-7c749659a591";

  }

  else if(name.toLowerCase().includes("chicken")){

    image =
    "https://images.unsplash.com/photo-1604503468506-a8da13d82791";

  }

  else if(name.toLowerCase().includes("fries")){

    image =
    "https://images.unsplash.com/photo-1576107232684-1279f390859f";

  }

  foods.push({

    name:name,
    price:price,
    image:image

  });

  saveFoods();

  displayFoods();

  document.getElementById("foodName").value = "";
  document.getElementById("foodPrice").value = "";
  document.getElementById("foodImage").value = "";

}

/* DELETE FOOD */

function deleteFood(index){

  foods.splice(index,1);

  saveFoods();

  displayFoods();

}

/* ORDER FOOD */

function orderFood(name,price){

  const customer =
  prompt("Enter your name");

  if(!customer) return;

  orders.push({

    customer:customer,
    food:name,
    price:price

  });

  saveOrders();

  displayOrders();

  alert("Order placed successfully!");

}

/* DISPLAY ORDERS */

function displayOrders(){

  ordersList.innerHTML = "";

  if(orders.length === 0){

    ordersList.innerHTML =
    "No orders yet";

    return;
  }

  orders.forEach((order,index)=>{

    ordersList.innerHTML += `

      <div class="order-item">

        <strong>${order.customer}</strong>

        ordered

        <strong>${order.food}</strong>

        for K${order.price}

      </div>

    `;
  });

}

/* START */

displayFoods();

displayOrders();

</script>

</body>
</html>
