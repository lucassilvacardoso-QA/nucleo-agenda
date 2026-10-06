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

## 3. Estratégia

### 3.1 Níveis de teste

- **Unitário (Vitest):** valida a **lógica pura** das regras, isolada do
  banco de dados e da API: cálculos de horário, comparações, validações de
  formato e tamanho. É onde ficam as **variações e os casos de borda**,
  porque os testes são rápidos e baratos.
- **API (Playwright):** valida o **comportamento real** do sistema, pelo
  caminho completo: requisição → API → banco → resposta. Cobre as regras
  que dependem de dados já salvos (situação do recurso, agendamentos
  existentes) e confirma que toda regra chega até quem chama a API, com
  a mensagem correta.
- **Manual / revisão:** para requisitos que não podem ser verificados por
  teste automatizado (procedimento de instalação e revisão de código).

**Princípio (pirâmide de testes):** toda regra tem pelo menos um caso
positivo e um negativo na API. Quando a regra é lógica pura, as variações
e os casos de borda ficam no unitário, para manter a suíte rápida.

**Controle do tempo:** as regras que dependem do "momento atual" (RN18,
RN23, RN25 e RN26) serão testadas com o relógio controlado pelo teste,
para que os resultados não dependam do dia ou da hora em que a suíte roda.

### 3.2 Distribuição das regras de negócio

| Regra | Unitário | API | Observação |
|---|---|---|---|
| RN01 – Nome de recurso único | ✅ | ✅ | Unitário: comparação (maiúsculas e espaços). API: existência de outro recurso |
| RN02 – Dados do recurso | ✅ | ✅ | Unitário: limites de tamanho |
| RN03 – Situação inicial | | ✅ | |
| RN04 – Edição com agendamentos | | ✅ | |
| RN05 – Mudança para a mesma situação | | ✅ | |
| RN06 – Inativação com agendamentos | | ✅ | |
| RN07 – Exclusão de recurso | | ✅ | |
| RN08 – Formato do horário | ✅ | ✅ | Unitário: múltiplos de 30 min |
| RN09 – Início antes do fim | ✅ | ✅ | |
| RN10 – Edição do horário com agendamentos | | ✅ | |
| RN11 – Recurso sem horário | | ✅ | |
| RN12 – Fuso horário | ✅ | ✅ | Unitário: conversão de horários |
| RN13 – Confirmação automática | | ✅ | |
| RN14 – Recurso ativo | | ✅ | |
| RN15 – Dentro do horário de funcionamento | ✅ | ✅ | Unitário: limites do intervalo |
| RN16 – Duração | ✅ | ✅ | Unitário: 29 min, 30 min, 4 h, 4h01 |
| RN17 – Múltiplos de 30 minutos | ✅ | ✅ | |
| RN18 – Antecedência | ✅ | ✅ | Relógio controlado |
| RN19 – Conflito de horário | ✅ | ✅ | Unitário: todas as formas de sobreposição |
| RN20 – Dados do cliente | ✅ | ✅ | Unitário: tamanho do nome e formato do telefone |
| RN21 – Limite por cliente | | ✅ | |
| RN22 – Identificação do cliente | | ✅ | |
| RN23 – Prazo de cancelamento pelo cliente | ✅ | ✅ | Relógio controlado |
| RN24 – Cancelamento pelo dono | | ✅ | |
| RN25 – Cancelamento inválido | ✅ | ✅ | Relógio controlado |
| RN26 – Horários disponíveis | ✅ | ✅ | Unitário: cálculo da grade |
| RN27 – Consulta por período | ✅ | ✅ | Unitário: limite de 31 dias |

### 3.3 Requisitos não funcionais

| Requisito | Como será verificado |
|---|---|
| RNF01 – Desempenho | Automatizado: medição do tempo de resposta nos testes de API |
| RNF02 – Qualidade | Pipeline do GitHub Actions + rastreabilidade regra → caso de teste |
| RNF03 – Mensagens de erro | Automatizado: os testes de API verificam a mensagem de cada recusa |
| RNF04 – Portabilidade | Manual: subir o projeto do zero seguindo o README |
| RNF05 – Privacidade (LGPD) | Revisão: conferir os campos coletados no banco e na API |
| RNF06 – Armazenamento em UTC | Automatizado: teste de API que confere o valor gravado no banco |

