 <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Font Lao -->
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Lao:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- QRCode.js library -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>

    <style>
        * { font-family: 'Noto Sans Lao', sans-serif; }
        .glass {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        .glass-card {
            background: rgba(15, 23, 42, 0.6);
            backdrop-filter: blur(8px);
            border: 1px solid rgba(255, 255, 255, 0.05);
        }
    </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen pb-10">

    <!-- Header Navigation -->
    <header class="glass sticky top-0 z-40 w-full px-6 py-4 flex flex-wrap justify-between items-center shadow-xl border-b border-slate-800">
        <div class="flex items-center space-x-3">
            <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-blue-600 to-indigo-500 flex items-center justify-center text-white text-xl font-bold shadow-lg shadow-blue-500/30">
                <i class="fa-solid me-0 fa-wallet"></i>
            </div>
            <div>
                <h1 class="text-xl font-bold bg-gradient-to-r from-blue-400 to-indigo-300 bg-clip-text text-transparent">PayCollector Online</h1>
                <p class="text-xs text-slate-400">ລະບົບເກັບເງິນ ແລະ ສະຫຼຸບຍອດອັດໂນມັດ</p>
            </div>
        </div>

        <div class="flex items-center gap-3 mt-3 sm:mt-0">
            <button onclick="exportToCSV()" class="px-4 py-2 text-sm bg-slate-800 hover:bg-slate-700 text-emerald-400 border border-emerald-500/30 rounded-xl transition flex items-center gap-2 shadow-lg">
                <i class="fa-solid fa-file-excel"></i> Export CSV
            </button>
        </div>
    </header>

    <div class="max-w-7xl mx-auto px-4 sm:px-6 mt-8 space-y-8">

        <!-- 1. Full Statistics Dashboard -->
        <section class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
            <!-- Grand Total LAK -->
            <div class="glass-card p-5 rounded-2xl relative overflow-hidden">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-sm font-medium text-slate-400">ຍອດເງິນຮັບທັງໝົດ (LAK)</p>
                        <h2 id="grandTotalLAK" class="text-2xl font-bold text-emerald-400 mt-2">0 ₭</h2>
                    </div>
                    <div class="p-3 bg-emerald-500/10 text-emerald-400 rounded-xl border border-emerald-500/20">
                        <i class="fa-solid fa-money-bill-wave text-xl"></i>
                    </div>
                </div>
                <!-- USD & THB Conversion -->
                <div class="mt-4 pt-3 border-t border-slate-800 flex justify-between text-xs text-slate-400">
                    <span>≈ <strong id="totalUSD" class="text-slate-200">0.00</strong> USD</span>
                    <span>≈ <strong id="totalTHB" class="text-slate-200">0.00</strong> THB</span>
                </div>
            </div>

            <!-- Total Transactions -->
            <div class="glass-card p-5 rounded-2xl">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-sm font-medium text-slate-400">ຈຳນວນລາຍການທັງໝົດ</p>
                        <h2 id="totalCount" class="text-2xl font-bold text-blue-400 mt-2">0 ລາຍການ</h2>
                    </div>
                    <div class="p-3 bg-blue-500/10 text-blue-400 rounded-xl border border-blue-500/20">
                        <i class="fa-solid fa-list-check text-xl"></i>
                    </div>
                </div>
                <div class="mt-4 pt-3 border-t border-slate-800 flex justify-between text-xs text-slate-400">
                    <span>ສະຖານະ ສຳເລັດ: <strong id="paidCount" class="text-emerald-400">0</strong></span>
                    <span>ລໍຖ້າ: <strong id="pendingCount" class="text-amber-400">0</strong></span>
                </div>
            </div>

            <!-- Transfer / BCEL One Total -->
            <div class="glass-card p-5 rounded-2xl">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-sm font-medium text-slate-400">ຍອດໂອນ / BCEL One</p>
                        <h2 id="totalTransfer" class="text-2xl font-bold text-cyan-400 mt-2">0 ₭</h2>
                    </div>
                    <div class="p-3 bg-cyan-500/10 text-cyan-400 rounded-xl border border-cyan-500/20">
                        <i class="fa-solid fa-qrcode text-xl"></i>
                    </div>
                </div>
                <div class="mt-4 pt-3 border-t border-slate-800 text-xs text-slate-400">
                    ກວມເອົາ <span id="transferPercent" class="text-cyan-300 font-semibold">0%</span> ຂອງຍອດທັງໝົດ
                </div>
            </div>

            <!-- Cash Total -->
            <div class="glass-card p-5 rounded-2xl">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-sm font-medium text-slate-400">ຍອດເງິນສົດ</p>
                        <h2 id="totalCash" class="text-2xl font-bold text-amber-400 mt-2">0 ₭</h2>
                    </div>
                    <div class="p-3 bg-amber-500/10 text-amber-400 rounded-xl border border-amber-500/20">
                        <i class="fa-solid fa-coins text-xl"></i>
                    </div>
                </div>
                <div class="mt-4 pt-3 border-t border-slate-800 text-xs text-slate-400">
                    ກວມເອົາ <span id="cashPercent" class="text-amber-300 font-semibold">0%</span> ຂອງຍອດທັງໝົດ
                </div>
            </div>
        </section>

        <!-- Main Content Grid -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
            
            <!-- 2. Form & Payment Gateway Selection -->
            <section class="glass-card p-6 rounded-2xl h-fit border border-slate-800">
                <h3 class="text-lg font-bold text-slate-100 mb-4 flex items-center gap-2">
                    <i class="fa-solid fa-circle-plus text-blue-400"></i> ບັນທຶກການເກັບເງິນໃໝ່
                </h3>
                
                <form id="paymentForm" onsubmit="handleFormSubmit(event)" class="space-y-4">
                    <div>
                        <label class="block text-xs font-medium text-slate-400 mb-1">ຊື່ຜູ້ຈ່າຍເງິນ / ໂຄງການ</label>
                        <input type="text" id="payerName" required placeholder="ຕົວຢ່າງ: ທ່ານ ສົມໄຊ" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:border-blue-500 transition">
                    </div>

                    <div>
                        <label class="block text-xs font-medium text-slate-400 mb-1">ລາຍການ / ໝວດໝູ່</label>
                        <input type="text" id="itemTitle" required placeholder="ຕົວຢ່າງ: ບໍລິການອອກແບບ / ຄ່າສິນຄ້າ" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:border-blue-500 transition">
                    </div>

                    <div>
                        <label class="block text-xs font-medium text-slate-400 mb-1">ຈຳນວນເງິນ (LAK)</label>
                        <input type="number" id="amount" required min="1" placeholder="0" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:border-blue-500 transition">
                        
                        <!-- Quick Amount Buttons -->
                        <div class="flex gap-2 mt-2">
                            <button type="button" onclick="setQuickAmount(50000)" class="text-xs bg-slate-800 hover:bg-slate-700 text-slate-300 px-3 py-1 rounded-lg border border-slate-700">+50K</button>
                            <button type="button" onclick="setQuickAmount(100000)" class="text-xs bg-slate-800 hover:bg-slate-700 text-slate-300 px-3 py-1 rounded-lg border border-slate-700">+100K</button>
                            <button type="button" onclick="setQuickAmount(500000)" class="text-xs bg-slate-800 hover:bg-slate-700 text-slate-300 px-3 py-1 rounded-lg border border-slate-700">+500K</button>
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs font-medium text-slate-400 mb-1">ຮູບແບບການຊຳລະ</label>
                            <select id="paymentMethod" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3 py-2.5 text-sm focus:outline-none focus:border-blue-500">
                                <option value="BCEL ONE">BCEL One QR</option>
                                <option value="BANK">ໂອນຜ່ານທະນາຄານ</option>
                                <option value="CASH">ເງິນສົດ (Cash)</option>
                            </select>
                        </div>

                        <div>
                            <label class="block text-xs font-medium text-slate-400 mb-1">ສະຖານະ</label>
                            <select id="paymentStatus" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3 py-2.5 text-sm focus:outline-none focus:border-blue-500">
                                <option value="PAID">ຊຳລະແລ້ວ (Paid)</option>
                                <option value="PENDING">ລໍຖ້າ (Pending)</option>
                                <option value="REFUNDED">ສົ່ງເງິນຄືນ (Refunded)</option>
                            </select>
                        </div>
                    </div>

                    <button type="submit" class="w-full bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-500 hover:to-indigo-500 text-white font-medium py-3 rounded-xl transition shadow-lg shadow-blue-600/30 flex items-center justify-center gap-2 mt-4">
                        <i class="fa-solid fa-save"></i> ບັນທຶກ ແລະ ອອກໃບເສັດ
                    </button>
                </form>
            </section>

            <!-- Data Table Section -->
            <section class="glass-card p-6 rounded-2xl lg:col-span-2 border border-slate-800 flex flex-col">
                <div class="flex flex-wrap justify-between items-center gap-4 mb-4">
                    <h3 class="text-lg font-bold text-slate-100 flex items-center gap-2">
                        <i class="fa-solid fa-clock-rotate-left text-blue-400"></i> ປະວັດການເກັບເງິນ
                    </h3>
                    
                    <input type="text" id="searchInput" oninput="renderData()" placeholder="ຄົ້ນຫາຊື່/ລາຍການ..." class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-1.5 text-xs focus:outline-none focus:border-blue-500 w-full sm:w-48">
                </div>

                <div class="overflow-x-auto flex-1">
                    <table class="w-full text-left text-sm text-slate-300">
                        <thead class="text-xs uppercase bg-slate-900/80 text-slate-400 border-b border-slate-800">
                            <tr>
                                <th class="py-3 px-4">ວັນທີ-ເວລາ</th>
                                <th class="py-3 px-4">ຜູ້ຈ່າຍ / ລາຍການ</th>
                                <th class="py-3 px-4">ວິທີຊຳລະ</th>
                                <th class="py-3 px-4 text-right">ຈຳນວນເງິນ (LAK)</th>
                                <th class="py-3 px-4 text-center">ສະຖານະ</th>
                                <th class="py-3 px-4 text-center">ຈັດການ</th>
                            </tr>
                        </thead>
                        <tbody id="dataTableBody" class="divide-y divide-slate-800/50">
                            <!-- Rows rendered dynamically -->
                        </tbody>
                    </table>
                </div>
            </section>
        </div>
    </div>

    <!-- 3. Digital Receipt Modal with QR Code -->
    <div id="receiptModal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-md hidden z-50 flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-700 rounded-3xl max-w-sm w-full p-6 shadow-2xl relative text-slate-100">
            <button onclick="closeReceipt()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
                <i class="fa-solid fa-xmark text-xl"></i>
            </button>

            <!-- Printable Receipt Area -->
            <div id="printableReceipt" class="bg-white text-slate-900 p-6 rounded-2xl space-y-4">
                <div class="text-center border-b pb-3 border-slate-200">
                    <h4 class="text-xl font-bold uppercase tracking-wide">ໃບເສັດຮັບເງິນ</h4>
                    <p class="text-xs text-slate-500">Official Payment Receipt</p>
                    <p id="rcpDate" class="text-[10px] text-slate-400 mt-1"></p>
                </div>

                <div class="space-y-1 text-xs">
                    <div class="flex justify-between"><span class="text-slate-500">ລະຮັດອ້າງອີງ:</span> <strong id="rcpId">#0000</strong></div>
                    <div class="flex justify-between"><span class="text-slate-500">ຜູ້ຈ່າຍເງິນ:</span> <strong id="rcpPayer">--</strong></div>
                    <div class="flex justify-between"><span class="text-slate-500">ລາຍການ:</span> <span id="rcpItem" class="font-medium">--</span></div>
                    <div class="flex justify-between"><span class="text-slate-500">ວິທີຊຳລະ:</span> <span id="rcpMethod" class="font-medium">--</span></div>
                </div>

                <div class="bg-slate-100 p-3 rounded-xl text-center border border-slate-200">
                    <span class="text-xs text-slate-500 block">ຈຳນວນເງິນທັງໝົດ</span>
                    <span id="rcpAmount" class="text-xl font-extrabold text-blue-600">0 ₭</span>
                </div>

                <!-- Generated QR Code Container -->
                <div class="flex flex-col items-center justify-center pt-2">
                    <div id="qrcode" class="p-2 bg-white rounded-lg border border-slate-200"></div>
                    <span class="text-[9px] text-slate-400 mt-1">Scan to Verify Transaction</span>
                </div>
            </div>

            <!-- Receipt Modal Actions -->
            <div class="mt-4 flex gap-2">
                <button onclick="window.print()" class="flex-1 bg-blue-600 hover:bg-blue-500 text-white font-medium py-2.5 rounded-xl transition flex items-center justify-center gap-2 text-sm">
                    <i class="fa-solid fa-print"></i> ພິມໃບເສັດ
                </button>
                <button onclick="closeReceipt()" class="px-4 bg-slate-800 hover:bg-slate-700 text-slate-300 font-medium py-2.5 rounded-xl transition text-sm">
                    ປິດ
                </button>
            </div>
        </div>
    </div>

    <!-- JavaScript Application Logic -->
    <script>
        const USD_RATE = 22000; // 1 USD = 22,000 LAK
        const THB_RATE = 620;   // 1 THB = 620 LAK

        // 4. Data Persistence via LocalStorage
        let records = JSON.parse(localStorage.getItem('payment_records')) || [];

        function saveToStorage() {
            localStorage.setItem('payment_records', JSON.stringify(records));
        }

        function setQuickAmount(val) {
            document.getElementById('amount').value = val;
        }

        function handleFormSubmit(e) {
            e.preventDefault();
            const record = {
                id: 'TX' + Date.now().toString().slice(-6),
                payer: document.getElementById('payerName').value.trim(),
                item: document.getElementById('itemTitle').value.trim(),
                amount: parseFloat(document.getElementById('amount').value),
                method: document.getElementById('paymentMethod').value,
                status: document.getElementById('paymentStatus').value,
                timestamp: new Date().toLocaleString('lo-LA')
            };

            records.unshift(record);
            saveToStorage();
            renderData();
            document.getElementById('paymentForm').reset();
            openReceipt(record);
        }

        function deleteRecord(id) {
            if (confirm('ທ່ານຕ້ອງການລຶບລາຍການນີ້ແທ້ບໍ?')) {
                records = records.filter(r => r.id !== id);
                saveToStorage();
                renderData();
            }
        }

        function renderData() {
            const query = document.getElementById('searchInput').value.toLowerCase();
            const tbody = document.getElementById('dataTableBody');
            tbody.innerHTML = '';

            const filtered = records.filter(r => 
                r.payer.toLowerCase().includes(query) || 
                r.item.toLowerCase().includes(query)
            );

            filtered.forEach(r => {
                const tr = document.createElement('tr');
                tr.className = 'hover:bg-slate-900/40 transition border-b border-slate-800/40';

                let statusBadge = r.status === 'PAID' 
                    ? '<span class="px-2 py-0.5 text-xs font-semibold bg-emerald-500/10 text-emerald-400 rounded-md border border-emerald-500/20">PAID</span>'
                    : r.status === 'PENDING'
                    ? '<span class="px-2 py-0.5 text-xs font-semibold bg-amber-500/10 text-amber-400 rounded-md border border-amber-500/20">PENDING</span>'
                    : '<span class="px-2 py-0.5 text-xs font-semibold bg-rose-500/10 text-rose-400 rounded-md border border-rose-500/20">REFUNDED</span>';

                tr.innerHTML = `
                    <td class="py-3 px-4 text-xs text-slate-400">${r.timestamp}</td>
                    <td class="py-3 px-4">
                        <div class="font-medium text-slate-200">${escapeHtml(r.payer)}</div>
                        <div class="text-xs text-slate-400">${escapeHtml(r.item)}</div>
                    </td>
                    <td class="py-3 px-4 text-xs font-medium">${r.method}</td>
                    <td class="py-3 px-4 text-right font-semibold text-emerald-400">${r.amount.toLocaleString()} ₭</td>
                    <td class="py-3 px-4 text-center">${statusBadge}</td>
                    <td class="py-3 px-4 text-center">
                        <button onclick='openReceiptById("${r.id}")' class="text-blue-400 hover:text-blue-300 mx-1" title="ໃບເສັດ"><i class="fa-solid fa-receipt"></i></button>
                        <button onclick='deleteRecord("${r.id}")' class="text-rose-400 hover:text-rose-300 mx-1" title="ລຶບ"><i class="fa-solid fa-trash"></i></button>
                    </td>
                `;
                tbody.appendChild(tr);
            });

            updateDashboard();
        }

        function updateDashboard() {
            const totalLAK = records.reduce((sum, r) => r.status === 'PAID' ? sum + r.amount : sum, 0);
            const totalCash = records.reduce((sum, r) => (r.status === 'PAID' && r.method === 'CASH') ? sum + r.amount : sum, 0);
            const totalTransfer = records.reduce((sum, r) => (r.status === 'PAID' && r.method !== 'CASH') ? sum + r.amount : sum, 0);

            document.getElementById('grandTotalLAK').innerText = totalLAK.toLocaleString() + ' ₭';
            document.getElementById('totalUSD').innerText = (totalLAK / USD_RATE).toFixed(2);
            document.getElementById('totalTHB').innerText = (totalLAK / THB_RATE).toFixed(2);

            document.getElementById('totalCount').innerText = records.length + ' ລາຍການ';
            document.getElementById('paidCount').innerText = records.filter(r => r.status === 'PAID').length;
            document.getElementById('pendingCount').innerText = records.filter(r => r.status === 'PENDING').length;

            document.getElementById('totalTransfer').innerText = totalTransfer.toLocaleString() + ' ₭';
            document.getElementById('totalCash').innerText = totalCash.toLocaleString() + ' ₭';

            const transferPct = totalLAK > 0 ? Math.round((totalTransfer / totalLAK) * 100) : 0;
            const cashPct = totalLAK > 0 ? Math.round((totalCash / totalLAK) * 100) : 0;

            document.getElementById('transferPercent').innerText = transferPct + '%';
            document.getElementById('cashPercent').innerText = cashPct + '%';
        }

        // 3. Digital Receipt & Dynamic QR Generator
        function openReceipt(record) {
            document.getElementById('rcpId').innerText = '#' + record.id;
            document.getElementById('rcpPayer').innerText = record.payer;
            document.getElementById('rcpItem').innerText = record.item;
            document.getElementById('rcpMethod').innerText = record.method;
            document.getElementById('rcpAmount').innerText = record.amount.toLocaleString() + ' ₭';
            document.getElementById('rcpDate').innerText = record.timestamp;

            const qrContainer = document.getElementById('qrcode');
            qrContainer.innerHTML = '';
            new QRCode(qrContainer, {
                text: `VERIFY:${record.id}|AMT:${record.amount}|PAYER:${record.payer}`,
                width: 90,
                height: 90
            });

            document.getElementById('receiptModal').classList.remove('hidden');
        }

        function openReceiptById(id) {
            const record = records.find(r => r.id === id);
            if (record) openReceipt(record);
        }

        function closeReceipt() {
            document.getElementById('receiptModal').classList.add('hidden');
        }

        // CSV Export Feature
        function exportToCSV() {
            if (records.length === 0) {
                alert('ບໍ່ມີຂໍ້ມູນ ສຳລັບການ Export');
                return;
            }
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            csvContent += "ID,ວັນທີ,ຜູ້ຈ່າຍ,ລາຍການ,ວິທີຊຳລະ,ຈຳນວນເງິນ(LAK),ສະຖານະ\n";

            records.forEach(r => {
                csvContent += `"${r.id}","${r.timestamp}","${r.payer}","${r.item}","${r.method}","${r.amount}","${r.status}"\n`;
            });

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Payment_Report_${Date.now()}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }

        function escapeHtml(str) {
            return str.replace(/&/g, "&").replace(/</g, "<").replace(/>/g, ">");
        }

        // Initial Render
        renderData();
    </script>
</body>
</html>
