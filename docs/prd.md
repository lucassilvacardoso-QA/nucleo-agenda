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

Agendamento de recurso com data e hora, cancelamento de agendamento, cadastro de recurso, inativar recurso, excluir recurso.

#Fora do escopo

Integração com whatsapp, Tela (frontend)

## Regras de negócio 

RN1 - 

## Requisitos não funcionais 

## Dúvidas em aberto

D1 - O Dono do negócio pode não aceitar um agendamento de determinado cliente?
D2 - Como o sistema tratará fuso horários?
D3 - Como identificar um cliente sem login? 
