# Plano de Testes – Núcleo de Agenda (Fase 1)

## 1. Objetivo

Garantir que a API da Fase 1 cumpre todas as regras de negócio (RN01 a RN27)
e os requisitos não funcionais (RNF01 a RNF06) definidos no PRD, antes de
cada entrega.

As regras de negócio e os RNFs mensuráveis (desempenho e armazenamento de
datas) serão verificados por testes automatizados executados no CI. Os RNFs
que não podem ser automatizados (portabilidade e privacidade) serão
verificados por procedimento manual e revisão de código.

O resultado dos testes servirá como evidência da qualidade do sistema, tanto
para o portfólio quanto para a futura venda do produto.

## 2. Escopo dos testes

### Será testado
- **Regras de negócio de Recursos** (RN01 a RN07): cadastro, edição,
  inativação, ativação e exclusão.
- **Regras de negócio de Horário de funcionamento** (RN08 a RN11):
  cadastro, edição e validações do horário.
- **Regras de negócio de Agendamentos** (RN12 a RN27): realização,
  consulta, cancelamento, conflitos e horários disponíveis.
- **Requisitos não funcionais:**
  - RNF01 – Desempenho (tempo de resposta em ambiente local)
  - RNF02 – Cobertura das regras por testes automatizados no CI
  - RNF03 – Mensagens de erro claras e em português
  - RNF04 – Subida do projeto com um único comando (verificação manual)
  - RNF05 – Coleta apenas de nome e telefone (revisão de código)
  - RNF06 – Armazenamento de datas em UTC

### Não será testado nesta fase
- **Testes de ponta a ponta (E2E):** a Fase 1 não possui interface.
  Os testes E2E serão criados na Fase 2, junto com o frontend.
- **Funcionalidades fora do escopo do PRD** (login, preço, lembretes,
  integrações, multi-tenant etc.): não serão construídas nesta fase.
- **Testes de carga e estresse:** o RNF01 mede o tempo de resposta em
  ambiente local, e não o comportamento com muitos acessos simultâneos.
  Serão avaliados antes da venda para o primeiro cliente.
- **Testes de segurança aprofundados** (ex.: testes de invasão): sem
  login na Fase 1, a superfície de ataque é limitada. O risco de
  identificação apenas por telefone já está registrado na D3 do PRD.
- **Compatibilidade de navegadores e dispositivos:** depende da
  interface, prevista para a Fase 2.

## 3. Estratégia (níveis de teste e ferramentas)
## 4. Ambiente e dados de teste
## 5. Critérios de entrada e de saída
## 6. Riscos
## 7. Casos de teste
