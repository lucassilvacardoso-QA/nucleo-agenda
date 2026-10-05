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
