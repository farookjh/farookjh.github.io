<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Your Sweet Plug</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>
  <style>
    body {
      background: linear-gradient(to right, #141e30, #243b55);
      color: white;
      font-family: 'Segoe UI', sans-serif;
    }
    .hidden { display: none; }
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
    </form>

    <!-- Password Form -->
    <form id="passwordForm" onsubmit="return verifyPassword(event)" class="space-y-4 hidden mt-6">
      <label class="block">
        Enter Access Password
        <input type="password" id="password" required class="w-full p-2 mt-1 rounded bg-gray-700 border border-gray-600" />
      </label>
      <button class="w-full bg-green-500 hover:bg-green-600 text-white p-2 rounded">Enter</button>
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
      </div>

      <div class="text-right mt-2">
        <strong>Total: $<span id="cartTotal">0</span></strong>
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
      </div>

      <!-- Telegram -->
      <div class="text-center mt-4">
        <p>Join our Telegram:</p>
        <a href="https://t.me/yourtelegram" target="_blank" class="text-blue-400 font-bold">@yourtelegram</a>
      </div>
    </div>
  </div>

  <script>
    emailjs.init("A1jQbEhM9pMBjPJTv");

    let total = 0;
    const correctPassword = "plugaccess";

    function handleSignup(e) {
      e.preventDefault();
      const email = document.getElementById("email").value;

      emailjs.send("service_puxgena", "template_4269orl", {
        user_email: email,
        password: correctPassword
      }).then(() => {
        alert("✅ Email sent with password!");
        document.getElementById("signupForm").classList.add("hidden");
        document.getElementById("passwordForm").classList.remove("hidden");
      }).catch(error => {
        alert("❌ Failed to send email.");
        console.error(error);
      });
      return false;
    }

    function verifyPassword(e) {
      e.preventDefault();
      const password = document.getElementById("password").value;
      if (password === correctPassword) {
        document.getElementById("passwordForm").classList.add("hidden");
        document.getElementById("mainContent").classList.remove("hidden");
      } else {
        alert("Incorrect password.");
      }
    }

    function addToCart(amount) {
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




     
     
