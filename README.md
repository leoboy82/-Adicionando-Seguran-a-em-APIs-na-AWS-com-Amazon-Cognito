# Roteiro para o desenvolvimento da atividade prática do DIO Live Coding do dia 17/11/2021

Neste guia, você encontrará um passo a passo para desenvolver a atividade prática do DIO Live Coding, utilizando os seguintes serviços da AWS: Amazon Cognito, Amazon DynamoDB, Amazon API Gateway e AWS Lambda.

## Serviços AWS utilizados
- Amazon Cognito
- Amazon DynamoDB
- Amazon API Gateway
- AWS Lambda

## Etapas do desenvolvimento

### Criando uma API REST no Amazon API Gateway
1. Acesse o painel de controle do Amazon API Gateway.
2. Clique em "Create API" no painel do Dashboard.
3. Selecione "REST API" e clique em "Build".
4. Escolha o protocolo como "REST" e crie uma nova API com o nome [dio_live_api].
5. Selecione o tipo de Endpoint como "Regional" e clique em "Create API".
6. Em "Resources", clique em "Actions" e selecione "Create Resource".
7. Defina o nome do recurso como [Items] e clique em "Create Resource".

### No Amazon DynamoDB
1. Acesse o painel de controle do Amazon DynamoDB.
2. Clique em "Tables" e selecione "Create table".
3. Defina o nome da tabela como [Items].
4. Escolha a chave de partição como [id] e clique em "Create table".

### No AWS Lambda
1. Acesse o painel de controle do AWS Lambda.
2. Clique em "Create function" e defina o nome como [put_item_function].
3. Selecione o código da função [put_item_function.js] disponível na pasta /src e faça o deploy.
4. Vá para "Configuration" e clique em "Execution role".
5. No console do IAM, encontre a role aberta.
6. Em "IAM", clique em "Roles" e localize a role criada no passo anterior.
7. Acesse "Permissions" e adicione uma política inline.
8. Selecione o serviço "DynamoDB" e adicione a ação "putItem".
9. Em "Resources", adicione o ARN da tabela criada no DynamoDB.
10. Revise a política e defina o nome como [lambda_dynamodb_putItem_policy]. Crie a política.

### Integrando o API Gateway com o Lambda backend
1. Acesse o painel de controle do Amazon API Gateway.
2. Selecione a API criada e vá para "Resources".
3. Selecione o recurso criado e clique em "Action" e, em seguida, "Create method - POST".
4. Escolha o tipo de integração como "Lambda function".
5. Marque a opção "Use Lambda Proxy Integration" e selecione a função Lambda criada.
6. Clique em "Save".
7. Vá para "Actions" e selecione "Deploy API".
8. Crie um novo estágio de implantação com o nome [dev] e faça o deploy.

### No POSTMAN
1. Adicione uma nova request no POSTMAN.
2. Defina o método como POST e copie o endpoint gerado no Amazon API Gateway.
3. No corpo da requisição, selecione o formato "Raw", tipo "JSON" e adicione o seguinte body:
```json
{
  "id": "003",
  "price": 600
}

4. Envie a requisição.

### No Amazon Cognito
1. Acesse o painel de controle do Amazon Cognito.
2. Clique em "Manage User Pools".
3. Crie um novo User Pool com o nome [TestPool].
4. Selecione "Email address or phone number" como opção de login para os usuários.
5. Escolha o nível de segurança desejado para as senhas.
6. Desative a opção de Multi-Factor Authentication (MFA).
7. Personalize as mensagens de verificação por e-mail, caso desejado.
8. Defina as configurações de acesso do App Client. Nomeie-o como [TestClient] e crie o App Client.
9. Salve as alterações.

#### App Integration
1. Acesse as configurações de integração do aplicativo.
2. Habilite a opção "Cognito User Pool" em "Enabled Identity Providers".
3. Adicione a URL de callback [https://example.com/logout] para o aplicativo.
4. Em OAuth 2.0, permita os fluxos "Authorization code grant" e "Implicit grant".
5. Defina os escopos permitidos como "email" e "openid".
6. Salve as alterações.

#### Domain name
1. Configure um domínio para o Cognito.
2. Defina um prefixo de domínio como [diolive].
3. Salve as alterações.

### Criando um autorizador do Amazon Cognito para uma API REST no Amazon API Gateway
1. Acesse o painel de controle do Amazon API Gateway.
2. Selecione a API criada anteriormente.
3. Vá para a seção "Authorizers" e clique em "Create New Authorizer".
4. Defina o nome como [CognitoAuth].
5. Escolha o tipo como "Cognito" e selecione o User Pool criado anteriormente.
6. Defina a origem do token como [Authorization].
7. Vá para "Resources" e selecione o recurso criado anteriormente.
8. Selecione o método criado e vá para "Method Request".
9. Em "Authorization", selecione o autorizador criado.

### No POSTMAN
1. Adicione uma nova request no POSTMAN.
2. Vá para a seção de autorização.
3. Selecione o tipo de autorização como "OAuth 2.0".
4. Defina a URL de callback como [https://example.com/logout].
5. Adicione a URL de autenticação como [https://diolive.auth.sa-east-1.amazoncognito.com/login].
6. Insira o ID do cliente obtido do Cognito em "Client ID".
7. Defina o escopo como "email" e "openid".
8. Selecione a opção "Send client credentials in body" para autenticação do cliente.
9. Clique em "Get New Access Token".
10. Copie o token gerado.
11. Selecione a request criada para inserir o item.
12. Vá para a seção de autorização e selecione o tipo "Bearer Token".
13. Cole o token copiado.
14. Envie a requisição.

Isso conclui o roteiro para o desenvolvimento da atividade prática do DIO Live Coding do dia 17/11/2021. Certifique-se de seguir as etapas na ordem apresentada para garantir o correto funcionamento da solução.

