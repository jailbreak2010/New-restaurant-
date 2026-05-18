<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>QuickTaste Food Ordering</title>

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

    header{
      background:#ff5722;
      color:white;
      padding:18px;
      text-align:center;
      font-size:26px;
      font-weight:bold;
      position:sticky;
      top:0;
      z-index:1000;
    }

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

    .card{
      background:white;
      border-radius:14px;
      overflow:hidden;
      box-shadow:0 4px 10px rgba(0,0,0,0.1);
      transition:0.3s;
    }

    .card:hover{
      transform:translateY(-5px);
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
      font-weight:bold;
    }

    button{
      border:none;
      padding:10px 15px;
      border-radius:8px;
      cursor:pointer;
      font-weight:bold;
    }

    .order-btn{
      background:#ff5722;
      color:white;
      width:100%;
    }

    .admin-panel{
      background:white;
      margin-top:40px;
      padding:20px;
      border-radius:14px;
      box-shadow:0 4px 10px rgba(0,0,0,0.1);
    }

    .admin-panel input{
      width:100%;
      padding:12px;
      margin-bottom:12px;
      border:1px solid #ccc;
      border-radius:8px;
    }

    .admin-btn{
      background:#222;
      color:white;
      width:100%;
    }

    .orders{
      margin-top:40px;
      background:white;
      padding:20px;
      border-radius:14px;
      box-shadow:0 4px 10px rgba(0,0,0,0.1);
    }

    .order-item{
      border-bottom:1px solid #ddd;
      padding:12px 0;
    }

    .delete-btn{
      background:red;
      color:white;
      margin-top:10px;
      width:100%;
    }

    @media(max-width:600px){
      header{
        font-size:22px;
      }
    }
  </style>
</head>
<body>

<header>
  🍔 QuickTaste Food Ordering
</header>

<div class="container">

  <h2>Menu</h2>

  <div class="food-grid" id="foodGrid"></div>

  <!-- ADMIN PANEL -->
  <div class="admin-panel">
    <h2>Owner Admin Panel</h2>

    <input type="text" id="foodName" placeholder="Food Name">

    <input type="number" id="foodPrice" placeholder="Food Price">

    <input type="text" id="foodImage" placeholder="Food Image URL">

    <button class="admin-btn" onclick="addFood()">
      Add Food
    </button>
  </div>

  <!-- ORDERS -->
  <div class="orders">
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
    }
  ];

  let orders = JSON.parse(localStorage.getItem("orders")) || [];

  const foodGrid = document.getElementById("foodGrid");
  const ordersList = document.getElementById("ordersList");

  function saveFoods(){
    localStorage.setItem("foods", JSON.stringify(foods));
  }

  function saveOrders(){
    localStorage.setItem("orders", JSON.stringify(orders));
  }

  function displayFoods(){

    foodGrid.innerHTML = "";

    foods.forEach((food,index)=>{

      foodGrid.innerHTML += `
        <div class="card">
          <img src="${food.image}">
          
          <div class="card-content">
            <div class="food-name">${food.name}</div>

            <div class="price">
              K${food.price}
            </div>

            <button class="order-btn" onclick="orderFood('${food.name}',${food.price})">
              Order Now
            </button>

            <button class="delete-btn" onclick="deleteFood(${index})">
              Delete Food
            </button>
          </div>
        </div>
      `;
    });

  }

  function addFood(){

    const name = document.getElementById("foodName").value;
    const price = document.getElementById("foodPrice").value;
    const image = document.getElementById("foodImage").value;

    if(name === "" || price === "" || image === ""){
      alert("Fill all fields");
      return;
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

  function deleteFood(index){

    foods.splice(index,1);

    saveFoods();
    displayFoods();
  }

  function orderFood(name,price){

    const customer = prompt("Enter your name");

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

  function displayOrders(){

    ordersList.innerHTML = "";

    if(orders.length === 0){
      ordersList.innerHTML = "No orders yet";
      return;
    }

    orders.forEach((order,index)=>{

      ordersList.innerHTML += `
        <div class="order-item">
          <strong>${order.customer}</strong>
          ordered <strong>${order.food}</strong>
          for K${order.price}

          <br><br>

          <button onclick="removeOrder(${index})">
            Remove Order
          </button>
        </div>
      `;
    });
  }

  function removeOrder(index){

    orders.splice(index,1);

    saveOrders();
    displayOrders();
  }

  displayFoods();
  displayOrders();
</script>

</body>
</html>
