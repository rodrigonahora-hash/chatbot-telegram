# Chatbot Telegram com OpenWeather + IA Generativa

Chatbot desenvolvido no N8N para consulta climática em cidades brasileiras utilizando:

- Telegram Bot API
- OpenWeather API
- Google Gemini (IA Generativa)
- Fluxos de validação e fallback no N8N

O chatbot recebe uma cidade enviada via Telegram, consulta os dados climáticos na OpenWeather e retorna uma mensagem amigável ao usuário utilizando IA Generativa.

---

## Docker Compose (Opcional)

Caso utilize Docker para executar o n8n localmente, configure as seguintes variáveis de ambiente:

```env
NGROK_AUTHTOKEN=seu_token_ngrok
NGROK_DOMAIN=sua-url.ngrok-free.app
```

# Tecnologias utilizadas

- N8N
- Telegram Bot API
- OpenWeather API
- Google Gemini
- HTTP Request
- JSON Parsing
- Fluxos condicionais (IF)
- Manipulação de dados com Code Nodes

---

# Estrutura do workflow

O workflow possui as seguintes etapas:

1. Recebimento da mensagem via Telegram
2. Preparação e normalização da entrada
3. Consulta climática na OpenWeather
4. Extração e validação dos dados retornados
5. Geração de mensagem fallback
6. Refinamento da resposta com IA Generativa
7. Sanitização da resposta final
8. Envio da resposta ao usuário
9. Tratamento de erros para cidades inválidas

---

# Exemplo de uso

## Entrada do usuário

```text
São Paulo,SP,BR
```

## Resposta esperada

```text
🌤️ São Paulo: 24°C e céu parcialmente nublado.
```

---

# Exemplo de erro

## Entrada inválida

```text
Grifnória
```

## Resposta esperada

```text
❌ Cidade não encontrada. Use o formato Cidade,UF,BR (ex.: São Paulo,SP,BR).
```

---

# Como importar o workflow no N8N

1. Abra o N8N
2. Clique em "Import from File"
3. Selecione o arquivo:

```text
workflow-chatbot-telegram.json
```

4. Após importar, configure as credenciais necessárias

---

# Configuração das credenciais

## 1. Telegram Bot

Crie um bot no Telegram utilizando o BotFather:

https://t.me/BotFather

Após gerar o token do bot, configure uma credencial do tipo Telegram API dentro do N8N e associe aos nodes:

- Telegram Trigger
- Enviar previsão ao usuário
- Enviar erro ao usuário

O workflow utiliza a credencial:

```text
Token Telegram Chatbot Clima
```

Observação:
O token do Telegram não está embutido no workflow exportado, conforme os requisitos de segurança do desafio.

## 2. OpenWeather API

Crie uma conta em:

```text
https://openweathermap.org/api
```

Gere sua API Key e configure no nó HTTP Request.

Variável esperada:

```text
OPENWEATHER_API_KEY
```

Endpoint utilizado:

```text
https://api.openweathermap.org/data/2.5/weather
```

Parâmetros utilizados:

| Parâmetro | Valor |
|---|---|
| q | cidade enviada pelo usuário |
| units | metric |
| lang | pt_br |
| appid | OPENWEATHER_API_KEY |

---

## 3. Google Gemini

Crie uma API Key em:

```text
https://aistudio.google.com/
```

No N8N:

- Configure uma credencial Google Gemini (PaLM)

Modelo utilizado:

```text
gemini-2.5-flash
```

---

# Estrutura das mensagens

O workflow utiliza um sistema híbrido:

## Mensagem fallback

Gerada diretamente via JavaScript no N8N.

Exemplo:

```text
🌥️ A temperatura em Brasília é de 19°C, nublado.
```

## Refinamento com IA

A IA Generativa transforma a mensagem em uma resposta mais natural e amigável.

Exemplo:

```text
🌥️ Em Brasília, tá 19°C e nublado.
```

---

# Tratamento de erros

O workflow possui validações para:

- cidades inexistentes
- retorno inválido da API
- campos obrigatórios ausentes
- mensagens vazias da IA

Em caso de erro:

```text
❌ Cidade não encontrada. Use o formato Cidade,UF,BR (ex.: São Paulo,SP,BR).
```

---

# Segurança

O workflow exportado NÃO contém:

- tokens do Telegram
- API Keys da OpenWeather
- credenciais do Gemini
- segredos embutidos

Todas as credenciais devem ser configuradas manualmente no ambiente local do N8N.

---

# Fluxo principal

```text
Telegram Trigger
    ↓
Preparar entrada
    ↓
Consultar clima OpenWeather
    ↓
Extrair dados climáticos
    ↓
Validar resposta da API
    ↓
Gerar mensagem fallback
    ↓
Refinar com IA
    ↓
Sanitizar resposta final
    ↓
Enviar previsão ao usuário
```

---

# Evidências de execução

O repositório também contém imagens de evidência demonstrando:

- execução do workflow no N8N
- testes de sucesso com múltiplas cidades
- tratamento de erros para cidades inválidas
- funcionamento do bot no Telegram

Arquivos disponíveis na pasta:

```text
/evidencias

---

# Autor

Rodrigo Moura Araújo
