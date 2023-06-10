## Como usar a API ChatGPT no Node.js/JavaScript usando a biblioteca Node.js OpenAI

Da primeira vez que tentei usar a biblioteca Node.js OpenAI, encontrei a documentação carente em termos de fazer um script simples funcionar. Além disso, a documentação mostrou um exemplo com a API de Conclusões que o GPT 3.5 e o GPT 4 usam. Portanto, este é um tutorial rápido.

Certifique-se de ter o [Node.js](https://nodejs.org/) instalado em seu computador.

```javascript
npm i openai@3.2.1 dotenv@16.1.4
```
 O código do arquivo `app.js`.
```javascript
// Importa as classes Configuration e OpenAIApi da biblioteca openai
const { Configuration, OpenAIApi } = require("openai");
require('dotenv').config(); // Carrega as variáveis de ambiente do arquivo .env

// Cria uma instância de Configuration contendo a chave de API da OpenAI
const configuration = new Configuration({
    apiKey: process.env.OPENAI_API_KEY,
});

// Cria uma instância de OpenAIApi com a configuração criada anteriormente
const openai = new OpenAIApi(configuration);

// Define a função assíncrona para enviar a pergunta ao modelo conversacional
async function run() {
    try {
        // Envia uma solicitação para o modelo conversacional usando o método createChatCompletion
        const response = await openai.createChatCompletion({
            model: 'gpt-3.5-turbo', // Define qual modelo conversacional será utilizado
            messages: [
                {
                    role: 'system',
                    content: 'Você é uma assistente de inteligência artificial prestativa.' 
                    // Mensagem inicial que será enviada ao modelo conversacional, definindo seu papel
                },
                {
                    role: 'user',
                    content: 'Quantos estados o México possui?' 
                    // Mensagem do usuário que será enviada ao modelo conversacional
                }
            ]
        });
        // Exibe a resposta gerada pelo modelo conversacional
        console.log(response.data.choices[0].message.content);

    } catch (error) { // Lida com exceções geradas dentro do bloco try
        if (error.response) { // Se há uma resposta de erro definida...
            console.log(error.response.status);
            console.log(error.response.data);
        } else {
            console.log(error.message); // ...exibe as informações do erro obtido
        }
    }
}

// Chama a função assíncrona run() para iniciar a conversa com o modelo conversacional GPT-3 da OpenAI
run(); 
``` 
O seu arquivo `package.json` deve estar assim:
```json
{
  "dependencies": {
    "dotenv": "^16.1.4",
    "openai": "^3.2.1"
}
}
```
No arquivo `.env`, deve conter a sua API Key da OpenAI GPT, ela deve estar no seguinte formato:

```php
# API KEY OPENAI GPT
# Exemplo: OPENAI_API_KEY=sk-ymKpNA87PGQ1TqZdDYGST3Blbk8noGBRQQdaHJbI2QU98Taz
OPENAI_API_KEY=SUA API KEY AQUI
```
Utilize o comando ```node app.js```

## Antes de escrever qualquer código

Se você quiser usar a API OpenAI para trabalhar com a API ChatGPT ou qualquer outro modelo, é necessário fazer o seguinte:

-   ter uma conta na OpenAI (a mesma que você usa para ChatGPT está ok)
-   configurar a cobrança para sua conta na OpenAI (não importa se você tem ChatGPT Plus - a API é um produto separado)
-   gerar uma chave de API


 

## Enviar uma solicitação básica contra a API ChatGPT

Antes de tudo, adicione as dependências   `openai`  e  `dotenv` ao seu projeto.

```javascript 
npm i openai@3.2.1 dotenv@16.1.4
```

Se seu projeto usa módulos, você pode usar a `import` sintaxe:
```javascript
import { Configuration, OpenAIApi } from  "openai";
import  dotenv  from  "dotenv";

dotenv.config();
```
Caso contrário, você pode usar o estilo antigo com `require`:
```javascript
const { Configuration, OpenAIApi } =  require("openai");
require('dotenv').config();
```


O próximo passo é configurar o OpenAI com sua chave. É melhor não incluir a chave secreta em seu código, mas usar variáveis ​​de ambiente:
```javascript
const  configuration  =  new  Configuration({
apiKey:  process.env.OPENAI_API_KEY,
});
const  openai  =  new  OpenAIApi(configuration);
```
Agora você pode fazer a chamada da API  de bate-papo `gpt-3.5-turbo`.
Sem dúvida, algumas solicitações falharão. Portanto, vamos adicionar um pouco de código adicional para garantir que lidamos com os erros adequadamente.
```javascript
async function run() {
  try {
    const response = await openai.createChatCompletion({
      model: 'gpt-3.5-turbo',
      messages: [
        {
          role: 'system',
          content: 'Você é uma assistente de inteligência artificial prestativa.'
        },
        {
          role: 'user',
          content: 'Quantos estados o México possui?'
        }
      ]
    });

    console.log(response.data.choices[0].message.content);
  } catch (error) {
    if (error.response) {
      console.log(error.response.status);
      console.log(error.response.data);
    } else {
      console.log(error.message);
    }
  }
}

run();

```
Este exemplo usa o `gpt-3.5-turbo`modelo, mas se você tiver acesso ao `gpt-4`modelo, sinta-se à vontade para usá-lo. Observe que, mesmo se você for assinante do ChatGPT Plus com acesso GPT 4, isso não significa que você pode usar o modelo GPT 4 por meio da API.


# Solução de problemas

## Error: Additional properties are not allowed (‘some property’ was unexpected) — ‘messages.0’

Você especificou na lista de mensagens um objeto com uma propriedade que OpenAI não aceita. Os únicos nomes de propriedade aceitos para uma mensagem são:

-   `role`(que pode ser sistema, usuário ou assistente)
-   `content`(que é a mensagem que você está enviando)

## Error: That model is currently overloaded with other requests. You can retry your request, or contact us through our help center at help.openai.com if the error persists. (Please include the request ID `<SOME REQUEST ID>` in your message.

Agora você quebrou o OpenAI. Estou brincando. Parece que você está enviando muitas solicitações de uma só vez e sobrecarregou a API. Esse erro geralmente está associado a um código de status HTTP 429 — Too Many Requests. Vá devagar. Se necessário, adicione um intervalo de tempo entre as solicitações.

## Conclusão

Espero que este tutorial tenha ajudado você a começar a enviar solicitações contra a API ChatGPT de seus scripts Node.js. Deixe um comentário na seção abaixo se tiver alguma dúvida. Eu adoraria ouvir de você!