### 3.4 Ferramentas
- **Vitest:** testes unitários
- **Playwright:** testes de API
- **GitHub Actions:** execução automática dos testes a cada push
  
## 4. Ambiente e dados de teste

### 4.1 Ambiente
- **Local:** API e banco PostgreSQL executados via Docker Compose, com um
  banco **exclusivo para testes**, separado do banco de desenvolvimento.
- **CI:** os mesmos testes executados no GitHub Actions a cada push, com o
  mesmo Docker Compose, para que o resultado local e o do CI sejam iguais.

### 4.2 Dados de teste
- **Cada teste cria os próprios dados** pela API (recursos, horários e
  agendamentos) e não depende de dados criados por outro teste.
- **O banco é limpo entre os testes**, garantindo que cada um comece do
  mesmo ponto de partida.
- **Dados fictícios:** nomes e telefones usados nos testes são inventados
  (ex.: telefone 11900000001), sem nenhum dado pessoal real (LGPD).
- **Datas relativas:** os agendamentos usam datas calculadas a partir do
  relógio controlado (ex.: "amanhã às 10:00"), e não datas fixas, que
  ficariam no passado com o tempo.

## 5. Critérios de entrada e de saída

### 5.1 Critérios de entrada (quando os testes podem começar)
- PRD da Fase 1 aprovado.
- Funcionalidade implementada e disponível na branch de desenvolvimento.
- Ambiente de testes subindo com Docker Compose sem erros.

### 5.2 Critérios de saída (quando os testes da Fase 1 estão concluídos)
- Todas as regras de negócio (RN01 a RN27) com casos de teste
  rastreados no plano.
- 100% dos testes automatizados passando no GitHub Actions.
- Nenhum defeito de severidade **crítica** ou **alta** em aberto.
- RNF04 e RNF05 verificados manualmente, com o resultado registrado.

## 6. Riscos

| Risco | Impacto | Mitigação |
|---|---|---|
| Testes instáveis por depender do relógio ou do fuso do computador | Testes passam ou falham conforme o horário em que rodam | Relógio controlado nos testes; datas relativas; horários em UTC (RNF06) |
| Testes dependentes entre si | Um teste falha por causa de outro, dificultando encontrar o problema | Cada teste cria os próprios dados; banco limpo entre os testes |
| Diferença entre o ambiente local e o CI | Teste passa local e falha no CI (ou o contrário) | Mesmo Docker Compose nos dois ambientes; versões travadas no `package-lock.json` |
| Medição de desempenho variável (RNF01) | Tempo de resposta muda conforme a máquina | Medir em várias execuções e considerar o percentil 95, e não uma medição isolada |
| Mudança nas regras durante o desenvolvimento | Casos de teste ficam desatualizados | Toda alteração em uma RN atualiza o plano no mesmo commit |
| Tempo disponível limitado (projeto pessoal) | Cobertura incompleta ao fim da fase | Priorizar as regras de maior risco: RN19, RN15, RN18 e RN06 |

## 7. Casos de teste

### 7.1 Convenções

- **Relógio controlado:** em todos os casos, "agora" é **D 10:00** (dia D, às 10:00, horário de Brasília). D+1 é o dia seguinte, D+30 é 30 dias depois, e assim por diante.
- **Recurso padrão:** "Quadra 1", ativo, com horário de funcionamento de **08:00 às 22:00 em todos os dias da semana**, salvo indicação na pré-condição.
- **Cliente padrão:** nome "Cliente Teste", telefone 11900000001.
- **Casos unitários** descrevem a regra como lógica pura: a pré-condição e a entrada são os valores passados à validação, e o resultado esperado é o retorno dela (válido/inválido, há/não há conflito).
- **Casos de API** descrevem o comportamento do sistema completo. Todo caso negativo de API também verifica a mensagem de erro (RNF03).

