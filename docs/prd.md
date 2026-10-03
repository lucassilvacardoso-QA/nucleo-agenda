## Problema

Pequenos comerciantes anotam horários em planilhas, whatsapp ou cadernos. Isso pode causar duplicidade de agendamento, além de consumir tempo do dono do negócio e também a falta de atendimento fora do horário comercial

## Objetivo

Primeiro - Testes: 
- Criar um Software de Agendamentos simples para servir de portfólio para meus conhecimentos em testes de software:
- Testes unitários com Vitest
- Testes de integração (API) com Playwright 
- Testes de ponta a ponta (e2e) com Playwright
- Testes rodando automaticamente no CI (GitHub Actions)

Segundo - Produto: 
- Ter pelo menos 2 clientes pagando pelo software até 31/01/2026.

## Usuários

Usuários dos "dois lados":

1 - Dono do negócio
- 1.1 - Confere agendamentos por período
- 1.2 - Cadastra e edita um recurso
- 1.3 - Cadastra horário de funcionamento
- 1.4 - Cancela um agendamento
- 1.5 - Inativa e Exclui um recurso

2 - Cliente
- 2.1 - Consulta horários disponíveis
- 2.2 - Realiza agendamento
- 2.3 - Consulta agendamento realizado
- 2.4 - Cancela agendamento

Na Fase 1 não há tela; a API será usada por meio de testes automatizados e ferramentas de requisição, simulando as ações dos dois usuários.

## Glossário

- Recurso - O que será agendado, pode ser um profissional, mesa, quadra, box de lavagem.
- Agendamento - Reserva feita por um cliente de um intervalo de tempo para utilizar um recurso. Contém: recurso agendado, cliente que agendou, data, horário de início, duração e situação (ativo ou cancelado).
- Dono do negócio - Responsável pelo cadastro do recurso e horário de funcionamento, seria o comerciante
- Cliente - Pessoa que deseja agendar um recurso
- Horário de funcionamento - É o período em que um recurso pode ser agendado, esse período é cadastrado pelo dono do negócio.
- Horário disponível - Horários sem agendamentos realizados para aquele recurso, dentro do horário de funcionamento cadastrado pelo dono do negócio.

## Escopo 

### Cadastro de Recursos
- Cadastrar recurso
- Editar recurso
- Inativar recurso
- Ativar recurso
- Excluir recurso

### Horário de funcionamento
- Cadastrar horário de funcionamento de um recurso
- Editar horário de funcionamento de um recurso

### Agendamentos
- Consultar horário disponível de um recurso
- Realizar agendamento
- Consultar agendamento realizado
- Cancelar agendamento
- Consultar agendamentos de clientes por período

### Fora do escopo
Itens sem fase definida serão avaliados para as próximas fases.
- Cadastro de usuário
- Preço de um agendamento
- Agendamentos em lote
- Agendamento recorrente
- Lembrete de agendamento
- Integração com Google Agenda
- Agendar por WhatsApp
- Confirmar por WhatsApp
- Interface (frontend), Fase 2
- Copiar horário de funcionamento de outro recurso
- Atender vários negócios no mesmo sistema, cada um vendo apenas os próprios dados (multi-tenant)
- Módulos por nicho


## Regras de negócio 

RN1 - 

## Requisitos não funcionais 

## Dúvidas em aberto

**D1 – O dono do negócio pode recusar o agendamento de determinado cliente?**
- **Por que importa:** define se o agendamento é confirmado na hora ou se
  passa por aprovação. Afeta as situações possíveis de um agendamento,
  o fluxo do cliente e os cenários de teste.
- **Opções:**
  - **Não:** todo agendamento dentro das regras é confirmado
    automaticamente. Mais simples e mais rápido para o cliente.
  - **Sim, por bloqueio:** o dono mantém uma lista de clientes bloqueados
    (pelo telefone), que não conseguem agendar.
  - **Sim, por aprovação:** todo agendamento nasce "pendente" e o dono
    aprova ou recusa. Cria uma nova situação além de ativo e cancelado.
- **Decisão:** "Não" na Fase 1. Todo agendamento que cumpre as regras é
  confirmado automaticamente. O bloqueio de clientes fica para o Backlog.
- **Status:** Resolvida → será transformada em regra de negócio.

**D2 – Como o sistema trata fuso horário?**
- **Por que importa:** o Brasil tem mais de um fuso, e o servidor onde o
  sistema roda pode estar em outro fuso. Sem uma regra, um agendamento
  das 19h pode ser gravado ou exibido em horário errado. É uma das
  maiores fontes de bugs em sistemas de agenda.
