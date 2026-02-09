# Site de Receitas com Postagem Automática (diária)

Este projeto é um site completo (Next.js + Prisma + SQLite) que:
- mostra receitas (home, detalhe, categorias, busca)
- tem área admin simples (senha via ENV)
- cria receitas automaticamente por um endpoint de cron (`/api/cron/daily`)

## 1) Instalar e rodar local
```bash
npm install
cp .env.example .env
# edite .env e coloque uma senha forte e um CRON_SECRET
npm run db:push
npm run seed
npm run dev
```
Abra: http://localhost:3000

## 2) Admin
- Acesse `/admin`
- Login em `/admin/login`
- Crie receitas em `/admin/new`

## 3) Automatizar “todo dia”
O endpoint de cron exige:
- Header: `Authorization: Bearer SEU_CRON_SECRET`

Exemplo (local):
```bash
curl -H "Authorization: Bearer SEU_CRON_SECRET" "http://localhost:3000/api/cron/daily?n=6"
```

### Opção A: VPS (recomendado, fácil)
1. Suba o projeto em um VPS (Ubuntu).
2. Rode com PM2 ou Docker.
3. Crie um cron no servidor chamando o endpoint:

**Crontab (todo dia 08:00):**
```bash
0 8 * * * curl -s -H "Authorization: Bearer SEU_CRON_SECRET" "https://SEU-DOMINIO.com/api/cron/daily?n=6" >/dev/null 2>&1
```

### Opção B: GitHub Actions (grátis pra começar)
Crie `.github/workflows/daily.yml` chamando o seu endpoint (exemplo abaixo).

## 4) Trocar o gerador “demo” por IA ou API
O arquivo:
`src/app/api/cron/daily/route.ts`
tem `generateRecipe()`. Você pode:
- puxar receitas de uma API
- ou chamar um modelo de IA (e salvar no banco)

## 5) Produção
- Banco SQLite funciona em VPS simples.
- Para alta escala, troque o datasource do Prisma para PostgreSQL.

Boa! 🚀