### 7.2 Recursos

| ID | Regra | Cenário | Pré-condição | Entrada | Resultado esperado | Tipo | Nível |
|---|---|---|---|---|---|---|---|
| CT-001 | RN01 | Cadastrar recurso com nome diferente dos existentes | "Quadra 1" cadastrada | Cadastrar "Quadra 2" | Recurso criado | Positivo | API |
| CT-002 | RN01 | Cadastrar recurso com nome já existente | "Quadra 1" cadastrada | Cadastrar "Quadra 1" | Cadastro recusado | Negativo | API |
| CT-003 | RN01 | Nomes que diferem só em maiúsculas e minúsculas | – | Comparar "Quadra 1" e "quadra 1" | Nomes iguais | Borda | Unitário |
| CT-004 | RN01 | Nomes que diferem só por espaços no início e no fim | – | Comparar "Quadra 1" e "  Quadra 1  " | Nomes iguais | Borda | Unitário |
| CT-005 | RN01 | Nomes que diferem só por espaços repetidos | – | Comparar "Quadra 1" e "Quadra   1" | Nomes iguais | Borda | Unitário |
| CT-006 | RN01 | Nomes que diferem pela ausência de espaço | – | Comparar "Quadra 1" e "Quadra1" | Nomes diferentes | Borda | Unitário |
| CT-007 | RN01 | Cadastrar com nome de recurso inativo | "Quadra 1" inativa | Cadastrar "Quadra 1" | Cadastro recusado | Negativo | API |
| CT-008 | RN01 | Editar recurso para nome já existente | "Quadra 1" e "Quadra 2" cadastradas | Renomear "Quadra 2" para "Quadra 1" | Edição recusada | Negativo | API |
| CT-009 | RN01 | Editar recurso mantendo o próprio nome | "Quadra 1" cadastrada | Editar a descrição da "Quadra 1", mantendo o nome | Edição aceita (o recurso não conflita consigo mesmo) | Borda | API |
| CT-010 | RN02 | Nome com o tamanho mínimo | – | Nome com 2 caracteres | Válido | Borda | Unitário |
| CT-011 | RN02 | Nome abaixo do mínimo | – | Nome com 1 caractere | Inválido | Borda | Unitário |
| CT-012 | RN02 | Nome com o tamanho máximo | – | Nome com 60 caracteres | Válido | Borda | Unitário |
| CT-013 | RN02 | Nome acima do máximo | – | Nome com 61 caracteres | Inválido | Borda | Unitário |
| CT-014 | RN02 | Nome só com espaços | – | Nome "   " | Inválido | Negativo | Unitário |
| CT-015 | RN02 | Cadastro sem nome | – | Cadastrar recurso sem o campo nome | Cadastro recusado | Negativo | API |
| CT-016 | RN02 | Recurso sem descrição | – | Descrição não informada | Válido | Positivo | Unitário |
| CT-017 | RN02 | Descrição com o tamanho máximo | – | Descrição com 200 caracteres | Válido | Borda | Unitário |
| CT-018 | RN02 | Descrição acima do máximo | – | Descrição com 201 caracteres | Inválido | Borda | Unitário |
| CT-019 | RN03 | Situação do recurso recém-cadastrado | – | Cadastrar "Quadra 1" | Recurso criado com situação ativo | Positivo | API |
| CT-020 | RN04 | Editar recurso com agendamento futuro ativo | "Quadra 1" com agendamento ativo em D+1 10:00–11:00 | Renomear para "Quadra Central" | Edição aceita; agendamento continua ativo | Positivo | API |
| CT-021 | RN05 | Inativar recurso já inativo | "Quadra 1" inativa | Inativar "Quadra 1" | Recusado, informando que já está inativo | Negativo | API |
| CT-022 | RN05 | Ativar recurso já ativo | "Quadra 1" ativa | Ativar "Quadra 1" | Recusado, informando que já está ativo | Negativo | API |
| CT-023 | RN05 | Inativar recurso ativo | "Quadra 1" ativa, sem agendamentos | Inativar "Quadra 1" | Recurso inativo | Positivo | API |
| CT-024 | RN05 | Ativar recurso inativo | "Quadra 1" inativa | Ativar "Quadra 1" | Recurso ativo | Positivo | API |
| CT-025 | RN06 | Inativar recurso com agendamento futuro ativo | "Quadra 1" com agendamento ativo em D+1 10:00–11:00 | Inativar "Quadra 1" | Recusado, listando o agendamento a cancelar | Negativo | API |
| CT-026 | RN06 | Inativar recurso só com agendamento futuro cancelado | "Quadra 1" com agendamento cancelado em D+1 10:00–11:00 | Inativar "Quadra 1" | Recurso inativo | Borda | API |
| CT-027 | RN06 | Inativar recurso só com agendamentos passados | "Quadra 1" com agendamento ativo em D-1 10:00–11:00 | Inativar "Quadra 1" | Recurso inativo | Borda | API |
| CT-028 | RN07 | Excluir recurso sem nenhum agendamento | "Quadra 1" sem agendamentos | Excluir "Quadra 1" | Recurso excluído | Positivo | API |
| CT-029 | RN07 | Excluir recurso com agendamento cancelado | "Quadra 1" com 1 agendamento cancelado | Excluir "Quadra 1" | Recusado, orientando a inativar | Negativo | API |
| CT-030 | RN07 | Excluir recurso com agendamento passado | "Quadra 1" com agendamento em D-1 | Excluir "Quadra 1" | Recusado, orientando a inativar | Negativo | API |

