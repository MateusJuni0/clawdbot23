# Guia de Implementação: Arquitetura Dante Sovereign V3

Este guia resolve os problemas de **Rate Limit**, **Amnésia de Agentes** e **Falta de Delegação para VPS**.

---

## 🛠️ Passo 1: Atualizar o GitHub (clawdbot23)

Suba estes arquivos exatamente como estão (fornecidos no ZIP):

1.  **`IDENTITY.md`** -> Substitua o atual pelo `IDENTITY_V3.md`. (Ensina o Dante a ser submisso e economizar tokens).
2.  **`AGENTS.md`** -> Substitua o atual pelo `AGENTS_V3.md`. (Define o protocolo de delegação para a VPS).
3.  **`SYSTEM_STATE.json`** -> Crie este arquivo na raiz. (Evita que o Dante esqueça o que estava fazendo).
4.  **`config/openclaw.json`** -> Adicione `"contextTokens": 45000` e as configurações de agentes remotos.

---

## ⚙️ Passo 2: Configuração do Dante (O Prompt Mestre)

Após atualizar o GitHub, envie este comando para o Dante:

> "Dante, atualizei a arquitetura para a Versão 3.0 (Sovereign). Leia os novos `IDENTITY.md`, `AGENTS.md` e o `SYSTEM_STATE.json`. A partir de agora, você deve seguir a **Regra dos Dois Erros** e usar o **Nero na VPS** para qualquer tarefa que envolva mais de 5 arquivos ou processamento pesado. Confirme que entendeu o novo protocolo de submissão ao usuário."

---

## 🚀 Passo 3: Como usar a VPS de verdade

O Dante agora sabe que não deve sofrer sozinho. Quando você pedir algo grande:

1.  Ele vai criar um "Job" (arquivo JSON).
2.  Ele vai usar o comando `ssh` (que está no seu `TOOLS.md`) para mandar o Nero executar na VPS.
3.  Ele vai ler apenas o resultado, economizando 80% dos tokens de contexto.

---

## 🛡️ O que muda nos Erros e Rate Limit

- **Se der erro:** Ele para na 2ª tentativa. Ele não vai mais ficar tentando até travar tudo.
- **Se você mandar parar:** Ele para na hora. Sem "mas" ou "só um segundo".
- **Se ele resetar:** A primeira coisa que ele vai ler é o `SYSTEM_STATE.json`, então ele vai saber exatamente em qual projeto estava e qual era o último passo.

---

## 📊 Checklist de Saúde do Sistema

- [ ] `SYSTEM_STATE.json` existe na raiz?
- [ ] `contextTokens` está em 45000 no `openclaw.json`?
- [ ] As pastas duplicadas de skills foram removidas? (Use o script `cleanup_skills.sh` do primeiro ZIP).

---

## 💡 Dica de Ouro: O Modo MINIMAL
Se você perceber que o sistema está lento ou o Gemini está dando muitos erros, diga:
> "Dante, entre em MODO MINIMAL agora."

Ele vai reduzir o contexto para 25k, desligar todas as skills e focar apenas no comando puro, o que limpa o rate limit quase instantaneamente.
