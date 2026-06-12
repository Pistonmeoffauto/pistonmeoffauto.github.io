<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Prodigy RP Vehicle Catalog</title>

<style>
body{
    font-family: Arial, sans-serif;
    background:#111827;
    color:white;
    margin:0;
    padding:20px;
}

h1{
    text-align:center;
}

.controls{
    display:flex;
    gap:10px;
    justify-content:center;
    margin-bottom:20px;
    flex-wrap:wrap;
}

input, select{
    padding:10px;
    border:none;
    border-radius:8px;
}

.grid{
    display:grid;
    grid-template-columns:repeat(auto-fill,minmax(280px,1fr));
    gap:20px;
}

.card{
    background:#1f2937;
    border-radius:12px;
    overflow:hidden;
    box-shadow:0 4px 10px rgba(0,0,0,.3);
}

.card img{
    width:100%;
    height:180px;
    object-fit:cover;
}

.card-content{
    padding:15px;
}

.card h3{
    margin:0;
}

.badge{
    display:inline-block;
    background:#2563eb;
    padding:5px 10px;
    border-radius:999px;
    margin-top:8px;
}
</style>
</head>
<body>

<h1>Prodigy RP Vehicle Catalog</h1>

<div class="controls">
    <input type="text" id="search" placeholder="Search vehicles...">
    <select id="category">
        <option value="">All Classes</option>
        <option value="Sports">Sports</option>
        <option value="Super">Super</option>
        <option value="Muscle">Muscle</option>
        <option value="SUV">SUV</option>
        <option value="Motorcycle">Motorcycle</option>
    </select>
</div>

<div id="cars" class="grid"></div>

<script>
let cars = [];

async function loadCars() {
    const res = await fetch("cars.json");
    cars = await res.json();
    renderCars();
}

function renderCars() {
    const search = document
        .getElementById("search")
        .value
        .toLowerCase();

    const category = document
        .getElementById("category")
        .value;

    const filtered = cars.filter(car =>
        car.name.toLowerCase().includes(search) &&
        (!category || car.class === category)
    );

    document.getElementById("cars").innerHTML =
        filtered.map(car => `
        <div class="card">
            <img src="${car.image}" alt="${car.name}">
            <div class="card-content">
                <h3>${car.name}</h3>
                <p>${car.brand}</p>
                <span class="badge">${car.class}</span>
                <p>Price: $${car.price.toLocaleString()}</p>
            </div>
        </div>
    `).join("");
}

document.getElementById("search")
    .addEventListener("input", renderCars);

document.getElementById("category")
    .addEventListener("change", renderCars);

loadCars();
</script>

</body>
</html>
