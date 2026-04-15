
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ruan Assistec | Global Enterprise</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; overflow-x: hidden; }
        .view { display: none; }
        .view.active { display: flex; }
        .sidebar-link.active { background: #0ea5e9; color: white; }
    </style>
</head>
<body class="bg-slate-100 min-h-screen">

    <div class="fixed top-6 right-6 z-[200]">
        <div class="relative inline-block text-left">
            <button onclick="toggleLangMenu()" class="bg-white border shadow-lg rounded-full p-3 flex items-center gap-2 hover:bg-gray-50 transition">
                <i class="fas fa-globe-americas text-cyan-600 text-xl"></i>
                <span id="current-lang-name" class="text-xs font-bold text-slate-700 hidden md:block">Português (Brasil)</span>
            </button>
            <div id="lang-menu" class="hidden absolute right-0 mt-2 w-48 bg-white rounded-2xl shadow-2xl border border-slate-100 overflow-hidden">
                <button onclick="changeLang('pt')" class="w-full text-left p-4 text-sm hover:bg-slate-50 flex items-center gap-3">🇧🇷 Português</button>
                <button onclick="changeLang('en')" class="w-full text-left p-4 text-sm hover:bg-slate-50 flex items-center gap-3">🇺🇸 English</button>
                <button onclick="changeLang('es')" class="w-full text-left p-4 text-sm hover:bg-slate-50 flex items-center gap-3">🇪🇸 Español</button>
            </div>
        </div>
    </div>

    <div id="login-view" class="view active items-center justify-center min-h-screen bg-slate-900 p-4">
        <div class="bg-white p-8 md:p-12 rounded-[3rem] shadow-2xl w-full max-w-md text-center">
            <div class="mb-10">
                <i class="fas fa-microchip text-6xl text-cyan-500 mb-4"></i>
                <h1 class="text-4xl font-black tracking-tighter italic">RUAN <span class="text-cyan-500">ASSISTEC</span></h1>
                <p id="txt-login-subtitle" class="text-slate-400 text-[10px] font-bold tracking-widest uppercase mt-2 italic">Tecnologia & Mercado</p>
            </div>
            <div class="space-y-4">
                <button onclick="switchView('cliente')" id="btn-client" class="w-full bg-slate-100 text-slate-800 font-black py-5 rounded-2xl hover:bg-slate-200 transition uppercase text-xs">SOU CLIENTE</button>
                <div class="relative py-4 text-slate-200"><hr><span id="txt-admin-label" class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white px-4 text-[9px] font-black text-slate-400 uppercase tracking-widest">Acesso Administrativo</span></div>
                <input type="password" id="login-pass" class="w-full p-5 bg-slate-50 border border-slate-100 rounded-2xl focus:ring-2 focus:ring-cyan-500 outline-none text-center font-bold" placeholder="••••••••">
                <button onclick="checkLogin()" id="btn-admin" class="w-full bg-slate-900 text-white font-black py-5 rounded-2xl hover:bg-cyan-600 shadow-2xl transition uppercase text-xs">ENTRAR NO PAINEL</button>
            </div>
            <p id="login-error" class="text-red-500 text-[10px] mt-6 hidden font-black uppercase">Senha incorreta</p>
        </div>
    </div>

    <div id="admin-view" class="view flex-col md:flex-row min-h-screen">
        <aside class="w-full md:w-72 bg-slate-900 text-white flex flex-col p-6 shadow-2xl">
            <h2 class="text-2xl font-black italic mb-10">RUAN <span class="text-cyan-400 font-black">ADM</span></h2>
            <nav class="flex-1 space-y-3">
                <button onclick="switchTab('dashboard')" class="sidebar-link w-full text-left p-4 rounded-2xl flex items-center gap-4 font-bold transition active"><i class="fas fa-chart-line"></i> <span class="txt-nav-dash">Dashboard</span></button>
                <button onclick="switchTab('os')" class="sidebar-link w-full text-left p-4 rounded-2xl flex items-center gap-4 font-bold transition"><i class="fas fa-wrench"></i> <span class="txt-nav-os">Ordens</span></button>
                <button onclick="switchTab('estoque')" class="sidebar-link w-full text-left p-4 rounded-2xl flex items-center gap-4 font-bold transition"><i class="fas fa-store"></i> <span class="txt-nav-stock">Mercado</span></button>
            </nav>
            <button onclick="logout()" class="p-4 text-red-400 font-black flex items-center gap-3 mt-auto rounded-2xl hover:bg-red-500/10 transition">
                <i class="fas fa-power-off"></i> <span class="txt-logout">SAIR</span>
            </button>
        </aside>
        
        <main class="flex-1 p-6 md:p-12 overflow-y-auto">
            <div id="tab-dashboard" class="tab-content space-y-8">
                <h2 class="text-2xl font-black text-slate-800 txt-nav-dash">Dashboard</h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                    <div class="bg-white p-8 rounded-[2.5rem] shadow-sm"><p class="text-slate-400 text-xs font-black uppercase mb-2 txt-nav-os">Ordens</p><h3 id="dash-os" class="text-5xl font-black text-slate-900">0</h3></div>
                    <div class="bg-white p-8 rounded-[2.5rem] shadow-sm"><p class="text-slate-400 text-xs font-black uppercase mb-2 txt-nav-stock">Estoque</p><h3 id="dash-stock" class="text-5xl font-black text-cyan-600">0</h3></div>
                    <div class="bg-white p-8 rounded-[2.5rem] shadow-sm border-2 border-red-50"><p id="txt-alert-label" class="text-red-400 text-xs font-black uppercase mb-2">Alertas</p><h3 id="dash-alert" class="text-5xl font-black text-red-500">0</h3></div>
                </div>
            </div>

            <div id="tab-os" class="tab-content hidden space-y-6">
                <div class="bg-white p-8 rounded-[2.5rem] shadow-sm">
                    <h3 class="font-black mb-6 uppercase text-sm">Registrar Nova OS</h3>
                    <form id="osForm" class="grid grid-cols-1 md:grid-cols-4 gap-4">
                        <input type="text" id="os-cliente" placeholder="Cliente" class="p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500 transition" required>
                        <input type="number" id="os-whats" placeholder="WhatsApp (DDD)" class="p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500 transition" required>
                        <input type="text" id="os-aparelho" placeholder="Aparelho" class="p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500 transition" required>
                        <button class="bg-cyan-600 text-white font-black rounded-2xl uppercase text-xs">Abrir OS</button>
                    </form>
                </div>
                <div class="bg-white rounded-[2.5rem] shadow-sm overflow-hidden">
                    <table class="w-full text-left">
                        <thead class="bg-slate-50 text-[10px] uppercase font-black text-slate-400 border-b">
                            <tr><th class="p-6">ID</th><th class="p-6">Cliente</th><th class="p-6">Aparelho</th><th class="p-6">Status</th><th class="p-6 text-right">Ações</th></tr>
                        </thead>
                        <tbody id="osTableBody" class="divide-y divide-slate-50"></tbody>
                    </table>
                </div>
            </div>

            <div id="tab-estoque" class="tab-content hidden space-y-6">
                <div class="bg-white p-8 rounded-[2.5rem] shadow-sm">
                    <h3 class="font-black mb-6 uppercase text-sm text-cyan-600">Gerenciar Produtos</h3>
                    <form id="prodForm" class="grid grid-cols-1 md:grid-cols-4 gap-4">
                        <input type="text" id="p-nome" placeholder="Nome do Produto" class="p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500 transition" required>
                        <input type="number" id="p-qtd" placeholder="Qtd" class="p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500 transition" required>
                        <input type="number" step="0.01" id="p-preco" placeholder="Preço" class="p-4 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500 transition" required>
                        <button class="bg-slate-900 text-white font-black rounded-2xl uppercase text-xs">Cadastrar</button>
                    </form>
                </div>
                <div class="bg-white rounded-[2.5rem] shadow-sm overflow-hidden">
                    <table class="w-full text-left">
                        <thead class="bg-slate-50 text-[10px] uppercase font-black text-slate-400 border-b">
                            <tr><th class="p-6">Produto</th><th class="p-6">Qtd</th><th class="p-6">Preço</th><th class="p-6 text-right">Controle</th></tr>
                        </thead>
                        <tbody id="prodTableBody" class="divide-y divide-slate-50"></tbody>
                    </table>
                </div>
            </div>
        </main>
    </div>

    <div id="cliente-view" class="view flex-col min-h-screen">
        <header class="bg-white p-6 shadow-sm border-b sticky top-0 z-50 flex flex-col md:flex-row gap-6 justify-between items-center">
            <h1 class="text-2xl font-black italic">RUAN <span class="text-cyan-500">STORE</span></h1>
            <div class="flex-1 max-w-2xl w-full">
                <div class="relative">
                    <i class="fas fa-search absolute left-5 top-1/2 -translate-y-1/2 text-slate-300"></i>
                    <input type="text" id="searchBar" onkeyup="renderAll()" class="w-full p-5 pl-14 bg-slate-50 rounded-[1.5rem] border-none ring-1 ring-slate-100 outline-none focus:ring-2 focus:ring-cyan-500 transition" placeholder="O que você procura?">
                </div>
            </div>
            <div class="flex gap-4">
                <button onclick="openCart()" class="relative p-4 bg-slate-900 text-white rounded-2xl shadow-xl">
                    <i class="fas fa-shopping-basket"></i>
                    <span id="cart-count" class="absolute -top-2 -right-2 bg-cyan-500 text-white text-[10px] font-black px-2 py-0.5 rounded-full border-2 border-white">0</span>
                </button>
                <button onclick="logout()" class="p-4 bg-slate-50 rounded-2xl text-slate-400"><i class="fas fa-times"></i></button>
            </div>
        </header>
        <main class="container mx-auto p-8">
            <div id="loja-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8"></div>
        </main>
    </div>

    <div id="cart-modal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-md z-[300] hidden items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-[3rem] p-10 shadow-2xl">
            <div class="flex justify-between items-center mb-8 border-b pb-6">
                <h2 id="txt-cart-title" class="text-3xl font-black italic">Carrinho</h2>
                <button onclick="closeCart()" class="text-4xl text-slate-200 hover:text-slate-400">&times;</button>
            </div>
            <div id="cart-items" class="space-y-4 mb-8 max-h-60 overflow-y-auto px-2"></div>
            <div class="pt-6 space-y-6">
                <div class="flex justify-between items-end font-black"><span class="text-slate-400 text-xs uppercase tracking-widest">Total</span><span id="cart-total" class="text-3xl text-cyan-600">R$ 0,00</span></div>
                <div class="grid grid-cols-2 gap-4">
                    <input type="time" id="c-hora" class="p-5 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none font-bold">
                    <select id="c-pgto" class="p-5 bg-slate-50 rounded-2xl border-none ring-1 ring-slate-100 outline-none font-bold">
                        <option>Pix</option><option>Dinheiro</option><option>Cartão</option>
                    </select>
                </div>
                <button onclick="finalizarPedido()" id="btn-finish" class="w-full bg-green-500 text-white font-black py-6 rounded-3xl shadow-xl hover:bg-green-600 transition uppercase tracking-widest text-xs">Finalizar Pedido</button>
            </div>
        </div>
    </div>

    <script>
        const SENHA_ADM = "admin123";
        let ordens = JSON.parse(localStorage.getItem('r_global_os_v3')) || [];
        let produtos = JSON.parse(localStorage.getItem('r_global_pr_v3')) || [];
        let carrinho = [];
        let lang = 'pt';

        const dict = {
            pt: { name: "Português (Brasil)", client: "SOU CLIENTE", admin: "ENTRAR NO PAINEL", subtitle: "TECNOLOGIA & MERCADO", admLabel: "ACESSO ADMINISTRATIVO", dash: "Dashboard", os: "Ordens", stock: "Mercado", logout: "SAIR", alerts: "Alertas", search: "O que você procura?", cart: "Meu Carrinho", pick: "PEGAR", finish: "FINALIZAR PEDIDO" },
            en: { name: "English (USA)", client: "I'M CUSTOMER", admin: "LOGIN TO PANEL", subtitle: "TECH & MARKET", admLabel: "ADMINISTRATIVE ACCESS", dash: "Dashboard", os: "Orders", stock: "Market", logout: "LOGOUT", alerts: "Alerts", search: "Search products...", cart: "My Cart", pick: "GET IT", finish: "FINISH ORDER" },
            es: { name: "Español (ES)", client: "SOY CLIENTE", admin: "ENTRAR AL PANEL", subtitle: "TECNOLOGÍA Y MERCADO", admLabel: "ACCESO ADMINISTRATIVO", dash: "Tablero", os: "Órdenes", stock: "Mercado", logout: "SALIR", alerts: "Alertas", search: "¿Qué estás buscando?", cart: "Mi Carrito", pick: "COMPRAR", finish: "FINALIZAR PEDIDO" }
        };

        function toggleLangMenu() { document.getElementById('lang-menu').classList.toggle('hidden'); }

        function changeLang(l) {
            lang = l;
            document.getElementById('current-lang-name').innerText = dict[l].name;
            document.getElementById('btn-client').innerText = dict[l].client;
            document.getElementById('btn-admin').innerText = dict[l].admin;
            document.getElementById('txt-login-subtitle').innerText = dict[l].subtitle;
            document.getElementById('txt-admin-label').innerText = dict[l].admLabel;
            document.getElementById('searchBar').placeholder = dict[l].search;
            document.getElementById('txt-cart-title').innerText = dict[l].cart;
            document.getElementById('btn-finish').innerText = dict[l].finish;
            document.getElementById('txt-alert-label').innerText = dict[l].alerts;
            document.querySelectorAll('.txt-nav-dash').forEach(e => e.innerText = dict[l].dash);
            document.querySelectorAll('.txt-nav-os').forEach(e => e.innerText = dict[l].os);
            document.querySelectorAll('.txt-nav-stock').forEach(e => e.innerText = dict[l].stock);
            document.querySelectorAll('.txt-logout').forEach(e => e.innerText = dict[l].logout);
            document.getElementById('lang-menu').classList.add('hidden');
            renderAll();
        }

        function switchView(v) { document.querySelectorAll('.view').forEach(e => e.classList.remove('active')); document.getElementById(v + '-view').classList.add('active'); renderAll(); }
        function checkLogin() { if(document.getElementById('login-pass').value === SENHA_ADM) switchView('admin'); else document.getElementById('login-error').classList.remove('hidden'); }
        function logout() { location.reload(); }
        
        function switchTab(t) { 
            document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden')); 
            document.querySelectorAll('.sidebar-link').forEach(l => l.classList.remove('active')); 
            document.getElementById('tab-' + t).classList.remove('hidden'); 
            event.currentTarget.classList.add('active'); 
        }

        // Lógica ADM OS e Produtos
        document.getElementById('osForm').addEventListener('submit', (e) => {
            e.preventDefault();
            ordens.unshift({ id: Math.floor(Math.random()*9000)+1000, cliente: document.getElementById('os-cliente').value, whatsapp: document.getElementById('os-whats').value, aparelho: document.getElementById('os-aparelho').value, status: 'Pendente' });
            save(); e.target.reset();
        });

        document.getElementById('prodForm').addEventListener('submit', (e) => {
            e.preventDefault();
            produtos.push({ id: Date.now(), nome: document.getElementById('p-nome').value, qtd: parseInt(document.getElementById('p-qtd').value), preco: parseFloat(document.getElementById('p-preco').value) });
            save(); e.target.reset();
        });

        function save() { 
            localStorage.setItem('r_global_os_v3', JSON.stringify(ordens)); 
            localStorage.setItem('r_global_pr_v3', JSON.stringify(produtos)); 
            renderAll(); 
        }

        function renderAll() {
            const search = (document.getElementById('searchBar')?.value || "").toLowerCase();
            
            // Dashboard
            if(document.getElementById('dash-os')) {
                document.getElementById('dash-os').innerText = ordens.length;
                document.getElementById('dash-stock').innerText = produtos.length;
                document.getElementById('dash-alert').innerText = produtos.filter(p => p.qtd < 5).length;
            }

            // Tabelas ADM
            const osBody = document.getElementById('osTableBody');
            if(osBody) osBody.innerHTML = ordens.map((o,i) => `
                <tr class="hover:bg-slate-50 transition">
                    <td class="p-6 text-xs font-mono text-slate-400">#${o.id}</td>
                    <td class="p-6 font-bold text-slate-800">${o.cliente}<br><span class="text-[10px] text-slate-400">${o.whatsapp}</span></td>
                    <td class="p-6 font-medium">${o.aparelho}</td>
                    <td class="p-6"><span class="px-3 py-1 text-[10px] font-black rounded-full ${o.status==='Concluído'?'bg-green-100 text-green-700':'bg-amber-100 text-amber-700'}">${o.status}</span></td>
                    <td class="p-6 text-right">
                        <button onclick="mStatus(${i})" class="text-cyan-500 mr-3"><i class="fas fa-sync"></i></button>
                        <button onclick="dOS(${i})" class="text-red-300"><i class="fas fa-trash"></i></button>
                    </td>
                </tr>`).join('');

            const prBody = document.getElementById('prodTableBody');
            if(prBody) prBody.innerHTML = produtos.map((p,i) => `
                <tr class="hover:bg-slate-50 transition">
                    <td class="p-6 font-bold">${p.nome}</td>
                    <td class="p-6"><span class="px-3 py-1 rounded-lg bg-slate-100 font-black ${p.qtd < 5 ? 'text-red-500' : ''}">${p.qtd}</span></td>
                    <td class="p-6 font-bold text-green-600">R$ ${p.preco.toFixed(2)}</td>
                    <td class="p-6 text-right"><button onclick="dPr(${i})" class="text-red-400"><i class="fas fa-times-circle"></i></button></td>
                </tr>`).join('');

            // Loja Cliente
            const grid = document.getElementById('loja-grid');
            if(grid) grid.innerHTML = produtos.filter(p => p.nome.toLowerCase().includes(search)).map(p => `
                <div class="bg-white p-8 rounded-[3rem] shadow-sm border border-slate-50 flex flex-col justify-between hover:shadow-2xl transition">
                    <div class="w-full h-32 bg-slate-50 rounded-[2rem] mb-6 flex items-center justify-center text-slate-200 text-4xl"><i class
