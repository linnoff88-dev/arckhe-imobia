ï»¿# imobia ê¢â¬â imobiliêÂ¡ria no automêÂ¡tico

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![imobia](guia/assets/banner.jpg)](https://inematds.github.io/imobia/guia/)

**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

> **Status:** proposta em discussêÂ£o. O sistema ainda nêÂ£o foi construêÂ­do.

## O que êÂ©

O imobia êÂ© o plano de um sistema prêÂ³prio de atendimento para uma imobiliêÂ¡ria de venda e locaêÂ§êÂ£o com 5 corretores. O cliente manda mensagem no WhatsApp e uma inteligêÂªncia artificial responde na hora, entende o que ele procura e mostra sêÂ³ imêÂ³veis que existem no cadastro. Quando chega a hora de visitar ou negociar, a conversa passa para o corretor, que responde pelo Telegram. NêÂ£o tem formulêÂ¡rio para preencher: o que pode ser automêÂ¡tico, êÂ©.

## ê°Å¸ââ PêÂ¡gina do projeto

Landing com o plano: **https://inematds.github.io/imobia/guia/**

## Documentos

- [`docs/plano-arquitetura.md`](docs/plano-arquitetura.md): arquitetura proposta, componentes, dados, riscos e fases.
- [`docs/discussao-2026-10-13.md`](docs/discussao-2026-10-13.md): registro da conversa que originou o projeto.
- [`docs/pesquisa-2026-10-13.md`](docs/pesquisa-2026-10-13.md): referêÂªncias analisadas (imob360, wacrm, EspoCRM, Chatwoot, Twenty, Evolution API).

## Resumo da arquitetura

```
Cliente (WhatsApp) ê¢â â Evolution API ê¢â â App imobia (IA + match + agenda) ê¢â â Postgres
                                          ê¢â â
                         Bot do Telegram (equipe) êÂ· Painel web (sêÂ³ leitura)
```

## DecisêÂ£o em aberto

Como a equipe usa o Telegram: **A)** um têÂ³pico por cliente num grupo (recomendado), **B)** privado por corretor, **C)** misto.

---
[INEMA.CLUB](https://inema.club)

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>