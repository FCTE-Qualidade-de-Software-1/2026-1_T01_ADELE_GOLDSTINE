# Planejamento da Avaliação

A Fase 3 corresponde à etapa de **projetar a avaliação** dentro do Processo de Avaliação de Produto de Software (ISO/IEC 25040). Tendo como entrada a especificação da medição produzida na Fase 2 — os objetivos, questões e métricas definidos pelo método **Goal/Question/Metric (GQM)** para as características de **Confiabilidade** e **Manutenibilidade** — esta fase organiza *como* essas medições serão efetivamente obtidas na Fase 4.

Conforme Solingen e Berghout (1999), o principal artefato da etapa de planejamento de um programa de medição GQM é o **plano de avaliação**, que documenta os procedimentos, os recursos e o cronograma necessários para que a coleta seja executada de forma padronizada, repetível e auditável. É exatamente esse o escopo das seções a seguir.

!!! info "Rastreabilidade entre as fases"
    Este plano não introduz novas métricas. Cada elemento descrito aqui deriva diretamente das métricas e dos critérios de julgamento de **Confiabilidade** e **Manutenibilidade** estabelecidos na Fase 2 e do propósito declarado na Fase 1. A relação explícita é apresentada na [Seção 5 — Consistência entre as Fases 2 e 3](#5-consistencia-entre-as-fases-2-e-3).

---

## 1 - Plano de Avaliação

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Objetivo do plano</b></th>
      <td>Definir o método, os recursos, o ambiente e o cronograma para a coleta das medidas de Confiabilidade e Manutenibilidade do <i>Sua Grade UnB</i>.</td>
    </tr>
    <tr>
      <th><b>Objeto de avaliação</b></th>
      <td>Código-fonte, documentação e instância em execução do <i>Sua Grade UnB</i> (repositório <a href="https://github.com/unb-mds/2023-2-SuaGradeUnB">unb-mds/2023-2-SuaGradeUnB</a>).</td>
    </tr>
    <tr>
      <th><b>Características avaliadas</b></th>
      <td>Confiabilidade (Maturidade e Tolerância a falhas) e Manutenibilidade (Modularidade, Analisabilidade, Modificabilidade e Testabilidade).</td>
    </tr>
    <tr>
      <th><b>Avaliadores</b></th>
      <td>Equipe do projeto (disciplina de Qualidade de Software 1 — FCTE/UnB).</td>
    </tr>
    <tr>
      <th><b>Saída esperada</b></th>
      <td>Dados brutos e consolidados, armazenados no repositório, prontos para a análise e o julgamento da Fase 4.</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 1: Visão geral do plano de avaliação.</figcaption></font>
  </div>
</div>

A avaliação adota uma abordagem **mista**: medições **automáticas/estáticas** sobre o código-fonte (cobertura de testes, acoplamento, tratamento de exceções, logs) combinadas com uma medição **dinâmica e controlada** com usuários (MTBF) e uma medição **de processo** sobre o histórico do repositório (tempo médio de alteração). Essa combinação acompanha a recomendação de Solingen e Berghout (1999) de só recorrer a ferramentas automatizadas quando elas respondem objetivamente à questão, mantendo a coleta manual onde o dado mais valioso vem da observação humana.

---

## 2 - Recursos Necessários e Métodos de Avaliação

### 2.1 - Recursos Necessários para a Avaliação

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left; vertical-align: top;">
    <tr>
      <th><b>Categoria</b></th>
      <th><b>Recurso</b></th>
      <th><b>Finalidade</b></th>
    </tr>
    <tr>
      <td rowspan="2"><b>Ambiente</b></td>
      <td>Repositório local do <i>Sua Grade UnB</i> (código-fonte clonado)</td>
      <td>Base para todas as métricas de análise estática de código (M2.1 — Conf.; M1.1, M2.1 e M4.1 — Manut.)</td>
    </tr>
    <tr>
      <td>Instância em produção (<code>suagradeunb.com.br</code>)</td>
      <td>Ambiente da sessão controlada de uso para medição do MTBF (M1.1 — Conf.)</td>
    </tr>
    <tr>
      <td rowspan="5"><b>Ferramentas</b></td>
      <td><code>grep</code> e módulo <code>ast</code> do Python</td>
      <td>Varredura e contagem de operações críticas cobertas por <code>try/except</code> no backend (M2.1 — Conf.)</td>
    </tr>
    <tr>
      <td><code>pydeps</code></td>
      <td>Geração do grafo de dependências entre módulos Python para análise de acoplamento (M1.1 — Manut.)</td>
    </tr>
    <tr>
      <td><code>coverage</code> + <code>manage.py test</code> (Django test runner)</td>
      <td>Execução da suíte de testes automatizados e contagem de casos de teste do backend (M4.1 — Manut.)</td>
    </tr>
    <tr>
      <td><code>git log</code></td>
      <td>Extração de datas de commits para cruzamento com histórico de <i>issues</i> (M3.1 — Manut.)</td>
    </tr>
    <tr>
      <td>Sentry (<code>sentry-sdk[django]</code> / <code>@sentry/nextjs</code>)</td>
      <td>Verificação e centralização dos registros de log das operações críticas (M2.1 — Manut.)</td>
    </tr>
    <tr>
      <td><b>Plataformas</b></td>
      <td>GitHub Projects</td>
      <td>Coleta de dados de transição de estado das <i>issues</i> para cálculo do tempo de ciclo (M3.1 — Manut.)</td>
    </tr>
    <tr>
      <td><b>Instrumento de coleta</b></td>
      <td>Planilha</td>
      <td>Formulários de coleta e consolidação dos dados brutos de todas as métricas</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 2: Recursos necessários para a avaliação.</figcaption></font>
  </div>
</div>

### 2.2 - Instruções de coleta — Confiabilidade

**M1.1 — Tempo Médio Entre Falhas (MTBF)**

1. Preparar um **roteiro de tarefas** representativas do uso normal: (a) buscar disciplinas por nome de matéria e por professor; (b) selecionar turmas e montar uma grade; (c) gerar a grade horária; (d) exportar a grade em PDF; (e) autenticar com conta Google.
2. Acessar a instância em produção (`https://suagradeunb.com.br/`) e executar cada fluxo do roteiro, iniciando um cronômetro no início da execução e registrando o **tempo total decorrido em horas**.
3. A cada falha ou comportamento inesperado observado, registrar a ocorrência no **formulário de registro de falhas** (descrição, fluxo, horário, avaliador responsável) e somar +1 ao total de falhas.
4. Ao final, calcular a **taxa de falhas** = `(número de falhas) / (tempo da sessão em horas)`, em **problemas/hora**, e comparar com a pontuação de julgamento da Fase 2.

**M2.1 — Percentual de operações críticas com tratamento de exceção**

1. Fixar a lista de **operações críticas**: conexão/consulta ao banco de dados, manipulação de arquivos, manipulação dos dados de *web scraping* do SIGAA, e autenticação/autorização de usuários.
2. Varrer o código-fonte do backend com `grep` e análise de AST via módulo `ast` do Python para **contar todas as ocorrências** dessas operações; no frontend TypeScript, aplicar `grep` de forma equivalente.
3. Classificar manualmente quais ocorrências estão **dentro de um bloco de tratamento de exceção** — `try/except` no backend Python ou `try/catch` no frontend TypeScript — descartando falsos positivos.
4. Calcular `(operações críticas com tratamento de exceção / total de operações críticas) × 100`.
5. Registrar, para cada ocorrência, o arquivo e a linha, garantindo a rastreabilidade entre o dado bruto e a métrica.

### 2.3 - Instruções de coleta — Manutenibilidade

**M1.1 — Nível de independência dos componentes**

1. Levantar as **classes/módulos** do sistema (apps do backend Django e módulos do frontend Next.js).
2. **Identificar as dependências** entre eles por análise dos `import`s; utilizar `pydeps` no backend para gerar o grafo de dependências e apoiar a classificação visual.
3. Classificar como **independente** o componente que não depende de outro componente interno do sistema.
4. Calcular `(componentes independentes / total de componentes)` e comparar com a faixa de julgamento.

**M2.1 — Nível de qualidade dos logs**

1. A partir da lista de operações críticas (mesma do M2.1 de Confiabilidade), montar um **checklist do que deveria ser registrado** para monitorar o status do sistema.
2. Verificar no código-fonte do backend — pelas chamadas de `logger.*` (configuração `LOGGING` do Django) ou pelas integrações com o Sentry — e no frontend — pelas integrações com `@sentry/nextjs` — quantas operações críticas **efetivamente emitem registros**.
3. Calcular `(operações críticas com registro de log / total de operações críticas planejadas para registro)`.

**M3.1 — Tempo médio para realizar uma alteração**

1. Definir o período de coleta (ex.: as *issues* trabalhadas no último marco do projeto).
2. Para cada *issue*, medir o **tempo de ciclo** entre a transição "Em Andamento" → "Concluída" no GitHub Projects; cruzar com as datas dos *commits* relacionados via `git log` para validar a data efetiva de conclusão.
3. Calcular `(soma dos tempos de alteração / número de alterações)` em horas.

**M4.1 — Cobertura de testes**

> A métrica conta **casos de teste automatizados** em relação aos cenários que deveriam existir — não é cobertura de linhas de código.

1. Executar a suíte de testes do **backend** com `coverage run manage.py test` — o *test runner* do Django, já configurado no projeto (`coverage 7.3.2`) e na CI via Codecov — e **contar os casos de teste automatizados** existentes.
2. Verificar a existência de testes automatizados no **frontend** (`web/`) e registrar a cobertura efetiva encontrada.
3. Levantar, nas **especificações/histórias do projeto**, quantos cenários de teste **deveriam** existir.
4. Calcular `(casos de teste automatizados escritos / cenários que deveriam existir)` e comparar com a faixa de julgamento.

---

## 3 - Recursos e Ambiente de Avaliação

### 3.1 - Ambiente de execução

O *Sua Grade UnB* é distribuído via Docker, o que permite reproduzir o ambiente de avaliação de forma idêntica entre os avaliadores. São requisitos para executar e analisar o produto:

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Categoria</b></th>
      <th><b>Recurso</b></th>
      <th><b>Finalidade</b></th>
    </tr>
    <tr>
      <td rowspan="2"><b>Hardware</b></td>
      <td>Máquina com, no mínimo, 8 GB de RAM e acesso à internet</td>
      <td>Subir os contêineres e executar as suítes de teste</td>
    </tr>
    <tr>
      <td>Dispositivos variados (desktop e celular)</td>
      <td>Sessão de uso controlada do MTBF (abordagem <i>Mobile First</i> do produto)</td>
    </tr>
    <tr>
      <td rowspan="4"><b>Software (execução)</b></td>
      <td>Docker e Docker Compose</td>
      <td>Reproduzir o ambiente do backend e do banco PostgreSQL</td>
    </tr>
    <tr>
      <td>Python 3 + Django / Node.js</td>
      <td>Executar backend e frontend localmente</td>
    </tr>
    <tr>
      <td>Git e acesso ao GitHub (repositório e Projects)</td>
      <td>Coleta de M3.1 (histórico) e versionamento dos dados brutos</td>
    </tr>
    <tr>
      <td>Navegador web moderno</td>
      <td>Acesso à instância em produção na sessão de uso</td>
    </tr>
    <tr>
      <td rowspan="5"><b>Software (medição)</b></td>
      <td>Test runner do Django (<code>coverage run manage.py test</code>) + Codecov</td>
      <td>M4.1 — execução e contagem dos casos de teste (backend)</td>
    </tr>
    <tr>
      <td>Sentry (<code>sentry-sdk[django]</code> / <code>@sentry/nextjs</code>) + config <code>LOGGING</code> do Django</td>
      <td>M2.1 (Manutenibilidade) — instrumentação e centralização dos logs</td>
    </tr>
    <tr>
      <td>Análise de <i>imports</i> (apoio opcional: <code>pydeps</code>)</td>
      <td>M1.1 — Independência dos componentes</td>
    </tr>
    <tr>
      <td><code>grep</code> e módulo <code>ast</code> do Python (apoio à inspeção)</td>
      <td>M2.1 (Confiabilidade) — contagem de operações com <code>try</code></td>
    </tr>
    <tr>
      <td>Planilha (Google Sheets / LibreOffice Calc)</td>
      <td>Formulários de coleta e consolidação dos dados brutos</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 3: Recursos de hardware e software para a avaliação.</figcaption></font>
  </div>
</div>

### 3.2 - Massa de dados

- **MTBF (M1.1):** a sessão de uso depende de um **conjunto representativo de consultas** (nomes de disciplinas e professores reais ofertados pela UnB). Como os dados são obtidos por *web scraping* periódico do SIGAA, a própria base em produção fornece a massa necessária; o roteiro de tarefas fixa as consultas a serem realizadas para garantir comparabilidade entre os usuários.
- **Cobertura de testes (M4.1):** utiliza a suíte de testes já existente no repositório como massa de entrada, não exigindo geração de dados adicionais.

### 3.3 - Conhecimento exigido dos avaliadores

- **Domínio da aplicação:** conhecimento do processo de montagem de grade horária e matrícula na UnB — pré-requisito para conduzir a sessão de MTBF e interpretar falhas.
- **Conhecimento técnico:** noções de Python/Django e TypeScript/Next.js e familiaridade com Git, Docker e com as ferramentas de medição, necessárias para as métricas estáticas e de processo.
- **Nível de informática dos usuários da sessão:** usuários finais (estudantes) com conhecimento básico de navegação web, refletindo o público-alvo real do produto.

---

## 4 - Cronograma de Avaliação

O cronograma abaixo organiza a execução da Fase 4, distribuindo as atividades entre os integrantes e permitindo a coleta **em paralelo** por característica. As métricas estáticas e de processo são independentes entre si e podem ser conduzidas simultaneamente.

<div align="center">
  <table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: left;">
    <tr>
      <th><b>Etapa</b></th>
      <th><b>Atividade</b></th>
      <th><b>Métricas</b></th>
      <th><b>Responsáveis</b></th>
      <th><b>Período</b></th>
    </tr>
    <tr>
      <td>1</td>
      <td>Preparação do ambiente (clone do repositório, suítes de teste) e dos instrumentos (formulários)</td>
      <td>Todas</td>
      <td>Responsável: Equipe</td>
      <td>Dias 1–2</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Coleta das métricas estáticas de código</td>
      <td>Conf. M2.1; Manut. M1.1 e M2.1</td>
      <td>Responsável: -</td>
      <td>Dias 1–3</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Coleta da métrica de processo (histórico de <i>issues</i>)</td>
      <td>Manut. M3.1</td>
      <td>Responsável: -</td>
      <td>Dias 1–2</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Execução das suítes e coleta de cobertura de testes</td>
      <td>Manut. M4.1</td>
      <td>Responsável: -</td>
      <td>Dias 1–3</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Consolidação dos dados brutos no repositório e revisão</td>
      <td>Todas</td>
      <td>Responsável: Equipe</td>
      <td>Dias 4–5</td>
    </tr>
  </table>
  <div style="margin-top: 8px; text-align: center;">
    <font size="4"><figcaption>Tabela 4: Cronograma de execução da avaliação (Fase 4).</figcaption></font>
  </div>
</div>

---

## Bibliografia

- ISO/IEC 25040:2011 — *Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — Evaluation process*. Genebra: ISO, 2011.
- ISO/IEC 25010:2011 — *Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — System and software quality models*. Genebra: ISO, 2011.
- SOLINGEN, Rini van; BERGHOUT, Egon. **The Goal/Question/Metric Method: a practical guide for quality improvement of software development**. Londres: McGraw-Hill, 1999.
- BASILI, Victor R.; CALDIERA, Gianluca; ROMBACH, H. Dieter. The Goal Question Metric Approach. In: *Encyclopedia of Software Engineering*. Nova York: John Wiley & Sons, 1994.
- RAMOS, Cristiane Soares. **Processo de Avaliação de Produto de Software**. Slides da disciplina Qualidade de Software 1, Faculdade de Ciências e Tecnologias em Engenharia (FCTE), Universidade de Brasília, 2025/2.

## Histórico de Versão

| Versão | Data       | Descrição                          | Autor(es) |
|:------:|:-----------|:-----------------------------------|:----------|
| 1.0    | 2026-06-06 | Elaboração do plano de avaliação (método, recursos, cronograma e consistência) | Caio Felipe e Gabriel Magioli |
