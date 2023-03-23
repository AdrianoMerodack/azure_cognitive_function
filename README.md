# Azure Cognitive Services Function
1. Abra o VS Code e crie uma nova pasta para o seu projeto.

2. No terminal do VS Code, execute o comando npm init para criar um novo arquivo package.json.

3. Instale o pacote @azure/cognitiveservices-textanalytics executando o comando npm install @azure/cognitiveservices-textanalytics no terminal.

4. Crie um novo arquivo chamado index.js na pasta do seu projeto e adicione o código abaixo:

```
const { TextAnalyticsClient, AzureKeyCredential } = require("@azure/cognitiveservices-textanalytics");

module.exports = async function (context, req) {
    const { text, language } = req.body;

    if (!text || !language) {
        context.res = {
            status: 400,
            body: "Please provide a 'text' and 'language' property in the request body."
        };
        return;
    }

    const textAnalyticsClient = new TextAnalyticsClient(process.env.COGNITIVE_SERVICES_ENDPOINT, new AzureKeyCredential(process.env.COGNITIVE_SERVICES_KEY));

    const result = await textAnalyticsClient.analyzeSentiment(text, { language });

    context.res = {
        body: result
    };
};

```
Este código cria uma função que recebe um objeto JSON no corpo da solicitação HTTP com as propriedades text e language, e utiliza o pacote @azure/cognitiveservices-textanalytics para analisar o sentimento do texto fornecido. O resultado é retornado no corpo da resposta HTTP.

6. Configure as variáveis de ambiente necessárias. Crie um arquivo chamado local.settings.json na pasta do seu projeto com o seguinte conteúdo:

```
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "node",
    "COGNITIVE_SERVICES_ENDPOINT": "<COGNITIVE_SERVICES_ENDPOINT>",
    "COGNITIVE_SERVICES_KEY": "<COGNITIVE_SERVICES_KEY>"
  }
}

```
Substitua <COGNITIVE_SERVICES_ENDPOINT> e <COGNITIVE_SERVICES_KEY> pelos valores corretos para o seu serviço de Cognitive Services.

6. Execute a função localmente. No terminal do VS Code, execute o comando func start para iniciar a função localmente.

7. Faça uma solicitação HTTP para a função. Utilize um cliente HTTP, como o Postman ou o cURL, para fazer uma solicitação HTTP para a URL da função local. O corpo da solicitação deve ser um objeto JSON com as propriedades text e language. Exemplo:

```
{
    "text": "I love Azure Functions!",
    "language": "en"
}

```

A resposta deve ser um objeto JSON com o resultado da análise de sentimento.

8. Implante a função no Azure. Use o comando func azure functionapp publish <NOME_DO_APP> para implantar a função no Azure. Substitua <NOME_DO_APP> pelo nome do seu aplicativo Azure Functions.
