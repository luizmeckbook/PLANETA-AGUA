<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Medela Supermercado - Oficial</title>
    <style>
        :root { --primary: #e53935; --admin-bg: #2c3e50; --bg: #f4f4f4; --whatsapp: #25d366; }
        * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { margin: 0; background: var(--bg); color: #333; }
        
        /* Sistema de Telas */
        .tela { display: none; min-height: 100vh; padding-bottom: 50px; }
        .ativa { display: block !important; }
        
        .container { max-width: 500px; margin: auto; padding: 15px; }
        .card { background: white; padding: 20px; border-radius: 15px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); margin-bottom: 15px; }
        
        /* Inputs e Botões */
        input, select, textarea { width: 100%; padding: 14px; margin: 10px 0; border: 1px solid #ccc; border-radius: 10px; font-size: 16px; }
        button { width: 100%; padding: 14px; border: none; border-radius: 10px; font-weight: bold; cursor: pointer; font-size: 16px; transition: 0.3s; margin-top: 5px; }
        button:active { transform: scale(0.97); }
        
        .btn-main { background: var(--primary); color: white; }
        .btn-sec { background: var(--admin-bg); color: white; }
        .btn-whats { background: var(--whatsapp); color: white; display: flex; align-items: center; justify-content: center; gap: 10px; }
        
        /* Header */
        .header { background: var(--primary); color: white; padding: 15px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        
        /* Grid de Produtos */
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 10px; }
        .prod-card { background: white; padding: 15px; border-radius: 12px; text-align: center; border: 1px solid #eee; }
        .prod-preco { color: #27ae60; font-weight: bold; font-size: 18px; display: block; margin: 5px 0; }
        
        /* Carrinho Flutuante */
        .cart-float { position: fixed; bottom: 20px; left: 20px; width: 60px; height: 60px; background: #000; color: white; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 24px; cursor: pointer; box-shadow: 0 4px 15px rgba(0,0,0,0.4); z-index: 1000; }
        .badge { position: absolute; top: 0; right: 0; background: var(--primary); width: 22px; height: 22px; border-radius: 50%; font-size: 12px; display: flex; align-items: center; justify-content: center; }

        /* Estilo do Chat */
        .chat-box { height: 350px; overflow-y: auto; background: #efe7dd; padding: 15px; border-radius: 10px; display: flex; flex-direction: column; gap: 10px; border: 1px solid #ddd; }
        .msg { padding: 10px; border-radius: 10px; max-width: 85%; font-size: 14px; }
        .sent { align-self: flex-end; background: #dcf8c6; }
        .received { align-self: flex-start; background: white; }
    </style>
</head>
<body>

<div id="tela_login" class="tela ativa">
    <div class="container" style="padding-top: 50px; text-align: center;">
        <h1 id="txt_loja_nome">Medela Supermercado</h1>
        <div class="card">
            <input type="text" id="login_cpf" placeholder="CPF ou 'admin'">
            <input type="password" id="login_senha" placeholder="Senha">
            <button class="btn-main" onclick="funcaoLogin()">ENTRAR</button>
            <button class="btn-whats" onclick="abrirSuporte()">💬 SUPORTE CHAT</button>
            <p onclick="irPara('tela_cadastro')" style="margin-top:20px; color:var(--primary); cursor:pointer">Criar nova conta</p>
        </div>
    </div>
</div>

<div id="tela_cadastro" class="tela">
    <div class="container">
        <h2>Criar Conta</h2>
        <div class="card">
            <input id="cad_nome" placeholder="Nome Completo">
            <input id="cad_cpf" placeholder="CPF">
            <input id="cad_senha" type="password" placeholder="Senha">
            <button class="btn-main" onclick="funcaoCadastro()">CADASTRAR</button>
            <button onclick="irPara('tela_login')">VOLTAR</button>
        </div>
    </div>
</div>

<div id="tela_home" class="tela">
    <div class="header">
        <span id="txt_boas_vindas">Olá!</span>
        <button onclick="logout()" style="width:auto; margin:0; background:rgba(0,0,0,0.2); padding:8px 15px">Sair</button>
    </div>
    <div class="container">
        <button class="btn-sec" onclick="irPara('tela_rastreio')">📦 MEUS PEDIDOS / RASTREIO</button>
        <div class="grid" id="lista_produtos"></div>
    </div>
    <div class="cart-float" onclick="irPara('tela_carrinho')">
        🛒 <span class="badge" id="cart_count">0</span>
    </div>
    <button onclick="abrirSuporte()" style="position:fixed; bottom:20px; right:20px; width:60px; height:60px; border-radius:50%; background:var(--whatsapp); border:none; color:white; font-size:25px; box-shadow: 0 4px 10px rgba(0,0,0,0.3)">💬</button>
</div>

<div id="tela_carrinho" class="tela">
    <div class="header">
        <button onclick="irPara('tela_home')" style="width:auto; background:none; font-size:20px">←</button>
        <span>Meu Carrinho</span>
        <div style="width:40px"></div>
    </div>
    <div class="container">
        <div id="itens_no_carrinho"></div>
        <div class="card" id="checkout_box" style="display:none">
            <select id="sel_entrega" onchange="atualizarTotal()">
                <option value="retirada">Retirar na Loja (Grátis)</option>
                <option value="entrega">Entrega em Casa (+ R$ 7,00)</option>
            </select>
            <textarea id="endereco_entrega" placeholder="Endereço Completo" style="display:none"></textarea>
            <select id="sel_pgto">
                <option value="">Forma de Pagamento</option>
                <option value="Pix">Pix</option>
                <option value="Cartão">Cartão</option>
                <option value="Dinheiro">Dinheiro</option>
            </select>
            <h3 id="txt_total" style="text-align:right">Total: R$ 0,00</h3>
            <button class="btn-whats" onclick="gerarPedido()">🚀 FINALIZAR PEDIDO</button>
        </div>
    </div>
</div>

<div id="tela_rastreio" class="tela">
    <div class="header">
        <button onclick="irPara('tela_home')" style="width:auto; background:none; font-size:20px">←</button>
        <span>Meus Pedidos</span>
        <div style="width:40px"></div>
    </div>
    <div class="container" id="lista_meus_pedidos"></div>
</div>

<div id="tela_admin" class="tela">
    <div class="header" style="background:var(--admin-bg)">
        <span>PAINEL GESTOR</span>
        <button onclick="logout()" style="width:auto; margin:0; background:rgba(255,255,255,0.1)">Sair</button>
    </div>
    <div class="container">
        <div class="card" style="background:var(--whatsapp); color:white">
            <small>Faturamento Total</small>
            <h2 id="adm_faturamento">R$ 0,00</h2>
        </div>
        
        <div class="card">
            <h3>Novo Produto</h3>
            <input id="adm_p_nome" placeholder="Nome do Produto">
            <input id="adm_p_preco" type="number" placeholder="Preço de Venda">
            <button class="btn-main" onclick="admAddProduto()">ADICIONAR</button>
        </div>

        <div class="card">
            <h3>Configurações</h3>
            <input id="adm_cfg_nome" placeholder="Nome da Loja">
            <input id="adm_cfg_whats" placeholder="WhatsApp (DDI+DDD+NUM)">
            <button class="btn-sec" onclick="admSalvarConfig()">SALVAR CONFIG</button>
        </div>

        <div class="card">
            <h3>Pedidos Recebidos</h3>
            <div id="adm_lista_pedidos"></div>
        </div>
    </div>
</div>

<div id="tela_chat" class="tela">
    <div class="header" style="background:#075e54">
        <button onclick="irPara('tela_home')" style="width:auto; background:none; font-size:20px">←</button>
        <span>Suporte ao Cliente</span>
        <div style="width:40px"></div>
    </div>
    <div class="container">
        <div class="chat-box" id="chat_mensagens"></div>
        <div style="display:flex; gap:5px; margin-top:10px">
            <input id="chat_input" placeholder="Digite sua dúvida...">
            <button onclick="chatEnviar()" style="width:60px; margin:0" class="btn-whats">➤</button>
        </div>
    </div>
</div>

<script>
// --- BANCO DE DADOS ---
let db = {
    users: JSON.parse(localStorage.getItem("db_users")) || {},
    prods: JSON.parse(localStorage.getItem("db_prods")) || [],
    pedidos: JSON.parse(localStorage.getItem("db_pedidos")) || [],
    config: JSON.parse(localStorage.getItem("db_config")) || { nome: "Medela Supermercado", whats: "", cor: "#e53935" },
    chat: JSON.parse(localStorage.getItem("db_chat")) || {}
};

function salvar() {
    localStorage.setItem("db_users", JSON.stringify(db.users));
    localStorage.setItem("db_prods", JSON.stringify(db.prods));
    localStorage.setItem("db_pedidos", JSON.stringify(db.pedidos));
    localStorage.setItem("db_config", JSON.stringify(db.config));
    localStorage.setItem("db_chat", JSON.stringify(db.chat));
}

let sessao = "";
let carrinho = [];

// --- NAVEGAÇÃO ---
function irPara(telaId) {
    document.querySelectorAll('.tela').forEach(t => t.classList.remove('ativa'));
    document.getElementById(telaId).classList.add('ativa');
    
    if(telaId === 'tela_home') rendHome();
    if(telaId === 'tela_carrinho') rendCarrinho();
    if(telaId === 'tela_rastreio') rendRastreio();
    if(telaId === 'tela_admin') rendAdmin();
}

// --- LOGICA DE ACESSO ---
function funcaoLogin() {
    const c = document.getElementById("login_cpf").value.trim();
    const s = document.getElementById("login_senha").value.trim();

    if(c === 'admin' && s === 'admin123') {
        sessao = "admin";
        irPara('tela_admin');
        return;
    }

    if(db.users[c] && db.users[c].senha === s) {
        sessao = c;
        irPara('tela_home');
    } else {
        alert("Acesso Negado! Verifique os dados.");
    }
}

function funcaoCadastro() {
    const n = document.getElementById("cad_nome").value;
    const c = document.getElementById("cad_cpf").value;
    const s = document.getElementById("cad_senha").value;
    if(!n || !c || !s) return alert("Preencha tudo!");
    db.users[c] = { nome: n, senha: s };
    salvar();
    alert("Conta criada!");
    irPara('tela_login');
}

function logout() {
    sessao = "";
    irPara('tela_login');
}

// --- LOJA ---
function rendHome() {
    document.getElementById("txt_loja_nome").innerText = db.config.nome;
    document.getElementById("txt_boas_vindas").innerText = "Olá, " + (db.users[sessao]?.nome || "Cliente");
    
    const lista = document.getElementById("lista_produtos");
    lista.innerHTML = db.prods.map((p, index) => `
        <div class="prod-card">
            <strong>${p.nome}</strong>
            <span class="prod-preco">R$ ${p.preco.toFixed(2)}</span>
            <button class="btn-main" onclick="addCarrinho(${index})">ADICIONAR</button>
        </div>
    `).join('') || "Nenhum produto cadastrado.";
}

function addCarrinho(index) {
    carrinho.push(db.prods[index]);
    document.getElementById("cart_count").innerText = carrinho.length;
}

function rendCarrinho() {
    const box = document.getElementById("itens_no_carrinho");
    if(carrinho.length === 0) {
        box.innerHTML = "<p>Carrinho vazio.</p>";
        document.getElementById("checkout_box").style.display = "none";
        return;
    }
    document.getElementById("checkout_box").style.display = "block";
    box.innerHTML = carrinho.map((item, i) => `
        <div class="card" style="display:flex; justify-content:space-between">
            <span>${item.nome}</span>
            <b>R$ ${item.preco.toFixed(2)}</b>
        </div>
    `).join('');
    atualizarTotal();
}

function atualizarTotal() {
    let sub = carrinho.reduce((a, b) => a + b.preco, 0);
    const entrega = document.getElementById("sel_entrega").value;
    document.getElementById("endereco_entrega").style.display = entrega === 'entrega' ? 'block' : 'none';
    if(entrega === 'entrega') sub += 7;
    document.getElementById("txt_total").innerText = "Total: R$ " + sub.toFixed(2);
}

function gerarPedido() {
    const entrega = document.getElementById("sel_entrega").value;
    const pgto = document.getElementById("sel_pgto").value;
    if(!pgto) return alert("Escolha o pagamento!");

    let total = carrinho.reduce((a, b) => a + b.preco, 0);
    if(entrega === 'entrega') total += 7;

    const novo = {
        id: "#" + Math.floor(Math.random()*9000),
        user: sessao,
        itens: carrinho.map(i => i.nome).join(", "),
        total: total,
        status: "Pendente"
    };

    db.pedidos.unshift(novo);
    salvar();
    
    const texto = `Olá! Novo Pedido ${novo.id}\nItens: ${novo.itens}\nTotal: R$ ${total.toFixed(2)}`;
    window.open(`https://api.whatsapp.com/send?phone=${db.config.whats}&text=${encodeURIComponent(texto)}`);
    
    carrinho = [];
    document.getElementById("cart_count").innerText = "0";
    irPara('tela_rastreio');
}

function rendRastreio() {
    const meus = db.pedidos.filter(p => p.user === sessao);
    document.getElementById("lista_meus_pedidos").innerHTML = meus.map(p => `
        <div class="card">
            <b>Pedido ${p.id}</b><br>
            <small>${p.itens}</small><br>
            <div style="display:flex; justify-content:space-between; margin-top:10px">
                <span>Total: R$ ${p.total.toFixed(2)}</span>
                <span style="color:var(--primary)">${p.status}</span>
            </div>
        </div>
    `).join('') || "Você ainda não fez pedidos.";
}

// --- ADMIN ---
function rendAdmin() {
    let total = db.pedidos.reduce((a, b) => a + b.total, 0);
    document.getElementById("adm_faturamento").innerText = "R$ " + total.toFixed(2);
    document.getElementById("adm_cfg_nome").value = db.config.nome;
    document.getElementById("adm_cfg_whats").value = db.config.whats;

    document.getElementById("adm_lista_pedidos").innerHTML = db.pedidos.map(p => `
        <div style="border-bottom:1px solid #eee; padding:10px 0">
            <b>${p.id} - R$ ${p.total.toFixed(2)}</b><br>
            <small>${p.itens}</small>
        </div>
    `).join('');
}

function admAddProduto() {
    const n = document.getElementById("adm_p_nome").value;
    const p = parseFloat(document.getElementById("adm_p_preco").value);
    if(!n || !p) return;
    db.prods.push({ nome: n, preco: p });
    salvar();
    alert("Produto salvo!");
    rendAdmin();
}

function admSalvarConfig() {
    db.config.nome = document.getElementById("adm_cfg_nome").value;
    db.config.whats = document.getElementById("adm_cfg_whats").value;
    salvar();
    alert("Configurações atualizadas!");
}

// --- CHAT ---
function abrirSuporte() {
    irPara('tela_chat');
}

function chatEnviar() {
    const input = document.getElementById("chat_input");
    if(!input.value) return;
    if(!db.chat[sessao]) db.chat[sessao] = [];
    db.chat[sessao].push({ texto: input.value, autor: 'cliente' });
    salvar();
    input.value = "";
    rendChat();
}

function rendChat() {
    const msgs = db.chat[sessao] || [];
    document.getElementById("chat_mensagens").innerHTML = msgs.map(m => `
        <div class="msg ${m.autor === 'cliente' ? 'sent' : 'received'}">${m.texto}</div>
    `).join('');
}

window.onload = () => {
    document.getElementById("txt_loja_nome").innerText = db.config.nome;
};
</script>
</body>
</html>
