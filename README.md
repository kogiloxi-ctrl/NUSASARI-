#<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Es Kelapa Muda Nusa Sari</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f4f9f4;
            color: #333;
            padding-bottom: 80px;
        }

        header {
            background: linear-gradient(135deg, #2e7d32, #4caf50);
            color: white;
            text-align: center;
            padding: 30px 15px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        header h1 {
            font-size: 1.8rem;
            margin-bottom: 5px;
        }

        header p {
            font-size: 0.95rem;
            opacity: 0.9;
        }

        .container {
            max-width: 600px;
            margin: 20px auto;
            padding: 0 15px;
        }

        .menu-card {
            background: white;
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 8px rgba(0,0,0,0.06);
        }

        .menu-info h3 {
            font-size: 1.1rem;
            color: #1b5e20;
            margin-bottom: 4px;
        }

        .menu-info .price {
            font-weight: bold;
            color: #e65100;
        }

        .qty-control {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .qty-btn {
            background-color: #e8f5e9;
            color: #2e7d32;
            border: 1px solid #a5d6a7;
            width: 32px;
            height: 32px;
            border-radius: 50%;
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .qty-btn:active {
            background-color: #c8e6c9;
        }

        .qty-count {
            font-weight: bold;
            min-width: 20px;
            text-align: center;
        }

        .cart-summary {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: white;
            padding: 15px 20px;
            box-shadow: 0 -3px 10px rgba(0,0,0,0.1);
            display: flex;
            justify-content: space-between;
            align-items: center;
            max-width: 600px;
            margin: 0 auto;
        }

        .total-price {
            font-size: 1.1rem;
            font-weight: bold;
            color: #2e7d32;
        }

        .checkout-btn {
            background-color: #25d366;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 25px;
            font-weight: bold;
            font-size: 0.95rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            box-shadow: 0 4px 6px rgba(37, 211, 102, 0.3);
        }

        .checkout-btn:disabled {
            background-color: #ccc;
            box-shadow: none;
            cursor: not-allowed;
        }
    </style>
</head>
<body>

    <header>
        <h1>Es Kelapa Muda Nusa Sari</h1>
        <p>Segar, Murni, & Alami — Pesan Langsung via WhatsApp</p>
    </header>

    <div class="container">
        <!-- Menu Items -->
        <div class="menu-card">
            <div class="menu-info">
                <h3>Kelapa Murni</h3>
                <div class="price">Rp 13.000</div>
            </div>
            <div class="qty-control">
                <button class="qty-btn" onclick="updateQty('murni', -1)">-</button>
                <span class="qty-count" id="qty-murni">0</span>
                <button class="qty-btn" onclick="updateQty('murni', 1)">+</button>
            </div>
        </div>

        <div class="menu-card">
            <div class="menu-info">
                <h3>Es Kelapa Gula Putih</h3>
                <div class="price">Rp 6.000</div>
            </div>
            <div class="qty-control">
                <button class="qty-btn" onclick="updateQty('gulaPutih', -1)">-</button>
                <span class="qty-count" id="qty-gulaPutih">0</span>
                <button class="qty-btn" onclick="updateQty('gulaPutih', 1)">+</button>
            </div>
        </div>

        <div class="menu-card">
            <div class="menu-info">
                <h3>Es Kelapa Gula Merah</h3>
                <div class="price">Rp 6.000</div>
            </div>
            <div class="qty-control">
                <button class="qty-btn" onclick="updateQty('gulaMerah', -1)">-</button>
                <span class="qty-count" id="qty-gulaMerah">0</span>
                <button class="qty-btn" onclick="updateQty('gulaMerah', 1)">+</button>
            </div>
        </div>

        <div class="menu-card">
            <div class="menu-info">
                <h3>Kelapa Cepat Merah</h3>
                <div class="price">Rp 15.000</div>
            </div>
            <div class="qty-control">
                <button class="qty-btn" onclick="updateQty('cepatMerah', -1)">-</button>
                <span class="qty-count" id="qty-cepatMerah">0</span>
                <button class="qty-btn" onclick="updateQty('cepatMerah', 1)">+</button>
            </div>
        </div>
    </div>

    <!-- Bottom Bar -->
    <div class="cart-summary">
        <div>
            <small style="color: #666;">Total Pembelian:</small>
            <div class="total-price" id="total-price">Rp 0</div>
        </div>
        <button class="checkout-btn" id="btn-order" onclick="sendToWA()" disabled>
            Pesan via WA
        </button>
    </div>

    <script>
        // Nomor WA sudah diupdate
        const NOMOR_WA = "6282119350404";

        const products = {
            murni: { name: "Kelapa Murni", price: 13000, qty: 0 },
            gulaPutih: { name: "Es Kelapa Gula Putih", price: 6000, qty: 0 },
            gulaMerah: { name: "Es Kelapa Gula Merah", price: 6000, qty: 0 },
            cepatMerah: { name: "Kelapa Cepat Merah", price: 15000, qty: 0 }
        };

        function updateQty(key, change) {
            if (products[key].qty + change >= 0) {
                products[key].qty += change;
                document.getElementById(`qty-${key}`).innerText = products[key].qty;
                calculateTotal();
            }
        }

        function calculateTotal() {
            let total = 0;
            let hasItem = false;

            for (const key in products) {
                total += products[key].qty * products[key].price;
                if (products[key].qty > 0) hasItem = true;
            }

            document.getElementById('total-price').innerText = `Rp ${total.toLocaleString('id-ID')}`;
            document.getElementById('btn-order').disabled = !hasItem;
        }

        function sendToWA() {
            let message = "Halo *Es Kelapa Muda Nusa Sari*, saya mau pesan:\n\n";
            let total = 0;

            for (const key in products) {
                if (products[key].qty > 0) {
                    const subtotal = products[key].qty * products[key].price;
                    total += subtotal;
                    message += `• ${products[key].name} x${products[key].qty} = Rp ${subtotal.toLocaleString('id-ID')}\n`;
                }
            }

            message += `\n*Total Tagihan: Rp ${total.toLocaleString('id-ID')}*\n\nMohon dikonfirmasi pesanan dan alur pembayarannya. Terima kasih!`;

            const encodedMessage = encodeURIComponent(message);
            window.open(`https://wa.me/${NOMOR_WA}?text=${encodedMessage}`, '_blank');
        }
    </script>
</body>
</html>
 NUSASARI-
Website Menu Es Kelapa Muda Nusa Sari
