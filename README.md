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
  background:#f4f7f2;
  color:#222;
}

/* HEADER */

header{
  background:#1f7a4c;
  padding:20px;
  border-bottom-left-radius:30px;
  border-bottom-right-radius:30px;
  color:white;
}

.logo-box{
  display:flex;
  align-items:center;
  gap:12px;
}

.logo{
  width:55px;
  height:55px;
  background:white;
  color:#1f7a4c;
  border-radius:50%;
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:28px;
  font-weight:bold;
}

header h1{
  font-size:30px;
}

.tagline{
  opacity:0.9;
  font-size:14px;
  margin-top:3px;
}

/* MAIN */

.container{
  padding:20px;
}

.top-text{
  margin:20px 0;
}

.top-text h2{
  font-size:34px;
  line-height:1.2;
}

/* CATEGORY */

.categories{
  display:flex;
  gap:12px;
  overflow-x:auto;
  margin-bottom:25px;
}

.category{
  padding:12px 24px;
  background:white;
  border-radius:30px;
  border:2px solid #ddd;
  font-weight:bold;
  white-space:nowrap;
}

.active-category{
  background:#1f7a4c;
  color:white;
  border:none;
}

/* FOOD GRID */

.food-grid{
  display:grid;
  gap:25px;
}

/* CARD */

.card{
  background:white;
  border-radius:30px;
  overflow:hidden;
  box-shadow:0 5px 15px rgba(0,0,0,0.08);
}

.food-image{
  position:relative;
}

.food-image img{
  width:100%;
  height:250px;
  object-fit:cover;
}

.rating{
  position:absolute;
  top:15px;
  left:15px;
  background:#222;
  color:white;
  padding:8px 14px;
  border-radius:20px;
  font-weight:bold;
}

.time{
  position:absolute;
  top:15px;
  right:15px;
  background:#222;
  color:white;
  padding:8px 14px;
  border-radius:20px;
  font-weight:bold;
}

.card-content{
  padding:20px;
}

.food-name{
  font-size:28px;
  font-weight:bold;
  margin-bottom:10px;
}

.description{
  color:#666;
  line-height:1.5;
  margin-bottom:18px;
  font-size:17px;
}

.bottom-row{
  display:flex;
  align-items:center;
  justify-content:space-between;
}

.price{
  font-size:28px;
  font-weight:bold;
}

.order-btn{
  width:65px;
  height:65px;
  border:none;
  border-radius:50%;
  background:#1f7a4c;
  color:white;
  font-size:35px;
  cursor:pointer;
}

/* OWNER BUTTON */

.open-admin-btn{
  width:100%;
  padding:15px;
  margin-top:30px;
  border:none;
  border-radius:15px;
  background:#111;
  color:white;
  font-size:17px;
  font-weight:bold;
  cursor:pointer;
}

/* ADMIN PANEL */

.admin-panel,
.orders{
  background:white;
  margin-top:30px;
  padding:20px;
  border-radius:25px;
  display:none;
}

.admin-panel input{
  width:100%;
  padding:14px;
  margin-bottom:14px;
  border-radius:12px;
  border:1px solid #ccc;
}

.admin-btn{
  width:100%;
  padding:14px;
  border:none;
  border-radius:12px;
  background:#1f7a4c;
  color:white;
  font-size:16px;
  font-weight:bold;
  margin-top:10px;
}

.delete-btn{
  width:100%;
  padding:12px;
  border:none;
  border-radius:12px;
  background:red;
  color:white;
  font-weight:bold;
  margin-top:15px;
}

.order-item{
  padding:14px 0;
  border-bottom:1px solid #ddd;
}

/* MOBILE */

