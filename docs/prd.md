# PRD – Núcleo de Agenda (Fase 1)

**Status:** Aprovado
**Última atualização:** 05/10/2026

## Problema

Pequenos comerciantes anotam horários em planilhas, WhatsApp ou cadernos. Isso pode causar duplicidade de agendamento, além de consumir tempo do dono do negócio e impedir que o cliente agende fora do horário comercial.

## Objetivos

**Primeiro – Portfólio de testes:** criar um software de agendamentos simples que demonstre conhecimentos em testes de software, com:
- Testes unitários com Vitest
- Testes de integração (API) com Playwright
- Testes de ponta a ponta (E2E) com Playwright
- Testes rodando automaticamente no CI (GitHub Actions)

**Segundo – Produto:** ter pelo menos 2 clientes pagando pelo software até 31/01/2027.

## Usuários

Usuários dos "dois lados":

**1 – Dono do negócio**
- 1.1 – Confere agendamentos por período
- 1.2 – Cadastra e edita um recurso
- 1.3 – Cadastra e edita o horário de funcionamento de um recurso
- 1.4 – Cancela um agendamento
- 1.5 – Inativa, ativa e exclui um recurso

**2 – Cliente**
- 2.1 – Consulta horários disponíveis
- 2.2 – Realiza agendamento
- 2.3 – Consulta agendamento realizado
- 2.4 – Cancela agendamento

Na Fase 1 não há tela; a API será usada por meio de testes automatizados e ferramentas de requisição, simulando as ações dos dois usuários.

## Glossário

- **Recurso:** o que será agendado; pode ser um profissional, uma mesa, uma quadra, um box de lavagem. Contém: nome, descrição (opcional) e situação (ativo ou inativo).
- **Agendamento:** reserva feita por um cliente de um intervalo de tempo para utilizar um recurso. Contém: recurso agendado, cliente que agendou (nome e telefone), data, horário de início, duração e situação (ativo ou cancelado).
- **Dono do negócio:** o comerciante, responsável pelo cadastro dos recursos e dos horários de funcionamento.
- **Cliente:** pessoa que deseja agendar um recurso.
- **Horário de funcionamento:** período em que um recurso pode ser agendado, definido por dia da semana e cadastrado pelo dono do negócio.
- **Horário disponível:** horário sem agendamento ativo para aquele recurso, dentro do horário de funcionamento cadastrado pelo dono do negócio.

## Escopo

### Recursos
- Cadastrar recurso
- Editar recurso
- Inativar recurso
- Ativar recurso
- Excluir recurso

### Horário de funcionamento
- Cadastrar horário de funcionamento de um recurso
- Editar horário de funcionamento de um recurso

### Agendamentos
- Consultar horários disponíveis de um recurso
- Realizar agendamento
- Consultar agendamento realizado
- Cancelar agendamento
- Consultar agendamentos de clientes por período

### Fora do escopo
Itens sem fase definida serão avaliados para as próximas fases.
- Cadastro de usuário (login)
- Preço de um agendamento
- Agendamentos em lote
- Agendamento recorrente
- Lembrete de agendamento
- Integração com Google Agenda
- Agendar por WhatsApp
- Confirmar por WhatsApp
- Interface (frontend) – Fase 2
- Copiar horário de funcionamento de outro recurso
- Pausa dentro do horário de funcionamento (ex.: intervalo de almoço)
- Bloqueio de clientes pelo dono do negócio (ver D1)
- Fuso horário por recurso (ver D2)
- Confirmação de identidade do cliente por código (ver D3)
- Atender vários negócios no mesmo sistema, cada um vendo apenas os próprios dados (multi-tenant)
- Módulos por nicho

## Regras de negócio

### Recursos

- **RN01 – Nome de recurso único:** O sistema não deve permitir o cadastro ou a edição de um recurso com nome igual ao de outro recurso já cadastrado (ativo ou inativo). A comparação não diferencia maiúsculas e minúsculas, ignora espaços no início e no fim e considera espaços repetidos entre as palavras como um só.

- **RN02 – Dados do recurso:** O sistema deve exigir o nome do recurso, com 2 a 60 caracteres (contados após remover os espaços do início e do fim). A descrição é opcional e, quando informada, deve ter no máximo 200 caracteres.

- **RN03 – Situação inicial do recurso:** Todo recurso deve ser cadastrado com a situação ativo.

- **RN04 – Edição com agendamentos:** O sistema deve permitir a edição do nome e da descrição de um recurso, mesmo que ele possua agendamentos futuros ativos.

- **RN05 – Mudança para a mesma situação:** O sistema não deve permitir inativar um recurso que já está inativo, nem ativar um recurso que já está ativo, informando a situação atual.

- **RN06 – Inativação com agendamentos futuros:** O sistema não deve permitir a inativação de um recurso que possua agendamentos futuros ativos. Nesse caso, deve informar ao dono do negócio quais agendamentos precisam ser cancelados antes. *(origem: D4)*

