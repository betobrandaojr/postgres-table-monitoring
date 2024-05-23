# Utilização do Debezium para Monitoramento de Tabelas do Banco de Dados e Envio para o Kafka

## O que é o Debezium? 🐘🔄➡️

Debezium é uma plataforma open-source para captura de dados em mudança (Change Data Capture, ou CDC). Ele monitora as alterações nos dados em bancos de dados e transmite essas mudanças em tempo real para outros sistemas, como Apache Kafka. Isso permite que os sistemas downstream (consumidores) reajam rapidamente às alterações nos dados, mantendo a integridade e a sincronização entre diferentes sistemas e aplicações.

## Configuração Necessária ⚙️📝

Para realizarmos esta configuração, vamos precisar de 2 arquivos principais:

1. **docker-compose.yml**: Este arquivo será fundamental para orquestrar e gerenciar todos os serviços necessários para a captura de mudanças em dados usando Debezium. Isso inclui a configuração dos contêineres para PostgreSQL, Zookeeper, Kafka, Debezium e Kafdrop.

2. **debezium.json**: Este arquivo se trata do conector Debezium para PostgreSQL. Ele define como o conector irá se conectar ao banco de dados PostgreSQL e quais tabelas ele irá monitorar para capturar alterações (CDC - Change Data Capture).

## Instalando Docker Desktop 🐋💻

Para utilizar o `docker-compose`, é necessário ter o Docker Desktop instalado em sua máquina. Siga os passos abaixo para baixar e instalar o Docker Desktop:

1. Acesse o site oficial do Docker: [Docker Desktop](https://www.docker.com/products/docker-desktop)
2. Baixe o instalador apropriado para o seu sistema operacional (Windows, macOS, ou Linux).
3. Siga as instruções de instalação fornecidas no site para concluir a instalação.
4. Após a instalação, abra o Docker Desktop e verifique se está em execução.

## Passo-a-passo 📋🔧

### 1. Subir o docker-compose.yml 🚀
***
  Execute o comando abaixo no terminal para iniciar os serviços:
  ```sh
  docker-compose up -d
  ```
### 2. Criar as Tabelas (caso necessário) 🛠️📋
***
  Crie as tabelas no PostgreSQL utilizando os seguintes comandos SQL:
  ```sh
  sql
  CREATE TABLE users (id integer, name varchar);
  CREATE TABLE orders (id integer, name varchar);
  ```
### 3. Configurar as Tabelas para Monitoramento 👀🗂️
***
Execute o comando abaixo para configurar as tabelas que vamos monitorar:

  ```sh
  ALTER TABLE public.nome_tabela REPLICA IDENTITY FULL;
  ```
  Obs: Este comando é utilizado no contexto do Debezium para garantir que as alterações nos dados de uma tabela específica sejam corretamente capturadas e transmitidas.

### 4. Configurar o Conector no Kafka Connect 🔌🛠️
***
  Utilize as configurações especificadas no arquivo debezium.json para configurar um conector no Kafka Connect.
  
  Em sistemas Linux:
  
  ```sh
  curl -X POST -H "Accept:application/json" -H "Content-Type:application/json" http://localhost:8083/connectors/ --data "@debezium.json"
  ```
  Em sistemas Windows:
  ```sh
  Invoke-RestMethod -Uri "http://localhost:8083/connectors/" -Method Post -Headers @{Accept="application/json"; "Content-Type"="application/json"} -Body (Get-Content -Raw -Path "debezium.json")
  ```
### Conclusão 🚀✔
***
Com essas etapas, você terá configurado o Debezium para monitorar alterações nas tabelas do seu banco de dados PostgreSQL e enviar essas alterações em tempo real para o Kafka,
permitindo que seus sistemas consumam e reajam a essas mudanças de forma eficiente.