### 7.3 Horário de funcionamento

| ID | Regra | Cenário | Pré-condição | Entrada | Resultado esperado | Tipo | Nível |
|---|---|---|---|---|---|---|---|
| CT-031 | RN08 | Cadastrar horário semanal válido | "Quadra 1" sem horário | Seg–sex 08:00–18:00; sáb e dom fechado | Horário cadastrado | Positivo | API |
| CT-032 | RN08 | Horário que não é múltiplo de 30 minutos | – | Início 08:15 | Inválido | Negativo | Unitário |
| CT-033 | RN08 | Horário em meia hora | – | 08:30–18:30 | Válido | Positivo | Unitário |
| CT-034 | RN08 | Dois intervalos no mesmo dia | "Quadra 1" sem horário | Segunda 08:00–12:00 e 14:00–18:00 | Cadastro recusado | Negativo | API |
| CT-035 | RN09 | Início depois do fim | – | 18:00–08:00 | Inválido | Negativo | Unitário |
| CT-036 | RN09 | Início igual ao fim | – | 08:00–08:00 | Inválido | Borda | Unitário |
| CT-037 | RN09 | Menor intervalo possível | – | 08:00–08:30 | Válido | Borda | Unitário |
| CT-038 | RN10 | Reduzir horário deixando agendamento ativo de fora | Agendamento ativo em D+1 20:00–21:00 | Alterar o fim do funcionamento de D+1 para 20:00 | Recusado, listando o agendamento a cancelar | Negativo | API |
| CT-039 | RN10 | Reduzir horário até o fim exato de um agendamento | Agendamento ativo em D+1 20:00–21:00 | Alterar o fim do funcionamento de D+1 para 21:00 | Edição aceita | Borda | API |
| CT-040 | RN10 | Reduzir horário com agendamento cancelado de fora | Agendamento cancelado em D+1 20:00–21:00 | Alterar o fim do funcionamento de D+1 para 20:00 | Edição aceita | Borda | API |
| CT-041 | RN10 | Fechar um dia com agendamento ativo | Agendamento ativo em D+1 10:00–11:00 | Marcar o dia da semana de D+1 como fechado | Recusado, listando o agendamento a cancelar | Negativo | API |
| CT-042 | RN11 | Agendar recurso sem horário de funcionamento | "Quadra 1" sem horário cadastrado | Agendar D+1 10:00–11:00 | Agendamento recusado | Negativo | API |

### 7.4 Agendamentos

