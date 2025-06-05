<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Crypto Shop</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdn.jsdelivr.net/npm/ethers@6.7.0/dist/ethers.umd.min.js"></script>
  <script src="https://www.google.com/recaptcha/api.js" async defer></script>
</head>
<body class="bg-gray-100 font-sans">

<!-- Firebase (Optional) -->
<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.11.0/firebase-app.js";
  const firebaseConfig = {
    apiKey: "AIza...yourkey...",
    authDomain: "yourproject.firebaseapp.com",
    projectId: "yourproject",
    storageBucket: "yourproject.appspot.com",
    messagingSenderId: "123456789",
    appId: "yourappid"
  };
  const app = initializeApp(firebaseConfig);
</script>

<!-- Signup Overlay -->
<div id="signupOverlay" class="fixed inset-0 bg-white flex flex-col items-center justify-center z-50">
  <h2 class="text-2xl font-bold mb-4">Sign Up to Access</h2>
  <form id="signupForm" action="https://formspree.io/f/your-form-id" method="POST" class="space-y-4 w-80">
    <input type="email" name="email" placeholder="Email" required class="w-full px-4 py-2 border rounded" />
    <div class="g-recaptcha" data-sitekey="your-site-key"></div>
    <button type="submit" class="w-full bg-blue-600 text-white py-2 rounded">Sign Up</button>
  </form>
  <p id="signupSuccess" class="text-green-600 mt-4 hidden">✅ Signed up! Enter password...</p>
</div>

<!-- Passcode Overlay -->
<div id="passcodeOverlay" class="fixed inset-0 bg-white flex flex-col items-center justify-center z-40 hidden">
  <h2 class="text-xl font-bold mb-4">Enter Passcode</h2>
  <input id="passInput" type="password" placeholder="Password" class="px-4 py-2 border rounded mb-3" />
  <button onclick="checkPassword()" class="bg-green-600 text-white px-6 py-2 rounded">Submit</button>
  <p id="passError" class="text-red-500 mt-2 hidden">❌ Incorrect password</p>
</div>

<!-- Main Site -->
<div id="mainSite" class="hidden">
  <header class="bg-white shadow p-4 flex justify-between items-center">
    <h1 class="text-xl font-bold">🛒 Crypto Shop</h1>
    <div class="flex items-center space-x-4">
      <button id="connectButton" class="bg-indigo-600 text-white px-4 py-2 rounded">Connect Wallet</button>
      <div id="cart" class="bg-white border px-3 py-2 rounded shadow text-sm">Cart: 0</div>
    </div>
  </header>

  <main class="max-w-4xl mx-auto p-6">
    <section>
      <h2 class="text-xl font-semibold mb-4">Products</h2>
      <div id="productList" class="grid grid-cols-1 sm:grid-cols-3 gap-4"></div>
    </section>

    <section class="mt-10">
      <h2 class="text-xl font-semibold mb-2">Subscribe</h2>
      <form action="https://formspree.io/f/your-form-id" method="POST" class="bg-white p-4 rounded shadow">
        <input type="email" name="email" placeholder="Email" required class="w-full px-3 py-2 border rounded mb-2" />
        <button type="submit" class="bg-blue-600 text-white px-4 py-2 rounded">Subscribe</button>
      </form>
    </section>

    <section class="mt-10 text-center">
      <a href="https://t.me/yourchannel" target="_blank" class="bg-blue-500 text-white px-6 py-3 rounded-full">💬 Join Our Telegram</a>
    </section>
  </main>
</div>

<script>
  const PASSCODE = "your-password";
  const USDC_ADDRESS = "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174";
  const MY_WALLET = "your-wallet-address";
  const USDC_ABI = ["function transfer(address to, uint amount) returns (bool)", "function decimals() view returns (uint8)"];
  let cart = [];
  let signer;

  const products = [
    { id: 1, name: "Hoodie", emoji: "🧥", price: 5 },
    { id: 2, name: "Sticker", emoji: "📦", price: 1.5 },
    { id: 3, name: "Cup", emoji: "☕", price: 2.5 }
  ];

  // Signup
  document.getElementById("signupForm").addEventListener("submit", async function (e) {
    e.preventDefault();
    const form = e.target;
    const data = new FormData(form);
    const res = await fetch(form.action, { method: form.method, body: data, headers: { 'Accept': 'application/json' }});
    if (res.ok) {
      document.getElementById("signupSuccess").classList.remove("hidden");
      setTimeout(() => {
        document.getElementById("signupOverlay").classList.add("hidden");
        document.getElementById("passcodeOverlay").classList.remove("hidden");
      }, 1500);
    } else {
      alert("❌ Signup failed.");
    }
  });

  function checkPassword() {
    const input = document.getElementById("passInput").value;
    if (input === PASSCODE) {
      document.getElementById("passcodeOverlay").classList.add("hidden");
      document.getElementById("mainSite").classList.remove("hidden");
    } else {
      document.getElementById("passError").classList.remove("hidden");
    }
  }

  document.getElementById("connectButton").addEventListener("click", async () => {
    if (!window.ethereum) return alert("Please install MetaMask.");
    const provider = new ethers.BrowserProvider(window.ethereum);
    await provider.send("eth_requestAccounts", []);
    signer = await provider.getSigner();
    const addr = await signer.getAddress();
    document.getElementById("connectButton").textContent = `Connected: ${addr.slice(0, 6)}...${addr.slice(-4)}`;
  });

  function addToCart(id) {
    const product = products.find(p => p.id === id);
    cart.push(product);
    document.getElementById("cart").textContent = `Cart: ${cart.length}`;
  }

  function renderProducts() {
    const container = document.getElementById("productList");
    container.innerHTML = "";
    products.forEach(p => {
      const el = document.createElement("div");
      el.className = "bg-white p-4 rounded shadow text-center";
      el.innerHTML = `
        <div class="text-5xl mb-2">${p.emoji}</div>
        <h3 class="font-semibold">${p.name}</h3>
        <p class="text-gray-500 mb-2">$${p.price}</p>
        <button onclick="addToCart(${p.id})" class="bg-indigo-600 text-white px-3 py-1 rounded">Add to Cart</button>
        <button onclick="payUSDC(${p.price})" class="mt-2 bg-green-600 text-white px-3 py-1 rounded">Pay USDC</button>
      `;
      container.appendChild(el);
    });
  }

  async function payUSDC(amount) {
    if (!signer) return alert("🔌 Connect wallet first.");
    const contract = new ethers.Contract(USDC_ADDRESS, USDC_ABI, signer);
    const decimals = await contract.decimals();
    const value = ethers.parseUnits(amount.toString(), decimals);
    const tx = await contract.transfer(MY_WALLET, value);
    alert("✅ Payment sent! Tx: " + tx.hash);
  }

  renderProducts();
</script>
</body>
</html>





     
     
