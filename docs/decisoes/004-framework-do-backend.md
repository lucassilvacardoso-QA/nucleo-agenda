# 004 – Framework do backend

**Data:** 29/09/2026
**Status:** Aceita

## Contexto
A API será escrita em TypeScript e executada no Node.js. Era preciso
escolher um framework para organizar o código do backend. O projeto tem
dois objetivos: servir de portfólio para vagas de QA com automação e,
no futuro, ser a base de um produto que vai crescer com novos módulos.
As alternativas consideradas foram o Express e o NestJS. O ASP.NET Core
(.NET) também foi lembrado, por ser usado na empresa atual do autor,
mas foi descartado por exigir outra linguagem (C#).

## Decisão
Usar o NestJS como framework do backend.

## Motivo
- **Facilidade de testar:** a injeção de dependência permite trocar peças
  reais por falsas nos testes (por exemplo, testar uma regra de negócio
  sem o banco de dados). É o ganho mais importante para um projeto
  focado em testes.
- **Organização:** a separação entre controller, service e module mantém
  o código organizado desde o início, o que importa num produto que
  vai crescer.
- **Recursos prontos:** validação dos dados de entrada e documentação da
  API (Swagger) quase automáticas.
- **Não perde os fundamentos:** o NestJS roda sobre o Express, então os
  conceitos do Express continuam presentes.
- O Express foi descartado porque deixa a organização, a validação e a
  testabilidade por conta do desenvolvedor, o que é arriscado para quem
  ainda está aprendendo.

## Consequências
- **Positivas:** código organizado e fácil de testar; documentação da API
  sempre atualizada, útil para os testes de API.
- **Negativas:** mais conceitos para aprender no começo (módulos,
  decorators, injeção de dependência); mais "mágica", o que pode
  dificultar entender a causa de um erro; segundo a pesquisa Stack
  Overflow 2025, o NestJS é usado por cerca de 7% dos desenvolvedores
  profissionais, contra cerca de 20% do Express, o que significa menos
  vagas e uma comunidade menor.
- **Mitigação:** para vagas de QA, o framework da API pouco importa; e foi
  sugerido no Backlog recriar uma rota em Express puro, para praticar
  o fundamento diretamente.
