# Diagnóstico de Arquitetura - Dante OpenClaw (clawdbot23)

## 1. Falhas Críticas Identificadas

### A. O Paradoxo do Rate Limit (TPM vs. Contexto)
- **Problema:** O Dante está configurado para usar `google/gemini-3-flash-preview` com um limite de 900k TPM (Tokens Per Minute). Em projetos grandes, ele lê múltiplos arquivos sequencialmente.
- **Causa:** Cada leitura de arquivo (mesmo que pequeno) envia o contexto inteiro de volta. Se o contexto está em 60k, 15 requisições em um minuto (comum em loops de erro) somam 900k tokens, batendo no limite.
- **Sintoma:** O Dante entra em "cooldown" ou trava nos comandos do GitHub porque o rate limit impede a conclusão da operação, gerando erros que ele tenta corrigir, criando um loop infinito de consumo de tokens.

### B. Fragmentação Inexistente (VPS vs. Local)
- **Problema:** O usuário relata que o Dante não usa os agentes da VPS e esquece as configurações a cada reset.
- **Causa:** O arquivo `AGENTS.md` define a hierarquia (Dante, Nero, Lúcio), mas não há um **mecanismo de transporte de estado** ou **protocolo de chamada remota** funcional. O Dante tenta fazer tudo localmente (na máquina do usuário) em vez de delegar para o Nero na VPS.
- **Sintoma:** Sobrecarga de contexto local e incapacidade de realizar tarefas pesadas que deveriam estar na infraestrutura robusta.

### C. Amnésia de Sessão
- **Problema:** "A cada conversa que reseto ele esquece dos agentes e tenho que configurar tudo do zero".
- **Causa:** O Dante depende de ler `AGENTS.md`, `SOUL.md`, etc., no início de cada sessão. Se ele bate no rate limit logo no início, ele não consegue ler esses arquivos e "perde a identidade". Além disso, não há um arquivo de `SESSION_STATE.json` que persista o status atual do projeto entre resets.

### D. Loop de Erro e Teimosia
- **Problema:** "Digo pra gente parar de fazer uma coisa e ir fazer outra ele ignora".
- **Causa:** O System Prompt ou as instruções em `IDENTITY.md` não dão prioridade suficiente ao "Interrupt" do usuário. O Dante fica preso no "Execution Standard" (Proof Loop) definido no `AGENTS.md`, tentando provar algo que está dando erro.

---

## 2. Análise dos Arquivos Atuais

- **AGENTS.md:** Define bem a hierarquia, mas o "Execution Standard" é muito rígido e induz ao consumo excessivo de tokens em caso de erro.
- **TOOLS.md:** Contém informações sensíveis e limites de modelos, mas o Dante não parece estar respeitando os "Limites Seguros" definidos (900k TPM para Flash).
- **MEMORY.md:** Está muito focado em "vitórias" e pouco em "estado atual do sistema".

---

## 3. Plano de Re-Arquitetura (Visão Geral)

1.  **Protocolo de Delegação Real (Dante -> Nero):** Criar um sistema onde o Dante escreve um "Job" em um arquivo e o Nero (na VPS) executa, devolvendo apenas o resultado.
2.  **Gestão de Contexto Dinâmica:** Implementar um "Context Budget" onde o Dante deve resumir o histórico se ultrapassar X tokens.
3.  **Persistência de Estado de Projeto:** Criar um `CURRENT_PROJECT.json` que é a primeira coisa lida em qualquer sessão, contendo o status exato de onde parou.
4.  **Mecanismo de "Kill Switch" e Escalação:** Se um comando falhar 2 vezes, o Dante DEVE parar e pedir ajuda, em vez de tentar uma 3ª vez e bater no rate limit.
5.  **Limpeza de Identidade:** Consolidar os arquivos de identidade para reduzir o "overhead" inicial de leitura.
