
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ruan Assistec | Global Multi-Language System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; overflow: hidden; }
        .view { display: none; width: 100%; min-height: 100vh; }
        .view.active { display: flex; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .sidebar-link.active { background: #0ea5e9; color: white; }
        /* Custom scrollbar para o menu de países */
        #lang-menu::-webkit-scrollbar { width: 4px; }
        #lang-menu::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
    </style>
</head>
<body class="bg-slate-100">

    <div class="fixed top-6 right-6 z-[500]">
        <div class="relative inline-block text-left">
            <button onclick="toggleLangMenu()" class="bg-white border shadow-2xl rounded-full p-4 flex items-center gap-3 hover:scale-105 transition-all">
                <i class="fas fa-globe text-cyan-600 text-xl animate-spin-slow"></i>
                <span id="current-flag" class="text-xl">🇧🇷</span>
            </button>
            <div id="lang-menu" class="hidden absolute right-0 mt-3 w-64 max-h-96 bg-white rounded-[2rem] shadow-2xl border border-slate-100 overflow-y-auto z-[501]">
                <div class="p-4 border-b sticky top-0 bg-white z-10">
                    <input type="text" id="searchCountry" onkeyup="filterCountries()" placeholder="Search country..." class="w-full p-2 bg-slate-50 rounded-lg text-xs outline-none">
                </div>
                <div id="country-list" class="p-2 space-y-1">
                    </div>
            </div>
        </div>
    </div>

    <div id="login-view" class="view active items-center justify-center bg-slate-900 p-4 relative">
        <div id="lockdown-overlay" class="hidden absolute inset-0 bg-black/95 z-[300] flex flex-col items-center justify-center text-white text-center p-6">
            <i class="fas fa-shield-virus text-6xl text-red-500 mb-4 animate-pulse"></i>
            <h2 id="txt-lock-title" class="text-2xl font-black uppercase">SECURITY LOCK</h2>
            <p id="timer" class="text-4xl font-mono text-cyan-500 mt-4">30s</p>
        </div>

        <div class="bg-white p-8 md:p-12 rounded-[3rem] shadow-2xl w-full max-w-md text-center">
            <div class="mb-10">
                <div class="bg-cyan-500 w-20 h-20 rounded-3xl flex items-center justify-center mx-auto mb-4 shadow-lg">
                    <i class="fas fa-microchip text-4xl text-white"></i>
                </div>
                <h1 class="text-3xl font-black italic">RUAN <span class="text-cyan-500">ASSISTEC</span></h1>
            </div>
            
            <div class="space-y-4">
                <button onclick="switchView('cliente')" id="btn-client" class="w-full bg-slate-100 text-slate-800 font-black py-5 rounded-2xl hover:bg-slate-200 transition uppercase text-xs tracking-widest">Acesso Cliente</button>
                <div class="relative py-4"><hr><span id="txt-admin-label" class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white px-4 text-[9px] font-black text-slate-300 uppercase italic">Administração</span></div>
                <input type="password" id="login-pass" class="w-full p-5 bg-slate-50 border border-slate-100 rounded-2xl focus:ring-2 focus:ring-cyan-500 outline-none text-center font-bold" placeholder="••••••">
                <button onclick="checkLogin()" id="btn-auth" class="w-full bg-slate-900 text-white font-black py-5 rounded-2xl hover:bg-cyan-600 transition uppercase text-xs">Autenticar</button>
                <p id="login-error" class="text-red-500 text-[10px] hidden font-black uppercase mt-2">Invalid Password</p>
            </div>
        </div>
    </div>

    <div id="admin-view" class="view flex-col md:flex-row">
        <aside class="w-full md:w-72 bg-slate-900 text-white flex flex-col p-6 shadow-2xl">
            <h2 class="text-2xl font-black italic mb-10 text-cyan-400">RUAN ADM</h2>
            <nav class="flex-1 space-y-2">
                <button onclick="switchTab('dashboard')" id="nav-dashboard" class="sidebar-link w-full text-left p-4 rounded-xl flex items-center gap-4 font-bold active">
                    <i class="fas fa-chart-pie"></i> <span id="txt-nav-dash">Dashboard</span>
                </button>
                <button onclick="switchTab('os')" id="nav-os" class="sidebar-link w-full text-left p-4 rounded-xl flex items-center gap-4 font-bold">
                    <i class="fas fa-wrench"></i> <span id="txt-nav-os">Ordens</span>
                </button>
                <button onclick="switchTab('estoque')" id="nav-estoque" class="sidebar-link w-full text-left p-4 rounded-xl flex items-center gap-4 font-bold">
                    <i class="fas fa-store"></i> <span id="txt-nav-stock">Mercado</span>
                </button>
            </nav>
            <button onclick="location.reload()" class="p-4 text-red-400 font-black flex items-center gap-3 mt-auto hover:bg-red-500/10 rounded-xl transition">
                <i class="fas fa-power-off"></i> <span id="txt-logout">SAIR</span>
            </button>
        </aside>

        <main class="flex-1 p-6 md:p-12 overflow-y-auto bg-slate-100">
            <div id="tab-dashboard" class="tab-content active space-y-6">
                <h2 id="txt-dash-title" class="text-2xl font-black text-slate-800 uppercase tracking-tighter">Visão Geral</h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div class="bg-white p-8 rounded-3xl shadow-sm border-l-8 border-cyan-500">
                        <p id="txt-card-os" class="text-slate-400 text-xs font-black uppercase">O.S Ativas</p>
                        <h3 id="dash-os" class="text-4xl font-black">0</h3>
                    </div>
                    <div class="bg-white p-8 rounded-3xl shadow-sm border-l-8 border-slate-800">
                        <p id="txt-card-stock" class="text-slate-400 text-xs font-black uppercase">Itens</p>
                        <h3 id="dash-stock" class="text-4xl font-black">0</h3>
                    </div>
                </div>
            </div>

            <div id="tab-os" class="tab-content space-y-6">
                <div class="bg-white p-6 rounded-3xl shadow-sm grid grid-cols-1 md:grid-cols-4 gap-4">
                    <input type="text" id="os-cliente" placeholder="Cliente" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                    <input type="text" id="os-aparelho" placeholder="Aparelho" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                    <button onclick="addOS()" id="btn-add-os" class="bg-cyan-600 text-white font-black rounded-xl uppercase text-xs">Criar O.S</button>
                </div>
                <div class="bg-white rounded-3xl shadow-sm overflow-hidden">
                    <table class="w-full text-left">
                        <tbody id="osTableBody" class="divide-y"></tbody>
                    </table>
                </div>
            </div>

            <div id="tab-estoque" class="tab-content space-y-6">
                <div class="bg-white p-6 rounded-3xl shadow-sm grid grid-cols-1 md:grid-cols-4 gap-4">
                    <input type="text" id="p-nome" placeholder="Produto" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                    <input type="number" id="p-qtd" placeholder="Qtd" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                    <input type="number" id="p-preco" placeholder="Preço" class="p-4 bg-slate-50 rounded-xl outline-none ring-1 ring-slate-200">
                    <button onclick="addProduto()" id="btn-add-prod" class="bg-slate-900 text-white font-black rounded-xl uppercase text-xs">Salvar</button>
                </div>
                <div class="bg-white rounded-3xl shadow-sm overflow-hidden">
                    <table class="w-full text-left">
                        <tbody id="prodTableBody" class="divide-y"></tbody>
                    </table>
                </div>
            </div>
        </main>
    </div>

    <div id="cliente-view" class="view flex-col overflow-y-auto">
        <header class="bg-white p-6 shadow-md flex justify-between items-center sticky top-0 z-50">
            <h1 class="text-2xl font-black italic">RUAN <span class="text-cyan-500">STORE</span></h1>
            <input type="text" id="searchBar" onkeyup="renderAll()" class="flex-1 max-w-xl mx-4 p-4 bg-slate-50 rounded-2xl outline-none ring-1 ring-slate-100" placeholder="...">
            <button onclick="location.reload()" class="p-4 bg-slate-100 rounded-2xl text-slate-500"><i class="fas fa-home"></i></button>
        </header>
        <main class="p-8 container mx-auto">
            <div id="loja-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6"></div>
        </main>
    </div>

    <script>
        const SENHA_ADM = "admin123";
        let tentativas = 0;
        let bloqueado = false;
        let currentLang = localStorage.getItem('ruan_lang') || 'pt';

        // DATABASE DE IDIOMAS
        const languages = {
            pt: { flag: '🇧🇷', client: "Acesso Cliente", admin: "Administração", auth: "Autenticar", dash: "Dashboard", os: "Ordens", stock: "Estoque", logout: "SAIR", dashTitle: "Visão Geral", cardOS: "O.S Ativas", cardStock: "Itens", addOS: "Criar O.S", addProd: "Salvar", buy: "Comprar", error: "Senha Incorreta", lock: "BLOQUEIO DE SEGURANÇA" },
            en: { flag: '🇺🇸', client: "Customer Access", admin: "Management", auth: "Authenticate", dash: "Dashboard", os: "Orders", stock: "Market", logout: "LOGOUT", dashTitle: "Overview", cardOS: "Active O.S", cardStock: "Items", addOS: "Create O.S", addProd: "Save", buy: "Buy Now", error: "Wrong Password", lock: "SECURITY LOCK" },
            es: { flag: '🇪🇸', client: "Acceso Cliente", admin: "Administración", auth: "Autenticar", dash: "Panel", os: "Órdenes", stock: "Mercado", logout: "SALIR", dashTitle: "Resumen", cardOS: "O.S Activas", cardStock: "Items", addOS: "Crear O.S", addProd: "Guardar", buy: "Comprar", error: "Contraseña Incorrecta", lock: "BLOQUEO DE SEGURIDAD" },
            fr: { flag: '🇫🇷', client: "Accès Client", admin: "Gestion", auth: "Authentifier", dash: "Tableau", os: "Commandes", stock: "Stock", logout: "SORTIR", dashTitle: "Aperçu", cardOS: "O.S Actifs", cardStock: "Articles", addOS: "Créer O.S", addProd: "Sauver", buy: "Acheter", error: "Mot de passe incorrect", lock: "VERROUILLAGE SÉCURITÉ" },
            de: { flag: '🇩🇪', client: "Kunden-Zugang", admin: "Verwaltung", auth: "Anmelden", dash: "Dashboard", os: "Aufträge", stock: "Markt", logout: "LOGOUT", dashTitle: "Übersicht", cardOS: "Aktive O.S", cardStock: "Artikel", addOS: "O.S Erstellen", addProd: "Speichern", buy: "Kaufen", error: "Falsches Passwort", lock: "SICHERHEITSSPERRE" },
            it: { flag: '🇮🇹', client: "Accesso Cliente", admin: "Gestione", auth: "Autenticare", dash: "Dashboard", os: "Ordini", stock: "Mercato", logout: "ESCI", dashTitle: "Panoramica", cardOS: "O.S Attivi", cardStock: "Articoli", addOS: "Crea O.S", addProd: "Salva", buy: "Acquista", error: "Password Errata", lock: "BLOCCO SICUREZZA" }
        };

        const countries = [
            { code: 'pt', name: 'Brasil / Portugal', flag: '🇧🇷' },
            { code: 'en', name: 'United States / UK', flag: '🇺🇸' },
            { code: 'es', name: 'España / México', flag: '🇪🇸' },
            { code: 'fr', name: 'France', flag: '🇫🇷' },
            { code: 'de', name: 'Deutschland', flag: '🇩🇪' },
            { code: 'it', name: 'Italia', flag: '🇮🇹' }
        ];

        // DADOS
        let ordens = JSON.parse(localStorage.getItem('db_os_global')) || [];
        let produtos = JSON.parse(localStorage.getItem('db_pr_global')) || [];

        // FUNÇÕES DE TRADUÇÃO
        function toggleLangMenu() { document.getElementById('lang-menu').classList.toggle('hidden'); }

        function setLanguage(langCode) {
            currentLang = langCode;
            localStorage.setItem('ruan_lang', langCode);
            const l = languages[langCode];
            
            // Aplicar textos
            document.getElementById('current-flag').innerText = l.flag;
            document.getElementById('btn-client').innerText = l.client;
            document.getElementById('txt-admin-label').innerText = l.admin;
            document.getElementById('btn-auth').innerText = l.auth;
            document.getElementById('txt-nav-dash').innerText = l.dash;
            document.getElementById('txt-nav-os').innerText = l.os;
            document.getElementById('txt-nav-stock').innerText = l.stock;
            document.getElementById('txt-logout').innerText = l.logout;
            document.getElementById('txt-dash-title').innerText = l.dashTitle;
            document.getElementById('txt-card-os').innerText = l.cardOS;
            document.getElementById('txt-card-stock').innerText = l.cardStock;
            document.getElementById('btn-add-os').innerText = l.addOS;
            document.getElementById('btn-add-prod').innerText = l.addProd;
            document.getElementById('login-error').innerText = l.error;
            document.getElementById('txt-lock-title').innerText = l.lock;

            document.getElementById('lang-menu').classList.add('hidden');
            renderAll();
        }

        function populateCountries() {
            const list = document.getElementById('country-list');
            list.innerHTML = countries.map(c => `
                <button onclick="setLanguage('${c.code}')" class="w-full text-left p-3 hover:bg-slate-50 rounded-xl flex items-center gap-3 transition">
                    <span class="text-xl">${c.flag}</span>
                    <span class="text-xs font-bold text-slate-700">${c.name}</span>
                </button>
            `).join('');
        }

        function filterCountries() {
            const q = document.getElementById('searchCountry').value.toLowerCase();
            const btns = document.getElementById('country-list').querySelectorAll('button');
            btns.forEach(b => {
                const text = b.innerText.toLowerCase();
                b.style.display = text.includes(q) ? 'flex' : 'none';
            });
        }

        // LÓGICA DE SISTEMA
        function switchView(viewId) {
            document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
            document.getElementById(viewId + '-view').classList.add('active');
            renderAll();
        }

        function checkLogin() {
            if(bloqueado) return;
            const pass = document.getElementById('login-pass').value;
            if(pass === SENHA_ADM) {
                switchView('admin');
                tentativas = 0;
            } else {
                tentativas++;
                document.getElementById('login-error').classList.remove('hidden');
                if(tentativas >= 3) {
                    bloqueado = true;
                    document.getElementById('lockdown-overlay').classList.remove('hidden');
                    let t = 30;
                    const timer = setInterval(() => {
                        t--;
                        document.getElementById('timer').innerText = t + "s";
                        if(t <= 0) { clearInterval(timer); bloqueado = false; tentativas = 0; document.getElementById('lockdown-overlay').classList.add('hidden'); }
                    }, 1000);
                }
            }
        }

        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
            document.querySelectorAll('.sidebar-link').forEach(l => l.classList.remove('active'));
            document.getElementById('tab-' + tabId).classList.add('active');
            document.getElementById('nav-' + tabId).classList.add('active');
        }

        function save() {
            localStorage.setItem('db_os_global', JSON.stringify(ordens));
            localStorage.setItem('db_pr_global', JSON.stringify(produtos));
            renderAll();
        }

        function addOS() {
            const c = document.getElementById('os-cliente').value;
            if(!c) return;
            ordens.unshift({ id: Date.now(), cliente: c, aparelho: document.getElementById('os-aparelho').value, status: 'Pendente' });
            document.getElementById('os-cliente').value = ''; document.getElementById('os-aparelho').value = '';
            save();
        }

        function addProduto() {
            const n = document.getElementById('p-nome').value;
            const q = document.getElementById('p-qtd').value;
            if(!n || !q) return;
            produtos.push({ id: Date.now(), nome: n, qtd: q, preco: parseFloat(document.getElementById('p-preco').value || 0) });
            document.getElementById('p-nome').value = ''; document.getElementById('p-qtd').value = ''; document.getElementById('p-preco').value = '';
            save();
        }

        function renderAll() {
            if(document.getElementById('dash-os')) {
                document.getElementById('dash-os').innerText = ordens.length;
                document.getElementById('dash-stock').innerText = produtos.length;
            }

            const osBody = document.getElementById('osTableBody');
            if(osBody) osBody.innerHTML = ordens.map((o, i) => `
                <tr>
                    <td class="p-6 font-bold text-slate-800">${o.cliente}</td>
                    <td class="p-6 font-medium text-slate-500">${o.aparelho}</td>
                    <td class="p-6 text-right"><button onclick="ordens.splice(${i},1);save()" class="text-red-300"><i class="fas fa-trash"></i></button></td>
                </tr>
            `).join('');

            const prBody = document.getElementById('prodTableBody');
            if(prBody) prBody.innerHTML = produtos.map((p, i) => `
                <tr>
                    <td class="p-6 font-bold">${p.nome}</td>
                    <td class="p-6 font-black text-cyan-600">${p.qtd}</td>
                    <td class="p-6 text-right font-bold text-slate-400">R$ ${p.preco.toFixed(2)}</td>
                </tr>
            `).join('');

            const grid = document.getElementById('loja-grid');
            if(grid) grid.innerHTML = produtos.map(p => `
                <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-100 flex flex-col justify-between">
                    <h3 class="font-black text-slate-800 uppercase text-sm">${p.nome}</h3>
                    <p class="text-2xl font-black text-cyan-600 mt-4">R$ ${p.preco.toFixed(2)}</p>
                    <button class="w-full mt-4 bg-slate-900 text-white py-3 rounded-xl font-black text-[10px] uppercase tracking-widest">${languages[currentLang].buy}</button>
                </div>
            `).join('');
        }

        // INIT
        populateCountries();
        setLanguage(currentLang);
    </script>
</body>
</html>
