# 003 – Horário de funcionamento

**Data:** 29/09/2026
**Status:** Aceita

## Contexto
O sistema precisa saber em quais dias e horas cada recurso pode ser
agendado. Havia duas formas de modelar isso: um horário único para o
estabelecimento inteiro, ou um horário próprio para cada recurso.
Na Fase 1 não existe o conceito de "estabelecimento" no sistema;
o recurso é a peça central do núcleo de agenda.

## Decisão
Cada recurso tem o seu próprio horário de funcionamento, cadastrado pelo
dono do negócio.

## Motivo
- **Reflete a vida real:** numa barbearia, cada profissional pode trabalhar
  em dias diferentes; numa arena, uma quadra pode estar fechada para
  manutenção num dia específico. Um horário único não atenderia esses casos.
- **Combina com o núcleo genérico:** o recurso já é o centro do sistema,
  e o horário de funcionamento "pertence" a ele.
- **Evita criar um conceito novo:** o horário por estabelecimento exigiria
  criar o cadastro de "estabelecimento", que está fora do escopo da Fase 1.

## Consequências
- **Positivas:** o sistema atende negócios com recursos de horários
  diferentes, sem adaptação.
- **Negativas:** mais trabalho de cadastro para o dono quando todos os
  recursos têm o mesmo horário; mais regras de negócio e mais cenários
  de teste (ex.: no mesmo horário, o recurso A aceita agendamento e o
  recurso B recusa, porque está fechado).
- **Mitigação:** foi registrada no Backlog a ideia de "copiar o horário de
  funcionamento de outro recurso", para reduzir o retrabalho do dono.
