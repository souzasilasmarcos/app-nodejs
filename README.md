# app-nodejs

Aplicativo de amostra Implementando um aplicativo Node.js para Amazon Web Services usando Docker.

Índice

1 - Docker e AWS ja utilizando estrutura do projeto autoscaling-app
2 - Criação de um Dockerfile
3 - Construir uma imagem docker
4 - Executando um contêiner docker
5 - Criação do Registro (ECR) e upload da imagem do aplicativo
6 - Criação de uma nova definição de tarefa
7 - Criar um serviço para executá-lo

-- Neste repositorio foi necessario em primeiro step a subida da estrutura do node.

-- Dentro do repo local Dockerfile:

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

mkdir sample-nodejs-app

// change to new directory

cd sample-nodejs-app

// Initialize npm

npm init -y

// install express

npm install express

// create an server.js file

touch server.js

##### ESTRUTURA DO PROJETO

Aqui, estamos construindo nossa imagem Docker usando a imagem Node.js oficial do Dockerhub importando a imagem nova para nosso ECR na AWS
(um repositório para imagens de base).

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

Os contêineres do Docker podem fazer conexões com o mundo externo, porem o mundo externo não pode se conectar aos nossos contêineres. -p publica todas as portas expostas nas interfaces do host. 
Aqui, publicamos o aplicativo para a porta 80: 3000. Como estamos executando o Docker localmente, acesse http://public-DNS para visualizar.

