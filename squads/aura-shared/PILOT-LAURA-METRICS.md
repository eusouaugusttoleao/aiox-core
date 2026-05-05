# Piloto Laura — Resultado da Extração de Skills

**Branch:** `AIOSSQUAD-AURA-MAI2026`
**Data:** 2026-05-04
**Workflow original:** `02-laura-conversation.json` (intocado)
**Workflow piloto:** `02-laura-conversation-v15-otimizado.json`

## Economia validada (medida, não estimada)

| Estágio | V14 tokens | V15 tokens | Δ tokens | Δ % |
|--------|------------:|------------:|----------:|------:|
| abertura | 783 | 529 | -254 | **-32.5%** |
| diagnostico | 783 | 530 | -253 | -32.3% |
| espelho | 784 | 528 | -256 | -32.6% |
| qualificacao | 783 | 687 | -96 | -12.2% |
| convite | 781 | 689 | -92 | -11.8% |
| agendamento | 772 | 678 | -94 | -12.2% |
| nurturing | 779 | 531 | -248 | -31.9% |
| fora_icp | 782 | 534 | -248 | -31.7% |
| **Média** | **781** | **588** | **-193** | **-24.7%** |

## O que mudou

1. **Skills extraídas** para `/squads/aura-shared/skills/`:
   - `aura-tone-voice.md`
   - `aura-identity-signature.md`
   - `aura-canal-rules.md`
   - `hormozi-objection-handler.md` ← **carrega só em qualificacao/convite/agendamento**
   - `aura-journey-12w.md`

2. **System-prompt da Laura** ficou enxuto: apenas o que é único da função (espelho estratégico, contexto de qualificação Hormozi, instrução de estágio).

3. **Carregamento condicional** de objection-handler: estágios iniciais (abertura, diagnostico, espelho) e finais (nurturing, fora_icp) **não carregam** a tabela de objeções → -32%.

## Como testar (Franklin)

1. Importar `02-laura-conversation-v15-otimizado.json` no n8n cloud (manter o WF-02 atual ativo).
2. Configurar o webhook do v15 num path diferente (ex: `whatsapp-webhook-v15`).
3. Rodar 5-10 conversas reais com leads de teste.
4. Comparar:
   - **Comportamento:** mesmas respostas com mesmo tom?
   - **Tags:** continuam sendo emitidas corretamente?
   - **Custo:** OpenAI dashboard mostra a redução?
5. Se OK → repetir extração de skills nos outros 10 agentes na próxima branch.

## Riscos identificados

- **Texto compactado pode perder nuance:** monitorar se as respostas viraram "robóticas demais". Se sim, restaurar 1-2 frases por skill.
- **TAGS de controle:** a versão v15 usa formato compacto `[STAGE:DIAGNOSTICO]` em vez de listar todas com descrição. Verificar se o classificador downstream ainda parseia.
- **Objections só em 3 estágios:** se o lead trouxer objeção em estágio "abertura" (raro mas possível), Laura vai responder sem o reframe Hormozi. Mitigação: detector de objeção no node anterior pode forçar o estágio pra `qualificacao`.

## Próxima branch (sugestão)

`AIOSSQUAD-AURA-JUN2026` — replicar extração nos outros 10 agentes:

| Agente | Estimativa de economia (com base no piloto) |
|--------|---------------------------------------------|
| Salomão | -28% (system-prompt enorme com 12 semanas + 4 atos crise) |
| ErickGrana | -22% (já é mais focado) |
| PedroSup | -25% |
| Cris | -30% |
| BonnerText | -25% |
| Outros 5 | -20 a -30% |

**Economia projetada total (somando 11 agentes):** ~-25% em tokens estáticos por mensagem, todos os agentes.