@media(max-width:600px){

  .food-name{
    font-size:22px;
  }

  .description{
    font-size:15px;
  }

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
        Fresh & Healthy Food
      </p>
    </div>

  </div>

</header>

<div class="container">

  <div class="top-text">

    <h2>
      What would you like<br>
      to eat today?
    </h2>

  </div>

  <!-- CATEGORIES -->

  <div class="categories">

    <div class="category active-category">
      All
    </div>

    <div class="category">
      Burgers
    </div>

    <div class="category">
      Pizza
    </div>

    <div class="category">
      Chicken
    </div>

  </div>

  <!-- FOODS -->

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

    <br>

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
      placeholder="Optional Image URL"
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
  >

    <h2>Customer Orders</h2>

    <br>

    <div id="ordersList"></div>

  </div>

</div>

<script>

let foods = JSON.parse(localStorage.getItem("foods")) || [

{
  name:"Classic Beef Burger",
  price:12.99,
  image:"https://images.unsplash.com/photo-1568901346375-23c9450c58cd",
  rating:"4.8",
  time:"15 min",
  description:"Juicy beef burger with cheese and sauce."
},

{
  name:"Margherita Pizza",
  price:14.99,
  image:"https://images.unsplash.com/photo-1513104890138-7c749659a591",
  rating:"4.7",
  time:"20 min",
  description:"Fresh mozzarella pizza with basil."
},

{
  name:"Chicken Bowl",
  price:10.99,
  image:"https://images.unsplash.com/photo-1547592180-85f173990554",
  rating:"4.9",
  time:"18 min",
  description:"Healthy chicken bowl with vegetables."
},

{
  name:"French Fries",
  price:5.99,
  image:"https://images.unsplash.com/photo-1576107232684-1279f390859f",
  rating:"4.6",
  time:"10 min",
  description:"Crispy golden fries."
},

{
  name:"Hot Dog",
  price:7.99,
  image:"https://images.unsplash.com/photo-1612392062798-8f5d5d6fef47",
  rating:"4.5",
  time:"12 min",
  description:"Hot dog with sausage and ketchup."
},

{
  name:"Chicken Pizza",
  price:16.99,
  image:"https://images.unsplash.com/photo-1594007654729-407eedc4be65",
  rating:"4.8",
  time:"22 min",
  description:"Pizza loaded with chicken toppings."
},

{
  name:"Double Burger",
  price:15.99,
  image:"https://images.unsplash.com/photo-1550547660-d9450f859349",
  rating:"4.9",
  time:"17 min",
  description:"Double meat burger with cheese."
},

{
  name:"Fried Chicken",
  price:13.99,
  image:"https://images.unsplash.com/photo-1562967916-eb82221dfb92",
  rating:"4.7",
  time:"16 min",
  description:"Crunchy fried chicken pieces."
},

{
  name:"Shawarma",
  price:11.99,
  image:"https://images.unsplash.com/photo-1529006557810-274b9b2fc783",
  rating:"4.8",
  time:"14 min",
  description:"Delicious chicken shawarma wrap."
},

{
  name:"Club Sandwich",
  price:9.99,
  image:"https://images.unsplash.com/photo-1528735602780-2552fd46c7af",
  rating:"4.6",
  time:"11 min",
  description:"Triple layer sandwich with fries."
},

{
  name:"Tacos",
  price:8.99,
  image:"https://images.unsplash.com/photo-1552332386-f8dd00dc2f85",
  rating:"4.7",
  time:"13 min",
  description:"Mexican tacos with beef filling."
},

{
  name:"Pasta",
  price:12.99,
  image:"https://images.unsplash.com/photo-1621996346565-e3dbc646d9a9",
  rating:"4.5",
  time:"19 min",
  description:"Creamy Italian pasta."
},

{
  name:"Chicken Wings",
  price:13.49,
  image:"https://images.unsplash.com/photo-1567620905732-2d1ec7ab7445",
  rating:"4.8",
  time:"15 min",
  description:"Spicy grilled chicken wings."
},

{
  name:"Salad Bowl",
  price:9.49,
  image:"https://images.unsplash.com/photo-1546069901-ba9599a7e63c",
  rating:"4.6",
  time:"9 min",
  description:"Healthy green salad bowl."
},

{
  name:"Ice Cream",
  price:6.99,
  image:"https://images.unsplash.com/photo-1563805042-7684c019e1cb",
  rating:"4.9",
  time:"5 min",
  description:"Sweet cold ice cream dessert."
},

{
  name:"Donuts",
  price:4.99,
  image:"https://images.unsplash.com/photo-1551024601-bec78aea704b",
  rating:"4.7",
  time:"6 min",
  description:"Chocolate glazed donuts."
},

{
  name:"Coffee",
  price:3.99,
  image:"https://images.unsplash.com/photo-1495474472287-4d71bcdd2085",
  rating:"4.8",
  time:"4 min",
  description:"Fresh hot coffee."
},

{
  name:"Milkshake",
  price:7.49,
  image:"https://images.unsplash.com/photo-1577805947697-89e18249d767",
  rating:"4.9",
  time:"7 min",
  description:"Creamy vanilla milkshake."
},

{
  name:"Steak",
  price:22.99,
  image:"https://images.unsplash.com/photo-1544025162-d76694265947",
  rating:"4.9",
  time:"25 min",
  description:"Premium grilled steak."
},

{
  name:"Seafood Pizza",
  price:18.99,
  image:"https://images.unsplash.com/photo-1513104890138-7c749659a591",
  rating:"4.7",
  time:"24 min",
  description:"Pizza topped with seafood."
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

/* LOGIN */

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

  }

  else{

    alert("Wrong Password");

  }

}

/* CHANGE PASSWORD */

function changePassword(){

  const oldPassword =
  prompt("Current Password");

  if(oldPassword !== ownerPassword){

    alert("Wrong Password");

    return;
  }

  const newPassword =
  prompt("New Password");

  if(!newPassword) return;

  ownerPassword = newPassword;

  localStorage.setItem(
    "ownerPassword",
    newPassword
  );

  alert("Password Changed");

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

        <div class="food-image">

          <img src="${food.image}">

          <div class="rating">
            ⭐ ${food.rating}
          </div>

          <div class="time">
            ⏱ ${food.time}
          </div>

        </div>

        <div class="card-content">

          <div class="food-name">
            ${food.name}
          </div>

          <div class="description">
            ${food.description}
          </div>

          <div class="bottom-row">

            <div class="price">
              $${food.price}
            </div>

            ${
              !isOwner
              ?
              `
              <button
                class="order-btn"
                onclick="orderFood('${food.name}',${food.price})"
              >
                +
              </button>
              `
              :
              ""
            }

          </div>

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

  foods.push({

    name:name,
    price:price,
    image:image || "https://images.unsplash.com/photo-1546069901-ba9599a7e63c",
    rating:"4.8",
    time:"20 min",
    description:"Fresh and delicious food from Flam Be."

  });

  saveFoods();

  displayFoods();

}

/* DELETE */

function deleteFood(index){

  foods.splice(index,1);

  saveFoods();

  displayFoods();

}

/* ORDER */

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

  alert("Order Placed");

}

/* DISPLAY ORDERS */

function displayOrders(){

  ordersList.innerHTML = "";

  if(orders.length === 0){

    ordersList.innerHTML =
    "No orders yet";

    return;
  }

  orders.forEach((order)=>{

    ordersList.innerHTML += `

      <div class="order-item">

        <strong>${order.customer}</strong>

        ordered

        <strong>${order.food}</strong>

        for

        <strong>$${order.price}</strong>

      </div>

    `;
  });

}

displayFoods();

displayOrders();

</script>

</body>
</html>
