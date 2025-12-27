# 🚀 Deploy do Bot WhatsApp no Render

## Pré-requisitos
- Conta no [Render](https://render.com) (gratuita)
- Repositório no GitHub com este código

## Passo a Passo

### 1. Prepare o Repositório GitHub
```bash
# Se ainda não inicializou o git:
git init
git add .
git commit -m "Initial commit - WhatsApp Bot"

# Crie um repositório no GitHub e faça push:
git remote add origin https://github.com/SEU_USUARIO/SEU_REPO.git
git branch -M main
git push -u origin main
```

### 2. Criar Web Service no Render

1. Acesse [dashboard.render.com](https://dashboard.render.com)
2. Clique em **"New +"** → **"Web Service"**
3. Conecte seu repositório GitHub
4. Selecione o repositório do bot

### 3. Configurações do Deploy

Preencha os campos:

| Campo | Valor |
|-------|-------|
| **Name** | `whatsapp-bot` (ou nome que preferir) |
| **Region** | `Oregon (US West)` (mais próximo do Brasil) |
| **Branch** | `main` |
| **Runtime** | `Docker` |
| **Instance Type** | `Free` (para começar) |

### 4. Variáveis de Ambiente (Opcional)

Se precisar configurar variáveis, clique em **"Advanced"** e adicione:

```
NODE_ENV=production
PORT=3001
```

> ⚠️ **Importante**: O Render usa a porta definida no `EXPOSE` do Dockerfile (3001)

### 5. Deploy

1. Clique em **"Create Web Service"**
2. O Render vai:
   - Clonar seu repositório
   - Buildar a imagem Docker
   - Fazer deploy (leva ~5-10 minutos)

### 6. Conectar o WhatsApp

Após o deploy concluir:

1. Acesse a URL do seu serviço (ex: `https://whatsapp-bot-xxxx.onrender.com`)
2. Use o endpoint `/status` para obter o QR Code:
   ```bash
   curl https://whatsapp-bot-xxxx.onrender.com/status
   ```
3. O QR Code estará no campo `qr` do JSON
4. Escaneie com seu WhatsApp

### 7. Testar o Bot

```bash
# Verificar status
curl https://seu-app.onrender.com/status

# Enviar mensagem de teste
curl -X POST https://seu-app.onrender.com/send \
  -H "Content-Type: application/json" \
  -d '{
    "phone": "5511999999999",
    "message": "Olá! Bot funcionando no Render!"
  }'
```

## 📡 URLs Importantes

| Endpoint | Método | Descrição |
|----------|--------|-----------|
| `/status` | GET | Verifica se bot está conectado e retorna QR |
| `/send` | POST | Envia mensagem (body: `{phone, message}`) |
| `/logout` | POST | Desconecta e limpa sessão |

## 🔥 Plano Free do Render

### Limitações:
- ⏸️ **Dorme após 15min de inatividade**
- 🐌 **Pode levar ~30s para "acordar"**
- ⏱️ **750h/mês grátis** (suficiente se não rodar 24/7)

### Manter Sempre Ativo:

#### Opção 1: Upgrade para Plano Pago ($7/mês)
- Serviço não dorme
- Melhor para produção

#### Opção 2: Ping Automático (Plano Free)
Use um serviço como [UptimeRobot](https://uptimerobot.com) ou [Cron-Job.org](https://cron-job.org):
- Configure para fazer ping no endpoint `/status` a cada 5-10 minutos
- Mantém o serviço "acordado"

## 🛠️ Troubleshooting

### Bot não conecta no Render
1. Verifique os logs: Dashboard → Logs
2. Problemas comuns:
   - Chrome não instalado corretamente (verifique Dockerfile)
   - Falta de memória (upgrade para plano pago)

### Sessão perdida após restart
- Normal no plano free (serviço reinicia)
- Solução: Use variáveis de ambiente para persistir sessão ou upgrade para plano pago

### QR Code não aparece
```bash
# Ver logs em tempo real
# No dashboard do Render, acesse "Logs" e procure por "QR CODE RECEBIDO"
```

## 🔄 Atualizações

Sempre que fizer alterações:

```bash
git add .
git commit -m "Descrição da mudança"
git push origin main
```

O Render vai fazer deploy automático! 🎉

## 📞 Suporte

- [Documentação Render](https://render.com/docs)
- [Comunidade Render](https://community.render.com)
