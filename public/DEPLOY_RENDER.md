# Dossier de Deploy: Projeto Antigravity na Render

Este documento centraliza todas as configurações e padronizações necessárias para realizar o deploy do projeto **antigravity** na plataforma Render com sucesso.

---

## 1. Padronização do Código (Alterações Necessárias)

A Render atribui dinamicamente uma porta para a sua aplicação através da variável de ambiente `PORT`. Seu código **não pode** usar uma porta estática (como `3000` ou `5000`) em produção.

### 🌐 Em ambientes Node.js (Express/Fastify)
Certifique-se de que o arquivo principal do seu servidor (ex: `server.js`, `index.js` ou `app.ts`) esteja configurado assim:

```javascript
const PORT = process.env.PORT || 3000;

app.listen(PORT, '0.0.0.0', () => {
    console.log(`Antigravity subiu com sucesso na porta ${PORT}`);
});
```

---

## 2. Configuração do Render (Passo a Passo)

### 1. Criação da Conta
- **Plataforma**: Render (<https://render.com/>)
- **Tipo**: Crie uma conta gratuita usando seu e-mail da faculdade ou GitHub.

### 2. Novo Serviço
- No dashboard, clique em **New** > **Web Service**.
- Conecte seu GitHub (autorize o acesso).
- Selecione o repositório do projeto.

### 3. Configuração da Build
Preencha os campos exatamente assim:

| Campo | Valor |
| :--- | :--- |
| **Branch** | `main` (ou `master`) |
| **Root Directory** | Deixe vazio (ou `/`) |
| **Auto Build** | **Enable** |
| **Build Command** | `npm install` |
| **Start Command** | `npm start` |
| **Node Version** | `20.x` ou superior |
| **Environment** | **Node** |

### 4. Variáveis de Ambiente
Na seção **Environment**, adicione as seguintes variáveis de ambiente (elas serão usadas pelo NextAuth para conectar ao Supabase):

| Variável | Valor |
| :--- | :--- |
| `DATABASE_URL` | `postgresql://neondb_owner:[EMAIL_ADDRESS]/neondb?sslmode=require&channel_binding=require` (ou a URL do seu banco) |
| `NEXTAUTH_SECRET` | *Gere uma string aleatória longa* (Use: `openssl rand -base64 32`) |
| `NEXTAUTH_URL` | `https://seu-projeto.onrender.com` (o nome que aparecerá quando você deployar) |

**Importante:** O `NEXTAUTH_SECRET` é crucial para a segurança. Não use um texto simples como "banana123"; use o gerador de código.

### 5. Deploy
- Clique em **Create Web Service**.
- Aguarde a conclusão da build. Se houver erros, eles aparecerão no log ao vivo.