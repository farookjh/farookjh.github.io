<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Crypto Store</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdn.jsdelivr.net/npm/ethers@6.7.0/dist/ethers.umd.min.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore-compat.js"></script>
  <script src="https://www.google.com/recaptcha/api.js" async defer></script>
</head>
<body class="bg-gradient-to-br from-gray-50 to-indigo-100 min-h-screen font-sans">

  <!-- Signup overlay -->
  <div id="signupOverlay" class="fixed inset-0 bg-white z-50 flex flex-col items-center justify-center">
    <h2 class="text-xl font-bold mb-4">Sign Up to Continue</h2>
    <form id="signupForm" class="space-y-4">
      <input type="email" id="signupEmail" placeholder="Your email" required class="px-4 py-2 border rounded w-72" />
      <div class="g-recaptcha" data-sitekey="your-site-key"></div>
      <button type="submit" class="bg-indigo-600 text-white px-4 py-2 rounded w-full">Sign Up</button>
    </form>
    <p id="signupMsg" class="mt-4 text-sm text-green-600 hidden">✅ Email registered! Please enter passcode.</p>
  </div>

  <!-- Passcode overlay -->
  <div id="passcodeOverlay" class="fixed inset-0 bg-white z-40 flex flex-col items-center justify-center hidden">
    <h2 class="text-xl font-bold mb-4">Enter Passcode</h2>
    <input id="passcodeInput" type="password" placeholder="Enter passcode" class="px-4 py-2 border rounded mb-2" />
    <button onclick="checkPasscode()" class="bg-indigo-600 text-white px-4 py-2 rounded">Submit</button>
    <p id="passcodeError" class="text-red-500 mt-2 hidden">Incorrect passcode.</p>
  </div>

  <!-- Main site -->
  <div id="mainSite" class="hidden">
    <header class="bg-white shadow p-4 flex justify-between items-center">
      <h1 class="text-2xl font-bold">YourSweetPlug</h1>
      <div class="flex items-center space-x-4">
        <button id="connectButton" class="bg-indigo-600 text-white px-4 py-2 rounded">Connect Wallet</button>
        <div id="cart" class="relative bg-white border px-3 py-2 rounded shadow text-sm text-gray-700">Cart: 0</div>
      </div>
    </header>

    <main class="max-w-4xl mx-auto p-6">
      <section>
        <h2 class="text-xl font-semibold mb-4">Products</h2>
        <div id="productList" class="grid grid-cols-1 sm:grid-cols-3 gap-4"></div>
      </section>

      <section class="mt-10 text-center">
        <a href="https://t.me/sweetplugs" target="_blank" class="inline-block mt-4 bg-blue-500 text-white px-6 py-3 rounded-full hover:bg-blue-600">
          💬 Join Our Telegram
        </a>
      </section>
    </main>
  </div>

  <script>
    // Firebase Config
    const firebaseConfig = {
      apiKey: "AIzaSyCt1b48nMK0gnA7E1qCYL51yG5q_gEPfYw",
      authDomain: "sweetts-5e2fd.firebaseapp.com",
      projectId: "sweetts-5e2fd",
      storageBucket: "sweetts-5e2fd.appspot.com",
      messagingSenderId: "670662407268",
      appId: "1:670662407268:web:3ce06313059e59104e1b71",
      measurementId: "G-KS9HM75WWJ"
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();

    // Site Variables
    const USDC_ADDRESS = "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174";
    const USDC_ABI = ["function transfer(address to, uint256 amount) returns (bool)", "function decimals() view returns (uint8)"];
    const MY_WALLET = "0xYourWalletHere";
    const PASSCODE = "mysecret";
    const products = [
      { id: 1, name: 'Hoodie', price: 5.00, emoji: '🧥' },
      { id: 2, name: 'Cap', price: 2.50, emoji: '🧢' },
      { id: 3, name: 'Mug', price: 3.75, emoji: '☕' },
    ];
    let cart = [];
    let signer;

    // Sign Up Logic
    document.getElementById("signupForm").addEventListener("submit", async function (e) {
      e.preventDefault();
      const email = document.getElementById("signupEmail").value;
      try {
        await db.collection("users").add({ email, timestamp: Date.now() });
        document.getElementById("signupOverlay").classList.add("hidden");
        document.getElementById("passcodeOverlay").classList.remove("hidden");
      } catch (err) {
        alert("Error: " + err.message);
      }
    });

    // Passcode
    function checkPasscode() {
      const input = document.getElementById("passcodeInput").value;
      if (input === PASSCODE) {
        document.getElementById("passcodeOverlay").classList.add("hidden");
        document.getElementById("mainSite").classList.remove("hidden");
      } else {
        document.getElementById("passcodeError").classList.remove("hidden");
      }
    }

    // MetaMask Connect
    document.getElementById('connectButton').addEventListener('click', async () => {
      if (!window.ethereum) return alert("Please install MetaMask.");
      const provider = new ethers.BrowserProvider(window.ethereum);
      await provider.send("eth_requestAccounts", []);
      signer = await provider.getSigner();
      const addr = await signer.getAddress();
      document.getElementById('connectButton').textContent = `Connected: ${addr.slice(0,6)}...${addr.slice(-4)}`;
    });

    // Cart and Products
    function renderProducts() {
      const container = document.getElementById('productList');
      container.innerHTML = '';
      products.forEach(p => {
        const div = document.createElement('div');
        div.className = 'bg-white p-4 rounded shadow text-center';
        div.innerHTML = `
          <div class="text-5xl mb-2">${p.emoji}</div>
          <h3 class="font-semibold text-lg">${p.name}</h3>
          <p class="text-gray-500">$${p.price.toFixed(2)}</p>
          <button class="mt-2 bg-indigo-600 text-white px-3 py-1 rounded" onclick="addToCart(${p.id})">Add to Cart</button>
        `;
        container.appendChild(div);
      });
    }

    function updateCartUI() {
      document.getElementById('cart').textContent = `Cart: ${cart.length}`;
    }

    async function addToCart(id) {
      const product = products.find(p => p.id === id);
      cart.push(product);
      updateCartUI();
      try {
        await db.collection("orders").add({
          product: product.name,
          price: product.price,
          timestamp: Date.now()
        });
      } catch (err) {
        console.error("Error saving order:", err);
      }
    }

    renderProducts();
    updateCartUI();
  </script>
</body>
</html>




     
     