- **Opções:**
  - **Fuso único:** a Fase 1 considera apenas o horário de Brasília.
    Simples, mas não atende negócios em outros fusos.
  - **Fuso por recurso:** cada recurso informa o seu fuso, e o sistema
    converte. Completo, porém mais complexo.
  - **Em ambos os casos:** guardar as datas no banco em UTC (o horário
    padrão mundial) e converter só na entrada e na saída, prática comum
    no mercado.
- **Decisão:** fuso único (horário de Brasília) na Fase 1, com as datas
  guardadas em UTC, para que a mudança futura seja simples.
- **Status:** Resolvida → será transformada em regra de negócio.

**D3 – Como identificar um cliente sem login?**
- **Por que importa:** o cliente precisa consultar e cancelar o próprio
  agendamento, mas na Fase 1 não há login. A forma de identificação define
  os campos do agendamento, quem consegue cancelar o agendamento de quem,
  quais dados pessoais o sistema guarda (LGPD) e os cenários de teste.
- **Opções consideradas:**
  - **CPF:** único por pessoa, mas é um dado pessoal forte, pode ser
    considerado excessivo pela LGPD (princípio da necessidade) e gera
    atrito para o cliente.
  - **Telefone:** já é coletado, o cliente sabe de cabeça e combina com a
    futura integração com WhatsApp, mas não comprova que quem consulta é
    o dono do número.
  - **Código do agendamento:** não exige dado extra, mas o cliente pode
    perder o código.
  - **Código + telefone:** mais seguro, com um passo a mais para o cliente.
- **Decisão:** identificar o cliente pelo **telefone**, por ser a opção
  mais simples e não exigir coleta de dados adicionais.
- **Risco aceito:** qualquer pessoa que conheça o telefone de um cliente
  pode consultar e cancelar os agendamentos dele; clientes que
  compartilham o mesmo número (ex.: família) podem cancelar o horário um
  do outro. O risco é aceitável na Fase 1, que não tem clientes reais.
  Deve ser revisto **antes da venda para o primeiro cliente**, por
  exemplo com confirmação por código enviado ao telefone.
- **Status:** Resolvida → será transformada em regra de negócio.

**D4 – O que acontece com os agendamentos futuros de um recurso inativado?**
- **Por que importa:** inativar é temporário (manutenção, férias de um
  profissional). Se não houver regra, o cliente pode chegar e encontrar
  o recurso indisponível, ou o sistema pode ficar com agendamentos
  "órfãos".
- **Opções:**
  - **Impedir a inativação** enquanto houver agendamentos futuros: o dono
    precisa cancelá-los antes. Seguro, mas trabalhoso.
  - **Cancelar automaticamente** os agendamentos futuros ao inativar.
    Prático para o dono, ruim para o cliente, que perde o horário sem aviso
    (lembrete e WhatsApp estão fora do escopo).
  - **Manter os agendamentos existentes** e bloquear apenas os novos.
    O recurso "sai da vitrine", mas honra o que já foi marcado.
- **Decisão:** impedir a inativação enquanto houver agendamentos futuros
  ativos. Na Fase 1 não há como avisar o cliente, então é a opção que
  evita surpresas. O sistema informa ao dono quais agendamentos precisam
  ser cancelados antes.
- **Status:** Resolvida → será transformada em regra de negócio.

**D5 – O que acontece com os agendamentos de um recurso excluído? Qual a
diferença de uso entre inativar e excluir?**
- **Por que importa:** excluir é definitivo. Se o recurso for apagado do
  banco, os agendamentos antigos ficam apontando para algo que não existe
  mais, e o histórico se perde.
- **Opções:**
  - **Exclusão física:** o recurso é apagado do banco. Só seria possível
    se ele nunca teve nenhum agendamento.
  - **Exclusão lógica (soft delete):** o recurso é marcado como excluído,
    some de todas as listas e não pode ser reativado, mas continua no banco
    para preservar o histórico.
  - **Remover a exclusão do escopo** e manter só a inativação.
- **Diferença de uso:** inativar é temporário e reversível (manutenção,
  férias); excluir é definitivo e serve para recursos cadastrados por
  engano ou que deixaram de existir.
- **Decisão:** exclusão física apenas para recursos que nunca tiveram
  nenhum agendamento. Nos demais casos, a exclusão é impedida e o dono
  é orientado a inativar o recurso.
- **Status:** Resolvida → será transformada em regra de negócio.

