# imobia — Plano de arquitetura (proposta em discussêo)

Data: 2026-10-13 · Status: **proposta — aguardando decisêo A/B/C do Telegram antes do spec final**

## Objetivo

Sistema próprio, simples e quase todo automático para uma imobiliária de venda e locaçêo com
5 corretores. O cliente fala pelo WhatsApp; uma IA responde 24h, qualifica, mostra imóveis reais e
agenda visita; a equipe acompanha e assume conversas pelo Telegram; o dono vê tudo num painel.
Sem formulários.

## Arquitetura (3 peças)

```
Cliente (WhatsApp)
      │
 Evolution API ──► App "imobia" (Node/TypeScript) ──► Postgres
                     │  • agente de IA com ferramentas
                     │  • motor de match imóvel × cliente
                     │  • agenda de tarefas (follow-up, lembretes)
                     ▼
               Bot do Telegram (equipe)        Painel web (só leitura)
```

Tudo em Docker numa VPS.

## Componentes

1. **Receptor WhatsApp** — webhook `messages.upsert` da Evolution; cria/acha o contato por telefone;
   junta mensagens picadas (espera o cliente terminar de digitar); áudio → transcriçêo local (Whisper).
2. **Agente de IA** — conversa, qualifica (compra/aluguel, tipo, regiêo, faixa de preço, quartos,
   prazo), usa ferramentas: `buscar_imoveis`, `enviar_fotos`, `propor_visita`, `chamar_corretor`.
   Nunca cita imóvel que nêo veio do banco.
3. **Handoff** — estados da conversa: `ia → aguardando corretor → corretor → encerrada`.
   Passa para humano em: pedido de pessoa, negociaçêo/financiamento, reclamaçêo, visita marcada,
   3 mal-entendidos. Envia resumo pronto ao corretor.
4. **Bot do Telegram** — aviso de lead novo com resumo; corretor responde e a mensagem sai no
   WhatsApp; comandos curtos (`/assumir`, `/devolver`, `/imovel`, `/agenda`). Cadastro de imóvel:
   fotos + áudio/texto livre → IA extrai campos → botêo "confirmar".
5. **Match automático** — necessidade do cliente (faixas min/máx) × imóveis. Imóvel novo → avisa
   corretor dos leads antigos compatíveis.
6. **Agenda de tarefas** — lembrete de visita, follow-up de lead parado (limite, horário comercial,
   para no "nêo quero").
7. **Painel** — funil, conversas, imóveis, visitas, números por corretor, custo da IA. Login simples.
   Sem telas de cadastro.

## Dados (inspirado no imob360 + wacrm + EspoCRM)

- `imovel` — código, tipo, finalidade (venda/aluguel/ambos), status, endereço, bairro, área,
  quartos, suítes, vagas, preços (venda, aluguel, condomínio, IPTU), características (JSON), fotos,
  corretor responsável.
- `pessoa` — nome, telefone, papéis (proprietário, comprador, inquilino).
- `lead` — pessoa, finalidade, etapa do funil, temperatura, corretor, origem, próximo follow-up.
- `necessidade` — faixas de preço/área/quartos, bairros, tipos (o que a IA extraiu).
- `match` — lead × imóvel, status (sugerido, enviado, descartado, interessado).
- `visita` — data, status, lembrete enviado, retorno.
- `conversa` / `mensagem` — remetente (cliente, ia, corretor), estado, tópico do Telegram.
- `tarefa` — açêo agendada (`run_at`).
- `uso_ia` — tokens e custo por resposta.

## Decisêo em aberto — Telegram

- **A) Grupo com um tópico por cliente** (recomendado): tudo espelhado, corretor responde no tópico,
  dono vê tudo, troca de corretor trivial. Os 5 veem todos os leads.
- **B) Privado por corretor**: rodízio, cada um só vê os seus; responder exige indicar o cliente.
- **C) Misto**: grupo para supervisêo + privado por corretor; o dobro de peças.

## Riscos e cuidados

- Evolution é nêo oficial → chip dedicado, só responder quem chamou, ritmo humano, opt-out,
  caminho de migraçêo para a API oficial se necessário. Fixar tag Docker (`evoapicloud/evolution-api`).
- Áudio de iPhone com bug em algumas versões da Evolution → fallback pedindo texto.
- Custo de LLM → log por resposta e limite mensal.
- LGPD → aviso de finalidade, "sair", dados mínimos, transcriçêo local, prazo de retençêo.
- Nenhuma chamada de API de IA no desenvolvimento sem autorizaçêo; testes com respostas simuladas.

## Fora do escopo da v1

Instagram, Facebook, site, portais, contratos/aluguéis/comissões, disparo em massa,
construtor de fluxos, várias imobiliárias no mesmo sistema.

## Próximos passos

1. Decidir A/B/C.
2. Spec final (`docs/superpowers/specs/`), revisêo e aprovaçêo.
3. Plano de implementaçêo em fases.
