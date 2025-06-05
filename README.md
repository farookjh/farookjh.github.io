<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Your Sweet Plug</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>Add commentMore actions
  <script src="https://cdn.jsdelivr.net/npm/emailjs-com@2.6.4/dist/email.min.js"></script>
  <style>
    body {
      background: linear-gradient(to right, #141e30, #243b55);
      color: white;
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
    .cart-total { margin-top: 20px; font-weight: bold; }
  </style>
</head>
<body class="flex items-center justify-center min-h-screen p-4">

  <div class="bg-gray-800 p-6 rounded shadow-lg w-full max-w-xl">
    <h1 class="text-3xl font-bold text-center mb-4">Your Sweet Plug</h1>

    <!-- Email Sign-Up Form -->
    <form id="signupForm" onsubmit="return handleSignup(event)" class="space-y-4">
      <label class="block">
        Email
        <input type="email" id="email" required class="w-full p-2 mt-1 rounded bg-gray-700 border border-gray-600" />
      </label>
      <button class="w-full bg-blue-500 hover:bg-blue-600 text-white p-2 rounded">Sign Up</button>
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
    <form id="passwordForm" onsubmit="return verifyPassword(event)" class="space-y-4 hidden mt-6">
      <label class="block">
        Enter Access Password
        <input type="password" id="password" required class="w-full p-2 mt-1 rounded bg-gray-700 border border-gray-600" />
      </label>
      <button class="w-full bg-green-500 hover:bg-green-600 text-white p-2 rounded">Enter</button>
    <form id="passwordForm" class="hidden" onsubmit="return verifyPassword(event)">
      <h2>Enter Access Password</h2>
      <label for="password">Password</label>
      <input type="password" id="password" required />
      <button type="submit">Enter</button>
    </form>

    <!-- Main Content -->
    <div id="mainContent" class="hidden mt-6 space-y-6">
      <h2 class="text-xl font-bold text-center">Welcome to the Plug 👑</h2>

      <!-- Products -->
      <div class="space-y-4">
        <div class="bg-gray-700 p-4 rounded">
          <p><strong>Product 1</strong>: $10</p>
          <button onclick="addToCart(10)" class="bg-blue-500 mt-2 p-2 rounded hover:bg-blue-600">Add to Cart</button>
        </div>
        <div class="bg-gray-700 p-4 rounded">
          <p><strong>Product 2</strong>: $20</p>
          <button onclick="addToCart(20)" class="bg-blue-500 mt-2 p-2 rounded hover:bg-blue-600">Add to Cart</button>
        </div>
    <!-- Main Site Content -->
    <div id="mainContent" class="hidden">
      <h2>Welcome to the Plug 👑</h2>

      <div class="product">
        <p><strong>Product 1:</strong> $10</p>
        <button onclick="addToCart(10)">Add to Cart</button>
      </div>

      <div class="text-right mt-2">
        <strong>Total: $<span id="cartTotal">0</span></strong>
      <div class="product">
        <p><strong>Product 2:</strong> $20</p>
        <button onclick="addToCart(20)">Add to Cart</button>
      </div>

      <button id="checkoutButton" onclick="showCheckoutForm()" class="w-full bg-yellow-500 hover:bg-yellow-600 p-2 rounded">Checkout</button>

      <!-- Checkout Form -->
      <form id="orderForm" onsubmit="return submitOrder(event)" class="space-y-4 hidden mt-4">
        <h3 class="text-lg font-bold">Checkout</h3>
        <input type="text" id="fullName" name="fullName" required placeholder="Full Name"
               class="w-full p-2 bg-gray-700 border border-gray-600 rounded" />
        <input type="email" id="checkoutEmail" name="email" required placeholder="Email"
               class="w-full p-2 bg-gray-700 border border-gray-600 rounded" />
        <textarea id="address" name="address" required placeholder="Shipping Address"
                  class="w-full p-2 bg-gray-700 border border-gray-600 rounded"></textarea>
        <button type="submit" class="w-full bg-green-500 hover:bg-green-600 p-2 rounded">Place Order</button>
      </form>

      <!-- Wallet Info -->
      <div class="mt-4 p-4 bg-gray-700 rounded text-center">
        <strong>Send USDC (Polygon) to:</strong>
        <p class="break-all mt-2 text-yellow-300">0xyour_wallet_address</p>
      <div class="cart-total">Cart Total: $<span id="cartTotal">0</span></div>

      <div class="wallet-info">
        <strong>Send USDC (Polygon) to:</strong><br />
        <code>0xYourWalletAddressHere</code>
      </div>

      <!-- Telegram -->
      <div class="text-center mt-4">
        <p>Join our Telegram:</p>
        <a href="https://t.me/yourtelegram" target="_blank" class="text-blue-400 font-bold">@yourtelegram</a>
      <div class="telegram">
        <p>Join our Telegram for updates:</p>
        <a href="https://t.me/yourtelegram" target="_blank">@yourtelegram</a>
      </div>
    </div>
  </div>
@@ -90,29 +109,32 @@
    emailjs.init("A1jQbEhM9pMBjPJTv");

    let total = 0;
    const correctPassword = "plugaccess";

    function handleSignup(e) {
      e.preventDefault();
    function handleSignup(event) {
      event.preventDefault();
      const email = document.getElementById("email").value;

      emailjs.send("service_puxgena", "template_4269orl", {
        user_email: email,
        password: correctPassword
      }).then(() => {
        alert("✅ Email sent with password!");
        to_email: email,
        site_password: "plugaccess"
      })
      .then(function(response) {
        alert("Password has been sent to your email!");
        document.getElementById("signupForm").classList.add("hidden");
        document.getElementById("passwordForm").classList.remove("hidden");
      }).catch(error => {
        alert("❌ Failed to send email.");
        console.error(error);
      }, function(error) {
        alert("Email failed to send. Try again later.");
        console.error("EmailJS error:", error);
      });

      return false;
    }

    function verifyPassword(e) {
      e.preventDefault();
    function verifyPassword(event) {
      event.preventDefault();
      const password = document.getElementById("password").value;
      const correctPassword = "plugaccess";

      if (password === correctPassword) {
        document.getElementById("passwordForm").classList.add("hidden");
        document.getElementById("mainContent").classList.remove("hidden");
@@ -125,42 +147,7 @@
      total += amount;
      document.getElementById("cartTotal").textContent = total;
    }

    function showCheckoutForm() {
      document.getElementById("orderForm").classList.remove("hidden");
    }

    function submitOrder(e) {
      e.preventDefault();
      const name = document.getElementById("fullName").value;
      const email = document.getElementById("checkoutEmail").value;
      const address = document.getElementById("address").value;

      emailjs.send("service_puxgena", "template_4269orl", {
        fullName: name,
        email: email,
        address: address,
        total: total
      }).then(() => {
        alert("✅ Order placed successfully!");
        total = 0;
        document.getElementById("cartTotal").textContent = total;
        document.getElementById("orderForm").reset();
        document.getElementById("orderForm").classList.add("hidden");
      }).catch(error => {
        alert("❌ Failed to send order.");
        console.error(error);
      });

      return false;
    }
  </script>

</body>
</html>
