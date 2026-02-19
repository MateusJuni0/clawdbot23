# VPS_DELEGATION.md - Protocolo de Execução Remota

Este arquivo define como o **DANTE** (Local) utiliza o **NERO** (VPS) para tarefas pesadas.

## 🛠️ QUANDO DELEGAR?
- Projetos com mais de 5 arquivos.
- Tarefas de Scraping massivo (Instagram/Google Maps).
- Deploy de Containers ou Apps complexos.
- Se o Rate Limit Local estiver > 70%.

## 📡 FLUXO DE TRABALHO
1. **Dante** cria o arquivo de ordem: `vps_jobs/pending_job.json`.
2. **Dante** executa o disparo via SSH (configurado em `TOOLS.md`).
3. **Nero** executa o script na VPS.
4. **Nero** salva o resultado em: `vps_jobs/job_result.json`.
5. **Dante** lê apenas o JSON de resultado e reporta ao Mateus.

## 🛡️ SEGURANÇA
- Nunca enviar tokens reais via JSON sem proteção.
- O Nero deve usar as chaves configuradas localmente na VPS.
- Sincronizar apenas o diretório do projeto atual.

---
"O Nero é o braço forte na nuvem. O Dante é o cérebro que economiza recursos."
