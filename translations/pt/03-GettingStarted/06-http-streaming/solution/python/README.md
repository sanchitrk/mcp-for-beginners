<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "4c4da5949611d91b06d8a5d450aae8d6",
  "translation_date": "2025-07-13T21:19:09+00:00",
  "source_file": "03-GettingStarted/06-http-streaming/solution/python/README.md",
  "language_code": "pt"
}
-->
# Executar este exemplo

Aqui está como executar o servidor e cliente de streaming HTTP clássico, bem como o servidor e cliente de streaming MCP usando Python.

### Visão geral

- Vai configurar um servidor MCP que transmite notificações de progresso para o cliente enquanto processa itens.
- O cliente irá mostrar cada notificação em tempo real.
- Este guia cobre pré-requisitos, configuração, execução e resolução de problemas.

### Pré-requisitos

- Python 3.9 ou superior
- O pacote Python `mcp` (instale com `pip install mcp`)

### Instalação e Configuração

1. Clone o repositório ou descarregue os ficheiros da solução.

   ```pwsh
   git clone https://github.com/microsoft/mcp-for-beginners
   ```

1. **Crie e ative um ambiente virtual (recomendado):**

   ```pwsh
   python -m venv venv
   .\venv\Scripts\Activate.ps1  # On Windows
   # or
   source venv/bin/activate      # On Linux/macOS
   ```

1. **Instale as dependências necessárias:**

   ```pwsh
   pip install "mcp[cli]"
   ```

### Ficheiros

- **Servidor:** [server.py](../../../../../../03-GettingStarted/06-http-streaming/solution/python/server.py)
- **Cliente:** [client.py](../../../../../../03-GettingStarted/06-http-streaming/solution/python/client.py)

### Executar o Servidor de Streaming HTTP Clássico

1. Navegue até ao diretório da solução:

   ```pwsh
   cd 03-GettingStarted/06-http-streaming/solution
   ```

2. Inicie o servidor de streaming HTTP clássico:

   ```pwsh
   python server.py
   ```

3. O servidor irá arrancar e mostrar:

   ```
   Starting FastAPI server for classic HTTP streaming...
   INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
   ```

### Executar o Cliente de Streaming HTTP Clássico

1. Abra um novo terminal (ative o mesmo ambiente virtual e diretório):

   ```pwsh
   cd 03-GettingStarted/06-http-streaming/solution
   python client.py
   ```

2. Deve ver as mensagens transmitidas impressas sequencialmente:

   ```text
   Running classic HTTP streaming client...
   Connecting to http://localhost:8000/stream with message: hello
   --- Streaming Progress ---
   Processing file 1/3...
   Processing file 2/3...
   Processing file 3/3...
   Here's the file content: hello
   --- Stream Ended ---
   ```

### Executar o Servidor de Streaming MCP

1. Navegue até ao diretório da solução:
   ```pwsh
   cd 03-GettingStarted/06-http-streaming/solution
   ```
2. Inicie o servidor MCP com o transporte streamable-http:
   ```pwsh
   python server.py mcp
   ```
3. O servidor irá arrancar e mostrar:
   ```
   Starting MCP server with streamable-http transport...
   INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
   ```

### Executar o Cliente de Streaming MCP

1. Abra um novo terminal (ative o mesmo ambiente virtual e diretório):
   ```pwsh
   cd 03-GettingStarted/06-http-streaming/solution
   python client.py mcp
   ```
2. Deve ver as notificações impressas em tempo real à medida que o servidor processa cada item:
   ```
   Running MCP client...
   Starting client...
   Session ID before init: None
   Session ID after init: a30ab7fca9c84f5fa8f5c54fe56c9612
   Session initialized, ready to call tools.
   Received message: root=LoggingMessageNotification(...)
   NOTIFICATION: root=LoggingMessageNotification(...)
   ...
   Tool result: meta=None content=[TextContent(type='text', text='Processed files: file_1.txt, file_2.txt, file_3.txt | Message: hello from client')]
   ```

### Passos-chave da Implementação

1. **Crie o servidor MCP usando FastMCP.**
2. **Defina uma ferramenta que processa uma lista e envia notificações usando `ctx.info()` ou `ctx.log()`.**
3. **Execute o servidor com `transport="streamable-http"`.**
4. **Implemente um cliente com um handler de mensagens para mostrar as notificações à medida que chegam.**

### Explicação do Código
- O servidor usa funções async e o contexto MCP para enviar atualizações de progresso.
- O cliente implementa um handler de mensagens async para imprimir notificações e o resultado final.

### Dicas e Resolução de Problemas

- Use `async/await` para operações não bloqueantes.
- Trate sempre exceções tanto no servidor como no cliente para maior robustez.
- Teste com vários clientes para observar atualizações em tempo real.
- Se encontrar erros, verifique a versão do Python e assegure-se de que todas as dependências estão instaladas.

**Aviso Legal**:  
Este documento foi traduzido utilizando o serviço de tradução automática [Co-op Translator](https://github.com/Azure/co-op-translator). Embora nos esforcemos pela precisão, por favor tenha em conta que traduções automáticas podem conter erros ou imprecisões. O documento original na sua língua nativa deve ser considerado a fonte autorizada. Para informações críticas, recomenda-se tradução profissional humana. Não nos responsabilizamos por quaisquer mal-entendidos ou interpretações incorretas decorrentes da utilização desta tradução.