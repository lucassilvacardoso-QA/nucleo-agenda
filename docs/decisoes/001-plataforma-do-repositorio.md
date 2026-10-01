# 001 – Plataforma do repositório

**Data:** 28/09/2026
**Status:** Aceita

## Contexto
O projeto precisa de uma plataforma para hospedar o código, a documentação
e o quadro de tarefas. O repositório tem dois objetivos: servir de
portfólio público para vagas de QA e, no futuro, ser a base de um produto.
As opções consideradas foram o GitHub e o GitLab. O GitLab é a ferramenta
usada no trabalho atual do autor, o que permitiria praticar no projeto
pessoal a mesma plataforma do dia a dia.

## Decisão
Usar o GitHub como repositório principal do projeto.

## Motivo
- **Visibilidade:** o GitHub é a plataforma onde recrutadores procuram
  portfólios, e o perfil exibe o histórico de atividade do autor.
- **Aprendizado:** como o GitLab já é usado no trabalho, o projeto pessoal
  é a oportunidade de aprender uma ferramenta nova (GitHub Actions,
  GitHub Projects), ampliando o conhecimento em vez de repeti-lo.
- O GitLab foi descartado porque sua principal vantagem (familiaridade)
  pesa menos que a visibilidade, e porque os conceitos (commits, branches,
  revisão de código, pipelines) são os mesmos nas duas plataformas.

## Consequências
- **Positivas:** maior visibilidade para recrutadores; aprendizado de
  GitHub Actions, o que permite dizer em entrevistas que o autor conhece
  as duas plataformas.
- **Negativas:** o autor não pratica o GitLab CI no projeto pessoal, que é
  a ferramenta de pipeline usada no trabalho.
- **Mitigação:** foi criado um card no Backlog para espelhar o repositório
  no GitLab e configurar um pipeline com GitLab CI numa etapa futura.
