<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Your Sweet Plug</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdn.jsdelivr.net/npm/emailjs-com@2.6.4/dist/email.min.js"></script>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(to right, #141e30, #243b55);
      color: #fff;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    .container {
      background: #1e293b;
      padding: 40px;
      border-radius: 12px;
      width: 100%;
      max-width: 600px;
      box-shadow: 0 0 20px rgba(0,0,0,0.5);
      overflow-y: auto;
      max-height: 90vh;
    }
    h1, h2 { text-align: center; margin-bottom: 20px; }
    label { display: block; margin-top: 15px; }
    input[type="email"], input[type="password"] {
      width: 100%; padding: 10px; margin-top: 8px;
      border: none; border-radius: 6px;
    }
    button {
      margin-top: 20px; width: 100%; padding: 12px;
      background: #00c9a7; border: none; border-radius: 6px;
      color: white; font-size: 16px; cursor: pointer;
    }
    button:hover { background: #00a894; }
    .hidden { display: none; }
    .wallet-info {
      background: #0f172a; padding: 20px; border-radius: 8px; margin-top: 20px;
    }
    .telegram {
      text-align: center; margin-top: 20px;
    }
    .telegram a {
      color: #1da1f2; text-decoration: none; font-weight: bold;
    }
    .product {
      border: 1px solid #444; padding: 15px; margin: 10px 0;
      border-radius: 8px; background: #2d3748;
    }
    .cart-total, .cart-items { margin-top: 20px; }
    .cart-item {
      display: flex; justify-content: space-between; align-items: center;
      padding: 10px; background: #334155; border-radius: 6px; margin-bottom: 5px;
    }
    .cart-item button {
      background: red; padding: 5px 10px; font-size: 12px;
      border: none; border-radius: 4px; cursor: pointer;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>Join Your Sweet Plug</h1>

    <!-- Sign-up Form -->
    <form id="signupForm" onsubmit="return handleSignup(event)">
      <label for="email">Email</label>
      <input type="email" id="email" name="email" required />
      <button type="submit">Sign Up</button>
    </form>

    <!-- Password Form -->
    <form id="passwordForm" class="hidden" onsubmit="return verifyPassword(event)">
      <h2>Enter Access Password</h2>
      <label for="password">Password</label>
      <input type="password" id="password" required />
      <button type="submit">Enter</button>
    </form>

    <!-- Main Site Content -->
    <div id="mainContent" class="hidden">
      <h2>Welcome to the Plug 👑</h2>

      <div class="product">
        <p><strong>Product 1:</strong> $10</p>
        <button onclick="addToCart('Product 1', 10)">Add to Cart</button>
      </div>

      <div class="product">
        <p><strong>Product 2:</strong> $20</p>
        <button onclick="addToCart('Product 2', 20)">Add to Cart</button>
      </div>

      <div class="cart-items" id="cartItems"></div>
      <div class="cart-total">Cart Total: $<span id="cartTotal">0</span></div>

      <div class="wallet-info">
        <strong>Send USDC (Polygon) to:</strong><br />
        <code>0xYourWalletAddressHere</code>
      </div>

      <div class="telegram">
        <p>Join our Telegram for updates:</p>
        <a href="https://t.me/yourtelegram" target="_blank">@yourtelegram</a>
      </div>
    </div>
  </div>

  <script>
    emailjs.init("A1jQbEhM9pMBjPJTv");

    let cart = [];

    function handleSignup(event) {
      event.preventDefault();
      const email = document.getElementById("email").value;

      emailjs.send("service_puxgena", "template_4269orl", {
        to_email: email,
        site_password: "plugaccess"
      })
      .then(() => {
        alert("Password has been sent to your email!");
        document.getElementById("signupForm").classList.add("hidden");
        document.getElementById("passwordForm").classList.remove("hidden");
      })
      .catch((error) => {
        alert("Email failed to send. Try again later.");
        console.error("EmailJS error:", error);
      });
    }

    function verifyPassword(event) {
      event.preventDefault();
      const password = document.getElementById("password").value;
      const correctPassword = "plugaccess";

      if (password === correctPassword) {
        document.getElementById("passwordForm").classList.add("hidden");
        document.getElementById("mainContent").classList.remove("hidden");
      } else {
        alert("Incorrect password.");
      }
    }

    function addToCart(name, price) {
      cart.push({ name, price });
      updateCart();
    }

    function removeFromCart(index) {
      cart.splice(index, 1);
      updateCart();
    }

    function updateCart() {
      const cartContainer = document.getElementById("cartItems");
      const totalDisplay = document.getElementById("cartTotal");
      cartContainer.innerHTML = "";
      let total = 0;

      cart.forEach((item, index) => {
        total += item.price;
        const div = document.createElement("div");
        div.className = "cart-item";
        div.innerHTML = `
          <span>${item.name} - $${item.price}</span>
          <button onclick="removeFromCart(${index})">Remove</button>
        `;
        cartContainer.appendChild(div);
      });

      totalDisplay.textContent = total;
    }
  </script>

</body>
</html>
