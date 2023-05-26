PROJETO PRÁTICO BOOTCAMP CLOUD AWS ꞏ DIO
Experiência Linux
🗒️Infraestrutura como Código com Serverless Framework na AWS
🔧Ferramentas que utilizo
Emblema Git sem servidor Emblema Git Emblema Git Emblema Git Distintivo do Github Distintivo do Github
📈Diagrama do projeto
Diagrama AWS
☁️Serviços AWS neste projeto
CLOUDFORMATION: Serviço que fornece aos desenvolvedores e empresas uma forma fácil de criar um conjunto de recursos relacionados da AWS e de terceiros para provisioná-los e gerenciá-los de forma organizada e previsível. Saiba mais.
API GATEWAY: Serviço gerenciado que permite criação, publicação, manutenção, monitoramento e proteção de APIs REST e WebSocket em qualquer escala. Saiba mais.
LAMBDA: Serviço de computação sem servidor e orientado a eventos que permite executar código para praticamente qualquer tipo de aplicação ou serviço de backend sem provisionar ou gerenciar servidores. Saiba mais.
DYNAMODB: Banco de dados de chave-valor NoSQL, sem servidor e totalmente gerenciado, projetado para executar aplicações de alta performance em qualquer escala. Saiba mais.
📄Descrição do desafio do projeto
Criação de uma infraestrutura em nuvem AWS para a publicação de funções CRUD com CloudFormation, Lambda, DynamoDB e API Gateway utilizando o Serverless Framework.

📝Pré-requisitos:
Possuir conta na AWS
Node.js instalado (Página de download: https://nodejs.org/en/download )
AWS CLI instalado (Veja como instalar: https://docs.aws.amazon.com/pt_br/cli/latest/userguide/getting-started-install.html )
🆔Credenciais AWS
Criar usuário
Console de gerenciamento da AWS➡️Painel IAM➡️Criar novo usuário➡️<nome do usuário> :right_arrow: Permissions "Administrator Access"➡️Acesso programático👉Chaves de download
Sem terminais:$ aws configure ➡️colar as credenciais geradas anteriormente
Configurar o framework Serverless
$ npm i -g serverless

Desenvolvimento do projeto
$ serverless
Login/Register: No
Update: No
Type: Node.js REST API
Name: dio-live
$ cd dio-live
$ code .
Nenhum arquivo serverless.ymladicionar a região region: us-east-1dentro do escopo deprovider:
Salvar e realizar o deploy$ serverless deploy -v
Estruturar o código
Crie o diretório "src" e mova o arquivo "handler.js" para dentro dele
Renomear o arquivo "handler.js" para "hello.js"
atualizar o código
const hello = async (event) => {
/////
module.exports = {
    handler:hello
}
Atualizar o arquivo "serverless.yml "
handler: src/hello.handler
$ serverless deploy -v 

DynamoDBName
Atualizar o arquivo serverless.yml

resources:
  Resources:
    ItemTable:
      Type: AWS::DynamoDB::Table
      Properties:
          TableName: ItemTable
          BillingMode: PAY_PER_REQUEST
          AttributeDefinitions:
            - AttributeName: id
              AttributeType: S
          KeySchema:
            - AttributeName: id
              KeyType: HASH
Desenvolvedor funções lambda
- Pasta /src do repositório
Obtenha arn da tabela no DynamoDB AWS Console -> DynamoDB -> Overview -> Amazon Resource Name (ARN)
Atualizar arquivo serverless.yml com o código a seguir, abaixo doregion:
	iam:
    role:
        statements:
          - Effect: Allow
            Action:
              - dynamodb:PutItem
              - dynamodb:UpdateItem
              - dynamodb:GetItem
              - dynamodb:Scan
            Resource:
              - arn:aws:dynamodb:us-east-1:167880115321:table/ItemTable
Instalar dependências
npm init npm i uuid aws-sdk

Atualizar lista de funções no arquivo serverless.yml
functions:
hello:
  handler: src/hello.handler
  events:
    - http:
        path: /
        method: get
insertItem:
  handler: src/insertItem.handler
  events:
    - http:
        path: /item
        method: post
fetchItems:
  handler: src/fetchItems.handler
  events:
    - http:
        path: /items
        method: get
fetchItem:
  handler: src/fetchItem.handler
  events:
    - http:
        path: /items/{id}
        method: get
updateItem:
  handler: src/updateItem.handler
  events:
    - http:
        path: /items/{id}
        method: put
