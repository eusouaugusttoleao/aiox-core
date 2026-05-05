# aura-shared — Skills compartilhadas pelos agentes AURA

Pasta de skills reutilizadas pelos 11 agentes de produção AURA (Laura, Salomão, ErickGrana, PedroSup, Luciana, Jubsuporte, Cris, BonnerText, CaioDesigner, Maestro, Leiloca).

## Por que existe

Antes desta extração, cada um dos 11 system-prompts repetia internamente:
- Regra de assinatura/identificação
- Estrutura da Jornada de 12 semanas
- Tom de voz / vocabulário oficial
- Tabela de objeções Hormozi
- Regras do canal único WhatsApp

Resultado: **~1k tokens repetidos por agente em todas as chamadas OpenAI** = custo recorrente alto e drift entre agentes (cada um evoluindo seu próprio "tom").

## Skills disponíveis

| Skill | Tokens | Quando carregar |
|-------|--------|-----------------|
| [aura-identity-signature](skills/aura-identity-signature.md) | ~180 | Sempre que o agente vai gerar mensagem (texto ou áudio) |
| [aura-journey-12w](skills/aura-journey-12w.md) | ~220 | Quando conteúdo se referir a aulas, blocos ou ferramentas |
| [aura-tone-voice](skills/aura-tone-voice.md) | ~200 | Sempre — define vocabulário oficial e proibido |
| [hormozi-objection-handler](skills/hormozi-objection-handler.md) | ~250 | Estágios: qualificação, convite, fechamento, retenção |
| [aura-canal-rules](skills/aura-canal-rules.md) | ~200 | Sempre — define escalação humana e emergência |

## Como integrar (n8n)

Há 3 padrões de integração possíveis:

### Padrão A — Inline mínimo (carrega apenas as skills relevantes ao estágio)
Recomendado para **Laura** (piloto). O node `System Prompt + Stage` decide por estágio quais skills carregar inline. Veja `02-laura-conversation-v15-otimizado.json`.

### Padrão B — Lookup em Sheets (uma aba `skills` com 5 linhas)
Cada workflow lê só as skills necessárias. Mais flexível, mas adiciona latência (1 request Sheets a mais por mensagem).

### Padrão C — Concatenação total (carrega todas as 5 sempre)
Mais simples; perde-se a economia de tokens. **Não recomendado.**

## Atualização

Quando uma skill mudar, **promover via nova branch** `AIOSSQUAD-AURA-{MES}{ANO}` no fork `eusouaugusttoleao/aiox-core`. Franklin valida + aplica nos workflows n8n manualmente.
