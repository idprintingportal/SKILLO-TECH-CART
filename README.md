<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TechCart - कंप्यूटर एवं एक्सेसरीज़ स्टोर</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .product-card:hover {
            transform: translateY(-4px);
            transition: all 0.2s ease-in-out;
        }
    </style>
</head>
<body class="bg-gray-100 font-sans min-h-screen flex flex-col">

    <!-- Header / Navbar -->
    <header class="bg-slate-900 text-white shadow-lg sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-3 flex justify-between items-center">
            <div class="flex items-center space-x-3 cursor-pointer" onclick="showCustomerView()">
                <i class="fa-solid fa-desktop text-2xl text-blue-400"></i>
                <span class="text-2xl font-bold tracking-wide">Tech<span class="text-blue-400">Cart</span></span>
            </div>

            <div class="flex items-center space-x-4">
                <button id="cartBtn" onclick="toggleCart()" class="relative bg-blue-600 hover:bg-blue-700 px-4 py-2 rounded-lg flex items-center gap-2 transition">
                    <i class="fa-solid fa-cart-shopping"></i>
                    <span class="hidden sm:inline font-medium">कार्ट</span>
                    <span id="cartCount" class="bg-red-500 text-white text-xs font-bold px-2 py-0.5 rounded-full">0</span>
                </button>

                <button id="navLoginBtn" onclick="openLoginModal()" class="bg-gray-700 hover:bg-gray-600 px-4 py-2 rounded-lg flex items-center gap-2 transition text-sm">
                    <i class="fa-solid fa-user-lock"></i>
                    <span id="loginBtnText">ओनर लॉगिन</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="max-w-7xl mx-auto px-4 py-6 flex-grow w-full">

        <!-- ================= CUSTOMER STORE VIEW ================= -->
        <div id="customerView">
            <!-- Banner / Hero Section -->
            <div class="bg-gradient-to-r from-blue-700 to-indigo-800 text-white rounded-2xl p-6 sm:p-10 mb-8 shadow-md">
                <h1 class="text-2xl sm:text-4xl font-extrabold mb-2">कंप्यूटर एक्सेसरीज़ की सर्वश्रेष्ठ दुकान</h1>
                <p class="text-blue-100 text-sm sm:text-base">प्रिंटर, मॉनिटर, सीपीयू, कीबोर्ड, माउस और बहुत कुछ खरीदें आसान डिलीवरी के साथ!</p>
            </div>

            <!-- Filter Buttons -->
            <div class="flex gap-2 overflow-x-auto pb-4 mb-6 scrollbar-none">
                <button onclick="filterCategory('All')" class="cat-btn active-cat bg-blue-600 text-white px-4 py-2 rounded-full text-sm font-medium whitespace-nowrap shadow-sm hover:bg-blue-700 transition">सभी (All)</button>
                <button onclick="filterCategory('Printer')" class="cat-btn bg-white text-gray-700 px-4 py-2 rounded-full text-sm font-medium whitespace-nowrap shadow-sm hover:bg-gray-100 transition">प्रिंटर (Printer)</button>
                <button onclick="filterCategory('Monitor')" class="cat-btn bg-white text-gray-700 px-4 py-2 rounded-full text-sm font-medium whitespace-nowrap shadow-sm hover:bg-gray-100 transition">मॉनिटर (Monitor)</button>
                <button onclick="filterCategory('CPU')" class="cat-btn bg-white text-gray-700 px-4 py-2 rounded-full text-sm font-medium whitespace-nowrap shadow-sm hover:bg-gray-100 transition">सीपीयू (CPU)</button>
                <button onclick="filterCategory('Keyboard')" class="cat-btn bg-white text-gray-700 px-4 py-2 rounded-full text-sm font-medium whitespace-nowrap shadow-sm hover:bg-gray-100 transition">कीबोर्ड (Keyboard)</button>
                <button onclick="filterCategory('Mouse')" class="cat-btn bg-white text-gray-700 px-4 py-2 rounded-full text-sm font-medium whitespace-nowrap shadow-sm hover:bg-gray-100 transition">माउस (Mouse)</button>
                <button onclick="filterCategory('Accessories')" class="cat-btn bg-white text-gray-700 px-4 py-2 rounded-full text-sm font-medium whitespace-nowrap shadow-sm hover:bg-gray-100 transition">अन्य एक्सेसरीज़</button>
            </div>

            <!-- Products Grid -->
            <div id="productsGrid" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
                <!-- Dynamic Products -->
            </div>
        </div>

        <!-- ================= OWNER DASHBOARD VIEW ================= -->
        <div id="ownerView" class="hidden">
            <div class="flex justify-between items-center mb-6 bg-white p-4 rounded-xl shadow-sm border border-gray-200">
                <div>
                    <h2 class="text-2xl font-bold text-gray-800">ओनर कंट्रोल पैनल (Admin Dashboard)</h2>
                    <p class="text-sm text-gray-500">उत्पाद जोड़ें और ग्राहकों के ऑर्डर्स प्रबंधित करें</p>
                </div>
                <button onclick="logoutOwner()" class="bg-red-600 hover:bg-red-700 text-white px-4 py-2 rounded-lg text-sm font-semibold transition">
                    <i class="fa-solid fa-right-from-bracket mr-1"></i> लॉगआउट
                </button>
            </div>

            <!-- Dashboard Tabs -->
            <div class="flex border-b border-gray-300 mb-6 bg-white rounded-t-xl px-4 pt-2">
                <button id="tabOrdersBtn" onclick="switchAdminTab('orders')" class="py-3 px-6 font-bold text-blue-600 border-b-2 border-blue-600 focus:outline-none flex items-center gap-2">
                    <i class="fa-solid fa-boxes-packing"></i> प्राप्त ऑर्डर्स (<span id="ordersCount">0</span>)
                </button>
                <button id="tabAddProductBtn" onclick="switchAdminTab('add')" class="py-3 px-6 font-bold text-gray-500 hover:text-blue-600 focus:outline-none flex items-center gap-2">
                    <i class="fa-solid fa-plus-circle"></i> नए उत्पाद जोड़ें (Multi-Box)
                </button>
            </div>

            <!-- TAB 1: ORDERS LIST -->
            <div id="adminOrdersSection" class="space-y-4">
                <div id="ordersList" class="space-y-4">
                    <!-- Orders dynamically populated -->
                </div>
            </div>

            <!-- TAB 2: MULTI PRODUCT GENERATOR (200+ BOXES) -->
            <div id="adminAddSection" class="hidden">
                <div class="bg-white p-6 rounded-xl shadow-sm border border-gray-200 mb-6">
                    <h3 class="text-lg font-bold text-gray-800 mb-2">उत्पाद बॉक्स जनरेटर (Quick Box Generator)</h3>
                    <p class="text-sm text-gray-600 mb-4">आप एक साथ जितने चाहें (उदा: 10, 50, 100, 200) फॉर्म बॉक्सेस बनाकर बल्क में उत्पाद जोड़ सकते हैं:</p>

                    <div class="flex items-center gap-4 max-w-md">
                        <input type="number" id="boxCountInput" min="1" max="500" value="3" class="w-full border border-gray-300 rounded-lg p-2.5 focus:ring-2 focus:ring-blue-500 outline-none" placeholder="बॉक्सेस की संख्या (जैसे 200)">
                        <button onclick="generateProductBoxes()" class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold px-6 py-2.5 rounded-lg whitespace-nowrap transition">
                            बॉक्सेस बनाएं
                        </button>
                    </div>
                </div>

                <form id="bulkProductForm" onsubmit="saveBulkProducts(event)">
                    <div id="bulkBoxesContainer" class="space-y-6">
                        <!-- Boxes populated dynamically -->
                    </div>

                    <div class="mt-6 flex justify-end">
                        <button type="submit" class="bg-green-600 hover:bg-green-700 text-white font-bold px-8 py-3 rounded-xl shadow-lg transition">
                            <i class="fa-solid fa-cloud-arrow-up mr-2"></i> सभी उत्पाद पब्लिश करें
                        </button>
                    </div>
                </form>
            </div>
        </div>

    </main>

    <!-- ================= CART & CHECKOUT SLIDE-OVER / MODAL ================= -->
    <div id="cartModal" class="fixed inset-0 bg-black bg-opacity-50 z-50 hidden flex justify-end">
        <div class="bg-white w-full max-w-md h-full flex flex-col shadow-2xl overflow-hidden">
            <!-- Header -->
            <div class="p-4 bg-slate-900 text-white flex justify-between items-center">
                <h3 class="text-lg font-bold flex items-center gap-2">
                    <i class="fa-solid fa-cart-shopping text-blue-400"></i> आपकी शॉपिंग कार्ट
                </h3>
                <button onclick="toggleCart()" class="text-gray-400 hover:text-white text-xl">&times;</button>
            </div>

            <!-- Cart Items List -->
            <div id="cartItems" class="flex-grow overflow-y-auto p-4 space-y-4">
                <!-- Cart items appended here -->
            </div>

            <!-- Footer / Order Form -->
            <div class="border-t p-4 bg-gray-50 max-h-[60vh] overflow-y-auto">
                <div class="flex justify-between items-center mb-4 text-lg font-bold">
                    <span>कुल राशि:</span>
                    <span id="cartTotal" class="text-blue-600">₹0</span>
                </div>

                <!-- Mandatory Checkout Form -->
                <form id="checkoutForm" onsubmit="processOrder(event)" class="space-y-3">
                    <h4 class="font-bold text-gray-800 text-sm border-b pb-1">सुरक्षा एवं डिलीवरी विवरण (अनिवार्य)</h4>

                    <div>
                        <label class="block text-xs font-semibold text-gray-600 mb-1">पूरा नाम *</label>
                        <input type="text" id="custName" required class="w-full border rounded p-2 text-sm outline-none focus:border-blue-500" placeholder="आपका पूरा नाम">
                    </div>

                    <div class="grid grid-cols-2 gap-2">
                        <div>
                            <label class="block text-xs font-semibold text-gray-600 mb-1">फ़ोन नंबर *</label>
                            <input type="tel" id="custPhone" required pattern="[0-9]{10}" class="w-full border rounded p-2 text-sm outline-none focus:border-blue-500" placeholder="10-अंकीय नंबर">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-gray-600 mb-1">अल्टरनेट फ़ोन *</label>
                            <input type="tel" id="custAltPhone" required pattern="[0-9]{10}" class="w-full border rounded p-2 text-sm outline-none focus:border-blue-500" placeholder="दूसरा फ़ोन नंबर">
                        </div>
                    </div>

                    <div>
                        <label class="block text-xs font-semibold text-gray-600 mb-1">ईमेल आईडी *</label>
                        <input type="email" id="custEmail" required class="w-full border rounded p-2 text-sm outline-none focus:border-blue-500" placeholder="example@mail.com">
                    </div>

                    <div>
                        <label class="block text-xs font-semibold text-gray-600 mb-1">पूरा डिलीवरी का पता *</label>
                        <textarea id="custAddress" required rows="2" class="w-full border rounded p-2 text-sm outline-none focus:border-blue-500" placeholder="मकान नंबर, गली, लैंडमार्क, पिनकोड..."></textarea>
                    </div>

                    <!-- GPS Live Location -->
                    <div class="bg-blue-50 p-2.5 rounded border border-blue-200">
                        <label class="block text-xs font-bold text-blue-900 mb-1">GPS लाइव लोकेशन शेयरिंग *</label>
                        <p class="text-[11px] text-gray-600 mb-2">सुरक्षा के लिए सटीक लोकेशन देना अनिवार्य है:</p>
                        <button type="button" onclick="captureGPSLocation()" id="gpsBtn" class="w-full bg-blue-600 hover:bg-blue-700 text-white text-xs font-bold py-2 rounded flex items-center justify-center gap-1">
                            <i class="fa-solid fa-location-crosshairs"></i> <span id="gpsStatus">लाइव लोकेशन प्राप्त करें</span>
                        </button>
                        <input type="hidden" id="custGPS" required>
                    </div>

                    <!-- ID Card Upload Option -->
                    <div class="bg-amber-50 p-2.5 rounded border border-amber-200">
                        <label class="block text-xs font-bold text-amber-900 mb-1">पहचान पत्र सत्यापन (ID Proof Upload) *</label>
                        <div class="flex gap-4 mb-2">
                            <label class="inline-flex items-center text-xs">
                                <input type="radio" name="idType" value="Aadhaar Card" checked class="text-blue-600">
                                <span class="ml-1">आधार कार्ड</span>
                            </label>
                            <label class="inline-flex items-center text-xs">
                                <input type="radio" name="idType" value="Voter ID Card" class="text-blue-600">
                                <span class="ml-1">वोटर आईडी</span>
                            </label>
                        </div>
                        <input type="file" id="custIdFile" accept="image/*,.pdf" required class="w-full text-xs text-gray-500 file:mr-2 file:py-1 file:px-2 file:rounded file:border-0 file:text-xs file:font-semibold file:bg-amber-600 file:text-white hover:file:bg-amber-700 cursor-pointer">
                    </div>

                    <button type="submit" id="submitOrderBtn" class="w-full bg-green-600 hover:bg-green-700 text-white font-bold py-3 rounded-lg shadow-md transition text-sm">
                        ऑर्डर कन्फर्म करें (Place Order)
                    </button>
                </form>
            </div>
        </div>
    </div>

    <!-- ================= OWNER LOGIN MODAL ================= -->
    <div id="loginModal" class="fixed inset-0 bg-black bg-opacity-60 z-50 hidden flex justify-center items-center p-4">
        <div class="bg-white rounded-2xl max-w-sm w-full p-6 shadow-2xl relative">
            <button onclick="closeLoginModal()" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600">&times;</button>
            <div class="text-center mb-6">
                <i class="fa-solid fa-user-shield text-4xl text-blue-600 mb-2"></i>
                <h3 class="text-xl font-bold text-gray-800">ओनर प्रवेश (Admin Login)</h3>
                <p class="text-xs text-gray-500">डिफ़ॉल्ट पासवर्ड: <span class="font-mono bg-gray-100 px-1 py-0.5 rounded font-bold">admin123</span></p>
            </div>

            <form onsubmit="handleLogin(event)" class="space-y-4">
                <div>
                    <label class="block text-xs font-semibold text-gray-600 mb-1">पासवर्ड दर्ज करें</label>
                    <input type="password" id="adminPassword" required class="w-full border rounded-lg p-2.5 text-sm outline-none focus:ring-2 focus:ring-blue-500" placeholder="••••••••">
                </div>
                <button type="submit" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-2.5 rounded-lg transition text-sm">
                    लॉगिन करें
                </button>
            </form>
        </div>
    </div>

    <!-- JavaScript Logic -->
    <script>
        // Sample Initial Products
        let products = [
            { id: 1, name: "HP LaserJet Pro M126a प्रिंटर", category: "Printer", price: 16499, image: "https://images.unsplash.com/photo-1612815154858-60aa4c59eaa6?w=400", desc: "मल्टी-फंक्शन ऑल-इन-वन लेजर प्रिंटर" },
            { id: 2, name: "Dell 24 inch Full HD मॉनिटर", category: "Monitor", price: 9999, image: "https://images.unsplash.com/photo-1527443224154-c4a3942d3acf?w=400", desc: "IPS पैनल, 75Hz रिफ्रेश रेट, बॉर्डरलेस" },
            { id: 3, name: "Intel Core i5 गेमिग CPU", category: "CPU", price: 34999, image: "https://images.unsplash.com/photo-1587202372775-e229f172b9d7?w=400", desc: "16GB RAM, 512GB SSD, Cabinet RGB" },
            { id: 4, name: "Logitech Wireless कीबोर्ड और माउस", category: "Keyboard", price: 1499, image: "https://images.unsplash.com/photo-1587829741301-dc798b83add3?w=400", desc: "2.4GHz वायरलेस कॉम्बो सेट" },
            { id: 5, name: "RGB गेमिन्ग माउस", category: "Mouse", price: 699, image: "https://images.unsplash.com/photo-1615663245857-ac93bb7c39e7?w=400", desc: "7 बटन, high DPI गेमिंग माउस" },
            { id: 6, name: "USB HD वेबकैम Mic के साथ", category: "Accessories", price: 1299, image: "https://images.unsplash.com/photo-1585060544812-6b45742d762f?w=400", desc: "1080p फुल एचडी वीडियो रिकॉर्डिंग" }
        ];

        let cart = [];
        let orders = [];
        let isLoggedIn = false;
        let selectedCategory = 'All';

        // Load LocalStorage Data on Init
        window.onload = function() {
            const savedProducts = localStorage.getItem('techcart_products');
            if(savedProducts) products = JSON.parse(savedProducts);

            const savedOrders = localStorage.getItem('techcart_orders');
            if(savedOrders) orders = JSON.parse(savedOrders);

            renderProducts();
            renderOrders();
            generateProductBoxes(3); // Initial 3 boxes for admin panel
        };

        // Render Products to Customer View
        function renderProducts() {
            const grid = document.getElementById('productsGrid');
            grid.innerHTML = '';

            const filtered = selectedCategory === 'All' 
                ? products 
                : products.filter(p => p.category === selectedCategory);

            if(filtered.length === 0) {
                grid.innerHTML = `<div class="col-span-full text-center py-10 text-gray-500">कोई उत्पाद उपलब्ध नहीं है।</div>`;
                return;
            }

            filtered.forEach(p => {
                grid.innerHTML += `
                    <div class="bg-white rounded-xl shadow-sm border border-gray-200 overflow-hidden flex flex-col product-card">
                        <img src="${p.image}" alt="${p.name}" class="h-44 w-full object-cover bg-gray-50">
                        <div class="p-4 flex-grow flex flex-col justify-between">
                            <div>
                                <span class="text-[10px] bg-blue-100 text-blue-800 font-bold px-2 py-0.5 rounded-full uppercase">${p.category}</span>
                                <h3 class="font-bold text-gray-800 mt-2 text-base leading-snug">${p.name}</h3>
                                <p class="text-xs text-gray-500 mt-1 line-clamp-2">${p.desc || ''}</p>
                            </div>
                            <div class="mt-4 flex justify-between items-center pt-2 border-t border-gray-100">
                                <span class="text-lg font-extrabold text-gray-900">₹${p.price.toLocaleString()}</span>
                                <button onclick="addToCart(${p.id})" class="bg-blue-600 hover:bg-blue-700 text-white px-3 py-1.5 rounded-lg text-xs font-semibold flex items-center gap-1 transition">
                                    <i class="fa-solid fa-plus"></i> कार्ट में जोड़ें
                                </button>
                            </div>
                        </div>
                    </div>
                `;
            });
        }

        // Category Filter
        function filterCategory(cat) {
            selectedCategory = cat;
            document.querySelectorAll('.cat-btn').forEach(b => {
                b.classList.remove('bg-blue-600', 'text-white');
                b.classList.add('bg-white', 'text-gray-700');
            });
            event.target.classList.remove('bg-white', 'text-gray-700');
            event.target.classList.add('bg-blue-600', 'text-white');
            renderProducts();
        }

        // Cart Actions
