# Núcleo de Agenda

API de agendamento de recursos (quadras, profissionais, mesas, boxes de lavagem) construída com **foco em qualidade de software**.

> 🚧 **Projeto em desenvolvimento.** Atualmente na Fase 1: documentação concluída e plano de testes em construção.

## Sobre o projeto

Pequenos negócios ainda controlam horários em cadernos, planilhas e WhatsApp, o que gera agendamentos duplicados e perda de tempo. Este projeto resolve esse problema com uma API de agendamento e, ao mesmo tempo, serve como portfólio de **QA e automação de testes**.

O projeto simula o ciclo completo de um time de produto: requisitos antes do código, decisões técnicas registradas, testes planejados desde o início (*shift-left*) e trabalho organizado em sprints.

## Documentação

| Documento | Descrição |
|---|---|
| [PRD – Documento de Requisitos do Produto](docs/prd.md) | Problema, objetivos, escopo, 27 regras de negócio e requisitos não funcionais |
| [Decisões técnicas (ADRs)](docs/decisoes/) | Registro das decisões de arquitetura, com contexto, motivo e consequências |
| [Plano de testes](docs/plano-de-testes.md) | Estratégia, níveis de teste e casos de teste rastreados às regras *(casos de teste em construção)* |

## Estratégia de testes

O projeto segue a **pirâmide de testes**:

- **Unitários (Vitest):** a lógica das regras de negócio isolada (conflito de horários, limites de duração, validações).
- **API (Playwright):** o comportamento real do sistema, da requisição ao banco de dados.
- **E2E (Playwright):** previstos para a Fase 2, junto com a interface.

Todos os testes serão executados automaticamente a cada *push* no **GitHub Actions**. Cada caso de teste é rastreado até a regra de negócio que ele verifica.

## Stack

TypeScript · Node.js · NestJS · PostgreSQL · Docker · Vitest · Playwright · GitHub Actions

## Roadmap

- [x] **Sprint 0** – Estrutura do repositório e do quadro de tarefas
- [ ] **Fase 1** – API de agendamento com testes unitários, de API e CI
  - [x] PRD
  - [x] Decisões técnicas iniciais
  - [x] Plano de testes
  - [ ] Projeto técnico (rotas da API e banco de dados)
  - [ ] Implementação e testes automatizados
- [ ] **Fase 2** – Interface (frontend) e testes E2E

## Como executar

As instruções de instalação e execução serão adicionadas assim que a primeira versão da API estiver disponível.

## Autor

**Lucas da Silva Cardoso** – Tech Lead de QA
[LinkedIn](https://www.linkedin.com/in/lucas-cardoso-a256a01a6)
