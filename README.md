# imobia â€” imobiliÃ¡ria no automÃ¡tico

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![imobia](guia/assets/banner.jpg)](https://inematds.github.io/imobia/guia/)

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

> **Status:** proposta em discussÃ£o. O sistema ainda nÃ£o foi construÃ­do.

## O que Ã©

O imobia Ã© o plano de um sistema prÃ³prio de atendimento para uma imobiliÃ¡ria de venda e locaÃ§Ã£o com 5 corretores. O cliente manda mensagem no WhatsApp e uma inteligÃªncia artificial responde na hora, entende o que ele procura e mostra sÃ³ imÃ³veis que existem no cadastro. Quando chega a hora de visitar ou negociar, a conversa passa para o corretor, que responde pelo Telegram. NÃ£o tem formulÃ¡rio para preencher: o que pode ser automÃ¡tico, Ã©.

## ðŸ“– PÃ¡gina do projeto

Landing com o plano: **https://inematds.github.io/imobia/guia/**

## Documentos

- [`docs/plano-arquitetura.md`](docs/plano-arquitetura.md): arquitetura proposta, componentes, dados, riscos e fases.
- [`docs/discussao-2026-10-13.md`](docs/discussao-2026-10-13.md): registro da conversa que originou o projeto.
- [`docs/pesquisa-2026-10-13.md`](docs/pesquisa-2026-10-13.md): referÃªncias analisadas (imob360, wacrm, EspoCRM, Chatwoot, Twenty, Evolution API).

## Resumo da arquitetura

```
Cliente (WhatsApp) â†’ Evolution API â†’ App imobia (IA + match + agenda) â†’ Postgres
                                          â†“
                         Bot do Telegram (equipe) Â· Painel web (sÃ³ leitura)
```

## DecisÃ£o em aberto

Como a equipe usa o Telegram: **A)** um tÃ³pico por cliente num grupo (recomendado), **B)** privado por corretor, **C)** misto.

---
[INEMA.CLUB](https://inema.club)

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>