- **RN07 – Exclusão de recurso:** O sistema só deve permitir a exclusão de um recurso que nunca teve nenhum agendamento (ativo ou cancelado). Nos demais casos, a exclusão deve ser recusada, orientando o dono do negócio a inativar o recurso. *(origem: D5)*

### Horário de funcionamento

- **RN08 – Formato do horário de funcionamento:** O horário de funcionamento deve ser definido por dia da semana, com um único intervalo (horário de início e de fim) ou com o dia marcado como fechado. Os horários de início e de fim devem ser múltiplos de 30 minutos (ex.: 08:00, 08:30).

- **RN09 – Início antes do fim:** O sistema não deve aceitar um horário de funcionamento em que o horário de início seja igual ou posterior ao horário de fim.

- **RN10 – Edição do horário com agendamentos:** O sistema não deve permitir uma edição do horário de funcionamento que deixe agendamentos futuros ativos fora do novo horário. Nesse caso, deve informar ao dono do negócio quais agendamentos precisam ser cancelados antes.

- **RN11 – Recurso sem horário de funcionamento:** O sistema não deve aceitar agendamentos para um recurso sem horário de funcionamento cadastrado.

### Agendamentos

- **RN12 – Fuso horário:** Todos os horários informados ao sistema e devolvidos por ele devem ser interpretados no horário de Brasília. *(origem: D2)*

- **RN13 – Confirmação automática:** Todo agendamento que cumpre as regras de negócio deve ser registrado com a situação ativo, sem necessidade de aprovação do dono do negócio. *(origem: D1)*

- **RN14 – Recurso ativo:** O sistema não deve aceitar agendamentos para um recurso inativo.

- **RN15 – Dentro do horário de funcionamento:** O sistema não deve aceitar um agendamento que comece ou termine fora do horário de funcionamento do recurso naquele dia da semana, nem em um dia marcado como fechado.

- **RN16 – Duração:** O agendamento deve ter duração mínima de 30 minutos e máxima de 4 horas.

- **RN17 – Horários em múltiplos de 30 minutos:** O início e o fim do agendamento devem ser múltiplos de 30 minutos (ex.: 10:00, 10:30).

- **RN18 – Antecedência:** O sistema não deve aceitar agendamentos com início anterior ao momento atual, nem com início mais de 30 dias após o momento atual.

- **RN19 – Conflito de horário:** O sistema não deve aceitar um agendamento que se sobreponha a outro agendamento ativo do mesmo recurso. Um agendamento que termina em um horário e outro que começa nesse mesmo horário não estão em conflito (ex.: 10:00–11:00 e 11:00–12:00).

- **RN20 – Dados do cliente:** O sistema deve exigir o nome do cliente, com 2 a 100 caracteres, e um telefone brasileiro com DDD, com 10 ou 11 dígitos (considerando apenas os números).

- **RN21 – Limite por cliente:** O sistema não deve permitir que um mesmo telefone tenha mais de 2 agendamentos futuros ativos no mesmo recurso.

- **RN22 – Identificação do cliente:** Para consultar ou cancelar um agendamento, o cliente deve informar o telefone usado no agendamento. *(origem: D3)*

- **RN23 – Prazo de cancelamento pelo cliente:** O cliente só pode cancelar um agendamento com, no mínimo, 2 horas de antecedência em relação ao início.

- **RN24 – Cancelamento pelo dono do negócio:** O dono do negócio pode cancelar um agendamento a qualquer momento antes do início.

- **RN25 – Cancelamento inválido:** O sistema não deve permitir o cancelamento de um agendamento já cancelado, nem de um agendamento cujo início já tenha passado.

- **RN26 – Horários disponíveis:** A consulta de horários disponíveis deve devolver apenas os horários de início, em intervalos de 30 minutos, que respeitem as regras RN11, RN14, RN15, RN18 e RN19.

- **RN27 – Consulta por período:** Na consulta de agendamentos por período, a data inicial deve ser anterior ou igual à data final, e o período deve ter no máximo 31 dias.

## Requisitos não funcionais

- **RNF01 – Desempenho:** 95% das requisições à API devem ser respondidas em até 500 ms, em ambiente local.
- **RNF02 – Qualidade:** todas as regras de negócio devem ser cobertas por testes automatizados, executados a cada push no GitHub Actions.
- **RNF03 – Mensagens de erro:** toda recusa deve retornar uma mensagem clara, em português, indicando qual regra foi violada.
- **RNF04 – Portabilidade:** o projeto deve subir com um único comando (Docker Compose), seguindo as instruções do README.
- **RNF05 – Privacidade (LGPD):** o sistema deve coletar apenas o nome e o telefone do cliente, sem nenhum outro dado pessoal.
- **RNF06 – Armazenamento de datas:** as datas e horários devem ser armazenados em UTC e convertidos para o horário de Brasília apenas na entrada e na saída. *(origem: D2)*

## Dúvidas em aberto