| ID | Regra | Cenário | Pré-condição | Entrada | Resultado esperado | Tipo | Nível |
|---|---|---|---|---|---|---|---|
| CT-043 | RN12 | Horário devolvido no fuso de Brasília | – | Agendar D+1 10:00–11:00 | Resposta mostra 10:00–11:00 | Positivo | API |
| CT-044 | RN12 | Conversão de Brasília para UTC | – | D+1 10:00 (Brasília) | D+1 13:00 (UTC) | Positivo | Unitário |
| CT-045 | RN13 | Confirmação automática | – | Agendar D+1 10:00–11:00 | Agendamento criado com situação ativo, sem aprovação | Positivo | API |
| CT-046 | RN14 | Agendar recurso inativo | "Quadra 1" inativa | Agendar D+1 10:00–11:00 | Agendamento recusado | Negativo | API |
| CT-047 | RN15 | Agendamento no início do funcionamento | Funcionamento 08:00–22:00 | 08:00–09:00 | Dentro do horário | Borda | Unitário |
| CT-048 | RN15 | Agendamento começando antes do funcionamento | Funcionamento 08:00–22:00 | 07:30–08:30 | Fora do horário | Borda | Unitário |
| CT-049 | RN15 | Agendamento no fim do funcionamento | Funcionamento 08:00–22:00 | 21:00–22:00 | Dentro do horário | Borda | Unitário |
| CT-050 | RN15 | Agendamento terminando depois do funcionamento | Funcionamento 08:00–22:00 | 21:30–22:30 | Fora do horário | Borda | Unitário |
| CT-051 | RN15 | Agendar em dia fechado | Dia da semana de D+1 marcado como fechado | Agendar D+1 10:00–11:00 | Agendamento recusado | Negativo | API |
| CT-052 | RN16 | Duração mínima | – | 30 minutos | Válido | Borda | Unitário |
| CT-053 | RN16 | Duração abaixo do mínimo | – | 29 minutos | Inválido | Borda | Unitário |
| CT-054 | RN16 | Duração máxima | – | 4 horas | Válido | Borda | Unitário |
| CT-055 | RN16 | Duração acima do máximo | – | 4 horas e 1 minuto | Inválido | Borda | Unitário |
| CT-056 | RN16 | Fim antes do início | – | 11:00–10:00 | Inválido | Negativo | Unitário |
| CT-057 | RN16 | Agendamento acima do máximo pela API | – | Agendar D+1 10:00–14:30 | Agendamento recusado | Negativo | API |
| CT-058 | RN17 | Início e fim em meia hora | – | 10:30–11:00 | Válido | Positivo | Unitário |
| CT-059 | RN17 | Início fora da grade de 30 minutos | – | Início 10:15 | Inválido | Negativo | Unitário |
| CT-060 | RN17 | Fim fora da grade de 30 minutos | – | Fim 11:10 | Inválido | Negativo | Unitário |
| CT-061 | RN17 | Agendamento fora da grade pela API | – | Agendar D+1 10:15–11:15 | Agendamento recusado | Negativo | API |
| CT-062 | RN18 | Início no passado | Agora: D 10:00 | Início D 09:30 | Inválido | Negativo | Unitário |
| CT-063 | RN18 | Início exatamente agora | Agora: D 10:00 | Início D 10:00 | Válido | Borda | Unitário |
| CT-064 | RN18 | Início logo depois de agora | Agora: D 10:00 | Início D 10:30 | Válido | Positivo | Unitário |
| CT-065 | RN18 | Antecedência máxima | Agora: D 10:00 | Início D+30 10:00 | Válido | Borda | Unitário |
| CT-066 | RN18 | Acima da antecedência máxima | Agora: D 10:00 | Início D+30 10:30 | Inválido | Borda | Unitário |
| CT-067 | RN18 | Agendar no passado pela API | Agora: D 10:00 | Agendar D 09:00–09:30 | Agendamento recusado | Negativo | API |
| CT-068 | RN19 | Novo agendamento começa quando o existente termina | Existente 10:00–11:00 | Novo 11:00–12:00 | Sem conflito | Borda | Unitário |
| CT-069 | RN19 | Novo agendamento termina quando o existente começa | Existente 10:00–11:00 | Novo 09:00–10:00 | Sem conflito | Borda | Unitário |
| CT-070 | RN19 | Sobreposição no fim do existente | Existente 10:00–11:00 | Novo 10:30–11:30 | Há conflito | Negativo | Unitário |
| CT-071 | RN19 | Sobreposição no início do existente | Existente 10:00–11:00 | Novo 09:30–10:30 | Há conflito | Negativo | Unitário |
| CT-072 | RN19 | Novo agendamento dentro do existente | Existente 10:00–11:00 | Novo 10:00–10:30 | Há conflito | Negativo | Unitário |
| CT-073 | RN19 | Novo agendamento envolve o existente | Existente 10:00–11:00 | Novo 09:00–12:00 | Há conflito | Negativo | Unitário |
| CT-074 | RN19 | Mesmo horário do existente | Existente 10:00–11:00 | Novo 10:00–11:00 | Há conflito | Negativo | Unitário |
| CT-075 | RN19 | Horários distantes | Existente 10:00–11:00 | Novo 13:00–14:00 | Sem conflito | Positivo | Unitário |
| CT-076 | RN19 | Mesmo horário em outro recurso | "Quadra 1" com agendamento ativo em D+1 10:00–11:00 | Agendar "Quadra 2" em D+1 10:00–11:00 | Agendamento criado | Positivo | API |
| CT-077 | RN19 | Mesmo horário de um agendamento cancelado | Agendamento cancelado em D+1 10:00–11:00 | Agendar D+1 10:00–11:00 | Agendamento criado | Borda | API |
| CT-078 | RN19 | Sobreposição pela API | Agendamento ativo em D+1 10:00–11:00 | Agendar D+1 10:30–11:30 | Agendamento recusado | Negativo | API |
| CT-079 | RN19 | Agendamento encostado pela API | Agendamento ativo em D+1 10:00–11:00 | Agendar D+1 11:00–12:00 | Agendamento criado | Borda | API |
| CT-080 | RN20 | Nome do cliente com o tamanho mínimo | – | Nome com 2 caracteres | Válido | Borda | Unitário |
| CT-081 | RN20 | Nome do cliente abaixo do mínimo | – | Nome com 1 caractere | Inválido | Borda | Unitário |
| CT-082 | RN20 | Nome do cliente com o tamanho máximo | – | Nome com 100 caracteres | Válido | Borda | Unitário |
| CT-083 | RN20 | Nome do cliente acima do máximo | – | Nome com 101 caracteres | Inválido | Borda | Unitário |
| CT-084 | RN20 | Celular com máscara | – | "(11) 90000-0001" | Válido (11 dígitos) | Positivo | Unitário |
| CT-085 | RN20 | Telefone fixo | – | "1130000001" | Válido (10 dígitos) | Borda | Unitário |
| CT-086 | RN20 | Telefone com dígitos a menos | – | "113000000" (9 dígitos) | Inválido | Borda | Unitário |
| CT-087 | RN20 | Telefone com dígitos a mais | – | "119000000011" (12 dígitos) | Inválido | Borda | Unitário |
| CT-088 | RN20 | Telefone com letras | – | "abc" | Inválido | Negativo | Unitário |
| CT-089 | RN20 | Agendamento sem telefone | – | Agendar sem o campo telefone | Agendamento recusado | Negativo | API |
| CT-090 | RN21 | Segundo agendamento do mesmo cliente | 1 agendamento futuro ativo do telefone na "Quadra 1" | Novo agendamento na "Quadra 1" | Agendamento criado | Positivo | API |
| CT-091 | RN21 | Terceiro agendamento do mesmo cliente | 2 agendamentos futuros ativos do telefone na "Quadra 1" | Novo agendamento na "Quadra 1" | Agendamento recusado | Borda | API |
| CT-092 | RN21 | Limite atingido, mas em outro recurso | 2 agendamentos futuros ativos do telefone na "Quadra 1" | Novo agendamento na "Quadra 2" | Agendamento criado | Positivo | API |
| CT-093 | RN21 | Um dos agendamentos foi cancelado | 2 agendamentos futuros do telefone na "Quadra 1", 1 deles cancelado | Novo agendamento na "Quadra 1" | Agendamento criado | Borda | API |
| CT-094 | RN21 | Agendamentos anteriores já passaram | 2 agendamentos ativos do telefone na "Quadra 1", ambos em D-1 | Novo agendamento na "Quadra 1" | Agendamento criado | Borda | API |
| CT-095 | RN22 | Consultar com o telefone correto | Agendamento ativo do telefone 11900000001 | Consultar informando 11900000001 | Agendamento retornado | Positivo | API |
| CT-096 | RN22 | Consultar com outro telefone | Agendamento ativo do telefone 11900000001 | Consultar informando 11900000002 | Consulta recusada, sem expor dados do agendamento | Negativo | API |
| CT-097 | RN22 | Cancelar com outro telefone | Agendamento ativo do telefone 11900000001 | Cancelar informando 11900000002 | Cancelamento recusado | Negativo | API |
| CT-098 | RN22 | Consultar com o telefone em outro formato | Agendamento ativo do telefone 11900000001 | Consultar informando "(11) 90000-0001" | Agendamento retornado | Borda | API |
| CT-099 | RN23 | Cliente cancela com folga | Agendamento em D 14:00; agora: D 11:00 | Cliente cancela | Permitido | Positivo | Unitário |
| CT-100 | RN23 | Cliente cancela exatamente 2 horas antes | Agendamento em D 14:00; agora: D 12:00 | Cliente cancela | Permitido | Borda | Unitário |
| CT-101 | RN23 | Cliente cancela 1 minuto depois do prazo | Agendamento em D 14:00; agora: D 12:01 | Cliente cancela | Recusado | Borda | Unitário |
| CT-102 | RN23 | Cliente cancela fora do prazo pela API | Agendamento em D 11:30; agora: D 10:00 | Cliente cancela | Cancelamento recusado | Negativo | API |
| CT-103 | RN24 | Dono cancela fora do prazo do cliente | Agendamento em D 11:00; agora: D 10:00 | Dono cancela | Agendamento cancelado | Positivo | API |
| CT-104 | RN24 | Dono cancela 1 minuto antes do início | Agendamento em D 11:00; agora: D 10:59 | Dono cancela | Agendamento cancelado | Borda | API |
| CT-105 | RN25 | Cancelar agendamento já cancelado | Agendamento cancelado em D+1 10:00–11:00 | Cancelar novamente | Cancelamento recusado | Negativo | API |
| CT-106 | RN25 | Cancelar agendamento que já começou | Agendamento em D 09:30–10:30; agora: D 10:00 | Cancelar | Inválido | Negativo | Unitário |
| CT-107 | RN25 | Cancelar no minuto exato do início | Agendamento em D 10:00–11:00; agora: D 10:00 | Cancelar | Inválido | Borda | Unitário |
| CT-108 | RN25 | Dono tenta cancelar agendamento que já começou | Agendamento em D 09:30–10:30; agora: D 10:00 | Dono cancela | Cancelamento recusado | Negativo | API |
| CT-109 | RN26 | Grade completa de um dia livre | Funcionamento 08:00–22:00; sem agendamentos em D+1 | Consultar D+1 | 08:00, 08:30 … 21:30 (28 horários) | Positivo | Unitário |
| CT-110 | RN26 | Horários ocupados saem da grade | Agendamento ativo em D+1 10:00–11:00 | Consultar D+1 | Grade sem 10:00 e 10:30 | Positivo | Unitário |
| CT-111 | RN26 | Agendamento cancelado não ocupa a grade | Agendamento cancelado em D+1 10:00–11:00 | Consultar D+1 | Grade inclui 10:00 e 10:30 | Borda | Unitário |
| CT-112 | RN26 | Horários já passados no dia atual | Agora: D 10:00 | Consultar D | Grade começa em 10:00 | Borda | Unitário |
| CT-113 | RN26 | Dia fechado | Dia da semana de D+1 fechado | Consultar D+1 | Grade vazia | Negativo | Unitário |
| CT-114 | RN26 | Dia além da antecedência máxima | Agora: D 10:00 | Consultar D+31 | Grade vazia | Borda | Unitário |
| CT-115 | RN26 | Recurso inativo | "Quadra 1" inativa | Consultar D+1 | Grade vazia | Negativo | API |
| CT-116 | RN26 | Recurso sem horário de funcionamento | "Quadra 1" sem horário | Consultar D+1 | Grade vazia | Negativo | API |
| CT-117 | RN27 | Período com o tamanho máximo | – | Consultar de D a D+30 (31 dias) | Agendamentos do período retornados | Borda | API |
| CT-118 | RN27 | Período acima do máximo | – | Consultar de D a D+31 (32 dias) | Consulta recusada | Borda | API |
| CT-119 | RN27 | Data inicial depois da final | – | Consultar de D+5 a D | Consulta recusada | Negativo | API |
| CT-120 | RN27 | Período de um único dia | – | Consultar de D+1 a D+1 | Agendamentos do dia retornados | Borda | API |

