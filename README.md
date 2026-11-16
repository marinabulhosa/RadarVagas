# Radar de Vagas com IA

Automação em **n8n** que lê e-mails de alerta de vagas (LinkedIn, Gupy, Indeed, Vagas.com), avalia cada vaga com IA conforme o meu perfil e avisa só sobre as que combinam.

> Projeto feito para resolver um problema real: eu gastava horas abrindo alertas de vagas que não tinham nada a ver com o meu perfil.

## Como funciona

1. **Gmail Trigger** verifica a caixa de entrada a cada hora e pega os e-mails de alerta de vagas que chegaram desde a última verificação.
2. **Preparar texto** (JavaScript) limpa o HTML, preserva os links e monta o pedido para a IA.
3. **IA** (Gemini API, camada gratuita) extrai todas as vagas do e-mail e dá uma nota de 0 a 10 com justificativa.
4. **Separar vagas** (JavaScript) transforma cada vaga em um item individual.
5. **Filtro** deixa passar apenas notas maiores ou iguais a 7.
6. **Google Sheets** registra a vaga e o **Telegram** envia o aviso.

## Tecnologias

n8n · Gemini API · Gmail · Google Sheets · Telegram Bot · JavaScript

## Arquivos

| Arquivo | O que é |
|---|---|
| `radar_de_vagas_n8n.json` | O fluxo, pronto para importar no n8n |
| `prompt.md` | O prompt com o perfil do candidato |
| `index.html` | Página do estudo de caso (portfólio) |

## Como instalar

1. **Instale o Node.js** em uma versão aceita pelo n8n (20.19 a 24). Confira na [documentação](https://docs.n8n.io/) a faixa atual.
2. **Rode o n8n:** `npx n8n` e abra `http://localhost:5678`.
3. **Importe o fluxo:** *Create workflow → menu ⋯ → Import from file* e escolha `radar_de_vagas_n8n.json`.
4. **Crie as credenciais:**
   - **Gmail** e **Google Sheets** (OAuth2, via Google Cloud Console)
   - **Gemini:** chave gratuita em [aistudio.google.com](https://aistudio.google.com). No nó da IA, use *Header Auth* com `Authorization: Bearer SUA_CHAVE`
   - **Telegram:** token do @BotFather
5. **Crie a planilha** com a aba `Vagas` e os cabeçalhos:
   `Data, Titulo, Empresa, Local, Modelo, Salario, Nota, Motivo, Link, Status`
6. **Substitua os marcadores** no fluxo: `COLE_AQUI_O_ID_DA_PLANILHA` e `COLE_AQUI_SEU_CHAT_ID`.
7. **Configure os alertas** de vagas nos sites para chegarem no seu Gmail, teste com um e-mail real e depois publique o fluxo.

## Personalização

- **Perfil do candidato:** fica no prompt, dentro do nó **Preparar texto** (veja `prompt.md`).
- **Nota mínima:** nó **Nota >= 7?**.
- **Modelo de IA:** linha `model:` no nó **Preparar texto**.
- **Remetentes monitorados:** filtro do nó Gmail.

## Limitações conhecidas

- Roda apenas enquanto o n8n estiver ligado (no computador local, o computador precisa estar ligado).
- Não remove vagas repetidas: a mesma vaga pode chegar por alertas de sites diferentes.
- Com o app do Google em modo de teste, a conexão com o Gmail pode precisar ser refeita periodicamente.
- A camada gratuita do Gemini tem limites de uso e pode usar o conteúdo enviado para melhorar produtos do Google. Não envie dados sensíveis.

## Privacidade

Este repositório não contém chaves, IDs de planilha, chat IDs nem dados pessoais. Nunca publique suas credenciais.

## Próximos passos

- Evitar vagas duplicadas (checar o link na planilha antes de registrar)
- Rascunho automático de carta de apresentação
- Lembrete de follow-up após 7 dias
- Resumo da empresa para preparar entrevistas
