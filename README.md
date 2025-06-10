# PRISMA-INSTALL
Configurabdo o Prisma ORM com um banco de dados MySQL locamente em um projeto Node.js.
# 📘 Prisma + MySQL locamente com Node.js

## 📘 Descrição

Este passo a passo mostra como configurar o **Prisma ORM** com um banco de dados **MySQL** em um projeto **Node.js**. Ele inclui a instalação, configuração da conexão com o banco, definição de modelos, execução de migrações e uso básico do Prisma Client para consultar e inserir dados.

---

## ⚙️ Configuração do Prisma com MySQL

### ✅ Pré-requisitos

- Node.js instalado
- Banco de dados MySQL rodando
- Projeto Node.js iniciado (`npm init -y`)

---

### 📦 Instalação do Prisma

```bash
npm install prisma --save-dev
npx prisma init
```

> Isso cria a pasta `prisma/` com o arquivo `schema.prisma` e um `.env` na raiz do projeto.

---

### 🛠️ Configuração da conexão com o banco

Edite o arquivo `.env` com sua URL de conexão do MySQL:

```env
DATABASE_URL="mysql://USUARIO:SENHA@localhost:3306/NOME_DO_BANCO"
```

> Exemplo: `mysql://root:minhasenha@localhost:3306/NOME_DO_BANCO`

---

### ✏️ Definindo os modelos no Prisma

Edite `prisma/schema.prisma` com os modelos desejados. Exemplo:

```prisma
generator client {
  provider = "prisma-client-js"
  output   = "../generated/prisma"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}

model usuarios {
  id        Int      @id @default(autoincrement())
  name      String
  email     String   @unique
  createdAt DateTime @default(now())
}
```

---

### 🚀 Criando a base e gerando o client

```bash
npx prisma migrate dev --name inicial
```

> Isso:
> - Cria as tabelas no banco
> - Gera o Prisma Client
> - Cria uma pasta de migrações em `prisma/migrations`

---

### 🧬 Gerar o Prisma Client (manual, se necessário)

```bash
npx prisma generate
```

> Só é necessário se você **editar o schema** e **não rodar uma migração**.

---

### 🧪 Exemplo de uso no código

```js
const { PrismaClient } = require('../generated/prisma');
const prisma = new PrismaClient();

async function main() {
  const usuario = await prisma.usuarios.create({
    data: {
      name: 'João da Silva',
      email: 'joao@example.com',
    },
  });

  console.log(usuario);
}

main()
  .catch(e => console.error(e))
  .finally(() => prisma.$disconnect());
```

---

### 📚 Comandos úteis

```bash
# Criar migração e aplicar
npx prisma migrate dev --name nome_da_migracao

# Gerar o Prisma Client manualmente
npx prisma generate

# Visualizar o banco (após rodar migrate)
npx prisma studio
```