### 7.5 Requisitos não funcionais

| ID | Requisito | Cenário | Pré-condição | Procedimento | Resultado esperado | Tipo | Nível |
|---|---|---|---|---|---|---|---|
| CT-121 | RNF01 | Tempo de resposta | Ambiente local via Docker Compose | Executar 100 requisições de agendamento e de consulta | Percentil 95 até 500 ms | Positivo | API |
| CT-122 | RNF02 | Execução no CI | Repositório com os testes | Fazer um push | Pipeline executa todos os testes; toda RN tem caso de teste rastreado | Positivo | Manual |
| CT-123 | RNF03 | Mensagem de erro clara | Agendamento ativo em D+1 10:00–11:00 | Agendar D+1 10:30–11:30 | Mensagem em português indicando o conflito de horário (RN19) | Negativo | API |
| CT-124 | RNF04 | Subir o projeto do zero | Máquina com Docker instalado | Clonar o repositório e seguir o README | API responde com um único comando | Positivo | Manual |
| CT-125 | RNF05 | Dados pessoais coletados | – | Revisar os campos de cliente na API e no banco | Apenas nome e telefone | Positivo | Revisão |
| CT-126 | RNF06 | Armazenamento em UTC | – | Agendar D+1 10:00 (Brasília) e consultar o banco | Valor gravado: D+1 13:00 UTC | Positivo | API |

