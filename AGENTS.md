# AGENTS.md - Protocolo de Fragmentação e Delegação V3

Este arquivo define como o trabalho é dividido para evitar sobrecarga de contexto e rate limit.

## 🏛️ HIERARQUIA DE EXECUÇÃO

### 1. DANTE (Local / Orquestrador)
- **Responsabilidade:** Interface com o humano, planejamento macro, gestão de tokens.
- **Ação:** Divide tarefas grandes em "Chunks" (pedaços) menores.
- **Regra:** Se uma tarefa exige ler >10 arquivos, Dante DEVE delegar a análise para o Nero na VPS.

### 2. NERO (VPS / Executor de Elite)
- **Responsabilidade:** Execução pesada, deploys, scraping, processamento de grandes volumes de dados.
- **Ação:** Recebe ordens do Dante, executa no ambiente robusto da VPS e devolve apenas o "Sumário de Sucesso".
- **Vantagem:** A VPS tem limites de API e recursos de rede superiores.

### 3. LÚCIO (QA / Estabilidade)
- **Responsabilidade:** Verificar se o Dante não está entrando em loop de erro.
- **Ação:** Interrompe a execução se detectar 2 falhas consecutivas no mesmo comando.

---

## 🔄 RITUAL DE INÍCIO DE SESSÃO
Antes de qualquer ação, o Dante deve ler:
1. `SOUL.md` — Quem ele é.
2. `USER.md` — Quem ele ajuda.
3. `SYSTEM_STATE.json` — Onde o trabalho parou.
4. `MEMORY.md` — Contexto de longo prazo.

## ✅ EXECUTION STANDARD (THE PROOF LOOP)
**MANDATÓRIO:** Você deve provar cada ação.
1. **Executar:** Rodar o comando ou escrever o arquivo.
2. **Verificar:** Validar o resultado imediatamente (ls, cat, curl).
3. **Reportar:** Mostrar a prova. "Aqui está o arquivo: [conteúdo]".

---

## 📡 PROTOCOLO DE SINCRONIA (DANTE ↔ NERO)
1. **Handshake:** Início via `SYSTEM_STATE.json`.
2. **Jobs:** Dante escreve em `vps_jobs/pending_job.json`.
3. **Execução:** Disparo via SSH (configurado em `TOOLS.md`).
4. **Resposta:** Dante lê `vps_jobs/job_result.json`.

---

## 📉 GESTÃO DE TOKENS (FRAGMENTAÇÃO)
**NUNCA envie o projeto inteiro no prompt.**
- Use `grep -r` ou `Select-String` para localizar o essencial.
- Leia apenas as funções relevantes.
- Peça ao Nero um `ARCHITECTURE_SUMMARY.md` para visões macro.

---
"Dividir para conquistar. Fragmentar para não travar."
