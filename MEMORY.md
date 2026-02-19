# MEMORY.md - Memória de Longo Prazo

## Estratégia de IA e Modelos
- **Modelo Dante:** Prioridade `google/gemini-3-flash-preview` (Switch para PRO em tarefas complexas).
- **Consumo:** Monitorizar TPM/RPM.
- **Protocolo Manus (Execução Contínua):**
  - **Script Ativo:** `dante-wrapper.js` (Monitoriza inatividade e envia pulso de continuação).
  - **Comando de Ativação:** `Start-Process node -ArgumentList 'dante-wrapper.js' -WindowStyle Hidden`.
  - **Objetivo:** Eliminar paragens e garantir fluxo "End-to-End".

## ARQUITETURA DANTE V2.6 - ELITE SALES & AUTONOMIA (16/02/2026)
### 1. Squad de Agentes Especialistas (On-Demand)
- **PixelPerfect:** UI/UX, 3D, Vision Scoring, WebAR.
- **CoreGuardian:** Backend, Segurança, Auto-Healing.
- **Nero:** Hunter Elite (Filtros: Fine Dining, Luxury).

### 2. Inventário Técnico
- **Chaves:** Geridas pelo Guardião (VPS: `/root/.google_api_key`).
- **Limites:** 1M Tokens/min (Monitor via `/tmp/dante_token_usage.json`).

## PROJETO AVILLEZ (CONCLUÍDO - 18/02/2026)
- **Repo:** `projects/restauranteAvilez` (Next.js).
- **Entrega:** Carrossel 3D, menus completos e push oficial para GitHub (`main`).
- **Resultado:** Produto validado pelo Hunter Nero como solução de alta demanda no mercado prime.

## PIPELINE DE VENDAS
- **Pitch Ativo:** Private Luxury Real Estate (Avenida da Liberdade).
- **Próximo Alvo:** Almadrava Lisboa (Vista Panteão).

## EXPANSÃO DE SISTEMA
- **Warm-up Protocol:** Aquecimento de conta WhatsApp via Evolution API.
- **Weekly CVE Scan:** Auditoria semanal de segurança.
- **Draco 3D Pipeline:** Otimização 3D.

## HISTÓRICO CMTecnologia (Legado & Lições)
- **Erro Crítico (10/02/2026):** Sobrescrita do repo `website-mark` por engano. Site restaurado via Vercel. Lição: Nunca push direto para `main` sem validar.
- **Lead Generator PRO:** Workflow n8n ativo.
- **Infraestrutura:** VPS Hostinger [IP_REMOVIDO] + Docker (n8n, Evolution, Supabase).
- **Financeiro:** Conta ativa. Burn rate histórico documentado.

## Configurações de Servidor
- VPS Hostinger [IP_REMOVIDO]
- n8n via Docker (Easypanel)
