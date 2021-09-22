# app-nodejs

Sample app Deploying a Node.js application to Amazon Web Services using Docker.

Table of Contents

1. Prerequisites
2. A quick primer on Docker and AWS
3. What we’ll be deploying
4. Creating a Dockerfile
5. Building a docker image
6. Running a docker container
7. Creating the Registry (ECR) and uploading the app image to it
8. Creating a new task definition
9. Creating a cluster
10. Creating a service to run it

#

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
A instrução WORKDIR estabelece o subdiretório que criamos como o diretório de trabalho para quaisquer instruções RUN, CMD, ENTRYPOINT, COPY e ADD que o seguem no Dockerfile. / usr / src / app é nosso diretório de trabalho.
COPY nos permite copiar arquivos de uma origem para um destino. Copiamos o conteúdo de nosso código de aplicativo de nó (server.js e package.json) de nosso diretório atual para o diretório de trabalho em nossa imagem docker.
A instrução EXPOSE informa ao Docker que o contêiner escuta nas portas de rede especificadas no tempo de execução. Especificamos a porta 3000.
Por último, mas não menos importante, a instrução CMD especifica o comando para iniciar nosso aplicativo. Isso informa ao Docker como executar seu aplicativo. Aqui, usamos o node server.js, que normalmente é como os arquivos são executados no Node.js.

O aplicativo criando um único arquivo chamado Dockerfile na base do diretório do nosso projeto.

O Dockerfile é o projeto a partir do qual nossas imagens são construídas. 
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

