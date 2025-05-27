### Dockerfile Node 
```
FROM node:18 # Usa a imagem do node  18 como base.
```
```
WORKDIR /app # Define o diretório como /app.
```
```
COPY . . # Copia todos os arquivos do diretório para o container.
```
```
RUN npm install # Instala as dependências do node.
```
```
EXPOSE 3001 # Expõe o servidor na porta 3001.
```
```
CMD ["node", "index.js"] # Roda o comando para iniciar a aplicação pelo cmd.
```
---
### Dockerfile Python 
```
FROM python:3.11 # Usa a imagem do Python 3.11 como base.
```
```
WORKDIR /app # Define o diretório do container como /app.
```
```
COPY . . # Copia todos os arquivos para o container.
```
```
RUN pip install flask redis requests mysql-connector-python # Instala as dependências do python.

```
```
EXPOSE 3002 # Expõe o servidor na porta 3002.

```
```
CMD ["python", "index.py"] # Roda o comando para iniciar a aplicação pelo cmd.
```
---
### Dockerfile php 
```
FROM php:8.2-cli # Usa a imagem do php 8.2 como base.
```
```
WORKDIR /app # Copia todos os arquivos para o container. # Define o diretório como /app.

```
```
COPY . . # Copia todos os arquivos para o container.
```
```
EXPOSE 3003 # Expõe o servidor na porta 3001.
```
```
CMD ["php", "-S", "0.0.0.0:3003", "index.php"] # Roda o comando para iniciar a aplicação pelo cmd.
```
---
### Docker compose
```
version: '3.8' # Define a versão do Docker Compose.
```
```
services # Lista os serviços do container e define suas variáveis
```
```
  products
    build: ./api-node # Builda a imagem usando o Dockerfile da api-node.
    container_name: products # Define o nome do container como products.
    ports: 
      - "3001:3001" # Mapeia a porta 3001 do host para a porta 3001 do container.  
    networks: 
      -ecommerce # Conecta o container à rede ecommerce.
```
```
  orders
    build: ./api-python # Builda a imagem usando o Dockerfile da api-python.
    container_name: orders # Define o nome do container como orders.
    ports: 
      - "3002:3002" # Mapeia a porta 3002 do host para a porta 3002 do container.
    depends_on: 
      - products, 
      - db, 
      - redis # Garante que os containers products, db e redis sejam iniciados antes.
    networks: 
      - ecommerce # Conecta à rede ecommerce.
```
```
  payments
    build: ./api-php # Builda a imagem usando o Dockerfile da pasta api-php.
    container_name: payments # Define o nome do container como payments.
    ports: 
      - "3003:3003" # Mapeia a porta 3003 do host para a porta 3003 do container.
    depends_on:
      - orders # Garante que o container orders seja iniciado antes. 
    networks: 
      - ecommerce # Conecta à rede ecommerce.

```
```
  db
    image: mysql:8.0 # Usa a imagem do MySQL 8.0.
    container_name: db # Define o nome do container como db.
    environment:
      MYSQL_ROOT_PASSWORD: 932545 # Define a senha do banco de dados.
      MYSQL_DATABASE: ecommerce # Define o nome do banco de dados.
    ports: 
      - "3307:3306" # Mapeia a porta 3307 do host para a 3306 do container.
    networks: 
      - ecommerce # Conecta à rede ecommerce.
```
```
  redis
    image: redis:7 # Usa a imagem do Redis 7.
    container_name: redis # Define o nome do container como redis.
    ports: 
      - "6379:6379" # Mapeia a porta 6379 do host para a 6379 do container.
    networks: 
      - ecommerce # Conecta à rede ecommerce.
```
```
  networks:
    ecommerce: # Cria uma network para facilitar a comunicação entre os containeres.
      bridge: # Define que é uma rede bridge.
```
