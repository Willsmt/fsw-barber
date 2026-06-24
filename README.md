# 💈 FSW Barber

SaaS de agendamento para barbearias: descubra estabelecimentos, conheça os serviços oferecidos, faça reservas e avalie os atendimentos.

> ⚠️ **Status:** em desenvolvimento. A modelagem de dados, a infraestrutura e o tooling já estão prontos; a interface da aplicação ainda está sendo construída sobre o template inicial.

## ✨ Funcionalidades (domínio modelado)

- 👤 Usuários com autenticação (modelos compatíveis com NextAuth: `User`, `Account`, `Session`)
- 🏪 Cadastro de barbearias com endereço, fotos, descrição e telefones
- ✂️ Serviços por barbearia (nome, descrição, imagem e preço)
- 📅 Agendamentos (`Booking`) com status `CONFIRMED` / `COMPLETED` / `CANCELLED`
- ⭐ Avaliações (`Review`) vinculadas a um agendamento, com nota e comentário

## 🛠️ Tecnologias

| Camada        | Stack                                                                 |
| ------------- | --------------------------------------------------------------------- |
| Framework     | [Next.js 16](https://nextjs.org) (App Router + React Server Components)|
| Linguagem     | [TypeScript](https://www.typescriptlang.org) (strict)                 |
| UI            | [React 19](https://react.dev)                                         |
| Estilo        | [Tailwind CSS v4](https://tailwindcss.com), `tw-animate-css`          |
| Componentes   | [shadcn](https://ui.shadcn.com) (estilo `radix-nova`), Radix UI       |
| Ícones        | [lucide-react](https://lucide.dev)                                    |
| Utils         | `class-variance-authority`, `clsx`, `tailwind-merge`                  |
| ORM           | [Prisma 5](https://www.prisma.io)                                     |
| Banco         | [PostgreSQL 16](https://www.postgresql.org)                           |
| Qualidade     | ESLint 9, Prettier, Husky, lint-staged, Commitlint (Conventional)     |

## 📋 Pré-requisitos

- [Node.js](https://nodejs.org) 20+ (desenvolvido com v24)
- [Docker](https://www.docker.com) e Docker Compose (para o banco de dados)

## 🚀 Começando

### 1. Instale as dependências

```bash
npm install
```

### 2. Configure as variáveis de ambiente

Crie um arquivo `.env` na raiz do projeto:

```env
DATABASE_URL="postgresql://admin:admin123@localhost:5432/fsw_barber"
```

> Os valores acima correspondem ao banco definido no `docker-compose.yml`.

### 3. Suba o banco de dados

```bash
docker compose up -d
```

Isso inicia um PostgreSQL 16 em `localhost:5432` com o banco `fsw_barber`.

### 4. Aplique as migrations

```bash
npx prisma migrate dev
```

### 5. Popule o banco

Necessário para exibir conteúdo na aplicação. Cria 10 barbearias de exemplo, cada uma com seus serviços:

```bash
npx prisma db seed
```

### 6. Rode a aplicação

```bash
npm run dev
```

Acesse [http://localhost:3000](http://localhost:3000).

## 📜 Scripts

| Comando             | Descrição                                          |
| ------------------- | -------------------------------------------------- |
| `npm run dev`       | Inicia o servidor de desenvolvimento               |
| `npm run build`     | Gera o build de produção                           |
| `npm run start`     | Sobe o servidor de produção (após o build)         |
| `npm run lint`      | Roda o ESLint                                       |
| `npx prisma studio` | Abre o Prisma Studio para inspecionar o banco      |
| `npx prisma db seed`| Popula o banco com dados de exemplo                |

## 🗂️ Estrutura

```
fsw-barber/
├── app/                  # App Router (páginas, layouts, estilos globais)
│   ├── layout.tsx
│   ├── page.tsx
│   └── globals.css
├── prisma/
│   ├── schema.prisma     # Modelos de dados
│   ├── migrations/       # Histórico de migrations
│   └── seed.ts           # Script de seed
├── public/               # Assets estáticos
├── docker-compose.yml    # PostgreSQL para desenvolvimento
└── components.json       # Configuração do shadcn
```

## 🗃️ Modelo de dados

```
User ──< Booking >── BarbershopService >── Barbershop
  │         │                                   │
  └──< Review ──────────────────────────────────┘
```

- **User** — pode ter vários agendamentos e avaliações
- **Barbershop** — possui vários serviços, agendamentos e avaliações
- **BarbershopService** — pertence a uma barbearia e pode ter agendamentos
- **Booking** — liga usuário, serviço e barbearia em uma data, com status
- **Review** — avaliação 1:1 de um agendamento

## 🔒 Padrões de desenvolvimento

- **Commits:** seguem o padrão [Conventional Commits](https://www.conventionalcommits.org), validados pelo Commitlint via hook `commit-msg`.
- **Pre-commit:** `lint-staged` roda ESLint e Prettier nos arquivos `.ts`/`.tsx` alterados.
- **Formatação:** Prettier sem ponto e vírgula, `tabWidth: 2`, com ordenação automática de classes Tailwind.

Veja [`SECURITY.md`](./SECURITY.md) para notas de segurança e auditoria de dependências.

## 📄 Licença

Projeto privado, para fins de estudo.