**D1 – O dono do negócio pode recusar o agendamento de determinado cliente?**
- **Por que importa:** define se o agendamento é confirmado na hora ou se passa por aprovação. Afeta as situações possíveis de um agendamento, o fluxo do cliente e os cenários de teste.
- **Opções:**
  - **Não:** todo agendamento dentro das regras é confirmado automaticamente. Mais simples e mais rápido para o cliente.
  - **Sim, por bloqueio:** o dono mantém uma lista de clientes bloqueados (pelo telefone), que não conseguem agendar.
  - **Sim, por aprovação:** todo agendamento nasce "pendente" e o dono aprova ou recusa. Cria uma nova situação além de ativo e cancelado.
- **Decisão:** "Não" na Fase 1. Todo agendamento que cumpre as regras é confirmado automaticamente. O bloqueio de clientes fica fora do escopo.
- **Status:** Resolvida → RN13.

**D2 – Como o sistema trata fuso horário?**
- **Por que importa:** o Brasil tem mais de um fuso, e o servidor onde o sistema roda pode estar em outro fuso. Sem uma regra, um agendamento das 19h pode ser gravado ou exibido em horário errado. É uma das maiores fontes de bugs em sistemas de agenda.
- **Opções:**
  - **Fuso único:** a Fase 1 considera apenas o horário de Brasília. Simples, mas não atende negócios em outros fusos.
  - **Fuso por recurso:** cada recurso informa o seu fuso, e o sistema converte. Completo, porém mais complexo.
  - **Em ambos os casos:** guardar as datas no banco em UTC (o horário padrão mundial) e converter só na entrada e na saída, prática comum no mercado.
- **Decisão:** fuso único (horário de Brasília) na Fase 1, com as datas guardadas em UTC, para que a mudança futura seja simples.
- **Status:** Resolvida → RN12 e RNF06.

**D3 – Como identificar um cliente sem login?**
- **Por que importa:** o cliente precisa consultar e cancelar o próprio agendamento, mas na Fase 1 não há login. A forma de identificação define os campos do agendamento, quem consegue cancelar o agendamento de quem, quais dados pessoais o sistema guarda (LGPD) e os cenários de teste.
- **Opções consideradas:**
  - **CPF:** único por pessoa, mas é um dado pessoal forte, pode ser considerado excessivo pela LGPD (princípio da necessidade) e gera atrito para o cliente.
  - **Telefone:** já é coletado, o cliente sabe de cabeça e combina com a futura integração com WhatsApp, mas não comprova que quem consulta é o dono do número.
  - **Código do agendamento:** não exige dado extra, mas o cliente pode perder o código.
  - **Código + telefone:** mais seguro, com um passo a mais para o cliente.
- **Decisão:** identificar o cliente pelo **telefone**, por ser a opção mais simples e não exigir coleta de dados adicionais.
- **Risco aceito:** qualquer pessoa que conheça o telefone de um cliente pode consultar e cancelar os agendamentos dele; clientes que compartilham o mesmo número (ex.: família) podem cancelar o horário um do outro. O risco é aceitável na Fase 1, que não tem clientes reais. Deve ser revisto **antes da venda para o primeiro cliente**, por exemplo com confirmação por código enviado ao telefone.
- **Status:** Resolvida → RN22.

**D4 – O que acontece com os agendamentos futuros de um recurso inativado?**
- **Por que importa:** inativar é temporário (manutenção, férias de um profissional). Se não houver regra, o cliente pode chegar e encontrar o recurso indisponível, ou o sistema pode ficar com agendamentos "órfãos".
- **Opções:**
  - **Impedir a inativação** enquanto houver agendamentos futuros: o dono precisa cancelá-los antes. Seguro, mas trabalhoso.
  - **Cancelar automaticamente** os agendamentos futuros ao inativar. Prático para o dono, ruim para o cliente, que perde o horário sem aviso (lembrete e WhatsApp estão fora do escopo).
  - **Manter os agendamentos existentes** e bloquear apenas os novos. O recurso "sai da vitrine", mas honra o que já foi marcado.
- **Decisão:** impedir a inativação enquanto houver agendamentos futuros ativos. Na Fase 1 não há como avisar o cliente, então é a opção que evita surpresas. O sistema informa ao dono quais agendamentos precisam ser cancelados antes.
- **Status:** Resolvida → RN06.

**D5 – O que acontece com os agendamentos de um recurso excluído? Qual a diferença de uso entre inativar e excluir?**
- **Por que importa:** excluir é definitivo. Se o recurso for apagado do banco, os agendamentos antigos ficam apontando para algo que não existe mais, e o histórico se perde.
- **Opções:**
  - **Exclusão física:** o recurso é apagado do banco. Só seria possível se ele nunca teve nenhum agendamento.
  - **Exclusão lógica (soft delete):** o recurso é marcado como excluído, some de todas as listas e não pode ser reativado, mas continua no banco para preservar o histórico.
  - **Remover a exclusão do escopo** e manter só a inativação.
- **Diferença de uso:** inativar é temporário e reversível (manutenção, férias); excluir é definitivo e serve para recursos cadastrados por engano ou que deixaram de existir.
- **Decisão:** exclusão física apenas para recursos que nunca tiveram nenhum agendamento. Nos demais casos, a exclusão é impedida e o dono é orientado a inativar o recurso.
- **Status:** Resolvida → RN07.
