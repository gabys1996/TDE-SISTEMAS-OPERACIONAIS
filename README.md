import java.net.*;
import java.io.*;

public class DateServerClient {
    public static void main(String[] args) {
        if (args.length != 1) {
            System.out.println("Usage: java DateServerClient [server|client]");
            return;
        }

        String mode = args[0];

        if (mode.equals("server")) {
            // Código do servidor
            try {
                ServerSocket sock = new ServerSocket(6013);
                /* agora escuta conexões */
                while (true) {
                    Socket client = sock.accept();
                    PrintWriter pout = new PrintWriter(client.getOutputStream(), true);
                    /* grava a Data no socket */
                    pout.println(new java.util.Date().toString());
                    /* fecha o socket e volta */
                    /* a escutar conexões */
                    client.close();
                }
            } catch (IOException ioe) {
                System.err.println(ioe);
            }
        } else if (mode.equals("client")) {
            // Código do cliente
            try {
                /* estabelece conexão com o socket do servidor */
                Socket sock = new Socket("127.0.0.1", 6013);
                InputStream in = sock.getInputStream();
                BufferedReader bin = new BufferedReader(new InputStreamReader(in));
                /* lê a data no socket */
                String line;
                while ((line = bin.readLine()) != null)
                    System.out.println(line);
                /* fecha a conexão com o socket */
                sock.close();
            } catch (IOException ioe) {
                System.err.println(ioe);
            }
        } else {
            System.out.println("Usage: java DateServerClient [server|client]");
        }
    }
}
