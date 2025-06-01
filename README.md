<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Crypto Store - USDC (Polygon)</title>
  <script src="https://cdn.jsdelivr.net/npm/ethers@6.7.0/dist/ethers.umd.min.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gradient-to-br from-blue-50 to-purple-100 min-h-screen font-sans">

  <header class="bg-white shadow p-4 flex justify-between items-center">
    <h1 class="text-2xl font-bold">🛒 Polygon Crypto Shop</h1>
    <button id="connectButton" class="bg-purple-600 text-white px-4 py-2 rounded">Connect Wallet</button>
  </header>

  <main class="max-w-4xl mx-auto p-6">
<section class="mt-10">
  <h2 class="text-xl font-semibold mb-2">📧 Email Confirmation</h2>
  <form id="emailForm" class="bg-white p-4 rounded shadow">
    <label class="block mb-2">
      <span class="text-gray-700">Your Email</span>
      <input type="email" id="emailInput" required class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded">
    </label>
    <p class="text-sm text-gray-500 mt-1">We'll email you your order summary after checkout.</p>
  </form>
</section>

    <section>
      <h2 class="text-xl font-semibold mb-4">🛍️ Products</h2>
      <div id="productList" class="grid grid-cols-1 sm:grid-cols-3 gap-4"></div>
    </section>

    <section class="mt-10">
      <h2 class="text-xl font-semibold mb-2">🧾 Your Cart</h2>
      <div id="cart" class="bg-white p-4 rounded shadow">
        <p class="text-gray-500">Cart is empty.</p>
      </div>
    </section>

    <section class="mt-10 text-center">
      <a href="https://t.me/YourTelegramLink" target="_blank" class="inline-block mt-4 bg-blue-500 text-white px-6 py-3 rounded-full hover:bg-blue-600">
        💬 Join Our Telegram
      </a>
    </section>
  </main>

  <script>
    const USDC_ADDRESS = "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174"; // USDC on Polygon
    const USDC_ABI = [
      "function transfer(address to, uint256 amount) returns (bool)",
      "function decimals() view returns (uint8)"
    ];
    const MY_WALLET = "0xYourWalletHere"; // Replace with your Polygon wallet address

    const products = [
      { id: 1, name: 'Magic Hoodie', price: 5.00, emoji: '🧥' },
      { id: 2, name: 'Coffee Potion', price: 2.50, emoji: '☕' },
      { id: 3, name: 'Enchanted Mug', price: 3.75, emoji: '🍵' },
    ];

    let cart = [];
    let signer;

    document.getElementById('connectButton').addEventListener('click', async () => {
      if (!window.ethereum) return alert("Please install MetaMask.");
      const provider = new ethers.BrowserProvider(window.ethereum);
      await provider.send("eth_requestAccounts", []);
      signer = await provider.getSigner();
      const addr = await signer.getAddress();
      document.getElementById('connectButton').textContent = `Connected: ${addr.slice(0,6)}...${addr.slice(-4)}`;
    });

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
          <button class="mt-2 bg-purple-600 text-white px-3 py-1 rounded" onclick="addToCart(${p.id})">Add to Cart</button>
        `;
        container.appendChild(div);
      });
    }

    function renderCart() {
      const container = document.getElementById('cart');
      container.innerHTML = '';
      if (cart.length === 0) {
        container.innerHTML = '<p class="text-gray-500">Cart is empty.</p>';
        return;
      }

      let total = 0;
      cart.forEach((item, index) => {
        const div = document.createElement('div');
        div.className = 'flex justify-between items-center mb-2';
        div.innerHTML = `
          <span>${item.name} - $${item.price.toFixed(2)}</span>
          <button class="text-red-500 text-sm" onclick="removeFromCart(${index})">Remove</button>
        `;
        container.appendChild(div);
        total += item.price;
      });

      const totalEl = document.createElement('div');
      totalEl.className = 'mt-4 font-semibold';
      totalEl.textContent = `Total: $${total.toFixed(2)}`;
      container.appendChild(totalEl);

      const checkoutBtn = document.createElement('button');
      checkoutBtn.className = 'mt-3 bg-green-600 text-white px-4 py-2 rounded hover:bg-green-700';
      checkoutBtn.textContent = 'Checkout with USDC';
      checkoutBtn.onclick = () => checkout(total);
      container.appendChild(checkoutBtn);
    }

    function addToCart(id) {
      const product = products.find(p => p.id === id);
      cart.push(product);
      renderCart();
    }

    function removeFromCart(index) {
      cart.splice(index, 1);
      renderCart();
    }

    async function checkout(totalUSD) {
      if (!signer) return alert('Please connect your wallet.');

      try {
        const usdc = new ethers.Contract(USDC_ADDRESS, USDC_ABI, signer);
        const decimals = await usdc.decimals();
        const amount = ethers.parseUnits(totalUSD.toFixed(2), decimals);

        const tx = await usdc.transfer(MY_WALLET, amount);
        alert(`✅ Payment sent! TX hash: ${tx.hash}`);
        cart = [];
        renderCart();
      } catch (e) {
        alert('❌ Transaction failed.');
        console.error(e);
      }
    }

    renderProducts();
    renderCart();
  </script>
</body>
</html>


     
     
