import com.sun.net.httpserver.HttpServer;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpExchange;
import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;

public class App {
    public static void main(String[] args) throws IOException {
        // Cria o servidor na porta 8080
        HttpServer server = HttpServer.create(new InetSocketAddress(8080), 0);
        
        // Define a rota principal "/"
        server.createContext("/", new HomeHandler());
        
        server.setExecutor(null); 
        System.out.println("Servidor orgânico rodando em http://localhost:8080");
        server.start();
    }

    static class HomeHandler implements HttpHandler {
        @Override
        public void handle(HttpExchange exchange) throws IOException {
            // O conteúdo HTML gerado manualmente (Orgânico)
            String response = """
                <html>
                <head>
                    <meta charset="UTF-8">
                    <style>
                        body { font-family: 'Segoe UI', sans-serif; line-height: 1.6; max-width: 600px; margin: 40px auto; padding: 20px; background: #fafafa; }
                        h1 { color: #2e7d32; border-bottom: 2px solid #2e7d32; }
                        .principio { background: white; padding: 15px; margin-bottom: 10px; border-left: 5px solid #2e7d32; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
                    </style>
                </head>
                <body>
                    <h1>Nossos Princípios Orgânicos</h1>
                    <div class="principio"><strong>Integridade:</strong> Código limpo e transparente.</div>
                    <div class="principio"><strong>Leveza:</strong> Sem frameworks desnecessários.</div>
                    <div class="principio"><strong>Crescimento:</strong> Evolução constante do conhecimento.</div>
                </body>
                </html>
                """;

            exchange.getResponseHeaders().set("Content-Type", "text/html; charset=UTF-8");
            exchange.sendResponseHeaders(200, response.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}
