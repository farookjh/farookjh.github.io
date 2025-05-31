<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>🍬 Sweet Treats | Home</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background-color: #fff8f0;
      color: #333;
    }

    header {
      background-color: #ff66a3;
      color: white;
      padding: 1rem;
      text-align: center;
    }

    header nav a {
      color: white;
      margin: 0 1rem;
      text-decoration: none;
      font-weight: bold;
    }

    .hero {
      text-align: center;
      padding: 4rem 2rem;
      background-color: #fff0f5;
    }

    .hero h2 {
      font-size: 2rem;
    }

    .hero button {
      background-color: #ff66a3;
      color: white;
      border: none;
      padding: 0.75rem 1.5rem;
      font-size: 1rem;
      cursor: pointer;
      border-radius: 5px;
      margin-top: 1rem;
    }

    .menu {
      padding: 2rem;
      text-align: center;
    }

    .items {
      display: flex;
      justify-content: center;
      gap: 2rem;
      flex-wrap: wrap;
    }

    .item {
      background-color: white;
      border: 1px solid #eee;
      border-radius: 8px;
      padding: 1rem;
      width: 200px;
    }

    .item img {
      width: 100%;
      border-radius: 8px;
    }

    footer {
      background-color: #ff66a3;
      color: white;
      text-align: center;
      padding: 1rem;
      margin-top: 2rem;
    }
  </style>
</head>
<body>
  <header>
    <h1>🍬YourLocalPharmacy </h1>
    <nav>
      <a href="#">Home</a>
      <a href="#">Menu</a>
      <a href="#">About</a>
      <a href="#">Contact</a>
    </nav>
  </header>

  <section class="hero">
    <h2>Delicious sweets, made with love!</h2>
    <p>Come treat yourself to the finest candies and desserts in town.</p>
    <button onclick="orderNow()">Order Now</button>
  </section>

  <section class="menu">
    <h3>Our Bestsellers</h3>
    <div class="items">
      <div class="item">
        <img src="https://via.placeholder.com/150" alt="Chocolate Fudge" />
        <h4>Chocolate Fudge</h4>
        <p>$4.99</p>
      </div>
      <div class="item">
        <img src="https://via.placeholder.com/150" alt="Strawberry Candy" />
        <h4>Strawberry Candy</h4>
        <p>$2.99</p>
      </div>
      <div class="item">
        <img src="https://via.placeholder.com/150" alt="Rainbow Gummies" />
        <h4>Rainbow Gummies</h4>
        <p>$3.49</p>
      </div>
    </div>
  </section>

  <footer>
    <p>© 2025 Sweet Treats. All rights reserved.</p>
  </footer>

  <script>
    function orderNow() {
      alert("Thanks for your interest! Online ordering is coming soon!");
    }
  </script>
</body>
</html>