### 7.6 Matriz de rastreabilidade

| Regra | Casos de teste |
|---|---|
| RN01 | CT-001 a CT-009 |
| RN02 | CT-010 a CT-018 |
| RN03 | CT-019 |
| RN04 | CT-020 |
| RN05 | CT-021 a CT-024 |
| RN06 | CT-025 a CT-027 |
| RN07 | CT-028 a CT-030 |
| RN08 | CT-031 a CT-034 |
| RN09 | CT-035 a CT-037 |
| RN10 | CT-038 a CT-041 |
| RN11 | CT-042 |
| RN12 | CT-043, CT-044 |
| RN13 | CT-045 |
| RN14 | CT-046 |
| RN15 | CT-047 a CT-051 |
| RN16 | CT-052 a CT-057 |
| RN17 | CT-058 a CT-061 |
| RN18 | CT-062 a CT-067 |
| RN19 | CT-068 a CT-079 |
| RN20 | CT-080 a CT-089 |
| RN21 | CT-090 a CT-094 |
| RN22 | CT-095 a CT-098 |
| RN23 | CT-099 a CT-102 |
| RN24 | CT-103, CT-104 |
| RN25 | CT-105 a CT-108 |
| RN26 | CT-109 a CT-116 |
| RN27 | CT-117 a CT-120 |
| RNF01 a RNF06 | CT-121 a CT-126 |



