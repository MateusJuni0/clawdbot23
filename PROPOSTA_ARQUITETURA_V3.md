# Proposta de Arquitetura V3 - Dante "The Sovereign"

Esta arquitetura foca em **estabilidade**, **delegação real para VPS** e **proteção contra Rate Limit**.

---

## 1. Nova Estrutura de Arquivos Críticos

### A. `SYSTEM_STATE.json` (O Cérebro de Curto Prazo)
Este arquivo deve ser atualizado pelo Dante após CADA ação bem-sucedida ou mudança de rumo.
```json
{
  "current_mode": "SIMPLE",
  "active_project": "nome-do-projeto",
  "last_checkpoint": "step-2-ui-design",
  "vps_status": "connected",
  "rate_limit_usage": {
    "tpm_estimate": 450000,
    "last_request_time": "2026-02-19T10:30:00Z"
  },
  "interrupted_by_user": false
}
```

### B. `VPS_DELEGATION.md` (O Protocolo de Comunicação)
Define como o Dante (Local) fala com o Nero (VPS).
- **Regra:** Tarefas que envolvem mais de 5 arquivos ou deploys complexos DEVEM ser enviadas para a VPS via script de sincronização.
- **Fluxo:** Dante cria `vps_jobs/job_001.json` -> Dante executa `sync-to-vps.sh` -> Nero executa na VPS -> Dante lê o resultado.

---

## 2. Protocolos de Segurança Anti-Rate-Limit (Nível 2)

### Protocolo "Two-Strike Rule"
1. Se um comando (ex: `git push`) falhar uma vez: Tentar corrigir o erro óbvio.
2. Se falhar a segunda vez: **PARAR IMEDIATAMENTE**.
3. Não tentar a terceira vez. Reportar ao usuário: "Falha persistente no comando X. Parando para evitar Rate Limit. Sugestão: [Ação Manual]".

### Protocolo "Context Pruning" (Poda de Contexto)
- Antes de cada chamada, o Dante deve verificar o tamanho do histórico.
- Se o histórico de mensagens + arquivos lidos > 40k tokens:
  - O Dante deve criar um resumo da sessão atual em `memory/session_summary.md`.
  - Limpar o histórico interno (se a ferramenta permitir) ou focar apenas no resumo.

---

## 3. Integração Real com VPS (Nero)

Para que o Dante não esqueça os agentes, a configuração deve estar no `config/openclaw.json` e não apenas em arquivos Markdown.

### Ajuste no `config/openclaw.json`:
```json
"custom_agents": {
  "nero": {
    "type": "remote",
    "host": "vps-ip",
    "role": "executor",
    "enabled": true
  }
}
```

---

## 4. Melhoria na Memória Automática

O Dante deve ter uma "Skill de Curadoria" que roda a cada 10 mensagens:
1. Analisa o que foi feito.
2. Extrai lições aprendidas (ex: "O comando X não funciona nesta VPS").
3. Atualiza `MEMORY.md` e limpa logs temporários.

---

## 5. O "Mestre do Prompt" (Instrução de Identidade)

O novo `IDENTITY.md` deve conter esta cláusula de submissão:
> **PRIORIDADE DE COMANDO:** Se o usuário disser "PARE", "MUDE" ou "ESQUEÇA ISSO", você deve abortar qualquer loop de execução atual imediatamente. O desejo do usuário sobrepõe qualquer "Execution Standard" ou "Proof Loop".

---

## 6. Próximos Passos para o Usuário

Vou preparar os arquivos exatos para você subir no GitHub `clawdbot23`:
1. `IDENTITY.md` (Versão 3.0 - Com foco em submissão e economia)
2. `AGENTS.md` (Versão 3.0 - Com protocolo de delegação VPS)
3. `SYSTEM_STATE.json` (Template inicial)
4. `config/openclaw.json` (Otimizado para 45k-50k tokens e segurança)
