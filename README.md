# app-nodejs

Aplicativo de amostra Implementando um aplicativo Node.js para Amazon Web Services usando Docker.

Índice

1 - Uma introdução rápida sobre Docker e AWS ja utilizando estrutura do projeto autoscaling-app
2 - Criação de um Dockerfile
3 - Construir uma imagem docker
4 - Executando um contêiner docker
5 - Criação do Registro (ECR) e upload da imagem do aplicativo
6 - Criação de uma nova definição de tarefa
7 - Criar um serviço para executá-lo

-- Neste repositorio foi necessario em primeiro step a subida da estrutura do node.

dentro do repo local Dockerfile:

FROM node:6-alpine
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
COPY . .
RUN npm install
EXPOSE 3000
CMD [ "node", "server.js" ]

-- Depois de configurado os parametros na raiz do projeto no Dockerfile, nosso app deve estar configurada seguindo estes requisitos:

const express = require('express')
const app = express()
app.get('/', (req, res) => {
    res.send('Hello world from a Node.js app!')
})
app.listen(3000, () => {
    console.log('Server is up on 3000')
})

-- Agora com estrutura dos recursos no repo de dev vamos criar o build 

docker build -t sample-nodejs-app .

-- Realizar a subida da tag do projeto para nosso ECR

docker tag sample-nodejs-app:latest 374120343751.dkr.ecr.sa-east-1.amazonaws.com/sample-nodejs-app:latest

-- Precisamos subir esta imagem no repositorio ECR na AWS, dessa forma nossa automaçao ira usar esta repo para instaciar a aplicaçao em ambiente de container ou mesmo em nodes EC2:

docker push 374120343751.dkr.ecr.sa-east-1.amazonaws.com/sample-nodejs-app:latest

-- Por fim nossa autonaçao no projeto autoscaling-app podera instanciar nossa aplicaçao via docker:

docker run -p 80:3000 sample-nodejs-app

-- Criterio para estrutura do projeto em node js local

// create a new directory
mkdir app-nodejs

// change to new directory
cd app-nodejs

// Initialize npm
npm init -y

// install express
npm install express

// create an server.js file
touch server.js

##### ESTRUTURA DO PROJETO

Aqui, estamos construindo nossa imagem Docker usando a imagem Node.js oficial do Dockerhub (um repositório para imagens de base).

Inicie nosso Dockerfile com uma instrução FROM. É aqui que você especifica sua imagem de base.
A instrução RUN nos permitirá executar um comando para qualquer coisa que você desejar.
Criamos um subdiretório /usr/src /app que conterá o código do nosso aplicativo na imagem docker.
A instrução WORKDIR estabelece o subdiretório que criamos como o diretório de trabalho para quaisquer instruções RUN, CMD, ENTRYPOINT, COPY e ADD que o seguem no Dockerfile. /usr/src/app-node é nosso diretório de trabalho.
COPY nos permite copiar arquivos de uma origem para um destino. Copiamos o conteúdo de nosso código de aplicativo de nó (server.js e package.json) de nosso diretório atual para o diretório de trabalho em nossa imagem docker.
A instrução EXPOSE informa ao Docker que o contêiner escuta nas portas de rede especificadas no tempo de execução. Especificamos a porta 3000.
Por último, mas não menos importante, a instrução CMD especifica o comando para iniciar nosso aplicativo. Isso informa ao Docker como executar seu aplicativo. Aqui, usamos o node server.js, que normalmente é como os arquivos são executados no Node.js.

O aplicativo criando um único arquivo chamado Dockerfile na base do diretório do nosso projeto.

Novamente o Dockerfile é o projeto a partir do qual nossas imagens são construídas. 
E então as imagens se transformam em contêineres, nos quais executamos nossos aplicativos.

FROM node:8-alpine
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
COPY . .
RUN npm install
EXPOSE 3000
CMD [ "node", "server.js" ]

Os contêineres do Docker podem fazer conexões com o mundo externo, 
porem o mundo externo não pode se conectar aos nossos contêineres. -p publica todas as portas expostas nas interfaces do host. 
Aqui, publicamos o aplicativo para a porta 80: 3000. Como estamos executando o Docker localmente, acesse http://public-DNS para visualizar.

